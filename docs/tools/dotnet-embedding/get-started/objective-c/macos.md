---
title: "Приступая к работе с macOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 5e2f14e7b29f85e838563914089743f56239d7bb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-macos"></a>Приступая к работе с macOS


## <a name="what-you-will-need"></a>Необходимо будет

* Следуйте инструкциям в нашем [Приступая к работе с Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) руководства.

* Сборки .NET для использования с **Embeddinator 4000**.

* MacOS Cocoa приложения

Продолжайте после выполнения инструкции в нашем [Приступая к работе с Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) руководства. Если уже имеется сборка .NET можно пропустить непосредственно в **с помощью Embeddinator-4000** раздела.

## <a name="creating-a-net-assembly"></a>Создание сборки .NET

Для создания сборки .NET, вам потребуется открыть [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) и создайте новый **проекта библиотеки .NET** во время работы *файл > новое решение > других > .NET > Библиотека*. Нажмите кнопку Далее и предоставьте *погоды* как *имя проекта*и выберите команду *создать*.

Выполните следующие шаги.

1. Удалить **MyClass.cs** файла и **свойства** папки.

2. Щелкните правой кнопкой мыши *погоды проект > Добавить > новый файл.*

3. Выберите *пустым классом* и использовать **XAMWeatherFetcher** как имя, затем нажмите кнопку Создать.

4. Замените содержимое *XAMWeatherFetcher.cs* следующим кодом:

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

Можно будет заметить, что `Using System.Json;` приводит к возникновению ошибки; Чтобы исправить это необходимо сделать следующее:

1. Дважды щелкните **ссылки** папки.

2. Щелкните **пакетов** вкладки.

3. Проверьте **System.Json**.

4. Click **Ok**.

После указанных выше мы нужно лишь собирается нашей сборки .NET, щелкнув *меню "Построение" > собрать все* или ⌘ + b. Построение неудачным, в строке состояния top должно появиться сообщение.

Теперь щелкните правой кнопкой мыши *погоды* узел проекта и выберите *отобразить в средстве поиска*. В системе поиска перейдите к *bin/Debug* папки; внутри его найти **Weather.dll.**

## <a name="using-embeddinator-4000"></a>С помощью Embeddinator 4000

Если вы установили Embeddinator 4000, с помощью наших установщика pkg и запущен новый сеанс терминала после установки можно будет использовать **objcgen** команды (в противном случае можно использовать абсолютный путь к нему: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`); **objcgen** средство, которое необходимо для создания собственной библиотеки из сборки .NET.

Откройте терминалов `cd` в папку, содержащую Weather.dll и выполнение **objcgen** с аргументами, показано ниже:

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

Все, что вы должны будут помещены в **вывода** каталоге рядом с *Weather.dll*. Если у вас есть сборки .NET, замените *Weather.dll* с ним и необходимо выполнить действия, то выше.

## <a name="using-the-generated-output-in-an-xcode-project"></a>С помощью созданные выходные данные проекта Xcode

Откройте в Xcode, а затем создайте **macOS приложения Cocoa** и назовите его **MyWeather**. Щелкните правой кнопкой мыши *узел проекта MyWeather*выберите *добавить файлы для «MyWeather»*, перейдите к **выходные данные** каталога, созданных *Embeddinator 4000* и добавьте следующие файлы:

* Bindings.h
* embeddinator.h
* glib.h
* моно support.h
* mono_embeddinator.h
* objc support.h
* libWeather.dylib
* Weather.dll

Убедитесь, что **копирование элементов при необходимости** проверяется в панели «Параметры» диалогового окна файла.

Теперь нам нужно убедиться, что **libWeather.dylib** и **Weather.dll** завладеть набора приложений:

* Щелкните *узел проекта MyWeather*.
* Выберите *этапов построения* вкладки.
* Добавьте новый *этап копирования файлов*.
* На *назначения* выберите **платформ** и добавьте **libWeather.dylib**.
* Добавьте новый *этап копирования файлов*.
* На *назначения* выберите **исполняемые файлы**, добавьте **Weather.dll** и убедитесь, что *кода вход копирования* проверяется.

Теперь откройте **ViewController.m** и заменить его содержимое:

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

Наконец, запустите проект Xcode и будут отображаться примерно следующим образом:

![Запуск образца MyWeather](macos-images/weather-from-csharp-macos.png)

Пример более полным и изготовление стильной доступен [здесь](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
