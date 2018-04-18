---
title: 'UrhoSharp программирование на F #'
description: 'Как создать простое приложение UrhoSharp с помощью F # в Visual Studio для Mac'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 1496ff10a089829a01ad9993dfbca87d10b18991
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="programming-urhosharp-with-f"></a>UrhoSharp программирование на F #

_Как создать простое приложение UrhoSharp с помощью F # в Visual Studio для Mac_

UrhoSharp могут быть запрограммированы на F #, используя те же библиотеки и основные понятия, используемые программистами C#. [С помощью UrhoSharp](~/graphics-games/urhosharp/using.md) статья Общие сведения о подсистеме UrhoSharp и необходимо ознакомиться перед этой статьи.

Как многие библиотеки, созданные в мире C++ многие функции UrhoSharp возвращают логические значения или целые числа, об успехе или неудаче. Следует использовать `|> ignore` пропускать эти значения.

[Образец программы](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) возможности UrhoSharp из F # «Hello World».

## <a name="creating-an-empty-project"></a>Создавая пустой проект

Нет шаблонов F # для UrhoSharp еще доступны, поэтому для создания проекта UrhoSharp, вы можете либо начинаются с [пример](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) или выполните следующие действия:

1. Создать новую из Visual Studio для Mac **решения**. Выберите **iOS > приложения > одного представления приложения** и выберите **F #** как реализация языка. 
1. Удалить **Main.storyboard** файла. Откройте **Info.plist** файл и в **iPhone / iPod данных развертывания** области удалить `Main` строка в **главного интерфейса** раскрывающегося списка.
1. Удалить **ViewController.fs** также файл.

## <a name="building-hello-world-in-urho"></a>Построение Hello World в Urho

Теперь вы готовы начать определение классов в игру. Как минимум, необходимо определить подкласс `Urho.Application` и переопределите его `Start` метод. Чтобы создать этот файл, щелкните правой кнопкой мыши на проекте F #, выберите **добавить новый файл...**  и добавьте к проекту пустой класс F #. Новый файл будет добавлен в конец списка файлов в проекте, но его необходимо перетащить его отображения *перед* используется в **AppDelegate.fs**.

1. Добавьте ссылку на пакет Urho NuGet.
1. Из существующего проекта Urho копирования (крупный) каталогов **coredata в методе или** и **данных /** в ваш проект **ресурсы или** каталога. В проекте F #, щелкните правой кнопкой мыши **ресурсов** папок и использование **Добавление или добавить существующую папку** и добавьте все эти файлы в проект.

Структуры проекта должен выглядеть так:

![](fsharp-images/solutionpane.png "Структура проекта должен выглядеть так")

Определить к только что созданный класс в качестве подтипа `Urho.Application` и переопределите его `Start` метод:

```csharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Код выполняется очень просто. Она использует `Urho.Gui.Text` класса для отображения строки с выравниванием по центру определенного размера шрифта и цвета. 

Перед запуском этого кода, однако UrhoSharp необходимо инициализировать. 

Откройте файл AppDelegate.fs и изменить `FinishedLaunching` метод следующим образом:

```csharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default` Предоставляет параметры по умолчанию для приложения альбомной ориентации. Передайте эти `ApplicationOptions` конструктора по умолчанию для вашего `Application` подкласс (Обратите внимание, что при определении `HelloWorld` класса строки `inherit Application(o)` вызывает конструктор базового класса). 

`Run` Метод вашей `Application` инициирует программы. Он определяется как возврат `int`, который может быть выведен `ignore`. 

Итоговая программа должно иметь вид:

![](fsharp-images/helloworldfsharp.png "Итоговая программа должна выглядеть")








## <a name="related-links"></a>Связанные ссылки

- [Просмотр на GitHub (пример)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
