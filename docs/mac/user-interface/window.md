---
title: Windows
description: "В этой статье рассматривается работа с windows и панелей в приложении Xamarin.Mac. Здесь описывается создание windows и панелей в Xcode и интерфейс построитель, загружая их из раскадровки и файлы .xib и работа с ними программным способом."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f483fcfa9dfca1eb476ceab2b67e7a03bf4b6354
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="windows"></a>Windows

_В этой статье рассматривается работа с windows и панелей в приложении Xamarin.Mac. Здесь описывается создание windows и панелей в Xcode и интерфейс построитель, загружая их из раскадровки и файлы .xib и работа с ними программным способом._

При работе с C# и .NET в приложении Xamarin.Mac, имеют доступ к одной Windows и панелями, работающий в *Objective-C* и *Xcode* does. Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать в Xcode _интерфейс построителя_ для создания и поддержания окон и панелей (или их при необходимости создать непосредственно в код C#).

В зависимости от его цели приложения Xamarin.Mac могут представлять одно или несколько окон на экране для управления и координации он отображает данные и работает с. Ниже перечислены основные функции windows

1. Чтобы обеспечить область, в котором представления и элементы управления можно разместить и управлять.
2. Чтобы принять и реагирования на события в ответ на взаимодействие пользователя с помощью клавиатуры и мыши.

Windows можно в состоянии без режима (например, текстовый редактор, который может иметь одновременно открыть несколько документов) либо модальное (например, в диалоговом окне экспорта, необходимо отменить перед продолжением приложения).

Панели — это особый вид окна (подкласс базового `NSWindow` класса), как правило, которые обслуживают вспомогательные функции в приложении, таком как программа windows как инспекторы текстовый формат и системы палитра цветов.

[![](window-images/intro01.png "Редактирование в Xcode окна")](window-images/intro01.png#lightbox)

В этой статье мы обсудим основы работы с окнами и панелей в приложении Xamarin.Mac. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` команд используется для передачи до классов C# Objective-C-объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Введение в Windows

Как уже говорилось выше, окно предоставляет область, в котором представления и элементы управления можно разместить и управлять и реагирует на события, основанных на взаимодействии пользователя (или с помощью клавиатуры или мыши).

В соответствии с Apple существует пять основных типа окон в macOS приложения:

- **Окно документа** -окно документа содержит данные пользователя на основе файла в электронной таблице или текстового документа.
- **Окно приложения** — окно приложения является главного окна приложения, которое не основана на документ (например, в приложении "Календарь" на компьютере Mac).
- **Панель** - панель поверх других окон и предоставляет средства, или элементы управления, которые пользователи могут работать с пока документов, откройте. В некоторых случаях может быть прозрачные панели (например, при работе с большой графики).
- **Диалоговое окно** -диалоговое окно появляется в ответ на действия пользователя и обычно предоставляет пользователям способов выполнить действие. Диалоговое окно требует ответа от пользователя, прежде чем можно будет закрыть. (См. [работе с диалоговыми окнами](~/mac/user-interface/dialog.md))
- **Оповещения** -предупреждение — это специальный тип диалогового окна, которое появляется при возникновении серьезной ошибки (например, ошибки) или как предупреждение (например, Подготовка к удалению файла). Поскольку диалоговое окно предупреждение, необходимо также ответа пользователя перед закрытием. (См. [работа с оповещениями](~/mac/user-interface/alert.md))

Дополнительные сведения см. в разделе [Windows о](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Основной ключ и неактивных окнах

Windows в приложении Xamarin.Mac можно выглядят и работают так различаться в зависимости от как пользователь в настоящее время взаимодействия с ними. Прежде всего документа или окна приложения, находящегося в центре внимания пользователя называется _главное окно_. В большинстве случаев это окно также будет _окна ключ_ (окна, которая принимает ввод данных пользователем). Но это не всегда так, например, палитра цветов может быть открыто и окон ключ, который пользователь взаимодействует с изменение состояния элемента в окне документа (который по-прежнему будет главное окно).

Основной и раздел Windows (если они существуют отдельно) всегда активны, _неактивных окнах_ , открытых окон, которые не являются на переднем плане. Например, приложение текстового редактора может иметь более одного документа открыт одновременно, только главное окно остается активным, все остальные будут неактивны. 

Дополнительные сведения см. в разделе [Windows о](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Именование Windows

Окно может содержать строку заголовка и при отображении заголовка, это обычно имя приложения, имя документа, которые обрабатываются или функции (например, инспектор) окна. Некоторые приложения не отображать строку заголовка, поскольку они являются распознаваемым методом видимости и не работают с документами.

Apple предлагаются следующие рекомендации.

- Используйте имя вашего приложения для заголовка окна основной, не является документом. 
- Назовите новое окно документа `untitled`. Для первого нового документа не добавляется число заголовка (например, `untitled 1`). Если пользователь создает новый документ перед сохранением и титульных первый, вызова этого окна `untitled 2`, `untitled 3`и т. д.

Дополнительные сведения см. в разделе [именования Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Windows в полноэкранном режиме

На macOS, окно приложения, можно перейти скрытие весь экран любых значений, включая панели меню приложения (который может быть отображена, наведя курсор в верхнюю часть экрана) для предоставления отвлекаться свободного взаимодействие с ним содержимого.

Apple предлагает следующие рекомендации:

- Определите, имеет ли смысл для окна перейти на весь экран. Приложения, которые предоставляют краткое взаимодействия (например, калькулятор) не следует позволяют полноэкранный режим.
- Отображение панели инструментов, нуждается ли задача полноэкранный его. Панель инструментов скрыта, обычно в полноэкранном режиме.
- Полный размер окна должен иметь все возможности для пользователей необходимо выполнить задачу.
- По возможности избегайте взаимодействия с Finder, тогда как пользователь находится в полноэкранном режиме окна.
- Воспользоваться преимуществами повышения экраны без сдвиг фокус от основных задач.

Дополнительные сведения см. в разделе [полноэкранном режиме Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Панели

Панель это вспомогательный окно, в котором содержатся элементы управления и параметры, влияющие на активный документ или Выбор (например, систему палитра):

[![](window-images/panel01.png "Палитры цветов")](window-images/panel01.png#lightbox)

Может быть панелей _конкретного приложения_ или _всей системы_. Панели конкретного приложения поверх верхней части окна документов приложения и исчезают, когда приложение находится в фоновом режиме. Панелей всей системы (такие как **шрифты** панель), число с плавающей запятой поверх всех открытых окон, независимо от того, приложение. 

Apple предлагает следующие рекомендации:

- В общем случае использовать стандартную панель, прозрачные панели можно использовать только ограниченном количестве и графические задач.
- Рассмотрите возможность использования панели для предоставления пользователям простой доступ к важные элементы управления или сведения, который непосредственно влияет на своих задач.
- Скрытие и отображение панелей при необходимости.
- Панели всегда должна включать заголовок.
- Панели не должно включать кнопка свертывания active.

#### <a name="inspectors"></a>Инспекторы

Большинство современных приложений macOS представляет вспомогательные элементы управления и параметры, влияющие на активный документ или выделенный фрагмент как _инспекторов_ , которые являются частью основного окна (как **страниц** приложение показано ниже), Вместо использования Windows панели:

[![](window-images/panel02.png "Пример инспектор")](window-images/panel02.png#lightbox)

Дополнительные сведения см. в разделе [панелей](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) и на нашем [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) образец приложения для полной реализации **Интерфейс инспектора** в приложении Xamarin.Mac.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Создание и обслуживание Windows в Xcode

При создании нового приложения Xamarin.Mac Cocoa, вы получаете стандартное окно пустым, по умолчанию. В данном окне определяется в `.storyboard` файл автоматически включен в проект. Чтобы изменить проект windows в **обозревателе решений**, дважды щелкните `Main.storyboard` файла:

[![](window-images/edit01.png "При выборе основных раскадровки")](window-images/edit01.png#lightbox)

Откроется окно конструктора в построителе Xcode элемента интерфейса:

[![](window-images/edit02.png "Изменение пользовательского интерфейса в Xcode")](window-images/edit02.png#lightbox)

В **инспектора атрибут**, существует несколько свойств, которые можно использовать для определения и управления окна:

- **Заголовок** -это текст, который будет отображаться в окне заголовка.
- **Эта функция** -это _ключ_ , будет использоваться для идентификатор окна, если он является позиции, а параметры сохраняются автоматически.
- **Заголовок** -отображать строку заголовка окна.
- **Объединенные заголовка и инструментов** — Если окно включает в себя панель инструментов, должны быть частью строки заголовка.
- **Полный размер представления содержимого** -позволяет область содержимого должно находиться в строке заголовка окна.
- **Тень** -окно имеет тень.
- **Текстурой** -текстуры windows можно использовать эффекты (например, естественность) и можно перемещать путем перетаскивания их текст в любом месте.
- **Закройте** -окно имеет кнопку Закрыть.
- **Свести к минимуму** -окна есть кнопка свертывания.
- **Изменить размер** -окно имеет изменения размера элемента управления.
- **Кнопки панели инструментов** -окно имеет скрыть или Показать кнопки панели инструментов.
- **Могут быть восстановлены** -— положение окна и параметры автоматически сохраняется и восстанавливается.
- **Отображается запустите** -окна автоматически отображается, когда `.xib` загрузить файл.
- **Скрыть деактивировать** -окно скрывается, когда приложение входит в фоновом режиме.
- **Освободить при закрытии** -— это окно, удаляются из памяти при его закрытии.
- **Всегда отображения подсказки** -являются постоянно подсказках.
- **Повторно вычисляет Просмотр цикл** -является Просмотр заказа, пересчитываются перед рисованием окна.
- **Пробелы**, **Exposé** и **циклы** -все определения, как ведет себя в этих средах macOS окна. 
- **Весь экран** -определяет, если в этом окне можно ввести в полноэкранном режиме. 
- **Анимация** -определяет, какой тип анимации, доступные для окна.
- **Внешний вид** -управляет внешним видом окна. Теперь имеется только один вид голубой цвет.

В разделе Apple [Знакомство с Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) и [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) документации для получения дополнительных сведений.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Параметр по умолчанию размер и расположение

Чтобы задать начальное положение окна и контролировать его размер, переключитесь в **инспектора размер**:

[![](window-images/edit07.png "По умолчанию размер и расположение")](window-images/edit07.png#lightbox)

Здесь можно установить исходный размер окна, ей следует присвоить минимальный и максимальный размер, задайте начальное расположение на экране и контролировать границы вокруг окна.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Установка контроллера пользовательских главное окно

Чтобы иметь возможность создать выходов и действия, чтобы предоставить элементы пользовательского интерфейса для кода C#, Xamarin.Mac приложению потребуется использовать настраиваемый контроллер окна.

Выполните следующие действия:

1. Открытие раскадровки приложения в построителе интерфейса в Xcode.
2. Выберите `NSWindowController` в рабочей области конструирования.
3. Переключитесь в **удостоверение инспектора** Просмотр и ввод `WindowController` как **имя класса**: 

    [![](window-images/windowcontroller01.png "Задание имени класса")](window-images/windowcontroller01.png#lightbox)
4. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации.
5. Объект `WindowController.cs` файл будет добавлен в проект в **обозревателе решений** в Visual Studio для Mac: 

    [![](window-images/windowcontroller02.png "При выборе контроллера windows")](window-images/windowcontroller02.png#lightbox)
6. Снова откройте раскадровки в построителе интерфейса в Xcode.
7. `WindowController.h` Файл будет доступен для использования: 

    [![](window-images/windowcontroller03.png "Изменение файла WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Добавление элементов пользовательского интерфейса

Для определения содержимого окна, перетащите элементы управления из **инспектора библиотеки** на **редактор интерфейса**. См. в разделе нашей [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) Дополнительные сведения об использовании построителя интерфейс для создания и включения элементов управления.

Например, давайте перетащим инструментов из **инспектора библиотеки** на окно в **редактор интерфейса**:

[![](window-images/edit03.png "При выборе на панели инструментов из библиотеки")](window-images/edit03.png#lightbox)

Затем перетащите **представления текста** и сделать его для заполнения области под панелью инструментов:

[![](window-images/edit04.png "Добавление представления текста")](window-images/edit04.png#lightbox)

Поскольку мы хотим **представления текста** для сжатия и увеличения при изменении размера окна, давайте перейдите к **Редактор ограничений** и добавьте следующие ограничения:

[![](window-images/edit05.png "Изменение ограничений")](window-images/edit05.png#lightbox)

Щелкнув для **красным I балки** в верхней части редактора и затем выбрав **добавьте ограничения 4**, мы указываем текстовое представление придерживаться данного по осям X и Y координаты и расширение или Сжатие по горизонтали и вертикали как размер окна.

И наконец, давайте предоставить **представления текста** для кода с использованием **розетки** (обязательно выберите `ViewController.h` файл):

[![](window-images/edit06.png "Настройка выхода")](window-images/edit06.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Дополнительные сведения о работе с **торговцам** и **действия**, см. в разделе нашей [розетки и действие](~/mac/get-started/hello-mac.md#Outlets_and_Actions) документации.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Стандартное окно рабочего процесса

Любое окно, создания и работы с Xamarin.Mac приложения происходит по сути таким же, как то, что мы только что выполнили выше:

1. Для новых окон, которые не используются по умолчанию, автоматически добавляется в проект добавьте в проект новое определение окна. Это будет подробно ниже.
2. Дважды щелкните `Main.storyboard` файл, чтобы открыть окно конструктора для редактирования в Xcode интерфейс построителя.
3. Перетащите новое окно в пользовательский интерфейс конструктора и подключите окна в главное окно с помощью _Segues_ (Дополнительные сведения см. [Segues](~/mac/platform/storyboards/indepth.md#Segues) части нашей [работа с раскадровки](~/mac/platform/storyboards/indepth.md) документации).
3. Задайте свойства требуется окно **инспектора атрибут** и **инспектора размер**.
4. Перетащите элементы управления, необходимые для построения интерфейса и настройки их в **инспектора атрибут**.
5. Используйте **инспектора размер** обрабатывать изменения размера для элементов пользовательского интерфейса.
6. Предоставляет элементы пользовательского интерфейса окна, чтобы код C# с помощью **торговцам** и **действия**.
7. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь, когда у нас есть основные окна создан, мы рассмотрим типичный процессов Xamarin.Mac, приложение выполняет при работе с windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Отображение окна по умолчанию

По умолчанию новое приложение Xamarin.Mac автоматически отобразит окно, определенные в `MainWindow.xib` файла при его запуске:

[![](window-images/display01.png "Пример окна под управлением")](window-images/display01.png#lightbox)

Поскольку мы изменили структуру этого окна выше, теперь включает значение по умолчанию панель инструментов и **представления текста** элемента управления. В следующем разделе в `Info.plist` файла отвечает за отображение этого окна:

[![](window-images/display00.png "Редактирование Info.plist")](window-images/display00.png#lightbox)

**Главного интерфейса** раскрывающийся список используется для выбора раскадровки, который будет использоваться в качестве основной пользовательский Интерфейс приложения (в данном случае `Main.storyboard`).

Представление контроллер автоматически добавляется в проект для управления Windows Main, отображается (вместе с его первичное представление). Он определен в `ViewController.cs` файла и присоединяется к **владелец файла** в построителе интерфейс под **удостоверение инспектора**:

[![](window-images/display02.png "Установка владелец файла")](window-images/display02.png#lightbox)

Для окна, мы хотели бы в него необходимо добавить в качестве заголовка `untitled` при его открытии давайте переопределение `ViewWillAppear` метод `ViewController.cs` для выглядеть следующим образом:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Рекомендуется задать значение окна `Title` свойство в `ViewWillAppear` вместо метода `ViewDidLoad` метод так, как при представлении могут быть загружены в память, она не еще создан полностью. При попытке доступа к `Title` свойство в `ViewDidLoad` метод получаются `null` исключения, поскольку окно еще не был создан и проводной доступ к свойству еще.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Закрытие окна программными средствами

Возможны ситуации, которые вы хотите программное закрытие окна приложения Xamarin.Mac, отличные от необходимости пользователь щелкните окно **закрыть** кнопки или с помощью пункта меню. macOS предоставляет два способа закрытия `NSWindow` программно: `PerformClose` и `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Вызов `PerformClose` метод `NSWindow` имитирует пользователем кнопки окна **закрыть** кнопки, ненадолго выделение кнопки и затем закрыть окно.

Если приложение поддерживает `NSWindow` `WillClose` событие, он будет вызываться до закрытия окна. Возвращает событие `false`, окна не будут закрыты. Если окно не имеет **закрыть** кнопку или не может быть закрыт по любой причине, операционная система будет выдавать предупреждения звука.

Пример:

```csharp
MyWindow.PerformClose(this);
```

Будет предпринята попытка закрыть `MyWindow` `NSWindow` экземпляра. В случае его успешного окно будет закрыто, в противном случае звуковых сигналов оповещения будут выдаваться и будет остается открытым.

<a name="Close" />

### <a name="close"></a>Закрыть

Вызов `Close` метод `NSWindow` не имитирует пользователем кнопки окна **закрыть** кнопки с помощью моментально выделения кнопки, он просто закрывает окно.

Окно не должен быть видимым будет закрыта и `NSWindowWillCloseNotification` уведомление будет опубликовано в центре уведомлений по умолчанию для закрытия окна.

`Close` Метод в двух важных аспектах отличается от `PerformClose` метод:

1. Он не будет пытаться вызвать `WillClose` событий.
2. Он не моделирует щелкнув **закрыть** кнопки с помощью моментально выделения кнопки.

Пример:

```csharp
MyWindow.Close();
```

Для закрытия `MyWindow` `NSWindow` экземпляра.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Содержимое измененных Windows

В macOS, Apple предоставляет способ оповещения пользователя, содержимое окна (`NSWindow`) было изменено пользователем и необходимо сохранить. Если это окно содержит измененное содержимое, небольшой черная точка будет отображаться в его **закрыть** мини-приложения:

[![](window-images/close01.png "Окно с измененный маркера")](window-images/close01.png#lightbox)

Если пользователь пытается закрыть окно или выйти из приложения Mac, пока имеются несохраненные изменения содержимого окна, должен представить [диалоговое окно](~/mac/user-interface/dialog.md) или [модальное листа](~/mac/user-interface/dialog.md) и разрешить пользователям сохранять свои изменения Первый:

[![](window-images/close02.png "Сохранение таблицы, отображаемой при закрытии окна")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Пометка окна по мере изменения

Чтобы пометить окна по мере внесения изменений содержимого, используйте следующий код:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

И после сохранения изменений, снимите флаг измененный с помощью:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Сохранение изменений перед закрытием окна

Наблюдение за пользователю закрытии окна, их сохранить измененное содержимое заранее, необходимо создать подкласс `NSWindowDelegate` и переопределите его `WindowShouldClose` метод. Пример:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on resu;t
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Чтобы присоединить экземпляр этого делегата к окну, используйте следующий код:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Сохранение изменений перед закрытием приложения

Наконец приложение Xamarin.Mac проверить, если любой из его Windows содержат измененное содержимое и позволяет пользователю сохранить изменения перед выходом. Чтобы сделать это, измените вашей `AppDelegate.cs` файла, переопределите `ApplicationShouldTerminate` метод и сделать его выглядеть следующим образом:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>Работа с несколькими окнами

Большинство приложений документа на основе Mac можно изменить несколько документов, в то же время. Например текстовый редактор может иметь несколько текстовых файлов, открыт для редактирования, в то же время. По умолчанию имеет наше новое приложение Xamarin.Mac **файл** меню с **New** элемента автоматически проводной до `newDocument:` **действия**.

Мы собираемся активировать этого нового элемента и разрешить пользователю открыть несколько копий нашей главное окно, чтобы изменить сразу несколько документов.

Изменим наши `AppDelegate.cs` и добавьте следующие вычисляемые свойства файла:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Мы будем использовать для отслеживания числа несохраненные файлы, поэтому мы может предоставлять пользователю (согласно рекомендации Apple, как описано выше).

Добавьте следующий метод:

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Этот код создает новую версию нашей окна контроллера, загружает окно нового, Main и окно ключа делает и устанавливает его заголовок. Теперь если мы запустите приложение и выберите **New** из **файл** меню будет открыт и отображается новое окно редактора:

[![](window-images/display04.png "Было добавлено новое окно без имени")](window-images/display04.png#lightbox)

Если мы открыть **Windows** меню, вы увидите приложение автоматически отслеживания и обработки наших окон:

[![](window-images/display05.png "В меню окон")](window-images/display05.png#lightbox)

Дополнительные сведения о работе с меню в приложении Xamarin.Mac см. в разделе нашей [работа с меню](~/mac/user-interface/menu.md) документации.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Начало активной в настоящее время

В приложении Xamarin.Mac, которое можно открыть несколько окон (документов) бывают ситуации, когда необходимо получить текущий, верхних окне (окне ключа). Следующий код возвращает окно ключа:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Он может вызываться в любому классу или методу, которому требуется доступ к окну текущего, ключ. Если ни одного окна в данный момент открыт, он вернет `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Доступ к все приложения Windows

Возможны ситуации должны получать доступ ко всем windows, что в приложении Xamarin.Mac в настоящее время существует открытым. Например ли файл, который пользователь хочет, чтобы открыть еще открыт в окне с существующие.

`NSApplication.SharedApplication` Поддерживает `Windows` свойство, содержащее массив все открытые окна в приложении. Можно выполнять итерацию по этому массиву доступ ко всем windows текущего приложения. Пример:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

В примере кода выполняется приведение возвращаемого окна настраиваемого `ViewController` класс в нашем приложении и проверка значения пользовательского `Path` свойства и путь к файлу, пользователю необходимо открыть. Если файл уже открыт, мы приносят этого окна на передний план.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Изменение размера окна в коде

Бывают случаи, когда приложению требуется изменение размеров окна в коде. Чтобы изменить размер и положение окна, настройте его на `Frame` свойство. При изменении размера окна, обычно требуется также настроить его происхождения, чтобы оставить окно в том же расположении, из-за macOS в системе координат.

В отличие от iOS, где верхний левый угол — (0,0) macOS использует Математическая систему координат где — (0,0) в левом нижнем углу экрана. В iOS координаты увеличить при перемещении вниз по направлению к вправо. Координаты macOS, увеличьте значение вверх справа. 

Следующий пример кода изменяет размеры окна:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> При изменении windows размер и расположение в коде необходимо убедитесь, что учитывают минимальный и максимальный размер, которые задаются в интерфейс построителя. Это не будет учитываться автоматически, и можно будет сделать окно большего или меньшего размера, чем эти ограничения.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Мониторинг изменений размера окна

Возможны ситуации, когда необходимо отслеживать изменения в размере окна внутри приложения Xamarin.Mac. Например, для повторной отрисовки содержимого в соответствии с размером новый.

Чтобы отслеживать изменения размера, сначала убедитесь, что вы присвоили пользовательский класс контроллера окна в Xcode интерфейс построителя. Например `MasterWindowController` в следующем:

[![](window-images/resize01.png "Инспектор удостоверений")](window-images/resize01.png#lightbox)

Теперь измените пользовательский класс окна контроллера и монитор `DidResize` событий, чтобы получать уведомления об изменениях размер активных окно контроллера. Пример:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

При необходимости можно использовать `DidEndLiveResize` событий только уведомления после завершения изменения размера окна. Пример.

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>Настройка окна, заголовок и представленные в файл

При работе с windows, которые представляют документы, `NSWindow` имеет `DocumentEdited` , если свойство `true` отображается небольшой точка в "Закрыть", чтобы разрешить пользователям индикацией того, что файл был изменен и должен быть сохранен перед закрытием.

Изменим наши `ViewController.cs` файл и внесите следующие изменения:

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Мы также отслеживаем `WillClose` события в окне и проверки состояния `DocumentEdited` свойство. Если это `true` нам необходимо предоставить пользователю возможность сохранить изменения в файл. Если мы запуска нашего приложения и введите любой текст, будет отображаться точка:

[![](window-images/file01.png "Измененные окна")](window-images/file01.png#lightbox)

При попытке закрыть окно, мы получаем предупреждение:

[![](window-images/file02.png "Отображение сохранения диалоговое окно")](window-images/file02.png#lightbox)

Если мы загрузке документа из файла можно задать заголовок окна к файлу имя `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` метод (учитывая, что `path` является строкой, представляющей открытого файла). Кроме того, можно задать URL-адрес файла с помощью `window.RepresentedUrl = url;` метод.

Если URL-адрес указывает на файл типа, известного операционной системы, его значок отображается в заголовке окна. Если пользователь щелкнет правой кнопкой мыши значок, отображаемый путь к файлу.

Изменим `AppDelegate.cs` и добавьте следующий метод:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Теперь запустим нашего приложения выберите **открыть...**  из **файл** меню, выберите текстовый файл из **откройте** диалогового окна поле и откройте его:

[![](window-images/file03.png "Открытое окно")](window-images/file03.png#lightbox)

Файл будет отображаться, а заголовок задается со значком файла:

[![](window-images/file04.png "Загрузить содержимое файла")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Добавление в проект новое окно

Помимо основным окном документа приложении Xamarin.Mac может возникнуть необходимость отображения других типов windows для пользователя, такие как настройки или панели инспектора.

Чтобы добавить новое окно, выполните следующие действия.

1. В **обозревателе решений**, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса в Xcode.
2. Перетащите новую **окна контроллера** из **библиотеки** и поместите его на **конструктора**:

    [![](window-images/new01.png "При выборе нового окна контроллера в библиотеке")](window-images/new01.png#lightbox)
3. В **удостоверение инспектора**, введите `PreferencesWindow` для **идентификатор раскадровки**: 

    [![](window-images/new02.png "Задание идентификатора раскадровки")](window-images/new02.png#lightbox)
5. Разработка интерфейса. 

    [![](window-images/new03.png "Проектирование пользовательского интерфейса")](window-images/new03.png#lightbox)
6. Откройте меню приложения (`MacWindows`), выберите **установки...** , Щелкните элемент управления, а затем перетащите в новое окно: 

    [![](window-images/new05.png "Создание segue")](window-images/new05.png#lightbox)
7. Выберите **Показать** из всплывающего меню.
6. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Если выполнение кода и выберите **установки...**  из **меню приложения**, откроется окно:

[![](window-images/new04.png "Меню настройки образца")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Работа с панелями

Как уже говорилось в начале этой статьи, панель поверх других окон, а также средства или элементы управления, которые пользователи могут работать с, пока открыты документы. 

Так же, как и любой другой тип окна, создания и работы с Xamarin.Mac приложения процесса в основном аналогичен:

1. Добавьте в проект нового окна определения.
2. Дважды щелкните `.xib` файл, чтобы открыть окно конструктора для редактирования в Xcode интерфейс построителя.
2. Задайте свойства требуется окно **инспектора атрибут** и **инспектора размер**.
4. Перетащите элементы управления, необходимые для построения интерфейса и настройки их в **инспектора атрибут**.
5. Используйте **инспектора размер** обрабатывать изменения размера для элементов пользовательского интерфейса.
6. Предоставляет элементы пользовательского интерфейса окна, чтобы код C# с помощью **торговцам** и **действия**.
7. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

В **инспектора атрибут**, у вас есть следующие параметры, характерные для панелей:

[![](window-images/panel03.png "Инспектор атрибута")](window-images/panel03.png#lightbox)

- **Стиль** -можно настроить стиль панели из: регулярные панель (выглядит стандартного окна), программа панели (имеет меньший строки заголовка), HUD панели (прозрачная и в заголовке входит в фоновом режиме).
- **Активация не** -определяет, в панели становится окно ключа.
- **Документирование Modal** -Если документ модальное, панель будет только расположена над приложения windows, в противном случае отображается над всеми.


Добавление новой панели, выполните следующие действия.

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** .
2. В диалоговом окне новый файл, выберите **Xamarin.Mac** > **Cocoa окна с контроллером**:

    [![](window-images/panels00.png "Добавление нового окна контроллера")](window-images/panels00.png#lightbox)
3. Введите в поле **Имя** значение `DocumentPanel` и нажмите кнопку **Новый**.
4. Дважды щелкните `DocumentPanel.xib` файл, чтобы открыть его для редактирования в построителе интерфейса: 

    [![](window-images/new02.png "Изменение порта")](window-images/new02.png#lightbox)
5. Удалите существующее окно и перетащите из панели **инспектора библиотеки** в **редактор интерфейса**: 

    [![](window-images/panels01.png "Удаление существующего окна")](window-images/panels01.png#lightbox)
6. Подключить панели до **владелец файла*-**окна*- **розетки**: 

    [![](window-images/panels02.png "Перетаскивая на установление связи панели")](window-images/panels02.png#lightbox)
7. Переключитесь в **удостоверение инспектора** и присвоить класс панели `DocumentPanel`: 

    [![](window-images/panels03.png "Класс панель параметров")](window-images/panels03.png#lightbox)
6. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.
7. Изменить `DocumentPanel.cs` и измените определение класса на следующее: 

    `public partial class DocumentPanel : NSPanel`
8. Сохраните изменения в файле.

Изменить `AppDelegate.cs` и создайте `DidFinishLaunching` внешний метод следующим образом:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Если мы запустим приложение, будет отображаться панель:

[![](window-images/panels04.png "Панели в запущенном приложении")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Панель Windows являются устаревшими в Apple и должна быть заменена **интерфейсы инспекторов**. Полный пример создания **инспектора** в приложении Xamarin.Mac см. в нашем [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) пример приложения.

<a name="Summary" />

## <a name="summary"></a>Сводка

Подробное описание работы с Windows и панелей в приложении Xamarin.Mac вступила в этой статье. Мы уже видели различных типов и использует окон и панелей, как создавать и поддерживать Windows и панелей в Xcode интерфейс построителя и способы работы с Windows и панелей в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [MacWindows (пример)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (пример)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с меню](~/mac/user-interface/menu.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
