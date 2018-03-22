---
title: "Распознавание речи"
description: "В этой статье предоставляет новый API речи и показано, как реализовать его в приложении Xamarin.iOS для поддержки распознавания речи непрерывные и переписать Воспроизведение речи (прямые или записанные аудиопотоков) в текст."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e868c0ee71688e208c5217d9f5a89ea3acec988c
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="speech-recognition"></a>Распознавание речи

_В этой статье предоставляет новый API речи и показано, как реализовать его в приложении Xamarin.iOS для поддержки распознавания речи непрерывные и переписать Воспроизведение речи (прямые или записанные аудиопотоков) в текст._

Новые для iOS 10 Apple имеет выпустить API распознавания речи, который позволяет приложению iOS поддерживают распознавание речи непрерывные и переписать Воспроизведение речи (прямые или записанные аудиопотоков) в текст.

В соответствии с Apple API распознавания речи имеет следующие возможности и преимущества:

- Точные
- Современная
- Простота использования
- Быстрый
- Поддерживает несколько языков.
- Конфиденциальность пользователей отношениях

## <a name="how-speech-recognition-works"></a>Как работает распознавания речи

Распознавание речи реализуется в приложения iOS получении динамической либо предварительно записанного звука (в любой из языков, поддерживаемых API) и передает его на распознавателя речи, возвращающий подписка произносимых слов на обычный текст.

[![](speech-images/speech01.png "Как работает распознавания речи")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Диктовки клавиатуры

При большинство пользователей обработки распознавания речи на устройстве iOS, они думают о встроенных голосовой помощник Siri, который был выпущен вместе с клавиатуры диктовки в iOS 5 с iPhone 4S.

Клавиатура диктовки поддерживается любой элемент интерфейса, который поддерживает TextKit (такие как `UITextField` или `UITextArea`) и активируется пользователем кнопки диктовки (непосредственно слева от пробела) в виртуальной клавиатуры операций ввода-вывода.

Apple выпущена следующая статистика диктовки клавиатуры (собранных с момента 2011 г.):

- Клавиатура диктовки широко использует со времени выпуска в iOS 5.
- Примерно 65 000 приложений, использующих его в день.
- О треть всех операций ввода-вывода диктовки выполняется в приложении сторонних производителей.

Клавиатуры диктовки просты в использовании, поскольку для него не требуется никаких действий со стороны разработчика, без использования элемента интерфейса TextKit при разработке пользовательского интерфейса приложения. Преимущество диктовки клавиатуры также не требует запросы специальные права доступа из приложения, прежде чем можно будет использовать.

Приложения, использующие новые интерфейсы API распознавания речи потребуется особые разрешения должны быть предоставлены пользователем, поскольку для распознавания речи требуется передачи и временного хранения данных на серверах компании Apple. См. в нашем [безопасности и конфиденциальности улучшения](~/ios/app-fundamentals/security-privacy.md) Дополнительные сведения см.

Во время диктовки клавиатуры можно легко реализовать, он иметь некоторые ограничения и недостатки:

- Требуется использование текстового поля ввода и отображения на клавиатуре.
- Она работает с ввода только звук в реальном времени, и приложение не может управлять процессом записи звука.
- Он предоставляет не контролирует язык, который используется для интерпретации речи пользователя.
- Нет возможности для приложения и знать, если кнопка диктовки даже для пользователя.
- Приложение не может настраивать процесс записи звука.
- Он предоставляет очень неполную набора результатов, не содержит информации, такие как расписание и надежности.

### <a name="speech-recognition-api"></a>API распознавания речи

Новые iOS 10 Apple выпустила API распознавания речи, который обеспечивает более эффективный способ для приложения iOS для реализации распознавания речи. Этот API является тем же, что Apple использует питания Siri и диктовки клавиатуры и он способен предоставлять с точностью современные быстрого транскрипции.

Результаты в API распознавания речи прозрачно настраиваются для отдельных пользователей, без необходимости сбора или получить доступ к любой личных данных пользователей приложения.

API распознавания речи предоставляет результаты вызывающего приложения в приближенном к реальному времени как произношения пользователя и предоставляет дополнительные сведения о результатах преобразования не только текст. Сюда входит следующее.

- Говорят, что несколько интерпретаций пользователь.
- Уровни надежности для отдельных переводы.
- Сведения о времени.

Как уже говорилось выше, аудио для преобразования может быть указано либо путем динамического канала или из предварительно записанные источника и в любом из более чем 50 языков и диалекты, поддерживаемые iOS 10.

API распознавания речи может использоваться с любого устройства iOS под управлением iOS 10 и в большинстве случаев требуется активное подключение к Интернету, так как большая часть переводы выполняется на серверах компании Apple. С другой стороны, некоторые новые устройства всегда поддерживают на iOS, на устройстве перевода для конкретных языков.

Apple включила доступности API, чтобы определить доступность для перевода в текущий момент данного языка. Приложения следует использовать этот API вместо тестирования для подключения к Интернету, сам напрямую.

Как упоминалось выше в разделе диктовки клавиатуры, распознавания речи требует передачи и временного хранения данных на серверах компании Apple, через Интернет и таким образом, приложение _должен_ запрашивать разрешение пользователя на выполнение распознавание, включая `NSSpeechRecognitionUsageDescription` ключа в его `Info.plist` файла и вызывая метод `SFSpeechRecognizer.RequestAuthorization` метод. 

В зависимости от источника звука, используемый для распознавания речи другие изменения в приложение `Info.plist` может потребоваться файл. См. в нашем [безопасности и конфиденциальности улучшения](~/ios/app-fundamentals/security-privacy.md) Дополнительные сведения см.

## <a name="adopting-speech-recognition-in-an-app"></a>Внедрение распознавания речи в приложении

Существует четыре основных шагов, которые должен выполнить разработчик внедрению распознавания речи в приложении iOS.

- Описание использования в приложении `Info.plist` текстовый файл с помощью `NSSpeechRecognitionUsageDescription` ключа. Например, приложение камеры могут включать следующее описание _«Это позволяет принимать фотографию просто произнеся слово «Цилиндрическая».»_
- Запросить авторизацию путем вызова `SFSpeechRecognizer.RequestAuthorization` метод для представления объяснение (в `NSSpeechRecognitionUsageDescription` выше разделе) почему приложение хочет речи распознавания доступ к пользователю в диалоговом окне и разрешить им принять или отклонить.
- Создание запроса распознавания речи:
    * Предварительно записанного звукового на диске используйте `SFSpeechURLRecognitionRequest` класса.
    * Звук в реальном времени (или звука из памяти), используйте `SFSPeechAudioBufferRecognitionRequest` класса.
- Передать на запрос распознавания речи распознаватель речи (`SFSpeechRecognizer`) для начала распознавания. Приложения могут при необходимости удерживать возвращаемый `SFSpeechRecognitionTask` для наблюдения за результаты распознавания.

Ниже подробно рассматриваются следующие действия.

### <a name="providing-a-usage-description"></a>Предоставляет описание использования.

Для предоставления необходимых `NSSpeechRecognitionUsageDescription` ключа в `Info.plist` файла, выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните `Info.plist` файл, чтобы открыть его для редактирования.
2. Переключитесь в **источника** представления: 

    [![](speech-images/speech02.png "В представлении источника")](speech-images/speech02.png#lightbox)
3. Щелкните **добавьте новую запись**, введите `NSSpeechRecognitionUsageDescription` для **свойство**, `String` для **тип** и **Описание использования** как **значение**. Пример: 

    [![](speech-images/speech03.png "Добавление NSSpeechRecognitionUsageDescription")](speech-images/speech03.png#lightbox)
4. Если приложение будет обработки динамической аудио транскрипции, потребуется также описание использования микрофона. Щелкните **добавьте новую запись**, введите `NSMicrophoneUsageDescription` для **свойство**, `String` для **тип** и **Описание использования** как **значение**. Пример: 

    [![](speech-images/speech04.png "Добавление NSMicrophoneUsageDescription")](speech-images/speech04.png#lightbox)
4. Сохраните изменения в файле.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните `Info.plist` файл, чтобы открыть его для редактирования.
3. Щелкните **добавьте новую запись**, введите `NSSpeechRecognitionUsageDescription` для **свойство**, `String` для **тип** и **Описание использования** как **значение**. Пример: 

    [![](speech-images/speech03w.png "Добавление NSSpeechRecognitionUsageDescription")](speech-images/speech03w.png#lightbox)
4. Если приложение будет обработки динамической аудио транскрипции, потребуется также описание использования микрофона. Щелкните **добавьте новую запись**, введите `NSMicrophoneUsageDescription` для **свойство**, `String` для **тип** и **Описание использования** как **значение**. Пример: 

    [![](speech-images/speech04w.png "Добавление NSMicrophoneUsageDescription")](speech-images/speech04w.png#lightbox)
4. Сохраните изменения в файле.

-----

> [!IMPORTANT]
> Невозможность можно задать только одно из перечисленных выше `Info.plist` ключей (`NSSpeechRecognitionUsageDescription` или `NSMicrophoneUsageDescription`) может привести к приложения, при попытке доступа к распознавания речи или микрофон для динамической аудио завершается ошибкой без предупреждения.




### <a name="requesting-authorization"></a>Запрос авторизации

Для запроса авторизации пользователе, который позволяет приложению обращаться к распознавания речи, измените основной класс View Controller и добавьте следующий код:

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

`RequestAuthorization` Метод `SFSpeechRecognizer` класс запрашивать разрешение пользователя для распознавания речи доступ с помощью причина, по которой разработчик приведенного в `NSSpeechRecognitionUsageDescription` ключ `Info.plist` файла.

Объект `SFSpeechRecognizerAuthorizationStatus` результат возвращается `RequestAuthorization` процедура обратного вызова метода, который может использоваться для действий в зависимости от разрешений пользователя. 

> [!IMPORTANT]
> Apple предлагает ждать, пока пользователь начинает действия в приложении, которое требуется распознавания речи, прежде чем запросить это разрешение.

### <a name="recognizing-pre-recorded-speech"></a>Распознавание речи предварительно записанные

Если приложение желает распознавания речи из файла предварительно записанные WAV или MP3, его можно использовать следующий код:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Этот код, подробно касается, во-первых, он пытается создать распознавания речи (`SFSpeechRecognizer`). Если язык по умолчанию не поддерживается для распознавания речи `null` возвращается и выхода функции.

Если язык по умолчанию распознаватель речи, приложение проверяет, находится ли он в настоящее время доступны для использования распознавания `Available` свойство. Например распознавание могут оказаться недоступными в том случае, если на устройстве нет подключения к Интернету.

Объект `SFSpeechUrlRecognitionRequest` создается на основе `NSUrl` расположение файла предварительно записанные на устройстве iOS и он передается распознаватель речи для обработки с процедура обратного вызова.

При вызове функции обратного вызова, если `NSError` не `null` произошла ошибка, должны быть обработаны. Так как распознавание речи выполняется последовательно, процедура обратного вызова может быть вызван более одного раза, поэтому `SFSpeechRecognitionResult.Final` свойство проверяется ли преобразование завершается и записывается лучшую версию перевода (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Распознавание речи в режиме реального времени

Если приложение желает распознавания речи в режиме реального времени, очень похожа на распознавание речи предварительно записанные процесс. Пример:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Просматривая подробно этот код, он создает несколько закрытые переменные для обработки процесса распознавания:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

Она использует AV Foundation записывать звук, который будет передан `SFSpeechAudioBufferRecognitionRequest` для обработки запроса распознавания:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Приложение пытается начать запись и все ошибки обрабатываются в том случае, если не удается запустить запись:

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Распознавание задача запускается и дескриптор сохраняется для задач распознавания (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Обратный вызов используется таким же образом, используемой выше на предварительно записанные речи.

Если запись является APM остановлена пользователем, получат подсистема аудио и на запрос распознавания речи:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Если пользователь отменяет запрос распознавания, затем информируются Engine аудио и распознавания задач:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Очень важно вызвать `RecognitionTask.Cancel` перевод для освобождения памяти и процессора устройства был отменен пользователем.

> [!IMPORTANT]
> Если клиент не получит `NSSpeechRecognitionUsageDescription` или `NSMicrophoneUsageDescription` `Info.plist` ключей может привести в приложении произошел сбой без предупреждения при попытке доступа к распознавания речи или микрофон для звук в реальном времени (`var node = AudioEngine.InputNode;`). См. в разделе **предоставляет описание использования** Дополнительные сведения в разделе выше.

## <a name="speech-recognition-limits"></a>Ограничения распознавания речи

При работе с распознавания речи в приложении iOS, Apple налагает следующие ограничения:

- Распознавание речи предоставляется бесплатно для всех приложений, но его использование не является неограниченное:
    - Устройства iOS отдельных имеют ограниченное число отзывы, которые могут быть выполнены в день.
    - Приложения будет регулироваться глобально на основе запроса в день.
- Приложение необходимо подготовить для обработки сетевого подключения распознавание речи и сбои ограничение частоты использования.
- Распознавание речи может быть дорогостоящей стока батареи и увеличится сетевой трафик на устройства iOS, из-за этого, Apple накладывает ограничение в strict аудио длительность около минуты речи max.

Если приложение регулярно попадание пределы регулирование скорости, Apple запрашивает разработчик обратиться к нему.

## <a name="privacy-and-usability-considerations"></a>Конфиденциальность и удобство использования замечания

Apple имеет следующие подсказку для будут прозрачными и соблюдением конфиденциальности пользователей, при включении распознавания речи в приложение iOS.

- При записи речи пользователя, убедитесь, что четко указать, что записи выполняется в пользовательском интерфейсе приложения. Например приложение может звуковое уведомление «записи» и отображать индикатор записи.
- Не используйте распознавания речи конфиденциальные пользовательские сведения, такие как пароли, работоспособности или финансовые данные.
- Показать результаты распознавания _перед_ действует на них. Это не только предоставляет отзывы о том, что делает выполняет приложение, но дает пользователю возможность обработки ошибок распознавания, как они были сделаны.

## <a name="summary"></a>Сводка

Эта статья представлен новый API речи и было показано, как реализовать его в приложении Xamarin.iOS для поддержки распознавания речи непрерывные и переписать Воспроизведение речи (прямые или записанные аудиопотоков) в текст. 



## <a name="related-links"></a>Связанные ссылки

- [SpeakToMe (пример)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
