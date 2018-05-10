---
title: С помощью MonoGame PipelineTool
description: Средство конвейера MonoGame служит для создания и управления MonoGame проектов. Файлы проектов обрабатываются с помощью средства Monogame конвейера и выводимые в виде .xnb файлов для использования в приложениях CocosSharp и MonoGame.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 50e6c611e285cde9184eed242353ad08b2a941ee
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>С помощью средства MonoGame конвейера

_Средство конвейера MonoGame служит для создания и управления MonoGame проектов. Файлы проектов обрабатываются с помощью средства Monogame конвейера и выводимые в виде .xnb файлов для использования в приложениях CocosSharp и MonoGame._

Средство конвейера MonoGame предоставляет среду для использования для преобразования содержимого файлов в **.xnb** файлов для использования в приложениях CocosSharp и MonoGame. Сведения о содержимого конвейеров и почему они полезны при разработке игры см [этот вводные сведения о конвейерах содержимого](~/graphics-games/cocossharp/content-pipeline/introduction.md)

В этом пошаговом руководстве рассматривается следующее:

 - Установка средства MonoGame конвейера
 - Создание проекта CocosSharp
 - Создание проекта
 - Обработка файлов в средство MonoGame конвейера
 - С помощью файлов во время выполнения

В этом пошаговом руководстве используется проект CocosSharp для демонстрации как **.xnb** файлы можно загрузить и использовать в приложении. Пользователи из MonoGame также смогут ссылку в данном пошаговом руководстве, как CocosSharp и MonoGame использовать тот же **.xnb** файлы содержимого.

Готовое приложение будет отображаться один sprite отображение текстуры из **.xnb** файла и отображение шрифт sprite одну метку **.xnb** файла:

![](walkthrough-images/image1.png "Готовое приложение будет отображаться один sprite отображение текстуры из файла .xnb")


## <a name="monogame-pipeline-tool-discussion"></a>Обсуждение средство MonoGame конвейера

Средство конвейера MonoGame доступно на Windows, Linux и OS X. В этом пошаговом руководстве будет запустите средство на Windows, но он может располагаться после Mac и Linux также. Сведения о получении средства на Max или Linux см. в разделе [эту страницу](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

Возможность создания содержимого для приложений iOS даже при работе в Windows, поэтому разработчики, использующие средство конвейера MonoGame [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) смогут продолжать разработку для Windows.


## <a name="installing-the-monogame-pipeline-tool"></a>Установка средства MonoGame конвейера

Мы начнем с установкой MonoGame, включая конвейера содержимого MonoGame. Обратите внимание, что конвейера содержимого MonoGame отдельный загружаемый компонент для Mac. Можно найти на всех установщиков MonoGame [страницу загрузок MonoGame](http://www.monogame.net/downloads/). Мы будем загрузки MonoGame для Visual Studio, но после установки разработчик может использовать MonoGame в Visual Studio для Mac слишком:

![](walkthrough-images/image2.png "Загрузить MonoGame для Visual Studio, но после установки используйте разработчика MonoGame в Visual Studio для Mac слишком")

После загрузки, мы запуск с помощью программы установки и примите значения по умолчанию. По завершении работы установщика средство конвейера MonoGame устанавливается и можно найти в меню Пуск поиска:

![](walkthrough-images/image3.png "По завершении работы установщика средство конвейера MonoGame установлен и можно найти в меню Пуск поиска")

Запустите средство MonoGame конвейера:

![](walkthrough-images/image4.png "Запустите средство MonoGame конвейера")

После запуска средства конвейера MonoGame мы сможем сделать наш проекты игры и содержимого.


## <a name="creating-an-empty-cocossharp-project"></a>Создавая пустой проект CocosSharp

Следующим шагом является создание проекта CocosSharp. Очень важно, что мы CocosSharp сначала создать проект, чтобы наши содержимого проекта можно сохранить в структуре папки, созданные в проекте CocosSharp. Для понимания структуры проекта CocosSharp, взгляните на [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), который будет использоваться в этом руководстве. Однако при наличии существующего проекта CocosSharp, который вы хотите добавить содержимое, вы можете использовать этот проект вместо BouncingGame.

После создания проекта нам предстоит запустить его, чтобы проверить, что он создает и что у нас есть все, что настроен правильно.

![](walkthrough-images/image5.png "После создания проекта, запустите его, чтобы проверить, что сборка выполняется и что все компоненты настроены правильно")


## <a name="creating-a-content-project"></a>Создание проекта

Теперь, когда у нас есть проектом игры, можно создать проект MonoGame конвейера. Для этого выберите средство конвейера MonoGame **файл > создать...**  и перейдите в папку содержимого проекта. Для Android, папка находится в **[проекта root]\BouncingGame.Android\Assets\Content\**. Для операций ввода-вывода, папка находится в **[проекта root]\BouncingGame.iOS\Content\**.

Изменение **имя файла** для **ContentProject** и нажмите кнопку **Сохранить** кнопки:

![](walkthrough-images/image8.png "Измените имя файла на ContentProject и нажмите кнопку "Сохранить"")

После создания проекта средство конвейера MonoGame выведет информацию о проекте при корневой **ContentProject** выбран элемент:

![](walkthrough-images/image9.png "После создания проекта MonoGame конвейера выводится информацию о проекте при выборе ContentProject корневой элемент")

Рассмотрим некоторые из наиболее важных параметров для содержимого проекта.


### <a name="output-folder"></a>Выходная папка

Это папка (относительно самого содержимого проекта), где выходные данные **.xnb** файлы будут сохранены. Для простоты мы будем использовать ту же папку для хранения нашей входных данных и выходные файлы. Другими словами мы изменим **выходную папку** быть **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Platform

Этот параметр определяет целевую платформу для содержимого. Обратите внимание, что это **Windows** по умолчанию, поэтому нам требуется сделать наш целевой платформы, который является **Android** (или iOS, если вы создали вместе с проект iOS).

![](walkthrough-images/image11.png "Обратите внимание, что это Windows по умолчанию, поэтому изменить целевую платформу, являющийся Android или iOS, если вместе с проект iOS")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>Обработка файлов в средство MonoGame конвейера

Далее мы будем добавлять содержимое в нашем **ContentProject**. Для этого проекта мы будем добавлять файлы в корневой папке проекта, но крупных проектов обычно можно упорядочить их содержимого в папках.

Мы добавим в свой проект два файла:

 - Объект **.png** файл, который будет использоваться для рисования спрайт. Этот файл можно [загрузить здесь](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - Объект **.spritefont** файл, который будет использоваться для рисования текста на экране. Средство содержимого конвейера поддерживает создание новых файлов .spritefont, поэтому нет файла для загрузки.


### <a name="adding-a-png-file"></a>Добавление PNG-файл

Чтобы добавить **.png** файл в проект, мы будем сначала скопировать в каталог проекта конвейера, который имеет **.mgcb** расширения.

![](walkthrough-images/image12.png "Добавьте PNG-файл в проект")

Далее мы добавим файл в проект конвейера. Чтобы сделать это в средстве MonoGame конвейер, выберите **Правка > Добавить элемент...** выберите **ball.png** файла и нажмите кнопку **откройте**. Файл будет частью содержимого проекта и, при выборе его свойства отобразятся:

![](walkthrough-images/image13.png "Файл будет частью содержимого проекта и, при выборе его свойства отобразятся")

Мы будем оставив все значения в их значения по умолчанию, как изменения не требуются для загрузки файла .xnb в CocosSharp. Можно создать файл, выбрав **сборки > построения** параметр меню, который будет начать сборку и отображать данные о построении. Мы убедитесь, что построение работал правильно, проверьте **содержимого** папку для нового **ball.xnb** файла:

![](walkthrough-images/image14.png "Убедитесь, что построение работал правильно, проверьте папку содержимого для нового файла ball.xnb")


### <a name="adding-a-spritefont-file"></a>Добавление файла .spritefont

Можно создать файл .spritefont через программу MonoGame конвейера. CocosSharp требует шрифты в **шрифты** папки и шаблоны CocosSharp автоматически создавать папку шрифты автоматически. Эту папку можно добавить в средство MonoGame конвейера, выбрав **Правка > Добавить > существующую папку...** . Перейдите к **содержимого** папку и выберите **шрифты** папку и нажмите кнопку **ОК**:

![](walkthrough-images/browsetofonts.png "Перейдите к папке содержимого, выберите папку "Шрифты" и нажмите кнопку ОК")

Чтобы добавить новый файл .sprintefont, правой кнопкой мыши папку шрифты и выберите **Добавить > новый элемент...** выберите **описание SpriteFont** вариант, введите имя **arial 36**и нажмите кнопку **ОК**. CocosSharp требует определенных имен файлов шрифтов — они должны быть в формате [FontType]-[FontSize]. Если шрифт не совпадает с этим форматом имен, которые он не будет загружен по CocosSharp во время выполнения.

![](walkthrough-images/image15.png "Если шрифт не совпадает с этим форматом имен он не будет загружен по CocosSharp во время выполнения")

Файл .spritefont является фактически в XML-файл, который можно изменять в любом текстовом редакторе, включая Visual Studio для Mac. Наиболее распространенные переменных, изменения в файле .spritefont `FontName` и `Size` свойства:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Мы будем откройте файл в любом текстовом редакторе. Как наш **arial 36.spritefont** названия, вы должны будете изучить `FontName` как `Arial` , но изменить `Size` значение `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>С помощью файлов во время выполнения

Файлы .xnb теперь встроены и готов для использования в данном проекте. Мы будем добавлять файлы в Visual Studio для Mac, а затем мы добавим код к нашей `GameScene.cs` файл, чтобы загрузить эти файлы и их отображения.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Добавление файлов .xnb в Visual Studio для Mac

Сначала мы добавим файлы в свой проект. В Visual Studio для Mac будет дополнен **BouncingGame.Android** проекта, разверните **активы** папку, щелкните правой кнопкой мыши **содержимого** папки, а затем выберите **Добавить > Добавить файлы...** Во-первых, мы выберем **ball.xnb** мы построенный ранее и нажмите кнопку **откройте**. Затем повторите описанные выше шаги, но добавить **arial 36.xnb** файл. Мы выберем **сохранить файл в его текущем подкаталог** вариант, если система Visual Studio для Mac предлагает способ добавления файла. После завершения работы, оба файла должны быть частью проекта:

![](walkthrough-images/image20.png "После завершения работы, оба файла должны быть частью проекта")


### <a name="adding-gamescenecs"></a>Добавление **GameScene.cs**

Мы создадим класс с именем `GameScene,` которого будут содержать объекты нашей спрайт и текст. Для этого щелкните правой кнопкой мыши **BouncingGame** (не BouncingGame.Android) проект и выберите **Добавить > новый файл...** . Выберите **Общие** категории, выберите **пустым классом** параметр, а затем введите имя **GameScene**.

После создания мы изменим `GameScene.cs` файл, содержащий следующий код:


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

Мы не будем рассматривать приведенный выше код так, как рассматривается работа с CocosSharp визуальных объектов, таких как CCSprite и CCLabelTtf [BouncingGame руководство](~/graphics-games/cocossharp/bouncing-game.md).

Необходимо также добавить код для загрузки нашей только что созданный `GameScene`. Для этого будет открыт `GameAppDelegate.cs` файл (который находится в **BouncingGame** PCL) и измените `ApplicationDidFinishLaunching` метода, так как оно выглядит:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

При запуске, будет выглядеть наш игры:

![](walkthrough-images/image1.png "При запуске, игра будет выглядеть")


## <a name="summary"></a>Сводка

В этом пошаговом руководстве показано, как использовать средство MonoGame конвейер для создания файлов .xnb из входного PNG-файл, а также как создать новый файл .xnb из .sprintefont вновь созданного файла. Он также описаны способы структуры CocosSharp проектов для использования файлов .xnb и загрузить эти файлы во время выполнения.

## <a name="related-links"></a>Связанные ссылки

- [MonoGame загрузки](http://www.monogame.net/downloads/)
- [Документация по MonoGame конвейера](http://www.monogame.net/documentation/?page=Pipeline)
- [Запуск проекта BouncingGame для Android (пример)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.PNG график (пример)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
