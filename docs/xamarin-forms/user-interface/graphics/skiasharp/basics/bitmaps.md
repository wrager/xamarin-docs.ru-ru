---
title: Основы растрового изображения
description: Загрузить растровые изображения из различных источников и их отображения.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: 688c6218f9ac66e3dfd6cd157e43f9b639e124c6
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2018
---
# <a name="bitmap-basics"></a>Основы растрового изображения

_Загрузить растровые изображения из различных источников и их отображения._

Поддержка точечных рисунков в SkiaSharp достаточно большим. В этой статье описываются только основные &mdash; загрузить точечные рисунки и способ их отображения:

![](bitmaps-images/bitmapssample.png "Отображение двух точечных рисунков")

Битовая карта SkiaSharp — это объект типа [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Существует много способов создания растрового изображения, но в этой статье ограничится для [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) метод, который загружает точечный рисунок из [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) объекта, который ссылается на файл растрового изображения. Удобно использовать [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) класс, производный от `SKStream` , так как он имеет конструктор, принимающий .NET [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) объекта.

**Основные растровые изображения** страницы в **SkiaSharpFormsDemos** показано, как загрузить растровые изображения из трех различных источников:

- Через Интернет
- Из ресурса, внедренные в исполняемый файл
- Из библиотеки фото пользователя

Три `SKBitmap` объектов для этих трех источников, определяются в виде полей [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) класса:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Загрузка точечный рисунок из Интернета

Чтобы загрузить точечный рисунок на основе URL-адреса, можно использовать [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) класса, как показано в следующем коде выполняется в `BasicBitmapsPage` конструктора. URL-адрес указывает область на веб-сайт Xamarin некоторые образцы рисунков. Пакет веб-сайта позволяет добавления спецификацию для изменения размеров точечного рисунка для конкретного ширины:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Если растровое изображение были успешно загружены, метод обратного вызова передать `BeginGetResponse` метода. `EndGetResponse` Вызов должен быть в `try` блокировки в случае, если произошла ошибка. `Stream` Получен из `GetResponseStream` недостаточна на некоторых платформах, поэтому содержимое растрового изображения копируются в `MemoryStream` объекта. На этом этапе `SKManagedStream` объект может быть создан. Теперь ссылается на файл точечного рисунка, который, скорее всего, файл JPEG или PNG. `SKBitmap.Decode` Метод декодирует файл точечного рисунка и сохраняет результаты во внутреннем формате SkiaSharp.

Передаваемый в метод обратного вызова `BeginGetResponse` выполняется после завершения выполнения, конструктор, т. е., `SKCanvasView` необходимо недействительным, чтобы разрешить `PaintSurface` обработчика, чтобы обновить отображение. Тем не менее `BeginGetResponse` обратный вызов выполняется в второстепенный поток выполнения, поэтому необходимо использовать `Device.BeginInvokeOnMainThread` для запуска `InvalidateSurface` метод в потоке пользовательского интерфейса.

## <a name="loading-a-bitmap-resource"></a>Загрузка ресурса точечного рисунка

С точки зрения кода простой подход к загрузке рисунков — включая ресурса точечного рисунка в самом приложении. **SkiaSharpFormsDemos** программа включает в себя папку с именем **мультимедиа** с файлом растровое изображение с именем **monkey.png**. В **свойства** диалоговое окно для этого файла необходимо предоставить такой файл **действие при построении** из **внедренный ресурс**!

Каждый внедренный ресурс имеет *идентификатор ресурса* , включающая в себя имя проекта, папки и имя файла, всех подключенных точками: **SkiaSharpFormsDemos.Media.monkey.png**. Можно получить доступ к этому ресурсу, указав этот ресурс идентификатор в качестве аргумента для [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) метод [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) класса:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Это `Stream` объекта могут быть преобразованы прямо в `SKManagedStream` объекта.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Загрузке растрового изображения из библиотеки фото

Также пользователь может загрузить фотографию из библиотеки рисунков устройства. Эта функция позволяет Xamarin.Forms сам не предоставляется. Задание требуется служба зависимостей, как описано в статье [комплектации фотографию из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPicturePicker.cs** файла и трех **PicturePickerImplementation.cs** различных проектов для копирования файлов из этой статьи **SkiaSharpFormsDemos**решения и новые имена пространств имен. Кроме того, Android **MainActivity.cs** файл был изменен таким образом, как описано в статье, а проект iOS предоставлены разрешения на доступ к библиотеке фотографий с двумя строками в нижней части **info.plist**  файла.

`BasicBitmapsPage` Конструктор добавляет `TapGestureRecognizer` для `SKCanvasView` уведомляемых ответвлений. При получении касание `Tapped` обработчик получает доступ к рисунок выбора зависимостей службы и вызовы `GetImageStreamAsync`. Если `Stream` возвращается объект, а затем содержимое копируется в `MemoryStream`в соответствии с требованием, некоторые платформы. Остальная часть кода похожа на двух других методов:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Обратите внимание, что `Tapped` вызовов обработчика `InvalidateSurface` метод `SKCanvasView` объекта. Этот код создает новый вызов `PaintSurface` обработчика.

## <a name="displaying-the-bitmaps"></a>Отображение точечных рисунков

`PaintSurface` Обработчик должен отображать три растровых изображений. Обработчик предполагается, что телефон находится в книжной ориентации и делит холст по вертикали на три равные части.

Отображается первая растровое изображение с простой [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) метод. Все, что вам нужно указать являются координаты X и Y, где будет размещаться верхнего левого угла изображения:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Несмотря на то что `SKPaint` параметр определен, он имеет значение по умолчанию `null` и его можно пропустить. Пиксели в отображаемой области с взаимно-однозначное сопоставление просто передаются пикселей растрового изображения.

Программу можно получить в пикселах точечный рисунок с [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) и [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) свойства. Эти свойства позволяют рассчитать координаты для размещения растровое изображение в центре трети верхней части холста:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Два других точечные рисунки, отображаются с помощью версии [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) с `SKRect` параметр:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Третья версия [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) имеет два `SKRect` аргументы для указания прямоугольную часть точечного рисунка для отображения, но эта версия не используется в этой статье.

Ниже приведен код для отображения растрового изображения, загруженные из внедренного ресурса точечного рисунка:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

Точечный рисунок растягивается до размеров прямоугольника, поэтому monkey растягивается по горизонтали на этих снимках экрана:

[![](bitmaps-images/basicbitmaps-small.png "Тройной снимок экрана страницы основные растровые изображения")](bitmaps-images/basicbitmaps-large.png#lightbox "тройной снимок экрана страницы основные растровые изображения")

Третье изображение &mdash; которой будут отображаться только если вы запустите программу и загрузить фотографию из библиотеки рисунков &mdash; также отображается внутри прямоугольника, но прямоугольника положение и размер корректируются будет сохраняться соотношение сторон растрового изображения. Этот расчет немного сложнее, поскольку для нее требуется вычисление на основе размера точечного рисунка и прямоугольника назначения коэффициент масштабирования и центрирование прямоугольник, в этой области:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Если нет точечный рисунок еще загружены из библиотеки рисунков, а затем `else` текст для запроса у пользователя, коснитесь экрана содержит блок.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Подбор фотографию из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
