---
title: Знакомство с помощью DependencyService
description: В этой статье описывается, как класс с помощью Xamarin.Forms DependencyService работает для доступа к функциям собственной платформы.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 0b81d429b0488603c7a487421cb7f32c1f3cf890
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240997"
---
# <a name="introduction-to-dependencyservice"></a>Знакомство с помощью DependencyService

## <a name="overview"></a>Обзор

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) Разрешает приложениям вызове платформой функциональные возможности из общего кода. Эта функция позволяет приложениям Xamarin.Forms делать все, что можно сделать собственное приложение.

`DependencyService` Представляет Сопоставитель зависимостей. На практике определен интерфейс и `DependencyService` находит правильной реализации этого интерфейса в различных проектах платформы.

## <a name="how-dependencyservice-works"></a>Работа с помощью DependencyService

Xamarin.Forms приложения должны использовать четыре компонента `DependencyService`:

- **Интерфейс** &ndash; требуемую функциональность определяется интерфейс в общем коде.
- **Реализации на платформе** &ndash; классы, реализующие интерфейс должны добавляться к каждому проекту платформы.
- **Регистрация** &ndash; каждого реализующего класса должен быть зарегистрирован с `DependencyService` посредством атрибута метаданных. Регистрация позволяет `DependencyService` найти класс реализации и предоставление его вместо интерфейса во время выполнения.
- **Вызов с помощью DependencyService** &ndash; общего кода необходимо явным образом вызвать `DependencyService` запрашивать реализаций интерфейса.

Обратите внимание, что реализации должно быть указано для каждой платформы проекта в решении. Проекты платформы без реализации завершится ошибкой во время выполнения.

Структура приложения описан на следующей схеме:

![](introduction-images/overview-diagram.png "Структура приложений помощью DependencyService")

### <a name="interface"></a>Интерфейс

Интерфейс, который разработки будет определять при взаимодействии с платформой функциональные возможности. Будьте внимательны при разработке компонента в качестве компонента или пакета Nuget. Структура API можно сделать или прервать выполнение пакета. В приведенном ниже примере указывает простой интерфейс для озвучивания текста, который обеспечивает гибкость при определении слов, которые необходимо произнести, но оставляет реализация, которую нужно настроить для каждой платформы:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Реализация каждой платформы

После подходящий интерфейс было разработано, этот интерфейс должен быть реализован для каждой платформы, которая будет использоваться в проекте. Например, следующий класс реализует `ITextToSpeech` интерфейса на iOS:

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>Регистрация

Каждая реализация интерфейса должен быть зарегистрирован с `DependencyService` с помощью атрибута метаданных. Следующий код регистрирует реализацию для iOS:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Подведение итогов, специфический для платформы реализация выглядит следующим образом:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

Примечание:, регистрация выполняется на уровне пространства имен, а не на уровне класса.

#### <a name="universal-windows-platform-net-native-compilation"></a>Универсальные приложения для Windows платформа .NET компиляция в собственном коде

Проекты UWP, используйте параметр компиляции машинного кода .NET, должны соответствовать [немного другой конфигурации](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) при инициализации Xamarin.Forms. Компиляция машинного кода .NET также требует немного отличается регистрации для зависимых служб.

В **App.xaml.cs** файла, вручную зарегистрировать каждой службы зависимостей, определенных в проекте UWP с помощью `Register<T>` метода, как показано ниже:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Примечание: с помощью ручной регистрации `Register<T>` — действует только в выпуске построений с помощью компиляции машинного кода .NET. Если опустить эту строку, отладочные сборки по-прежнему будет работать, но окончательные сборки не удастся загрузить службы зависимостей.

### <a name="call-to-dependencyservice"></a>Вызов с помощью DependencyService

Когда проект настроен с общий интерфейс и реализации для каждой платформы, используйте `DependencyService` получить правой реализацию интерфейса во время выполнения:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` найти правильной реализации интерфейса `T`.

### <a name="solution-structure"></a>Структура решения

[Образец решения UsingDependencyService](https://developer.xamarin.com/samples/UsingDependencyService/) будет показано ниже для iOS и Android, и изменения кода, описанные выше выделена.

 [![iOS и Android решение](introduction-images/solution-sml.png "помощью DependencyService образец решения структуры")](introduction-images/solution.png#lightbox "помощью DependencyService пример структуры решения")

> [!NOTE]
> Вы **должен** обеспечить реализацию в каждом проекте платформы. При регистрации не реализацию интерфейса то `DependencyService` не сможет разрешить `Get<T>()` метод во время выполнения.


## <a name="related-links"></a>Связанные ссылки

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
