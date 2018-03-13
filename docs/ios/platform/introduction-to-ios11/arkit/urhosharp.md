---
title: "Использование ARKit с UrhoSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 94963e92f8372316a982bbe38f1fb653d38b2a3b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="using-arkit-with-urhosharp"></a>Использование ARKit с UrhoSharp

С появлением [ARKit](https://developer.apple.com/arkit/), Apple представляет собой простой разработчикам создавать приложения в режиме дополненной реальности. ARKit можно отслеживать точную позицию устройство обнаружения различных областей по всему миру и сами разработчику blend данных, поступающих из ARKit в код.

[UrhoSharp](~/graphics-games/urhosharp/index.md) предоставляет полную и простые в использовании 3D API, который можно использовать для создания приложения с трехмерной графикой.   Они могут быть объединены вместе, ARKit физического информацию о системе и Urho для отображения результатов.

Эта страница содержит сведения о подключении этих двух миров вместе для создания приложений, отличный режиме дополненной реальности.


## <a name="the-basics"></a>Основы

Мы хотим этого делать — представления 3D содержимого на основе мира, отображающееся iPhone.   Идея заключается в том, для использования содержимого, поступающих от камера телефона с 3D-контент и, когда пользователь телефона перемещает вокруг комнаты, чтобы убедиться, что трехмерный объект действовать так, как они входит в состав этой комнате — это делается путем привязки объектов в мире.

![Анимированный рисунок в ARKit](urhosharp-images/image1.gif)


Мы будем использовать библиотеку Urho загрузить подключающее ресурсы 3D и помещать их в мире, и мы будем использовать ARKit для возвращения потока видео, поступающих от камеры, а также расположение телефона в мире.   При перемещении с свой телефон, мы будем использовать изменения в расположении для обновления, отображающий Urho ядра системы координат.

Таким образом, когда помещения объекта в трехмерном пространстве и пользователь перемещает указатель, расположение 3D-объекта отражает месте и расположение, где был помещен.

## <a name="setting-up-your-application"></a>Настройка приложения

### <a name="ios-application-launch"></a>Запуск приложения iOS

Приложению iOS необходимо создать и запустить 3D-содержимого, для этого создается подкласс реализации [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) и предоставить код установки путем переопределения `Start` метод.  Это происходит, когда сцены заполняется данными событий, обработчиками установки и т. д.

Мы разработали `Urho.ArkitApp` класса, который наследуется от класса `Urho.Application` и на его `Start` метод выполняет всю тяжелую работу.   Все что нужно делать вашей существующей Urho приложение измените базовый класс типа `Urho.ArkitApp` и у вас есть приложение, которое будет выполняться ваш urho сцены в мире.

### <a name="the-arkitapp-class"></a>Класс ArkitApp

Этот класс предоставляет набор удобный значения по умолчанию, как это может быть с некоторых ключевых объектов, а также обработки событий ARKit малозаметными операционной системой.

Программа установки выполняется `Start` виртуального метода.   При переопределении этого метода на подкласс необходимо убедитесь, что в цепочке на родительский с помощью `base.Start()` на свою собственную реализацию.

`Start` Метод настраивает сцены, просмотра, камеры и направленный свет и предоставляет доступ к их как открытые свойства:

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) для хранения объектов,
- направленный [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) с тенью и расположение которого доступна через `LightNode` свойство
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) , компоненты которого обновляется, когда ARKit доставляет обновления приложения и
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) отображения результатов.


### <a name="your-code"></a>Код

Необходимо создать подкласс `ArkitApp` класса и переопределить `Start` метод.   Первое, что следует делать метод — цепочки до `ArkitApp.Start` путем вызова `base.Start()`.  После этого можно использовать любой Настройка свойства с помощью ArkitApp для добавления объектов в сцену, настроить индикаторы, тени и события, которые необходимо обработать.

В образце ARKit/UrhoSharp загружает анимированных символом текстур и воспроизводит анимации, со следующей реализацией:

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

И, фактически является все, что необходимо сделать на этом этапе, чтобы система отображается в режиме дополненной реальности 3D-содержимого.

Urho использует пользовательские форматы для трехмерных моделей и анимации, поэтому вам нужно экспортировать активов в этот формат.   Можно использовать такие средства, как [Urho3D Blender надстройки](https://github.com/reattiva/Urho3D-Blender) и [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , можно преобразовать эти ресурсы из распространенных форматов, например DBX, DAE, OBJ, Blend, 3D-Max, в формат, предусмотренного Urho.

Дополнительные сведения о создании трехмерных приложений с помощью Urho находятся [введение в UrhoSharp](~/graphics-games/urhosharp/introduction.md) руководства.

## <a name="arkitapp-in-depth"></a>ArkitApp в глубину

> [!NOTE]
> Этот раздел предназначен для разработчиков, которые требуется настроить среду по умолчанию UrhoSharp и ARKit или нужно получить получить более глубокое представление о работе интеграции.   Этот раздел необязателен.

ARKit API очень просто, создание и настройка [ARSession](https://developer.apple.com/documentation/arkit/arsession) объекта, который затем доставлять [ARFrame](https://developer.apple.com/documentation/arkit/arframe) объектов.   Они содержат изображения, захваченные камеры, а также ожидаемое положение реальных устройства.

Мы будет составление образы, которые доставляются с камеры к нам с нашей трехмерное содержимое и настроить в UrhoSharp сопоставляемое вероятность в расположении устройства и положение камеры.

На следующей схеме показано, что выполняется `ArkitApp` класса:

[![Схема классов и экранов в ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Подготовка к просмотру кадров

Идея проста, объединить видео, поступающих из камеры с нашей трехмерной графики, чтобы создать объединенный изображение.     Мы будет использоваться для получения ряд эти записанные образы в последовательности, и мы будет смешивать вводимых с Urho сцены.

Самый простой способ сделать это — для вставки [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) в главный [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Это набор команд, которые выполняются для рисования один кадр.  Эта команда будут заполнены с любые текстуры, которые передать его в области просмотра.    Мы этой настройки первый кадр, — процесс, а фактическое определение выполняется в th **ARRenderPath.xml** файл, загруженный на этом этапе.

Тем не менее мы столкнулись с необходимостью blend этих двух миров вместе две проблемы:


1. На iOS, текстур графического Процессора должен иметь разрешение, которое не является степенью двух, но не имеют разрешения, которые возведения в квадрат, например кадры, которые получаются из камеры: 1280 x 720.
2. Кадры, которые кодируются в [YUV](https://en.wikipedia.org/wiki/YUV) формате, представленный два изображения - яркость и цветности.

Кадры YUV иметь два различных способа устранения.  1280 x 720 изображение, представляющее яркости (по сути изображение оттенков серого) и гораздо меньше 640 x 360 цветности компонента:

![Демонстрация объединение Y и компоненты UV изображения](urhosharp-images/image3.png)


Для рисования полного цветной образа с помощью OpenGL ES нужно написать небольшой шейдера, который принимает яркости (компонент Y) и цветности (UV плоскости) из слотов текстуры.  В UrhoSharp они имеют имена — «sDiffMap» и «sNormalMap» и преобразовывать их в формате RGB:

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

Для подготовки к просмотру текстуры, которая не является степенью двух разрешения у нас есть определение Texture2D со следующими параметрами:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Поэтому для них возможность захваченного изображения к просмотру как фоновый рисунок и визуализации подобное пугающие объект-мутант любой сцены над ним.

### <a name="adjusting-the-camera"></a>Настройка камеры

`ARFrame` Объекты также содержат позиции предполагаемое устройства.  Мы теперь необходимо переместить игры камеры ARFrame — перед ARKit не голограммы проблема отслеживания ориентации устройства (накат, шаг, относительно оси y) и отрисовки закрепленных голограмму поверх видео -, но если немного Переместите устройство - будет смещения.

Что происходит, потому что встроенные датчики, например гироскопа не имеют возможности отслеживания изменений, они могут только ускорение.  Анализ ARKit каждой функции фрейма и извлекает точки для отслеживания и таким образом, способна пришлите нам точный преобразования матрица, содержащая данные, перемещения и поворота.

Например Вот как можно получить текущую позицию.

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Мы используем `-row.Z` поскольку ARKit используется правая система координат.


### <a name="plane-detection"></a>Обнаружение плоскости

ARKit может определять горизонтальной плоскости и эта возможность позволяет взаимодействовать с реального мира, например, можно поместить объект-мутант на реальную таблицу или этаж. Для этого проще всего использовать метод HitTest (raycasting). Он преобразует экранные координаты (0,5, 0,5 является центр) в реальном мире координаты (0; 0; 0 — первый кадр расположение).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

Теперь можно поместить объект-мутант на горизонтальной поверхности в зависимости от места на экране устройства, которые мы коснитесь:

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![Анимированный рисунок изменение плоскостей как перемещается представления](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Реалистичное освещение

В зависимости от условий освещения реального мира виртуальный сцены должно быть светлее или темнее для лучшего соответствия окружающей среды. ARFrame содержит LightEstimate свойство, которое можно использовать для настройки освещения Urho это делается следующим образом:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Помимо iOS - HoloLens

UrhoSharp [работает на всех основных операционных систем](~/graphics-games/urhosharp/platform/index.md), поэтому можно повторно использовать существующий код в другом месте.

HoloLens является одним из наиболее интересных платформами, к которым он выполняется.   Это означает, что можно легко переключаться между iOS и HoloLens для построения здорово дополнить реальности приложений, использующих UrhoSharp.

Можно найти источник MutantDemo в [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Связанные ссылки

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (с UrhoSharp) (пример)](https://github.com/EgorBo/ARKitXamarinDemo)
