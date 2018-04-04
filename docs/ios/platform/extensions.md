---
title: операций ввода-вывода расширения
description: Появился в iOS 8, расширения — это мини-приложения, связанным с iOS в стандартных контекстов, например в центре уведомлений при запросе пользовательской клавиатуры, или когда они фото редактирования. Все расширения устанавливаются вместе с приложением контейнера и активированы на определенный момент расширения в приложении узла.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: af52db5f1add040af025f2134f0e9a7b3936f4f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="ios-extensions"></a>операций ввода-вывода расширения

_Появился в iOS 8, расширения — это мини-приложения, связанным с iOS в стандартных контекстов, например в центре уведомлений при запросе пользовательской клавиатуры, или когда они фото редактирования. Все расширения устанавливаются вместе с приложением контейнера и активированы на определенный момент расширения в приложении узла._

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Создание расширений в iOS, по [университета Xamarin](https://university.xamarin.com/)**

Расширения, представленном в iOS 8, специализируются `UIViewControllers` , представлены по iOS внутри стандартных контексты таких, как в **центр уведомлений**, как типы пользовательских сочетаний, запрошенной пользователем для выполнения специализированных входные данные или других контекстах, например изменение фотографий, где модуль можно предоставить специальный эффект фильтры.

Все расширения устанавливаются вместе с приложением контейнера (с обоих элементов, написанные с использованием API-интерфейсов единой 64-разрядной) и активированы на определенный момент расширения в приложении узла. И так как они будут использоваться в качестве дополнения к существующим функциям системы, они должны быть высокая производительность, компактный и надежным. 

В этой статье рассматриваются следующие темы:

- [Точки расширения](#Extension-Points) -перечислены типы доступных точек расширения и тип расширения, которые могут быть созданы для каждой точки.
- [Ограничения](#Limitations) -расширения имеют ряд ограничений, некоторые из которых являются универсальными для всех типов, а другими типами расширения может иметь определенные ограничения на их использование.
- [Распространение, Установка и запуск расширения](#Distributing-Installing-and-Running-Extensions) -расширения распространяются через в приложении контейнера, который, в свою очередь отправки и распространяются через App Store. На этом этапе устанавливаются расширения, распространяемый с приложением, но пользователь должен явно включить все расширения. Разные типы расширений включаются различными способами.
- [Жизненный цикл расширения](#Extension-Lifecycle) — это расширения `UIViewController` будет создан и начать обычный жизненный цикл контроллера представления. Тем не менее в отличие от обычных приложений, расширения загружаются, выполняется и затем завершается несколько раз.
- [Создание расширения](#Creating-an-Extension) -при разработке расширения, решения будет содержать по крайней мере два проекта: приложение контейнера и один проект для каждого расширения контейнера обеспечивает.
- [Пошаговое руководство](#Walkthrough) — Описание создания простого **сегодня** расширение, которое предоставляет пользовательский интерфейс, либо с помощью раскадровки мини-приложения или в коде, установив расширение и его тестирование в эмуляторе iOS.
- [Взаимодействие с приложением узла](#Communicating-with-the-Host-App) -приводится краткое описание обмена данными с приложением узла из расширения.
- [Взаимодействие с родительского приложения](#Communicating-with-the-Parent-App) -приводится краткое описание связи с родительского приложения из расширения.
- [Меры предосторожности и рекомендации](#Precautions-and-Considerations) -список некоторых знать меры предосторожности и вопросы, которые следует принимать во внимание при разработке и реализации расширения.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>Точки расширения

|Тип|Описание|Точка расширения|Узел приложения|
|--- |--- |--- |--- |
|Действие|Специализированный редактор или средство просмотра для определенного типа носителя|`com.apple.ui-services`|Любой|
|Поставщик документа|Позволяет приложению использовать хранилище удаленных документов|`com.apple.fileprovider-ui`|Приложения, использующие [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|Клавиатура|Альтернативный клавиатуры|`com.apple.keyboard-service`|Любой|
|Редактирование фотографий|Фотография манипуляции и редактирования|`com.apple.photo-editing`|Редактор Photos.App|
|Общий доступ|Делится данными с социальными сетями, обмена сообщениями службы, и т. д.|`com.apple.share-services`|Любой|
|Сегодня|«Мини-приложения», отображаемые на экране сегодня или центр уведомлений|`com.apple.widget-extensions`|Сегодня и центр уведомлений|

[Дополнительные расширения точек](~/ios/platform/introduction-to-ios10/index.md#app-extensions) были добавлены в iOS 10.

<a name="Limitations" />

## <a name="limitations"></a>Ограничения

Расширения имеют ряд ограничений, некоторые из которых являются универсальными для всех типов (для экземпляра, тип расширения не может получить доступ к камеры или микрофонов) при других типах расширения может иметь определенные ограничения на их использование (например, пользовательские клавиатура не может использоваться для защиты данных полей ввода например паролей). 

Универсальные ограничения являются:

- [Kit работоспособности](~/ios/platform/healthkit.md) и [пользовательского интерфейса событий пакета](~/ios/platform/eventkit.md) не доступны платформы
- Расширения не может использовать [расширенных фоновые режимы](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Расширения не может обращаться камеры или микрофонов устройства (несмотря на то, что они могут получить доступ к существующие файлы мультимедиа)
- Расширения не могут получать воздуху удаления данных (несмотря на то, что они могут передавать данные через воздух Drop)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) и [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) недоступны; необходимо использовать расширения [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Несколько членов [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) недоступны: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` и `UIApplication.EndIgnoringInteractionEvents`
- iOS определяет ограничение использования памяти 16 МБ на современных расширения.
- По умолчанию модули клавиатуры не имеют доступа к сети. Это влияет на отладку на устройстве (ограничение не применяется в симуляторе), так как Xamarin.iOS требуется доступ к сети для работы отладчика. Существует возможность доступа к сети для запроса, задав `Requests Open Access` значение в файле Info.plist проекта для `Yes`. См. в разделе Apple [руководство по клавиатуры настраиваемых](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) Дополнительные сведения об ограничениях расширения клавиатуры.

Для отдельных ограничения см. в разделе Apple [руководство по программированию расширения приложения](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>Распространение, Установка и запуск расширения

Расширения распространяются из приложения контейнера, который, в свою очередь отправки и распространяются через App Store. На этом этапе устанавливаются расширения, распространяемый с приложением, но пользователь должен явно включить все расширения. Разные типы расширений включены различными способами; несколько пользователю необходимо перейти к **параметры** приложения и включить их оттуда. Хотя остальные включены в момент использования, например включения расширения для управления доступом, при отправке фото. 

Приложения, в котором используется расширение (где обнаружении точки расширения) называется **узла приложения**, так как это приложение, на котором размещена расширения во время выполнения. — Приложение, которое будет установлено расширение **приложения контейнера**, так как это приложение, в котором содержится расширение во время установки.  

Как правило приложение контейнера описывается расширение и проводит пользователя через процесс включения его.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>Расширение жизненного цикла

Расширение может быть простым как один [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) или более сложные расширения, предоставляющие нескольких экранах пользовательского интерфейса. При обнаружении _точек расширения_ (например, если совместное использование изображения), они будут иметь возможность выбрать расширения, зарегистрированные для этой точки расширения. 

При желании приложения, расширения, его `UIViewController` будет создан и начать обычный жизненный цикл контроллера представления. Однако в отличие от обычного приложения, которая будет приостановлена, но обычно не завершается, когда пользователь прекращает работать с ними, расширения загружаются, выполняется и затем завершается несколько раз.

Расширения могут взаимодействовать с их узла приложений через [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) объекта. Некоторые модули имеют операциям, получающим асинхронные обратные вызовы вместе с результатами. Эти обратные вызовы выполняются в фоновых потоках и расширения необходимо учитывать это; Например, с помощью [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) при необходимости обновления пользовательского интерфейса. В разделе [взаимодействует с приложением узла](#Communicating-with-the-Host-App) ниже, в разделе для получения дополнительных сведений.

По умолчанию расширения и приложения-контейнеры можно не взаимодействуют, несмотря на то, идет установка друг с другом. В некоторых случаях приложение контейнера — по существу пустой «shipping» контейнер, целью которого обслуживается после установки расширения. Тем не менее, если требуют обстоятельства, приложение контейнера и расширения могут совместно использовать ресурсы из общую область. Кроме того **сегодня расширения** может запросить его контейнер приложение на открытие URL-адреса. Это показано в [развивать мини-приложение обратного отсчета](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>Создание расширения

Расширения (и их приложения контейнера) должен быть 64-разрядных двоичных файлов и созданное с помощью Xamarin.iOS [единой API-интерфейсы](http://developer.xamarin.com/guides/cross-platform/macios/unified). При разработке расширения, решения будет содержать по крайней мере два проекта: приложение контейнера и один проект для каждого расширения контейнера обеспечивает. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>Требования к контейнеру проекта приложения

Приложение контейнера, используемое для установки расширения необходимо соблюдать следующие требования:

- Он должен поддерживать ссылку на проект расширения.   
- Он должен быть полный приложения (должен иметь возможность запустить и выполнить успешно) даже в том случае, если он ничего не делает больше, чем позволяют установить расширение. 
- Он должен иметь идентификатор пакета, который является основой для идентификатора пакета расширения проекта (см. ниже дополнительные сведения).

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>Требования проекта расширения

Кроме того проект расширения предъявляются следующие требования:

- Она должна иметь идентификатор пакета, который начинается с идентификатора пакета приложения его контейнера. Например, если приложение контейнера имеет идентификатор пакета `com.myCompany.ContainerApp`, возможно, идентификатор расширения `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Его необходимо определить ключ `NSExtensionPointIdentifier`, с соответствующим значением (такие как `com.apple.widget-extension` для **сегодня** центр уведомлений мини-приложения) в его `Info.plist` файла.
- Необходимо также определить *либо* `NSExtensionMainStoryboard` ключа или `NSPrincipalClass` ключа в его `Info.plist` файл с соответствующим значением:
    - Используйте `NSExtensionMainStoryboard` ключ, чтобы определить имя раскадровки, которая представляет основной пользовательский Интерфейс для расширения (минус `.storyboard`). Например `Main` для `Main.storyboard` файла.
    - Используйте `NSPrincipalClass` клавишу, чтобы указать класс, который будет инициализирован при запуске расширения. Значение должно соответствовать **зарегистрировать** значение вашей `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Определенные типы расширений могут применяться Дополнительные требования. Например **сегодня** или **центр уведомлений** расширения основной класс должен реализовывать [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> При запуске проекта с помощью одного расширения шаблоны, предоставляемые Visual Studio для Mac, большинство (если не все) эти требования предоставленный и автоматически выполняется автоматически с помощью шаблона.

<a name="Walkthrough" />

## <a name="walkthrough"></a>Пошаговое руководство 

В следующем пошаговом руководстве вы создадите пример **сегодня** графического элемента, который вычисляет количество дней, оставшихся в год и день:

[![](extensions-images/carpediemscreenshot-sm.png "Пример сегодня мини-приложение, вычисляющую день и количество дней, оставшихся в году")](extensions-images/carpediemscreenshot.png#lightbox)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>Создание решения

Чтобы создать необходимые решения, выполните следующее:

1. Во-первых, создайте новый iOS, **одного представления приложения** проекта и нажмите кнопку **Далее** кнопки: 

    [![](extensions-images/today01.png "Во-первых создайте новый iOS, проект единого представления приложения и нажмите кнопку "Далее"")](extensions-images/today01.png#lightbox)
2. Вызовите проекта `TodayContainer` и нажмите кнопку **Далее** кнопки: 

    [![](extensions-images/today02.png "Вызовите TodayContainer проекта и нажмите кнопку "Далее"")](extensions-images/today02.png#lightbox)
3. Проверьте **имя проекта** и **имя_решения** и нажмите кнопку **создать** кнопку, чтобы создать решения: 

    [![](extensions-images/today03.png "Проверьте правильность имени проекта и имя_решения и нажмите кнопку "Создать" для создания решения")](extensions-images/today03.png#lightbox)
4. Далее в **обозревателе решений**, щелкните правой кнопкой мыши решение и добавьте новую **iOS расширения** из проекта **сегодня расширения** шаблона: 

    [![](extensions-images/today04.png "После этого в обозревателе решений щелкните правой кнопкой мыши решение и добавьте новый проект расширения операций ввода-вывода на основе шаблона сегодня расширения")](extensions-images/today04.png#lightbox)
5. Вызовите проекта `DaysRemaining` и нажмите кнопку **Далее** кнопки: 

    [![](extensions-images/today05.png "Вызовите DaysRemaining проекта и нажмите кнопку "Далее"")](extensions-images/today05.png#lightbox)
6. Просмотр проекта и нажмите кнопку **создать** кнопку, чтобы создать его: 

    [![](extensions-images/today06.png "Просмотр проекта и нажмите кнопку "Создать" для его создания")](extensions-images/today06.png#lightbox)

Полученное в результате решение теперь у нас два проекта, как показано ниже:

[![](extensions-images/today07.png "Полученное в результате решение теперь у нас два проекта, как показано ниже")](extensions-images/today07.png#lightbox)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>Создание пользовательского интерфейса расширения

Далее необходимо создать интерфейс для вашего **сегодня** мини-приложения. Это можно сделать с помощью раскадровки, либо путем создания пользовательского интерфейса в коде. Оба метода будет рассматриваться ниже подробно.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>С помощью раскадровки

Для создания пользовательского интерфейса с раскадровкой, выполните следующее:

1. В **обозревателе решений**, дважды щелкните проект расширения `Main.storyboard` файл, чтобы открыть его для редактирования: 

    [![](extensions-images/today08.png "Дважды щелкните файл Main.storyboard проектов расширения, чтобы открыть его для редактирования")](extensions-images/today08.png#lightbox)
2. Выберите метку, который был автоматически добавлен к пользовательскому Интерфейсу шаблоном и присвойте ему **имя** `TodayMessage` в **мини-приложение** вкладке **свойства обозревателя**: 

    [![](extensions-images/today09.png "Выберите метку, который был автоматически добавлен к пользовательскому Интерфейсу шаблоном и присвойте ему имя TodayMessage на вкладке мини-приложения в обозревателе свойств")](extensions-images/today09.png#lightbox)
3. Сохраните изменения в раскадровку.

<a name="Using-Code" />

#### <a name="using-code"></a>Использование кода

Чтобы построить пользовательский Интерфейс в коде, выполните следующее: 

1. В **обозревателе решений**выберите **DaysRemaining** проекта, добавьте новый класс и назовите его `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining проекта, добавьте новый класс и вызывать его CodeBasedViewController")](extensions-images/code01.png#lightbox)
2. Опять же, в **обозревателе решений**, дважды щелкните расширения `Info.plist` файл, чтобы открыть его для редактирования: 

    [![](extensions-images/code02.png "Дважды щелкните файл Info.plist расширения, чтобы открыть его для редактирования")](extensions-images/code02.png#lightbox)
3. Выберите **представление источника** (в нижней части экрана) и откройте `NSExtension` узла: 

    [![](extensions-images/code03.png "Выберите представление источника из нижней части экрана, а затем разверните узел NSExtension")](extensions-images/code03.png#lightbox)
4. Удалить `NSExtensionMainStoryboard` раздел и добавить `NSPrincipalClass` со значением `CodeBasedViewController`: 

    [![](extensions-images/code04.png "Удалить ключ NSExtensionMainStoryboard и добавьте NSPrincipalClass со значением CodeBasedViewController")](extensions-images/code04.png#lightbox)
5. Сохраните изменения.

Далее следует изменить `CodeBasedViewController.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

Обратите внимание, что `[Register("CodeBasedViewContoller")]` совпадает со значением, которое указано для `NSPrincipalClass` выше.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>Создание кода расширения

С помощью пользовательского интерфейса создан откройте `TodayViewController.cs` или `CodeBasedViewController.cs` файловое (метода, используемого для создания пользовательского интерфейса выше), изменение **ViewDidLoad** метод и сделать его выглядеть следующим образом:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

Если с помощью кода на основе метода интерфейса пользователя, замените `// Insert code to power extension here...` комментарий с новым кодом выше. После вызова базовой реализации (и вставка метку для версии на основе кода), этот код выполняет простых вычислений, чтобы получить день года, и сколько дней осталось. Затем он отображает сообщение в метке (`TodayMessage`), созданный при разработке пользовательского интерфейса.

Обратите внимание, насколько схожи этот процесс заключается в обычный процесс написания приложения. Расширение `UIViewController` имеет же жизненного цикла, как представление-контроллер в приложении, за исключением того, расширения имеют фоновых режимов и не приостанавливается, когда этот пользователь завершит их использования. Вместо этого расширения повторно инициализировать и освобождена при необходимости.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>Создание контейнера пользовательского интерфейса приложения

В этом пошаговом руководстве приложение контейнера просто используется как метод поставка и Установка расширения и не добавляет новых функций, свои собственные. Изменить TodayContainer `Main.storyboard` файл и добавьте текст определения функции расширения и его установке:

[![](extensions-images/today10.png "Измените файл TodayContainers Main.storyboard и добавьте текст определения функции расширения и его установке")](extensions-images/today10.png#lightbox)

Сохраните изменения в раскадровку.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>Тестирование расширения

Для тестирования расширения в симуляторе iOS, запустите **TodayContainer** приложения. Будет отображено представление основного контейнера:

[![](extensions-images/run01.png "Отображается в основном представлении контейнеров")](extensions-images/run01.png#lightbox)

Далее попаданий **Главная** кнопки в имитаторе, проведите вниз в верхней части экрана, чтобы открыть **центр уведомлений**выберите **сегодня** и нажмите кнопку **Изменить** кнопки:

[![](extensions-images/run02.png "Нажмите кнопку "Домашняя" в имитаторе, проведите вниз в верхней части экрана, чтобы открыть центр уведомлений, перейдите на вкладку сегодня и нажмите кнопку "Изменить"")](extensions-images/run02.png#lightbox)

Добавить **DaysRemaining** расширение **сегодня** просмотра и нажмите кнопку **сделать** кнопки:

[![](extensions-images/run03.png "Добавьте расширение DaysRemaining представление сегодня и нажмите кнопку "Готово"")](extensions-images/run03.png#lightbox)

Новое мини-приложение будет добавляться к **сегодня** представление и результаты будут отображаться:

[![](extensions-images/run04.png "Новое мини-приложение будут добавлены в представление сегодняшнего дня и отображаются результаты")](extensions-images/run04.png#lightbox)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>Взаимодействие с помощью ведущего приложения

Пример сегодня расширения, созданной ранее не взаимодействует с его узла приложения ( **сегодня** экрана). В противном случае он будет использовать [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) свойство `TodayViewController` или `CodeBasedViewController` классы. 

Для расширений, которые будет получать данные из ведущего приложения, данные находятся в форме массива [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) объектов, хранящихся в [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) свойство [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) расширения `UIViewController`.

Другое расширение, например расширения редактирования фотографий, может различать пользователя выполнение или Отмена использования. Это будет отправлено обратно в приложение узла через [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) и [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) методы [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) свойство.

Дополнительные сведения см. в разделе Apple [руководство по программированию расширения приложения](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>Взаимодействие с родительского приложения

Группы приложений предоставляют различным приложениям (или одному приложению и его расширениям) общее место хранения файлов. Группы приложений можно использовать для следующих данных:

- [Параметры Apple Watch](~/ios/watchos/app-fundamentals/settings.md).
- [Общие NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Общие файлы](~/ios/watchos/app-fundamentals/parent-app.md#files).

Дополнительные сведения см. в разделе [группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) часть наших **работа с возможностями** документации.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

При работе с расширениями, используйте идентификатор универсальный тип (UTI) для создания и работы с данными, которыми обмениваются приложения, других приложений или служб.

`MobileCoreServices.UTType` Статический класс определяет следующие вспомогательные свойства, связанные с Apple `kUTType...` определения:

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

См. в следующем примере:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Дополнительные сведения см. в разделе [группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) часть наших **работа с возможностями** документации.


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>Меры предосторожности и рекомендации

Модули имеют значительно меньше памяти, доступные на них, чем у приложения. Они должны будут выполнять быстро и с минимальными вторжений пользователю и приложение, которое они размещаются в. Тем не менее это расширение также должен предоставить различение полезные функции много приложение с фирменной символикой пользовательский Интерфейс, который позволяет пользователю определить расширение разработчика или к приложению контейнера, к которой они принадлежат.

Учитывая эти тесной требование, следует развернуть только расширения, которые были тщательно проверены и оптимизированный для производительности и потребления памяти. 

<a name="Summary" />

## <a name="summary"></a>Сводка

Этот документ был проиллюстрирован расширения, что они представляют собой тип точки расширения и известные ограничения, накладываемые iOS на расширение. Он рассматривается создание, распространение, Установка и запуск расширения и расширения жизненного цикла. Он предоставляется пошаговое руководство по созданию простого **сегодня** мини-приложение, показывающая два способа создания графического элемента пользовательского интерфейса с помощью раскадровки или кода. Оно было показано, как проверить расширение в симуляторе iOS. Наконец кратко рассматриваются связи с хост-приложения и несколько меры предосторожности и вопросы, которые следует выполнить при разработке расширения. 


## <a name="related-links"></a>Связанные ссылки

- [ContainerApp (пример)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Создание расширений Xamarin.iOS (видео)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
