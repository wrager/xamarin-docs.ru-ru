---
title: Сводка главе 13. Растровые изображения
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 76551057abc1abdd150591c0a1be39e9f68c4278
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>Сводка главе 13. Растровые изображения

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемент отображает растровое изображение. Все платформы Xamarin.Forms поддерживают форматы файлов JPEG, GIF, PNG и BMP.

Растровые изображения в Xamarin.Forms поступать из четырех местах:

- Через Интернет в соответствии с URL-адреса
- Встроенного как ресурс в общих переносимой библиотеки классов
- Встроенного как ресурс в проектах приложений платформы
- В любом месте, которое можно ссылаться по .NET `Stream` объекта, включая `MemoryStream`

Ресурсы точечный рисунок в PCL являются зависят от платформы, а точечный рисунок ресурсов в проекты платформы зависят от платформы.

Битовая карта задается путем присвоения [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) свойство `Image` для объекта типа [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), абстрактный класс с тремя производные:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) для доступа к растрового изображения в Интернете, на основе `Uri` объекта установить его [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) свойство
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) для доступа к растрового изображения, хранимые в проекте приложения платформы на основе пути файлов и папок его [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) свойство
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) для загрузки растрового изображения с помощью .NET `Stream` объект, заданный путем возвращения `Stream` из `Func` присвоено его [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) свойство

В качестве альтернативы (и часто более) можно использовать следующие статические методы `ImageSource` класса, которые возвращают все `ImageSource` объектов:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) для доступа к растрового изображения в Интернете, на основе `Uri` объекта
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) для доступа к растрового изображения, хранящиеся в виде внедренного ресурса в PCL, приложение или [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) или [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) для доступа к растрового изображения в другой сборке источника
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) для доступа к растрового изображения из проекта приложения платформы
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) для загрузки растрового изображения на основе `Stream` объекта

Отсутствует эквивалент класса из `Image.FromResource` методы. `UriImageSource` Класс является полезным, если требуются для управления кэшированием. `FileImageSource` Класс удобно использовать в XAML. `StreamImageSource` полезно для асинхронной загрузки `Stream` объектов, в то время как `ImageSource.FromStream` является синхронным.

## <a name="platform-independent-bitmaps"></a>Независимая от платформы растровые изображения

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) проекта загружает точечный рисунок через веб-с помощью `ImageSource.FromUri`. `Image` Задан равным `Content` свойство `ContentPage`, поэтому он ограничен размер страницы. Независимо от размера растрового изображения, ограниченного `Image` элемент растягивается на размер контейнера и точечный рисунок отображается в максимального размера в `Image` элемент сохранением пропорции растрового изображения. Области `Image` за пределы растрового изображения окрашенные [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) пример аналогичен, но просто задает `Source` URL-адрес. Преобразование обрабатывается [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) класса.

### <a name="fit-and-fill"></a>Размеры и заполнения

Можно управлять растяжением растрового изображения, задав [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) свойство `Image` для одного из следующих членов [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) перечисления:

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): соблюдает пропорции (по умолчанию)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): заполняет область, не сохраняет пропорции
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): заполняет область, но соблюдает пропорции, возможно благодаря обрезки части растрового изображения

### <a name="embedded-resources"></a>Внедренные ресурсы

Можно добавить файл точечного рисунка для PCL или папку в PCL. Присвойте ему **действие при построении** из **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) образце демонстрируется использование `ImageSource.FromResource` для загрузки файла. Имя ресурса, который передается в метод состоит из имени сборки, за которым следует точка, за которым следует имя необязательно папки и точкой, а затем по имени файла.

Программа наборы `VerticalOptions` и `HorizontalOptions` свойства `Image` для `LayoutOptions.Center`, что делает `Image` неограниченное элемент. `Image` И растрового изображения имеют одинаковый размер:

- В iOS и Android `Image` размер пикселей растрового изображения. Имеется взаимно-однозначное сопоставление между пикселей растрового изображения и экран пикселей.
- Для платформ среды выполнения Windows `Image` размер пикселей растрового изображения в аппаратно независимых единицах. На большинство устройств каждого пикселя точечный рисунок занимает несколько точек.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) пример помещает `Image` в вертикальной `StackLayout` в XAML. Расширения разметки с именем [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) позволяет ссылаться на внедренный ресурс в XAML. Этот класс загружает только ресурсы из сборки, в котором она расположена, поэтому он не может быть помещен в библиотеку.

### <a name="more-on-sizing"></a>Дополнительные сведения об изменения размера

Часто желательно, к точечным рисункам размер постоянно среди всех платформ.
Эксперименты с **StackedBitmap**, можно задать `WidthRequest` на `Image` элемент в вертикальной `StackLayout` обеспечение согласованности между платформами, но размер можно только уменьшить размер, при использовании этого метода.

Можно также задать `HeightRequest` делает изображение размеров, согласованные на платформах, но ограничивает ограниченного Ширина растрового изображения универсальность этого метода. Для изображения в вертикальной `StackLayout`, `HeightRequest` следует избегать.

Лучшим решением является его с растровым изображением, превышает ширину в аппаратно независимых единицах телефона и устанавливает значение `WidthRequest` до требуемой ширины в аппаратно независимых единицах. Это показано в [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) образца.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) отображает главе 7 Льюис Carroll *Анны приключения в Wonderland* с исходной иллюстрации, Джон Tenniel:

[![Тройной экрана mad стороны чая](images/ch13fg16-small.png "Mad текст книги чая стороны Hatters")](images/ch13fg16-large.png#lightbox "Mad текст книги Hatters стороной чая")

### <a name="browsing-and-waiting"></a>Обзор и ожидания

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) образец позволяет пользователю выполнять поиск стандартных изображений, хранящихся на веб-сайте Xamarin. Она использует .NET `WebRequest` класс, чтобы загрузить файл JSON со списком точечных рисунков.

Программа использует [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) для указания, что что-то происходит. При загрузке каждого растрового изображения, доступной только для чтения [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) свойство `Image` — `true`. `IsLoading` Резервируется привязываемые свойства, поэтому `PropertyChanged` событие при изменении этого свойства. Программа присоединяет обработчик этого события и использует текущую настройку `IsLoaded` для задания [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) свойство `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Потоковая передача растровые изображения

`ImageSource.FromStream` Метод создает `ImageSource` основании .NET `Stream`. Метод должен быть передан `Func` объект, который возвращает `Stream` объекта.

### <a name="accessing-the-streams"></a>Доступ к потоки

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) образце демонстрируется использование `ImaageSource.FromStream` метод для загрузки растрового изображения, хранящегося в виде внедренного ресурса и для загрузки растрового изображения в Интернете.

### <a name="generating-bitmaps-at-run-time"></a>Создание растровых изображений во время выполнения

Все платформы Xamarin.Forms поддерживают несжатый формат файлов BMP, который можно легко создать в коде и затем сохранить в `MemoryStream`. Этот прием позволяет типовой Создание растровых изображений во время выполнения, как осуществляется в [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) класса в **Xamrin.FormsBook.Toolkit** библиотеки.

«Самообслуживания» [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) образце демонстрируется использование `BmpMaker` для создания растрового изображения с помощью образа градиента.

## <a name="platform-specific-bitmaps"></a>Специфический для платформы растровые изображения

Все платформы Xamarin.Forms Разрешить сохранение растровых изображений в сборки платформы приложений. При получении приложением Xamarin.Forms, платформа растровых изображения имеют тип [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Они используются для:

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) свойство [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) свойство [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) свойство `Button`

Сборки платформы уже содержат точечные рисунки, значки и экраны-заставки:

- В проекте iOS в **ресурсов** папки
- В проекте Android в подпапках **ресурсов** папки
- В проектах Windows в **активы** папку (несмотря на то, что к этой папке платформы Windows не ограничить растровые изображения)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) примере используется код для отображения значка из проектов платформы приложений.

### <a name="bitmap-resolutions"></a>Битовая карта разрешений экрана

Все платформы позволяют хранить несколько версий растровых изображений для различных устройств разрешений. Во время выполнения нужной версии загружается в зависимости от разрешения экрана устройства.

На iOS эти битовые карты, различаются по суффикс к имени файла:

- Без суффикса для устройств 160 точек на ДЮЙМ (1 пиксель в аппаратно независимая единица)
- "@2x" суффикс для устройств 320 точек на ДЮЙМ (2 точек в DIU)
- "@3x" суффикс для устройств 480 точек на ДЮЙМ (3 точек в DIU)

Точечный рисунок должен отображаться в виде квадрата 1 дюйм будет существовать в трех версиях:

- MyImage.jpg в квадрат 160 пикселей
- MyImage@2x.jpg на квадратный 320 пикселей
- MyImage@3x.jpg в квадратных 480 пикселей

Программа относится это растровое изображение как MyImage.jpg, но соответствующая версия извлекается во время выполнения на основе разрешения экрана. При отсутствии ограничений растровое изображение всегда будет отображаться на 160 аппаратно независимых единицах.

Для Android, битовые карты хранятся в различных вложенных папках **ресурсов** папки:

- drawable-ldpi (низкий точек на ДЮЙМ) для устройств 120 точек на ДЮЙМ (0,75 точек в DIU)
- drawable-mdpi (средний) для устройств 160 точек на ДЮЙМ (1 пиксель в DIU)
- drawable-hdpi (высокий) для устройств 240 точек на ДЮЙМ (1,5 точек в DIU)
- drawable-xhdpi (очень высокий) для устройств 320 точек на ДЮЙМ (2 точек в DIU)
- drawable-xxhdpi (лишние очень высокая) для устройств 480 точек на ДЮЙМ (3 точек в DIU)
- drawable-xxxhdpi (три дополнительных различных) для устройств 640 точек на ДЮЙМ (4 точек, в DIU)

Точечный рисунок предназначен для отображения на квадратный один дюйм различные версии растровое изображение будет иметь то же имя, но другого размера и хранящиеся в этих папках:

- drawable-ldpi или MyImage.jpg на 120 пикселей квадрат
- drawable-mdpi/MyImage.jpg в квадрат 160 пикселей
- drawable-hdpi-MyImage.jpg в квадрат 240 пикселей
- drawable-xhdpi/MyImage.jpg в квадрат 320 пикселей
- drawable-xxhdpi/MyImage.jpg в квадрат 480 пикселей
- drawable-xxxhdpi/MyImage.jpg в квадрат 640 пикселей

Растровое изображение всегда будет отображаться на 160 аппаратно независимых единицах. (Стандартный шаблон решения Xamarin.Forms включает только hdpi, xhdpi и xxhdpi папок.)

Проекты среды выполнения Windows поддерживает растрового изображения, состоящий из коэффициент масштабирования в пикселях аппаратно независимые единицы в процентах, например схему именования:

- MyImage.scale-200.jpg в квадрат 320 пикселей

Допустимы только некоторые проценты. Примеры программ для этой книги включать только изображения с **масштаб 200** суффиксы, но текущего решения шаблоны Xamarin.Forms, которые включают **масштаб 100**, **шкалы 125**, **масштаб 150**, и **масштаб 400**. 

При добавлении растровых изображений в проектах платформы **действие построения** должно быть:

- iOS: **BundleResource**
- Android — **AndroidResource**
- Среда выполнения Windows: **содержимого**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) образце создаются два объекта как кнопки, состоящий из `Image` элементы с `TapGestureRecognizer` установлен. Предполагается, что объекты были квадрат 1 дюйм. `Source` Свойство `Image` задается с помощью `OnPlatform` и `On` объектов для ссылки на имена других файлов на платформах. Растровые изображения включать цифры, указывающее, их размер в пикселях, чтобы можно было видеть, какие размер растрового изображения извлекается и подготовке к просмотру.

### <a name="toolbars-and-their-icons"></a>Панели инструментов и соответствующие значки

Одно из основных применений точечных рисунков платформой является инструментов Xamarin.Forms, которая создается с помощью добавления [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) объектов [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) определяется коллекции`Page`. `ToobarItem` является производным от [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) от которого он наследует некоторые свойства.

Наиболее важным `ToolbarItem` свойства являются:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) для текста, который может присутствовать в зависимости от платформы и `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) Тип `FileImageSource` для образа, который может присутствовать в зависимости от платформы и `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) Тип [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/), перечисление с тремя членами [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), и [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

Количество `Primary` должно быть ограничено трех или четырех элементов. Следует включить `Text` для всех элементов. Для большинства платформ, только `Primary` элементы требуется `Icon` , но требует Windows 8.1 `Icon` для всех элементов. Значки должны иметь размер 32 аппаратно независимых единицах квадрат. `FileImageSource` Тип указывает, что они специфический для платформы.

`ToolbarItem` Активируется [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) событие при касании очень похоже на `Button`. `ToolbarItem` также поддерживает [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) и [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) часто используется в связи с MVVM свойства. (См. [Глава 18, MVVM](chapter18.md)).

IOS и Android требуют страница, на которой отображается панель инструментов [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) или страницы, переход по `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) программы наборы `MainPage` свойство его `App` класса [ `NavigationPage` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) с `ContentPage` аргумент и демонстрируется построение и обработчиком событий панели инструментов.

### <a name="button-images"></a>Изображения кнопки

Растровые изображения платформой также можно использовать для задания [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) свойство `Button` к растровому изображению квадрата 32 аппаратно независимых единицах, как показано в предыдущем [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) образца.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст главе 13 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Образцы в главе 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Работа с образами](~/xamarin-forms/user-interface/images.md)
