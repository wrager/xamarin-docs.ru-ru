---
title: Элементы управления вручную камеры в Xamarin.iOS
description: Этот документ описывает использование framework AVFoundation операций ввода-вывода в с Xamarin.iOS элементов управления вручную камеры. Управление камерой вручную предоставить пользователю возможность управления фокус, Баланс белого и параметры экспозиции.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a0f605a38117df87a03801c3b9d86b0b7361c232
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790829"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Элементы управления вручную камеры в Xamarin.iOS

Элементы управления камеры вручную, предоставляемые `AVFoundation Framework` в iOS 8, разрешить мобильных приложению полный контроль над камеру на устройстве iOS. Этот уровень точного контроля можно использовать для создания профессиональных камеры уровня приложений и композиции исполнителя, изменив параметры камеры, при этом по-прежнему изображение или видео.

Эти элементы управления, также можно использовать при разработке приложений, научных исследований или промышленным результаты менее предназначены для определения правильности или красоту изображения куда предназначены для более выделение некоторые компонента или элемента, выполняемом изображения.

## <a name="avfoundation-capture-objects"></a>Объекты отслеживания измененных AVFoundation

Ли подсчета, видео или изображения с помощью камеру на устройстве iOS, процесс, используемый для создания этих образов не изменилась. Это верно для приложений, использующих элементы управления по умолчанию автоматическое камеры или те, которые воспользоваться преимуществами новых элементов управления камеры вручную:

 [![](intro-to-manual-camera-controls-images/image1.png "Общие сведения об объектах захвата AVFoundation")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Входные данные берутся из `AVCaptureDeviceInput` в `AVCaptureSession` посредством `AVCaptureConnection`. Результат выводится либо как по-прежнему изображение или видео потока. Управляет всем процессом `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Вручную элементы управления

С помощью новых API, предоставляемые iOS 8, приложение можно контролировать следующие функции камеры:

-  **Вручную фокус** —, позволяя пользователю контролировать фокус напрямую, приложение может обеспечивать больший контроль над изображением выполнены.
-  **Ручной выдержки** —, предоставляя вручную контролировать доступность, приложение может предоставлять пользователям большую свободу и разрешить им получите стилизованные результат.
-  **Ручной баланс белого** — используются баланс белого цвета изображения — часто, чтобы сделать его реалистичные. Разные источники света имеют разные цвета температуры, а параметры камеры, используемый для записи образа корректируется, чтобы компенсировать эти различия. Опять же позволяя пользователю контроль над баланс белого, пользователи могут вносить изменения, которые не может выполняться автоматически.


iOS 8 предоставляет расширения и усовершенствования существующих iOS API-интерфейсы для предоставления этого точного управления изображение процесса записи.

## <a name="bracketed-capture"></a>В квадратных скобках отслеживания

Заключенных в скобки сбора данных основан на параметрах от элементов управления вручную камеры, представленные выше, что позволяет приложению можно записать на момент времени, в ряд различными способами.

Проще говоря, заключенных в скобки записи будет по-прежнему изображения, полученные с различные параметры из рисунка рисунок в пакетном режиме.

## <a name="requirements"></a>Требования

Ниже перечислены необходимые условия для выполнения шагов, представленных в этой статье:

-  **Xcode 7 + и iOS 8 или более новой версии** — Apple Xcode 7 и iOS 8 или более новые API необходимо установить и настроить на компьютере разработчика.
-  **Visual Studio для Mac** — последнюю версию Visual Studio для Mac должен быть установлен и настроен на устройстве пользователя.
-  **iOS 8 устройства** — самая последняя версия iOS 8 устройства iOS. Функции камеры можно тестировать в эмуляторе iOS.


## <a name="general-av-capture-setup"></a>Настройка отслеживания AV Общие

При записи видео на устройстве iOS, имеется Общая Настройка код, который всегда является обязательным. В этом разделе рассматриваются минимальные установки, необходимые для записи видео с камеры устройства iOS и отображения видео в режиме реального времени в `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Делегат буфера образец вывода

Одна в первую очередь необходимо будет делегат для наблюдения за образец выходного буфера и отобразить изображение из буфера `UIImageView` в пользовательском Интерфейсе приложения.

Следующая процедура будет отслеживать образец буфера и обновление пользовательского интерфейса:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

С помощью этой процедуры в месте `AppDelegate` можно изменить, чтобы открыть AV сеанса записи для записи канала активного видео.

### <a name="creating-an-av-capture-session"></a>Создание сеанса записи AV

Сеанс захвата AV используется для управления записи потокового видео из камеры устройства iOS и для получения видео в приложения iOS требуется. Так как пример `ManualCameraControl` образец приложения использует сеанс захвата в разных местах, он будет настроен в `AppDelegate` и становятся доступными для всего приложения.

Выполните следующую команду, чтобы изменить приложение `AppDelegate` и добавить необходимый код:


1. Дважды щелкните `AppDelegate.cs` файл в обозревателе решений, чтобы открыть его для редактирования.
1. Добавьте следующие операторы using в верхней части файла:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Добавьте следующие частные переменные и вычисляемого свойства для `AppDelegate` класса:
    
    ```
    #region Private Variables
    private NSError Error;
    #endregion
    
    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```
  
1. Переопределить метод завершения и измените его на:
    
    ```
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;
    
        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }
    
        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }
    
        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);
    
        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();
    
        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }
    
        // Attach input to session
        Session.AddInput (Input);
    
        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;
    
        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);
    
        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```  
  
1. Сохраните изменения в файле.


Этот код в месте Управление камерой вручную, можно легко реализовать для экспериментов и тестирования.

## <a name="manual-focus"></a>Фокус вручную

Позволяя пользователю непосредственно использовать элементы управления фокус, приложение может обеспечивать более художественного контроль над копии образа.

Например, professional фотографии можно смягчить фокус изображения для достижения [Bokeh эффект](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Эффект Bokeh")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Или создать [эффект по запросу фокус](http://www.mediacollege.com/video/camera/focus/pull.html), такие как:

[![](intro-to-manual-camera-controls-images/image3.png "Эффект запросу фокус")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Для специалистов по анализу или модуль записи медицинские приложения приложение может потребоваться программно перемещать объектив для экспериментов. В любом случае новый интерфейс API позволяет конечному пользователю или приложению контроль над фокус на момент изображения.

### <a name="how-focus-works"></a>Как работает фокус

Прежде чем рассматривать подробности управление фокуса в приложении IOS 8. Давайте кратко рассмотрим, как работает фокус в устройстве iOS:

[![](intro-to-manual-camera-controls-images/image4.png "Как работает фокус в устройстве iOS")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Светлая вводит линзы камеру на устройстве iOS и основное внимание уделено изображения датчика. Расстояние от линзы от элементов управления датчика, где — это центральный узел (область, где отображается четкость изображения), в связи с датчиком. Чем дальше линза — от датчика, расстояние объектов показаться сильная и чем ближе рядом объектов показаться четкость.

В устройстве iOS объектив перемещается ближе к или дальше от датчика магниты и springs. В результате точное расположение объектив невозможна, как он будет зависеть от устройства к устройству и может повлиять на параметры, например ориентацию устройства или возраст устройства и spring.

### <a name="important-focus-terms"></a>Особое внимание нужно уделить условия

При работе с фокусом, существует несколько терминов, которые разработчик должен быть знаком с:

-  **Длина поля** — расстояние между объектами ближайшего и крайний фокус. 
-  **Макрос** -это near конце спектра фокус, близкому, по которому можно сосредоточиться объектив.
-  **Бесконечности** — это дальнем конце спектра фокуса и — это самый дальний расстояние, по которому можно сосредоточиться объектив.
-  **Расстояние Hyperfocal** — это точка в спектра фокус где только на другой стороне фокус будет самый дальний объекта в кадре. Другими словами это положение фокальной, обеспечивает максимальную глубину поля. 
-  **Дисторсии позиции** — это все вышеперечисленное определяет другие условия. Это является расстояние от линзы из датчика и тем самым фокуса.


С этими условиями и знания в виду новых элементов управления фокус вручную можно реализовать в приложении iOS 8 успешно.

### <a name="existing-focus-controls"></a>Существующие элементы управления фокус ввода

iOS 7 и более ранних версий, предоставляемые существующих элементов управления фокус через `FocusMode`свойство как:

-   `AVCaptureFocusModeLocked` — Фокус блокируется в точке одного фокус.
-   `AVCaptureFocusModeAutoFocus` — Камеры выполняет очистку линзы через все фокальной точки, пока не обнаруживает острым фокус и затем остается.
-   `AVCaptureFocusModeContinuousAutoFocus` — Камеры refocuses всякий раз при обнаружении условие из в фокусе.


Существующие элементы управления также предоставляется точки можно задать интересующие через`FocusPointOfInterest` свойства, чтобы пользователь сможет нажать сосредоточиться на определенной области. Приложение также можно отслеживать перемещение следить за состоянием отслеживая `IsAdjustingFocus` свойство.

Кроме того, ограничение диапазона был предоставлен `AutoFocusRangeRestriction` свойство как:

-   `AVCaptureAutoFocusRangeRestrictionNear` — Ограничивает autofocus близлежащих глубины. Полезно в ситуациях, такие как сканирование QR-код или штрих-код.
-   `AVCaptureAutoFocusRangeRestrictionFar` — Ограничивает autofocus отдаленных глубины. Полезно в ситуациях, где находятся объекты, которые были определены как не значения в поле зрения (например, для рамки окна).


И, наконец `SmoothAutoFocus` свойство, которое замедляет алгоритм автоматически фокус и его шаги более мелким шагом для устранения перемещений артефакты при записи видео.

### <a name="new-focus-controls-in-ios-8"></a>Новые элементы управления фокус в iOS 8

Помимо функций, уже поддерживаемых по iOS 7 и более поздних версий следующие функции доступны теперь управления фокус в iOS 8:

-  Полная ручное управление позиции следить за состоянием при блокировке фокус.
-  Наблюдение за ключ значение позиции следить за состоянием в любом режиме фокуса.


Для реализации описанных функций `AVCaptureDevice` класс была изменена, чтобы включать только для чтения `LensPosition` свойство, используемое для получения текущего положения объектив камеры.

Для ручного управления позицию следить за состоянием, устройство записи необходимо в режиме фокусировки заблокирован. Пример

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked` Метод устройства записи используется для настройки положения объектив камеры. Процедура необязательно обратного вызова может предоставлять получать уведомление, когда изменения вступят в силу. Пример

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Как показано в приведенном выше коде устройства записи должна быть заблокирована для конфигурации перед выполнением изменений в положении следить за состоянием. Допустимые значения позиции следить за состоянием находятся в диапазоне от 0,0 и 1,0.

### <a name="manual-focus-example"></a>Пример вручную фокус

Общая Настройка захвата AV кодом на месте `UIViewController` могут быть добавлены в раскадровку приложения и настроены следующим образом:

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController могут быть добавлены к приложениям раскадровки и настроено, как показано здесь")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Представление содержит следующие основные элементы:

-  Объект `UIImageView` , будет отображаться видео веб-канала.
-  Объект `UISegmentedControl` , будет изменен режим фокусировки автоматически заблокирована.
-  Объект `UISlider` , будет отображать и обновляют текущее положение следить за состоянием.


Выполните следующие действия для передачи вверх контроллера представления для элемента управления фокус вручную:


1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```  
  
1. Добавьте следующие вычисляемые свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Переопределить `ViewDidLoad` метода и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Переопределить `ViewDidAppear` метода и добавьте следующую команду, чтобы начать запись при загрузке представления:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. С камеры в режиме Auto ползунок перемещаются автоматически при камеры регулирует фокус:

    [![](intro-to-manual-camera-controls-images/image6.png "Ползунок перемещаются автоматически при камеры регулирует фокус в этот образец приложения")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Коснитесь сегмента Locked и перетащите положение ползунка позицию следить за состоянием вручную:

    [![](intro-to-manual-camera-controls-images/image7.png "Вручную Настройка положения следить за состоянием")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Завершите работу приложения.


Приведенный выше код показано, как отслеживать позицию следить за состоянием при камеры в автоматическом режиме, или используйте ползунок для управления позицией следить за состоянием, когда он находится в режиме заблокирована.

## <a name="manual-exposure"></a>Ручной выдержки

Доступность ссылается яркости изображения относительно яркости источника и определяется по свет достигает датчика, как долго, а также уровень усиления датчика (сопоставление ISO). Предоставляя вручную контролировать доступность, приложение может предоставить конечным пользователям большую свободу и разрешить им получите стилизованные результат.

Использование элементов управления раскрытия вручную, пользователь может принимать изображение из что ярко темной и moody.

[![](intro-to-manual-camera-controls-images/image8.png "Образец рисунок, демонстрирующий доступность из что ярко темной и moody")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Опять же это можно сделать с помощью программный элемент управления автоматически для научных приложений или вручную элементов управления пользовательского интерфейса приложения. В любом случае новый iOS 8 раскрытия API предоставляют тонкую настройку параметров камеры раскрытия.

### <a name="how-exposure-works"></a>Как работает раскрытия

Прежде чем рассматривать подробности управление уязвимость в приложении IOS 8. Давайте кратко рассмотрим, как работает раскрытия:

[![](intro-to-manual-camera-controls-images/image9.png "Как работает раскрытия")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Ниже приведены три основные элементы, которые объединяются в целях управления раскрытия.

-  **Скорость затвора** — это продолжительность времени, затвора открыт, чтобы позволить свет на датчик камеры. Наименьшая время затвора открыт меньше свет, пусть и четче изображение является (четкостью перемещения). Чем больше затвора открыто, более светлый является let в и более движения размытия, возникающее.
-  **Сопоставление ISO** — это термин, позаимствованная из фильма фотографии и относится к конфиденциальности химические вещества в фильмов света. Значения Low ISO в фильмов имеют меньше детализации и более точно цветопередачи; Низкие значения ISO на цифровые датчики имеют снижается уровень шума датчика, но меньше яркости. Чем выше значение ISO, светлый изображения, но с шумом несколько датчиков. Мера «ISO» на цифровой датчик [электронных рост](http://en.wikipedia.org/wiki/Gain), не физические характеристики. 
-  **Дисторсии отверстие** — это размер открывающие следить за состоянием. На всех устройствах iOS отверстие следить за состоянием фиксирована, поэтому только два значения, которые могут использоваться для настройки раскрытия скорость затвора и ISO.


### <a name="how-continuous-auto-exposure-works"></a>Как непрерывные Works раскрытия Auto

Прежде чем обучения как ручной выдержки работает, он является хорошим следует понять, как непрерывная воздействия автоматически работает на устройствах iOS.

[![](intro-to-manual-camera-controls-images/image10.png "Как непрерывного автоматического раскрытия работает на устройствах iOS")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Сначала это блок раскрытия автоматически, его задания вычисления идеальный уязвимость, постоянно вводятся Stats измерения. Он использует эти сведения для вычисления оптимальное сочетание ISO и скорость затвора, для получения хорошо горит сцены. Этот цикл называется AE цикла.

### <a name="how-locked-exposure-works"></a>Работает как заблокированные раскрытия

Теперь давайте рассмотрим как заблокированные раскрытия работает на устройствах iOS.

[![](intro-to-manual-camera-controls-images/image11.png "Как заблокированные раскрытия работает на устройствах iOS")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Также есть блок раскрытия автоматически, при попытке вычислить оптимальное iOS и значения длительности. Тем не менее в этом режиме AE блок отключен от обработчика отслеживания использования Stats.

### <a name="existing-exposure-controls"></a>Существующие элементы управления раскрытия

iOS 7 и более поздних версий, предоставляют следующие существующие элементы управления раскрытия через `ExposureMode` свойство:

-   `AVCaptureExposureModeLocked` — Примеры сцены один раз и использует эти значения во всей сцены.
-   `AVCaptureExposureModeContinuousAutoExposure` — Примеры сцены непрерывно, чтобы убедиться, также включен.


`ExposurePointOfInterest` Может использоваться для коснитесь, чтобы предоставлять сцены, выбрав целевой объект для предоставления и мониторинга приложения `AdjustingExposure` свойство, чтобы узнать, когда устанавливается раскрытия.

### <a name="new-exposure-controls-in-ios-8"></a>Новые элементы управления уязвимость в iOS 8

Помимо функций, уже поддерживаемых по iOS 7 и более поздних версий следующие функции доступны теперь управления уязвимость в iOS 8:

-  Полностью вручную пользовательских данных.
-  Get, Set и ключ значение наблюдать операций ввода-ВЫВОДА и скорости затвора (длительность).


Для реализации описанных функций новый `AVCaptureExposureModeCustom` режим был добавлен. Когда камеры в пользовательский режим, настроить длительность раскрытия и ISO можно использовать следующий код:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

В режиме Auto и заблокирована приложение можно настроить сдвиг автоматического раскрытия подпрограммы, используя следующий код:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Параметр минимального и максимального диапазонов зависящие от устройства приложения, на котором выполняется, поэтому они должны никогда не быть фиксированными. Вместо этого используйте следующие свойства для получения минимального и максимального значения диапазонов:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Как показано в приведенном выше коде устройства записи должна быть заблокирована для конфигурации, перед тем как сделать влияет на доступность.

### <a name="manual-exposure-example"></a>Пример ручной выдержки

Общая Настройка захвата AV кодом на месте `UIViewController` могут быть добавлены в раскадровку приложения и настроены следующим образом:

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController могут быть добавлены к приложениям раскадровки и настроено, как показано здесь")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Представление содержит следующие основные элементы:

-  Объект `UIImageView` , будет отображаться видео веб-канала.
-  Объект `UISegmentedControl` , будет изменен режим фокусировки автоматически заблокирована.
-  Четыре `UISlider` элементов управления, которые будет отображать и обновлять смещение, продолжительность, ISO и смещение.


Выполните следующие действия для передачи показатель представление-контроллер для экспозиции вручную:


1. Добавьте следующие операторы using:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```  
  
1. Добавьте следующие вычисляемые свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Переопределить `ViewDidLoad` метода и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;
    
        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;
    
        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });
    
            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });
    
            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });
    
            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        ISO.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        Bias.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Переопределить `ViewDidAppear` метода и добавьте следующую команду, чтобы начать запись при загрузке представления:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. С камеры в режиме Auto ползунки перемещаются автоматически при камеры корректирует раскрытия:

    [![](intro-to-manual-camera-controls-images/image13.png "Ползунки перемещаются автоматически при камеры корректирует раскрытия")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Коснитесь сегмента Locked и перетащите смещение ползунок для выбора смещение автоматического раскрытия вручную:

    [![](intro-to-manual-camera-controls-images/image14.png "Ручная настройка смещение автоматического раскрытия")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Коснитесь сегмента Custom и ползунков длительность и ISO вручную управлять раскрытия:

    [![](intro-to-manual-camera-controls-images/image15.png "Перетащите ползунки длительность и ISO вручную раскрытия элемента управления")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Завершите работу приложения.


Приведенный выше код показано, как отслеживать параметры экспозиции при камеры в автоматическом режиме и способах управления раскрытие, когда он находится в режимах заблокирована или настраиваемый с помощью ползунков.

## <a name="manual-white-balance"></a>Ручной баланс белого

Баланс белого элементы управления позволяют пользователям откорректировать баланс colosr изображения выглядела более реалистичным. Разные источники света имеют разные цвета температуры, а параметры камеры, используемый для записи образа должен быть настроен на компенсировать эти различия. Снова, позволяя пользователю контроль над баланс белого они могут сделать специалистов корректировки, которые автоматического подпрограммы не способны использовать для достижения таких эффектов художественного.

[![](intro-to-manual-camera-controls-images/image16.png "Образец изображение корректировки баланс белого вручную")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Для экземпляра (лето) имеет blueish приведения к типу, а лампа накаливания электрическом освещения имеет warmer, желтый оранжевый оттенок. (Ввести в заблуждение, цвета «холодные» имеют выше температуры цвет, чем «горячего». Цвет температуры используются собой физическая мера не восприятия одну.)

Человека виду очень удобен для компенсации для различия в температуры цвета, но это не поддерживается веб-камеры. Камера работает увеличивая цвет на противоположном спектра корректировать цвета различия.

Новый iOS 8 раскрытия API позволяет приложению для контроля процесса и обеспечивают точное управление баланс белого параметры камеры.

### <a name="how-white-balance-works"></a>Работает как белый баланс

Прежде чем рассматривать подробности управление баланс белого в приложении IOS 8. Давайте кратко рассмотрим, как белый баланс works:

В исследовании в восприятии цвет [CIE 1931 RGB цвета и XYZ CIE 1931 цвет пространства](http://en.wikipedia.org/wiki/CIE_1931_color_space) первый математически определенные цветовых схем. Они были созданы Международной комиссией на освещения (CIE) 1931.

[![](intro-to-manual-camera-controls-images/image17.png "Цветовое пространство RGB CIE 1931 и CIE 1931 XYZ цветовое пространство")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Выше диаграмма показывает все цвета отображается излучения, из Темно-синий ярко зеленый цвет на красный ярко. Любой точке на диаграмме могут вместе со значением X и Y, как показано на приведенной выше диаграмме.

Как видно на диаграмме существуют значения X и Y, которые могут размещаться в графе, которые будут находиться за пределами диапазона человеческого зрения и в результате эти цвета не могут быть воспроизведены веб-камеры.

Вызывается меньшего размера кривой на диаграмме выше [позволяет визуально Planckian](http://en.wikipedia.org/wiki/Planckian_locus), который выражает температуры (в градусах Кельвина) с большими номерами со стороны синий (еще важнее) и нижней номера на стороне красный (охладителе). Это полезно для освещения типичных ситуациях.

В смешанном освещении корректировки баланс белого потребуется отличаются от позволяет визуально Planckian для внесения необходимых изменений. В таких ситуациях необходимые корректировки быть сдвиг либо зеленый или красный пурпурный части CIE масштабирования.

Устройства iOS компенсировать оттенки увеличивая противоположной прибыль цвет. Для экземпляра Если сцена содержит слишком много синим, Усиление красного будет увеличен для компенсации. Прибыль, эти значения являются калиброванных для конкретных устройств, так, чтобы они зависят от устройства.

### <a name="existing-white-balance-controls"></a>Существующие элементы управления баланс белого

iOS 7 и более поздних версий, предоставляемые следующие существующие элементы управления баланс белого через `WhiteBalanceMode` свойство:

-   `AVCapture WhiteBalance ModeLocked` — Примеры сцены один раз и использование этих значений во всей сцены.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` — Примеры сцены непрерывно, чтобы убедиться, хорошо сбалансированы.


И приложения можно отслеживать `AdjustingWhiteBalance` свойство, чтобы узнать, когда устанавливается раскрытия.

### <a name="new-white-balance-controls-in-ios-8"></a>Новый баланс белый элементов управления в iOS 8

Помимо функций, уже поддерживаемых по iOS 7 и более поздних версий следующие функции теперь доступны для управления баланс белого в iOS 8:

-  Получает полностью ручное управление устройства RGB.
-  Get, Set и наблюдать за ключ-значение приобретает устройства RGB.
-  Поддержка с помощью карты серый баланс белого.
-  Процедуры преобразования в и из устройства независимых цветовых схем.


Для реализации описанных функций `AVCaptureWhiteBalanceGain` структура была добавлена в следующие члены:

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Увеличение максимального баланс белого в данный момент четырем (4) и готовы из `MaxWhiteBalanceGain` свойство. Поэтому допустимого диапазона — один (1) `MaxWhiteBalanceGain` (4) в настоящее время.

`DeviceWhiteBalanceGains` Свойство может использоваться для наблюдения за текущими значениями. Используйте `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` Чтобы откорректировать баланс увеличивается при камеры в режиме заблокированные баланс белого.

#### <a name="conversion-routines"></a>Процедуры преобразования

Процедуры преобразования были добавлены iOS 8 для преобразования в и из, устройство независимых цветовых схем. Для реализации процедуры преобразования, `AVCaptureWhiteBalanceChromaticityValues` структура была добавлена в следующие члены:

-   `X` -— значение от 0 до 1.
-   `Y` -— значение от 0 до 1.


`AVCaptureWhiteBalanceTemperatureAndTintValues` Структуры также были добавлены с помощью следующих элементов:

-   `Temperature` -является типом с плавающей запятой в градусах Кельвина.
-   `Tint` -— Это смещение от зеленый или пурпурный от 0 150 с положительными значениями сторону зеленый направление и отрицательное сторону пурпурный цвет.


Используйте `CaptureDevice.GetTemperatureAndTintValues`и `CaptureDevice.GetDeviceWhiteBalanceGains`методы для преобразования между температуры и оттенок, цветности и RGB получить цветовых схем.

> [!NOTE]
> Процедуры преобразования, более точны, чем ближе значение для преобразования — Planckian позволяет визуально.




#### <a name="gray-card-support"></a>Поддержка серый карт

Apple используется серый World для обращения к Поддержка карт серый, встроенные в iOS 8. Она позволяет пользователю сосредоточиться на физической карте серого цвета, которая охватывает по меньшей мере 50% center кадра, который используется, чтобы откорректировать баланс белый. Карты серый предназначена для достижения белый появившемся нейтральной.

Это может быть реализован в приложении, запрашивая разрешения пользователя для размещения физических серый карт камеры, наблюдение за `GrayWorldDeviceWhiteBalanceGains` свойство и ждать, пока значения сопоставления.

Приложение будет затем заблокировать баланс белого выигрыш для `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` метода, используя значения из `GrayWorldDeviceWhiteBalanceGains` свойство, чтобы применить изменения.

Устройство записи должна быть заблокирована для конфигурации, прежде чем будет выполнено изменение в балансе белый.

### <a name="manual-white-balance-example"></a>Пример ручной баланс белого

Общая Настройка захвата AV кодом на месте `UIViewController` могут быть добавлены в раскадровку приложения и настроены следующим образом:

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController могут быть добавлены к приложениям раскадровки и настроено, как показано здесь")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Представление содержит следующие основные элементы:

-  Объект `UIImageView` , будет отображаться видео веб-канала.
-  Объект `UISegmentedControl` , будет изменен режим фокусировки автоматически заблокирована.
-  Два `UISlider` элементов управления, будет отображать и обновлять оттенком и температуры.
-  Объект `UIButton` используется пример серого карты (серый World) пробел и задание баланс белого, использующих эти значения.


Выполните следующие действия для передачи вверх представление-контроллер для контроля сальдо белый вручную:


1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    #endregion
    ```
  
1. Добавьте следующие вычисляемые свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Добавьте приведенный ниже закрытый метод для установки нового пустого сбалансировать температуры и оттенок:

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }
    
    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```   
  
1. Переопределить `ViewDidLoad` метода и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ``` 
  
1. Переопределить `ViewDidAppear` метода и добавьте следующую команду, чтобы начать запись при загрузке представления:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Сохранить изменения для кода и запуск приложения.
1. С камеры в режиме Auto ползунки перемещаются автоматически при камеры корректирует белом баланс:

    [![](intro-to-manual-camera-controls-images/image19.png "Ползунки автоматически перемещаются как камеры корректирует белом баланс")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Коснитесь сегмента Locked и перетащите Temp и оттенок ползунки для регулировки баланс белого вручную:

    [![](intro-to-manual-camera-controls-images/image20.png "Перетащите Temp и оттенок ползунки для регулировки баланс белого вручную")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. С сегментом заблокирована по-прежнему выбран поместите физической серого карты на передней части камеры и коснитесь кнопки Gray карты для настройки белого баланса во всем мире серого:

    [![](intro-to-manual-camera-controls-images/image21.png "Коснитесь кнопки Gray карты для настройки белого баланса во всем мире серый")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Завершите работу приложения.

Приведенный выше код было показано, как отслеживать параметры баланс белого, когда камеры в автоматическом режиме или с помощью ползунков управления баланс белого, когда он находится в режиме заблокирована.

## <a name="bracketed-capture"></a>В квадратных скобках отслеживания

Заключенных в скобки сбора данных основан на параметрах от элементов управления вручную камеры, представленные выше, что позволяет приложению можно записать на момент времени, в ряд различными способами.

Проще говоря, заключенных в скобки записи будет по-прежнему изображения, полученные с различные параметры из рисунка рисунок в пакетном режиме.

[![](intro-to-manual-camera-controls-images/image22.png "Как работает заключенных в скобки захвата")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Использование скобок записи данных в iOS 8, приложение можно предустановку ряд элементов управления вручную камеры, одной команды и текущей сцене вернуть серию изображений для каждой из предустановок вручную.

### <a name="bracketed-capture-basics"></a>Основы системы отслеживания в квадратных скобках

Опять же заключенных в скобки захвата имеет еще изображения, полученные с помощью различных параметров, рисунок, рисунок в пакетном режиме. Ниже приведены типы из заключенных в скобки записи доступны.

-  **Auto скобка раскрытия** — где все образы имеют различные Сумма смещения.
-  **Вручную скобка раскрытия** — где все образы имеют разнообразных скорость затвора (длительность) и ISO сумма.
-  **Простой скобка прорыв** — набора по-прежнему изображений, в течение короткого промежутка времени.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Новые заключенных в скобки записывать элементы управления в iOS 8

Все команды, заключенных в скобки захвата, реализованные в `AVCaptureStillImageOutput` класса. Используйте `CaptureStillImageBracket`метод для получения набора изображений с заданным массивом параметров.

Для обработки параметров были реализованы две новые классы.

-   `AVCaptureAutoExposureBracketedStillImageSettings` - У него есть одно свойство, `ExposureTargetBias`которое используется для установки смещения для автоматической экспозиции. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` - Он имеет два свойства `ExposureDuration` и `ISO` используется для установки скорости затвора и ISO для ручной экспозиционной скобки. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>В квадратных скобках отслеживания управляет что следует и навигацией

#### <a name="dos"></a>Что делать

Ниже приведен список требований, которые должны выполняться при с помощью скобок сбора данных элементов управления в iOS 8.

-  Подготовка приложения для ситуации худший записи путем вызова `PrepareToCaptureStillImageBracket` метод.
-  Предположим, буферы образец собираетесь поступать из того же общего пула.
-  Чтобы освободить память, выделенную предыдущим вызовом prepare, вызовите `PrepareToCaptureStillImageBracket` еще раз и отправлять их в массив один объект.


#### <a name="donts"></a>Навигацией

Ниже приведен список требований, которые не следует делать при с помощью скобок сбора данных элементов управления в iOS 8.

-  Не смешивайте заключенных в скобки записать параметры типов в один символ.
-  Не запрашивать более `MaxBracketedCaptureStillImageCount` изображений в один символ.


### <a name="bracketed-capture-details"></a>Сведения о записи в квадратных скобках

Следующие сведения необходимо принять во внимание при работе с заключенных в скобки захвата в iOS 8.

-  Временное изменение параметров в квадратных скобках `AVCaptureDevice` параметры.
-  Флэш-памяти и по-прежнему изображения стабилизации параметры игнорируются.
-  Все образы необходимо использовать один и тот же формат вывода (jpeg, png, и т. д.)
-  Просмотр видео могут удалять кадры.
-  В квадратных скобках захват поддерживается на всех устройствах, совместимые с iOS 8.


С этой информацией в виду давайте рассмотрим пример использования заключенных в скобки захвата в iOS 8.

### <a name="bracket-capture-example"></a>Пример записи скобка

Общая Настройка захвата AV кодом на месте `UIViewController` могут быть добавлены в раскадровку приложения и настроены следующим образом:

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController могут быть добавлены к приложениям раскадровки и настроено, как показано здесь")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Представление содержит следующие основные элементы:

-  Объект `UIImageView` , будет отображаться видео веб-канала.
-  Три `UIImageViews` , отображаются результаты сбора данных.
-  Объект `UIScrollView` для размещения канал видео и результат представления.
-  Объект `UIButton` позволяет перевести заключенных в скобки захвата с заранее заданными параметрами.


Выполните следующие действия для передачи вверх контроллера представления для отслеживания заключенных в скобки:


1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```  
  
1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```    
  
1. Добавьте следующие вычисляемые свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. Добавьте приведенный ниже закрытый метод для создания представления выходного изображения:

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {
    
        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));
    
        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");
    
        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);
    
        // Add to scrolling view
        ScrollView.AddSubview (imageView);
    
        // Return new image view
        return imageView;
    }
    #endregion
    ```  
  
1. Переопределить `ViewDidLoad` метода и добавьте следующий код:
    
    ```
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);
    
        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));
    
        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };
    
        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;
    
            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });
    
            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);
    
                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);
    
                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```
    
  
1. Переопределить `ViewDidAppear` метода и добавьте следующий код:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
        }
    }
    
    ```  
    
1. Сохранить изменения для кода и запуск приложения.
1. Кадр сцены и коснитесь кнопки захвата скобки:

    [![](intro-to-manual-camera-controls-images/image24.png "Кадр сцены и коснитесь кнопки захвата скобка")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Проведите справа налево, чтобы увидеть три изображения, полученные при отслеживании заключенных в скобки:

    [![](intro-to-manual-camera-controls-images/image25.png "Проведите справа налево, чтобы увидеть три изображения, полученные при отслеживании заключенных в скобки")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Завершите работу приложения.


Приведенный выше код показано, как настроить и автоматического раскрытия заключенных в скобки захвата в iOS 8.

## <a name="summary"></a>Сводка

В этой статье мы рассматриваются общие сведения о новых элементов управления вручную камеры, предоставляемые iOS 8 и рассматриваются основные принципы их назначение и принципы их работы. Мы предоставили примеры фокус вручную, раскрытия вручную и Баланс белого вручную. Наконец предоставленным пример перевода, заключенных в скобки записи с использованием ранее элементов управления камеры вручную

## <a name="related-links"></a>Связанные ссылки

- [ManualCameraControls (образец)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
