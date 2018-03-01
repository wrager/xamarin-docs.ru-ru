---
title: "Пошаговое руководство — с помощью сенсорного ввода в iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 848db0af436ad43e07e68de4d278f641ab83136d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough--using-touch-in-ios"></a>Пошаговое руководство — с помощью сенсорного ввода в iOS

В этом пошаговом руководстве показано, как написать код, который реагирует на различные виды событий сенсорного экрана. В каждом примере содержится в отдельный экран:

- [Touch образцы](#Touch_Samples) — как реагировать на события касания.
- [Образцы распознаватель жестов](#Gesture_Recognizer_Samples) — как использовать встроенные жестов распознаватели.
- [Пользовательский распознаватель жестов образец](#Custom_Gesture_Recognizer) — способ построения пользовательского распознавателя.

Каждый раздел содержит инструкции по созданию кода с нуля.
[Запуск образца кода](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) уже включает в себя полный экран раскадровки и меню:

 [ ![](ios-touch-walkthrough-images/image3.png "Образец включает экрана меню")](ios-touch-walkthrough-images/image3.png)

Следуйте инструкциям ниже, чтобы добавить код в раскадровку и Дополнительные сведения о различных типах событий сенсорного ввода, доступных в iOS. Кроме того, можно открыть [законченном примере](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) для просмотра все заработает.

## <a name="touch-samples"></a>Образцы сенсорного ввода

В этом примере будут показаны некоторые touch API-интерфейсы. Выполните следующие действия, чтобы добавить код, необходимый для реализации событий сенсорного экрана.


1. Откройте проект **Touch_Start**. Первый, запустите проект, убедитесь, что все работает правильно и жестов **Touch образцы** кнопки. (Несмотря на то, что ни одна из кнопок будет работать), вы увидите экран, аналогично приведенным ниже:
    
    [![](ios-touch-walkthrough-images/image4.png "Образец приложения будут работать с кнопками нерабочего")](ios-touch-walkthrough-images/image4.png)


1. Измените файл **TouchViewController.cs** и добавьте следующие переменные два экземпляра класса `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. Реализация `TouchesBegan` метода, как показано в следующем коде:

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    Этот метод работает, проверив `UITouch` объекта, а если он существует выполнения некоторых операций, в зависимости от того, где произошло сенсорный:

    * _Внутри TouchImage_ — отображать текст `Touches Began` в метку и изменение изображения.
    * _Внутри DoubleTouchImage_ — изменить изображение, отображаемое, если жест двойного касания.
    * _Внутри DragImage_ — задать флаг, указывающий, что началось сенсорный. Метод `TouchesMoved` будет использовать этот флаг для определения, если `DragImage` должны перемещаться по экрану или нет, как будет показано на следующем шаге.


    Этот код обрабатывает только отдельные штрихи, по-прежнему не поведение, если пользователь перемещает пальцев на экране. Для ответа на перемещение, реализовать `TouchesMoved` как показано в следующем коде:

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Этот метод возвращает `UITouch` объекта, а затем проверяет, просмотра места возникновения сенсорный ввод. Если сенсорный возникла в `TouchImage`, затем переместить штрихи отображается на экране текст. 

    Если `touchStartedInside` имеет значение true, а затем мы знаем, что у пользователя есть пальцев `DragImage` и переместив ее. Код переместит `DragImage` , когда пользователь перемещает их пальцем по экрану.

1. Нам нужно обрабатывают в случае, когда пользователь отрывает свой палец с экрана или iOS отменяет событие сенсорный ввод. Для этого мы реализуем `TouchesEnded` и `TouchesCancelled` как показано ниже:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```
    
    Оба эти метода приведет к сбросу `touchStartedInside` флаг в значение false. `TouchesEnded` также будет отображения `TouchesEnded` на экране.

1. На этом этапе завершения экрана Touch образцов. Обратите внимание на то, как экрана изменяется взаимодействовать с каждым из изображения, как показано на следующем снимке экрана:
        
    [![](ios-touch-walkthrough-images/image4.png "Начальный экран приложения")](ios-touch-walkthrough-images/image4.png)
    
    [![](ios-touch-walkthrough-images/image5.png "После перетаскивания пользователем кнопки экрана")](ios-touch-walkthrough-images/image5.png)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>Образцы распознаватель жестов

[Предыдущего раздела](#Touch_Samples) было показано, как перетаскивание объекта по экрану с помощью событий сенсорного экрана.
В этом разделе будет избавиться от событий сенсорного экрана и показано, как использовать следующие распознавателей жестов:

-  `UIPanGestureRecognizer` Для перетаскивания изображения на экране.
-  `UITapGestureRecognizer` Реагировать на двойные нажатия на экране.

При запуске [запуск образца кода](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) и выберите команду **образцы распознаватель жестов** кнопку, появится следующий экран:

 [ ![](ios-touch-walkthrough-images/image6.png "Этот экран показывает, нажав кнопку образцы распознаватель жестов")](ios-touch-walkthrough-images/image6.png)

Выполните следующие действия для реализации распознавателей жестов.


1. Измените файл **GestureViewController.cs** и добавьте следующую переменную экземпляр:

    ```chsarp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Нам нужна эта переменная экземпляра для отслеживания предыдущего расположения изображения.
Распознаватель жестов панорамирование будет использовать `originalImageFrame` значение для расчета смещения, необходимые для повторной отрисовки изображения на экране.

1. Добавьте следующий метод в контроллер:

    ```chsarp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Этот код создает экземпляр `UIPanGestureRecognizer` экземпляром и добавляет его к представлению.
Обратите внимание, что целевой объект присвоенное жестов в виде метода `HandleDrag` — этот метод предоставляется в следующем шаге.

1. Чтобы реализовать HandleDrag, добавьте следующий код на контроллер:

    ```chsarp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Приведенный выше код будет сначала проверьте состояние распознаватель жестов и затем перенести образ по экрану. Этот код в месте контроллер теперь поддерживает перетаскивание одного изображения на экране.


1. Добавить `UITapGestureRecognizer` это приведет к изменению на изображение в DoubleTouchImage. Добавьте следующий метод `GestureViewController` контроллера:

    ```chsarp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Этот код очень похож на код для `UIPanGestureRecognizer` , но вместо использования делегата для целевого объекта, мы используем `Action`. 

1. Заключительную, нам нужно будет изменить `ViewDidLoad` , чтобы он вызывает методы, которые мы только что добавили. Изменить ViewDidLoad, таким образом, чтобы она похожа на следующий код:

    ```chsarp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Также Заметьте, что мы инициализации значения `originalImageFrame`.


1. Запустите приложение и работать с двумя образами.
Снимке экрана ниже приведен один пример таких взаимодействий.
    
    [![](ios-touch-walkthrough-images/image7.png "На этом снимке экрана показано взаимодействие перетаскивания")](ios-touch-walkthrough-images/image7.png)



## <a name="custom-gesture-recognizer"></a>Пользовательский распознаватель

В этом разделе будет применена основные понятия в предыдущих разделах, для построения пользовательского распознавателя. Пользовательский распознаватель будет подклассов `UIGestureRecognizer`и распознает когда пользователь рисует «V» на экране затем переключать растрового изображения. Снимке экрана ниже приведен пример этого экрана.

 [ ![](ios-touch-walkthrough-images/image8.png "Приложение определяет, когда пользователь рисует «V» на экране")](ios-touch-walkthrough-images/image8.png)

Выполните следующие действия для создания пользовательского распознавателя.


1. Добавьте новый класс в проект с именем `CheckmarkGestureRecognizer`и оформить ее как следующий код:

    ```chsarp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Метод Reset вызывается, когда `State` либо изменения свойств `Recognized` или `Ended`. Это время для сброса любое внутреннее состояние, в пользовательской распознавания.
Теперь класса можно начать с чистого листа следующий раз, когда пользователь взаимодействует с приложением, можно и Повторение попыток распознавания жестов.



1. Теперь, когда мы определили пользовательского распознавателя (`CheckmarkGestureRecognizer`) изменить **CustomGestureViewController.cs** и добавьте следующие переменные экземпляра два файла:

    ```chsarp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Чтобы создать и настроить нашей распознаватель жестов, добавьте следующий метод в контроллер:

    ```chsarp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Изменить `ViewDidLoad` , чтобы он вызывал `WireUpCheckmarkGestureRecognizer`, как показано в следующем фрагменте кода:

    ```chsarp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Запустите приложение и повторите изображения на экране «V». Вы увидите изображение отображаемых изменений, как показано на следующем снимке экрана:
    
    [![](ios-touch-walkthrough-images/image9.png "Проверка кнопки")](ios-touch-walkthrough-images/image9.png)
    
    [![](ios-touch-walkthrough-images/image10.png "Unchecked кнопки")](ios-touch-walkthrough-images/image10.png)



Указанных выше трех разделах демонстрации способов реагировать на сенсорный ввод события в iOS: с помощью событий сенсорного ввода, распознавателей жестов встроенного или пользовательского распознавателя.



## <a name="related-links"></a>Связанные ссылки

- [iOS Touch запуска (пример)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch окончательного (пример)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
