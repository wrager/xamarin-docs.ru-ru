---
title: "Xamarin.Forms представлений"
description: "Xamarin.Forms представления являются составными частями кросс платформенных мобильных пользовательских интерфейсов."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: c5bafe12c2cf8c5f8d75757b22223c708ae248dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms представлений

_Xamarin.Forms представления являются составными частями кросс платформенных мобильных пользовательских интерфейсов._

Представления являются объектами пользовательского интерфейса, например меток, кнопки и ползунки, которые обычно называются *элементов управления* или *мини-приложений* в других графический средах программирования. Представления, поддерживаемые Xamarin.Forms, являются производными от [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) класса. Их можно разделить на несколько категорий:

## <a name="views-for-presentation"></a>Представления презентации

### <a name="label"></a>Метка

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Отображает одну строку текста строки или блоки многострочного текста, либо с форматированием переменная или. Задать [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) строковое значение для константы форматирования или набор [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) свойства [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) объекта для переменной форматирование.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [руководство](~/xamarin-forms/user-interface/text/label.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Пример метки](views-images/Label.png "метки пример")](views-images/Label-Large.png#lightbox "метки пример")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Изображение

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) отображает растровое изображение. Растровые изображения можно загрузить через Интернет, внедренных в качестве ресурсов в проекте common или Проекты платформы или создан с помощью .NET `Stream` объекта.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [руководство](~/xamarin-forms/user-interface/images.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Пример изображения](views-images/Image.png "изображения пример")](views-images/Image-Large.png#lightbox "изображения пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Отображает цвет прямоугольника с заливкой [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) свойство. `BoxView` имеет 40 x 40 запрос на размер по умолчанию. Для других размеров назначить [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) и [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) свойства.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [руководство](~/xamarin-forms/user-interface/boxview.md) / [пример 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), и [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Пример BoxView](views-images/BoxView.png "пример BoxView")](views-images/BoxView-Large.png#lightbox "BoxView пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Веб-представление

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Отображает веб-страницы или HTML-содержимого, в зависимости от [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) свойству [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) или [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) объекта.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [руководство](~/xamarin-forms/user-interface/webview.md) / [пример 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) и [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Пример WebView](views-images/WebView.png "примере WebView")](views-images/WebView-Large.png#lightbox "пример веб-представление")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) Отображает рисунки OpenGL в проекты iOS и Android. Не поддерживается для универсальной платформы Windows. Проекты iOS и Android требуют наличия ссылки на **OpenTK 1.0** сборки или **OpenTK** сборки версии 1.0.0.0. `OpenGLView` позволяет использовать в общий проект; При использовании в библиотеку PCL или .NET Standard, затем зависимостей службы также потребуется (как показано в образце кода).<br /><br />Это средство только графики, встроенный в Xamarin.Forms, но приложение Xamarin.Forms также могут отрисовывать графики с помощью [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), или [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![Пример OpenGLView](views-images/OpenGLView.png "пример OpenGLView")](views-images/OpenGLView-Large.png#lightbox "OpenGLView пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Карта

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Отображает соответствие. **Xamarin.Forms.Maps** необходимо установить пакет Nuget. Android и универсальной платформы Windows требуется ключ авторизации карты.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [руководство](~/xamarin-forms/user-interface/map.md) / [образца](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Пример сопоставления](views-images/Map.png "сопоставить пример")](views-images/Map-Large.png#lightbox "пример сопоставления")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Представления, которые запускают команды

### <a name="button"></a>Кнопка

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) представляет собой прямоугольный объект, который отображает текст, и которое запускается [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) событие, если она нажата.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![Пример кнопки](views-images/Button.png "кнопку пример")](views-images/Button-Large.png#lightbox "кнопку пример")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Отображает область для пользователя тип текстовой строки и кнопка (или клавиши клавиатуры), который сообщает приложению для выполнения поиска. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) Свойство предоставляет доступ к тексту и [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) событие означает, что была нажата кнопка.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![Пример SearchBar](views-images/SearchBar.png "пример SearchBar")](views-images/SearchBar-Large.png#lightbox "SearchBar пример")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Представления для задания значений 

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) позволяет пользователю выбрать `double` значение из непрерывного диапазона, заданного с помощью [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) и [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) свойства.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) | [![Пример ползунка](views-images/Slider.png "примере ползунок")](views-images/Slider-Large.png#lightbox "пример ползунка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Пошаговым

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) позволяет пользователю выбрать `double` значение из диапазона добавочного значения, указанные с [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), и [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) свойства.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![Пример с пошаговым](views-images/Stepper.png "пример с пошаговым")](views-images/Stepper-Large.png#lightbox "пример с пошаговым")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Параметр 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) принимает форму включить или выключить коммутатора, чтобы позволить пользователю выбирать логическое значение. [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) Задано состояние коммутатора и [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) событие при изменении состояния.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![Переключение пример](views-images/Switch.png "переключения пример")](views-images/Stepper-Large.png#lightbox "переключения пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) позволяет пользователю выбрать дату с помощью выбора даты платформы. Задайте диапазон допустимых дат с [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) и [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) свойства. [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Свойство является выбранной даты и [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) событие при изменении этого свойства.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) | [![Пример DatePicker](views-images/DatePicker.png "DatePicker пример")](views-images/DatePicker-Large.png#lightbox "примере DatePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) позволяет пользователю выбрать время с Выбор времени платформы. [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) Свойство является выбранный период времени. Приложение может отслеживать изменения в `Time` свойство путем установки обработчика для [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) событий.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![Пример TimePicker](views-images/TimePicker.png "пример TimePicker")](views-images/TimePicker-Large.png#lightbox "TimePicker пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Для редактирования текста

Эти классы являются производными от [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) класс, определяющий [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) свойство.

<a name="entry" />

### <a name="entry"></a>Ввод

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) позволяет пользователю вводить и редактировать одну строку текста. Этот текст будет доступен как [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) свойство и [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) и [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) события запускаются изменения текста или пользователь Уведомляет о завершении, нажав клавишу ВВОД.<br /><br />Используйте [ `Editor` ](#editor) для ввода и редактирования несколько строк текста.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [руководство](~/xamarin-forms/user-interface/text/entry.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Пример записи](views-images/Entry.png "пример записи")](views-images/Entry-Large.png#lightbox "пример записи")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Редактор

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) позволяет пользователю вводить и редактировать несколько строк текста. Этот текст будет доступен как [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) свойство и [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) и [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) события запускаются изменения текста или пользователь Уведомляет о завершении.<br /><br />Используйте [ `Entry` ](#entry) представление для ввода и редактирования одну строку текста.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [руководство](~/xamarin-forms/user-interface/text/editor.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Пример записи](views-images/Editor.png "пример редактора")](views-images/Editor-Large.png#lightbox "пример редактора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Представления для указания действия

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) использует анимации, чтобы показать, что приложение занимается длительное время действия без указания какого-либо указания хода выполнения. [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Свойство управляет анимацией.<br /><br />Если известно, ход выполнения действия, используйте [ `ProgressBar` ](#progressbar) вместо него.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![Пример ActivityIndicator](views-images/ActivityIndicator.png "пример ActivityIndicator")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) использует анимации, чтобы показать, что приложение выполняет посредством длительное время действия. Задать [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) свойство значения от 0 до 1 для отображения хода.<br /><br />Если известен ход выполнения действия, используйте [ `ActivityIndicator` ](#activityindicator) вместо него.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![Пример элемента управления ProgressBar](views-images/ProgressBar.png "примере ProgressBar")](views-images/ProgressBar-Large.png#lightbox "пример элемента управления ProgressBar")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Представления, отображающие коллекций

### <a name="picker"></a>Средство выбора

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Отображение выбранного элемента из списка текстовых строк и при выборе этого элемента при выборе элемента представления. Задать [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) список строк, или [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) коллекция объектов. [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Событие возникает, когда элемент выбран.<br /><br />`Picker` Отображает список элементов только в том случае, если он установлен. Используйте [ `ListView` ](#listView) или [ `TableView` ](#tableView) прокручиваемый список, который остается на странице.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [руководство](~/xamarin-forms/user-interface/picker/index.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Пример выбора](views-images/Picker.png "пример выбора")](views-images/Picker-Large.png#lightbox "пример выбора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) является производным от [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) и отображает прокручиваемый список элементов данных, доступный для выбора. Задать [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) коллекция объектов, а также установите [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) свойства [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) объект, описывающий, как их для форматирования. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Событие сообщает, что был сделан выбор, который доступен как [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) свойство.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [руководство](~/xamarin-forms/user-interface/listview/index.md) / [образца](https://developer.xamarin.com/samples/WorkingWithListview) | [![Пример ListView](views-images/ListView.png "пример ListView")](views-images/ListView-Large.png#lightbox "пример ListView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Отображает список строк типа [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) с необязательные заголовки и подзаголовки. Задать [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) свойство для объекта типа [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)и добавьте [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) объектов, `TableRoot`. Каждый `TableSection` — это совокупность `Cell` объектов.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [руководство](~/xamarin-forms/user-interface/tableview.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Пример TableView](views-images/TableView.png "пример TableView")](views-images/TableView-Large.png#lightbox "TableView пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Образец Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
