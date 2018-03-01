---
title: "Реализация преобразования текста в речь"
description: "Использовать помощью DependencyService для вызова API преобразования текста в речь собственного каждой платформы"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: c5bd753aaa3a9e807a03a9fb05b233cfa39d0dc3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="implementing-text-to-speech"></a>Реализация преобразования текста в речь

В этой статье помогут вам при создании кросс платформенные приложения, в котором используется [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) для доступа к встроенным API преобразования текста в речь:

- **[Создание интерфейса](#Creating_the_Interface)**  &ndash; понять, как интерфейс создается в общем коде.
- **[Реализация iOS](#iOS_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинный код для iOS.
- **[Реализация Android](#Android_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинном коде для Android.
- **[Реализация Windows](#WindowsImplementation)**  &ndash; Узнайте, как реализовать интерфейс в машинном коде для Windows Phone и универсальной платформы Windows (UWP).
- **[Реализация в общем коде](#Implementing_in_Shared_Code)**  &ndash; использование `DependencyService` вызывать собственную реализацию из общего кода.

Приложения с помощью `DependencyService` будет иметь следующую структуру:

![](text-to-speech-images/tts-diagram.png "Структура приложений помощью DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Создание интерфейса

Сначала необходимо Создайте интерфейс в общий код, который выражает функциональные возможности, которые планируется реализовать. Например, интерфейс содержит один метод `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Процесс разработки для этого интерфейса в общий код позволит приложению доступ к API-интерфейсы речи на каждой платформе Xamarin.Forms.

> [!NOTE]
> **Примечание**: классы, реализующие интерфейс должен иметь конструктор для работы с `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Реализация iOS

Интерфейс должен быть реализован в каждом проекте специфический для платформы приложений. Обратите внимание, что класс имеет конструктор, чтобы `DependencyService` можно создавать новые экземпляры.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

`[assembly]` Атрибут класс регистрируется как реализация `ITextToSpeech` интерфейс, который означает, что `DependencyService.Get<ITextToSpeech>()` может использоваться в общий код, чтобы создать его экземпляр.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Реализация Android

Android код сложнее, чем iOS версии: требует реализации класса для наследования от конкретного Android `Java.Lang.Object` и реализовать `IOnInitListener` также интерфейс.

Xamarin.Forms также предоставляет `Forms.Context` объекта, который является экземпляром в текущем контексте Android. Многие методы пакета SDK для Android это требуется, хороший пример, в возможности вызова `StartActivity()`.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{

    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(Forms.Context, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

`[assembly]` Атрибут класс регистрируется как реализация `ITextToSpeech` интерфейс, который означает, что `DependencyService.Get<ITextToSpeech>()` может использоваться в общий код, чтобы создать его экземпляр.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone и реализации платформы универсальных приложений Windows

Windows Phone и универсальная платформа Windows имеют речи API в `Windows.Media.SpeechSynthesis` пространства имен. Единственная оговорка заключается в запомнить, для деления **микрофон** возможность в манифесте, в противном случае доступ к речи, API-интерфейсы, блокируются.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

`[assembly]` Атрибут класс регистрируется как реализация `ITextToSpeech` интерфейс, который означает, что `DependencyService.Get<ITextToSpeech>()` может использоваться в общий код, чтобы создать его экземпляр.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Реализация в общий код

Теперь мы можно писать и тестировать общий код, который обращается к интерфейсу озвучивания текста. Эта простая страница содержит кнопки, которая запускает функцию речи. Она использует `DependencyService` для получения экземпляра `ITextToSpeech` интерфейс &ndash; во время выполнения этот экземпляр будет реализации платформой, имеет полный доступ к собственного пакета SDK.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

Выполнение этого приложения на iOS, Android, или платформ Windows и нажатие кнопки приведет к разговора можно с помощью собственного speech SDK на каждой платформе приложения.

 ![iOS и Android озвучивания текста кнопки](text-to-speech-images/running.png "пример преобразования текста в речь")


## <a name="related-links"></a>Связанные ссылки

- [С помощью с помощью DependencyService (пример)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Преобразование текста в речь книги](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
