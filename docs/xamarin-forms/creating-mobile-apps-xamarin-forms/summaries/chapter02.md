---
title: "Сводка Глава 2. Составляющие приложение"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 893030170175403c7f7d6885e924e425b4f73c05
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Сводка Глава 2. Составляющие приложение

В приложении Xamarin.Forms объектов, которые занимают место на экране, называются *визуальные элементы*, инкапсулированный с [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) класса. Визуальные элементы можно разделить на три категории, соответствующий этих классов:

- [страница](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Макет](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [Вид](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

Объект `Page` производный класс занимает весь экран или почти во весь экран. Часто является потомком страницы `Layout` производный класс для упорядочения дочерних визуальных элементов. Дочерние элементы `Layout` могут выполняться другие `Layout` классы или `View` производные (часто называемые *элементы*), — знакомые объекты, такие как текст, растровые изображения, ползунки, кнопки, списки и т. д.

В этой главе показано, как создать приложение, сосредоточившись на [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), который является `View` производный класс, который отображает текст.

## <a name="say-hello"></a>Поприветствуйте

С платформой Xamarin установлено можно создать новое решение Xamarin.Forms в Visual Studio или Visual Studio для Mac. [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) решение использует переносимой библиотеки классов для общий код. Он демонстрирует решения Xamarin.Forms, созданного в Visual Studio без изменений. Решение состоит из шести проектов (последней, два из которых создаются без текущие шаблоны решения Xamarin.Forms).

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), Переносимая библиотека классов (PCL) совместно в других проектах
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), проект приложения для Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), проект приложения для iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), проект приложения для универсальной платформы Windows (Windows 10 и Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), проект приложения для Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), проект приложения для Windows Phone 8.1

Внесите любые из этих проектов приложений запускаемого проекта и затем постройте и запустите программу на устройстве или в симуляторе.

Во многих программах Xamarin.Forms не изменяя проекты приложений. Часто они остаются очень мала заглушки просто для запуска программы. Большая часть фокус будет переносимой библиотеки классов, общие для всех приложений.

## <a name="inside-the-files"></a>Внутри файлов

Визуальные элементы, отображаемые **Hello** программы определяются в конструкторе [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) класса. `App` является производным от класса Xamarin.Forms [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

**Ссылки** раздел **Hello** проект переносимой библиотеки Классов включает следующие сборки Xamarin.Forms:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Ссылки** разделы проектов пять приложения содержат дополнительных сборок, которые применяются для отдельных платформ:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Каждый из проектов приложений содержит вызов статического `Forms.Init` метод в `Xamarin.Forms` пространства имен. Это инициализирует библиотеку Xamarin.Forms. С другой версией `Forms.Init` определяется для каждой платформы. Вызов этого метода можно найти в следующих классов:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App` класса, `OnLaunched` метод](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` класса, `OnLaunched` метод](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` класса, `OnLaunched` метод](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Кроме того, необходимо создать экземпляр каждой платформы `App` класса расположение в PCL. Это происходит при вызове `LoadApplication` в следующие классы:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

В противном случае эти проекты приложений — это обычные программы «ничего не делать».

## <a name="pcl-or-sap"></a>PCL или SAP?

Можно создать решение Xamarin.Forms с общий код в переносимой библиотеки классов (PCL) или общего ресурса проекта (SAP). Чтобы создать решение SAP, выберите параметр Shared в Visual Studio. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) решение демонстрирует SAP шаблон без изменений.

Пакеты подход PCL общего кода в проекте библиотеки ссылается проектов платформы приложений. С данным подходом SAP общий код эффективно существует во всех проектах приложения платформы и распределяется между ними.

Большинство разработчиков Xamarin.Forms предпочтительнее подход PCL. В этой книге большинство решений являются PCL. Те, которые используют SAP включают **Sap** суффикс имени проекта.

Для поддержки всех платформ Xamarin.Forms, версии .NET, используемый PCL должны включать следующие платформы:

- .NET Framework 4,5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Classic)

Это называется 111 профиля ПК.

В случае SAP код в общий проект может выполнять разный код для различных платформ, с помощью директивы препроцессора C# (`#if`, #`elif`, и `#endif`) со следующими предопределенные идентификаторы:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

В PCL можно определить, какие платформы, вы используете во время выполнения, как можно будет увидеть ниже в этой главе.

## <a name="labels-for-text"></a>Метки для текста

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) решений показано, как добавить новый файл C# для **Greetings** проекта. Этот файл определяет класс с именем `GreetingsPage` , производный от `ContentPage`. В этой книге, большинство проектов содержат один `ContentPage` производный класс, имя которого является имя проекта с суффиксом `Page` добавляется.

`GreetingsPage` Конструктор создает [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) представление, которое является Xamarin.Forms представление, которое отображает текст. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Задано значение текста, отображаемого элементом `Label`. Эта программа задает `Label` для `Content` свойство `ContentPage`. Конструктор `App` создает экземпляр класса `GreetingsPage` и задает для его `MainPage` свойство.

Текст отображается в левом верхнем углу страницы. На iOS это означает, что он перекрывает строки состояния страницы. Существует несколько решений этой проблемы:

### <a name="solution-1-include-padding-on-the-page"></a>Решение 1. Включать заполнение на странице

Задать [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) свойство на странице. `Padding` относится к типу [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), структуру, используя четыре свойства:

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` Определяет область внутри страницы, где исключается содержимого. Это позволяет `Label` Чтобы избежать перезаписи в строке состояния операций ввода-вывода.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Решение 2. Включать заполнение только для операций ввода-вывода (SAP)

Задайте свойство «Заполнение» только для iOS с помощью SAP с директивой препроцессора C#. Это показано в [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) решения.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Решение 3. Включать заполнение только для операций ввода-вывода (PCL или SAP)

В версии Xamarin.Forms используется для книги `Padding` можно выбрать свойство для операций ввода-вывода в PCL или SAP с помощью [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) или [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) статический метод. Эти методы являются устаревшими

`Device.OnPlatform` Методы используются для выполнения кода под конкретную платформу или выберите конкретные платформы значения. Они убедитесь, использование [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) статическое свойство только для чтения, то возвращает член [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) перечисления:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) для Windows 8.1, Windows Phone 8.1 и все устройства UWP.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), ранее используется для идентификации Windows Phone 8.0, а также является теперь неиспользуемые
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) не используется

`Device.OnPlatform` Методы, `Device.OS` свойство и `TargetPlatform` перечисления всех теперь рекомендуется к использованию. Вместо этого используйте [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) свойство и сравнить `string` возвращаемое значение с следующие статические поля:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), строка «iOS» 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), строка «Android»
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), строка «UWP», ссылающийся на платформу среды выполнения Windows
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), строка «Windows» для среды выполнения Windows (Windows 8.1 и Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), строка «WinPhone» для Windows Phone 8.0 

[ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) Связанные статическое свойство только для чтения. Это возвращает член [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), который содержит следующие члены:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) не используется

Для iOS и Android, отсечение между `Tablet` и `Phone` с книжной шириной 600 единиц. Для платформы Windows `Desktop` указывает приложения UWP, запущенного с Windows 10 `Tablet` — это программа Windows 8.1 и `Phone` указывает приложения UWP, на компьютерах под управлением Windows 10 или приложения Windows Phone 8.1.

## <a name="solution-3a-set-margin-on-the-label"></a>Решение 3a. Задание границы метки

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Свойство было введено слишком поздно для включения в книге, но он также имеет тип `Thickness` , вы можете задать на `Label` для определения области за пределами представления, который включен в расчет макет представления.

`Padding` Свойство доступно только на [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) и [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) производных продуктов. `Margin` Свойство доступно для всех [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) производных продуктов.

## <a name="solution-4-center-the-label-within-the-page"></a>Решение 4. Метка на странице центра

Можно выровнять по центру `Label` в `Page` (или поместить его в один из восьми знаков), задав [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойства `Label` значение типа [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). `LayoutOptions` Структура определяет два свойства:

- [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) Свойство типа [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), перечисление с четырьмя элементами: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), означающее, что левого или верхнего в зависимости от Ориентация [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), означающее правому или нижнему в зависимости от ориентации и [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) Свойство типа `bool`.

Обычно эти свойства не используется напрямую. Вместо этого сочетания этих свойств предоставляются восьми статических свойств только для чтения типа `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` и `VerticalOptions` являются наиболее важные свойства в макете Xamarin.Forms и рассматриваются более подробно в [ **Доп. Прокрутка стека**](chapter04.md).

Ниже приведен результат с `HorizontalOptions` и `VerticalOptions` свойства `Label` равными `LayoutOptions.Center`:

[![Тройной экрана приветствия программы](images/ch02fg05-small.png "метки по горизонтали и вертикали по центру")](images/ch02fg05-large.png "метки по горизонтали и вертикали по центру")

## <a name="solution-5-center-the-text-within-the-label"></a>Решение 5. Центрирование текста в метке

Выравнивание текста по центру (или поместить его в восьми расположения на странице), задав [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) и [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) свойства `Label` членом [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) перечисления:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), значение left или top (в зависимости от ориентации)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), то есть правому или нижнему (в зависимости от ориентации)

Эти свойства определяются только `Label`, тогда как `HorizontalAlignment` и `VerticalAlignment` свойства, определенные в `View` и наследуется всеми `View` производных продуктов. Visual результаты покажутся похожими, но это совершенно разные, как показано в следующей главе.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 2 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Образцы Глава 2](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Образцы Глава 2 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Начало работы с Xamarin.Forms](~/xamarin-forms/get-started/index.md)
