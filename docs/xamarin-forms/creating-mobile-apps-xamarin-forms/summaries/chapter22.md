---
title: "Сводка главе 22. Анимация"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0ee99881a43b625cc8a70fb59e54710705c2d07a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-22-animation"></a>Сводка главе 22. Анимация

Вы уже видели, что анимации с помощью Xamarin.Forms таймера можно создавать или `Task.Delay`, но обычно проще с помощью Xamarin.Forms, предоставляемые возможности анимации. Три класса реализуют эти анимации:

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/), общий подход
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/), более универсально, но более сложные
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/), наиболее подход гибкость и самого низкого уровня

В общем случае анимации целевого свойства, которые архивируются привязываемые свойства. Это не является обязательным, но они только свойства, которые динамически в ответ на изменение.

Для этих анимаций интерфейса XAML, не существует, но можно интегрировать анимации в XAML, с помощью способов, приведенных в [ **Глава 23. Триггеры и поведения**](chapter23.md).

## <a name="exploring-basic-animations"></a>Изучение базовых анимаций

Базовая анимация функции являются методами расширения, в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) класса. Эти методы применяются к любому объекту, который является производным от `VisualElement`. Простой анимации целевого преобразования свойства подробно [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) показано, как `Clicked` обработчик событий для `Button` можно вызвать [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод расширения, чтобы запустить Кнопка в кружке.

`RotateTo` Изменение метода `Rotation` свойство `Button` от 0 до 360, в течение 1 4 секунды (по умолчанию). Если `Button` отводимый еще раз, однако он не выполняет никаких действий из-за `Rotation` свойство уже 360 градусов.

### <a name="setting-the-animation-duration"></a>Настройка длительности анимации

Второй аргумент `RotateTo` — это период времени в миллисекундах. Если задать большое значение, коснувшись `Button` во время анимации начинается новый анимации, начиная с текущего угла.

### <a name="relative-animations"></a>Относительный анимации

`RelRotateTo` Метод выполняет относительный поворот, добавив указанное значение к существующему значению. Этот метод позволяет `Button` можно касаться несколько раз и каждый раз увеличивается `Rotation` свойство 360 градусов.

### <a name="awaiting-animations"></a>Ожидание анимации

Все методы анимации в `ViewExtensions` возвращают `Task<bool>` объектов. Это означает, что можно определить ряд последовательных анимации с помощью `ContinueWith` или `await`. `bool` Завершения возвращаемое значение — `false` анимации, завершивший работу без прерывания или `true` , если она была отменена по [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) метод, который отменяет все анимации, инициированных другой метод в `ViewExtensions` , которые задаются на том же элементе.

### <a name="composite-animations"></a>Составной анимации

Можно комбинировать ожидаемой и не ожидать анимации для создания составного анимации. Это анимации в `ViewExtensions` , предназначенных для `TranslatonX`, `TranslationY`, и `Scale` преобразования свойств:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

Обратите внимание, что `TranslateTo` потенциально влияет на них `TranslationX` и `TranslationY` свойства.

### <a name="taskwhenall-and-taskwhenany"></a>Метода Task.WhenAll и Task.WhenAny

Можно также управлять одновременных анимации с помощью [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), который сигнализирует при нескольких задач все заключения вами, и [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), сигнализирует, когда первого из нескольких Завершает задачи.

### <a name="rotation-and-anchors"></a>Поворот и привязки

При вызове `ScaleTo`, `RelScaleTo`, `RotateTo`, и `RelRotateTo` методы, можно задать [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) и [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) свойств, чтобы указать Центр масштабирования и поворота.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) этот метод продемонстрирован вращением `Button` по центру страницы.

### <a name="easing-functions"></a>Функции плавности

Обычно анимации линейной с начальным значением для конечным значением. Функции плавности может привести ускорить или замедлить работу в течение их. Последний аргумент необязательные методы анимации имеет тип [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), класс, определяющий 11 статического поля только для чтения типа `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/), значение по умолчанию
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/), [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), и [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/), [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), и [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) и [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) и [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

`In` Суффикс указывает, что эффект в начале анимации, `Out` означает в конце, и `InOut` означает, что в начале и конце анимации.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) образце показано использование функции плавности.

### <a name="your-own-easing-functions"></a>Свои собственные функции плавности

Можно также определить вы собственные функции плавности путем передачи [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) для [ `Easing` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/). `Easing` также определяет неявное преобразование из `Func<double, double>` для `Easing`. Аргумент функции плавности всегда находится в диапазоне от 0 до 1, как анимация линейно переход от начала до конца. Функция *обычно* возвращает значение в диапазоне от 0 до 1, но может быть кратко отрицательным или превышать 1 (как в случае с `SpringIn` и `SpringOut` функции) или может нарушить правила, если известно, что делает.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) образец демонстрирует пользовательской функции для реалистичной анимации, и [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) демонстрирует другой.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) образец также демонстрирует функции плавности, а также способ изменения `AnchorX` и `AnchorY` свойства в последовательности анимации поворота.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека имеет [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) класс, который использует пользовательского замедления функция jiggle кнопки при щелчке. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) образец демонстрирует его.

### <a name="entrance-animations"></a>Входы анимации

Популярного вида анимации происходит при первом открытии страницы. Такие анимации, запускается [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) переопределить страницы. Для этих анимаций, мы рекомендуем настроить XAML для способ страницы для отображения *после* анимации, инициализации и анимировать макет из кода.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) примере используются [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод расширения, позволяющий появление содержимое страницы.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) примере используются [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод расширения для слайдов в содержимое страницы из сторон.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) примере используются [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод расширения для анимации `RotationY` свойство. Объект [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод также доступен.

### <a name="forever-animations"></a>Анимации

С другой стороны «forever» анимации запуска до завершения программы. Как правило, они предназначены для демонстрационных целей.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) примере используются [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) анимации для двух фрагментов текста и исчезновения.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) отображает palindrome, а затем последовательно поворачивает отдельных букв на 180 градусов, они будут все плюсом раскрывающегося списка. Затем для считывания совпадает с исходной строкой всю строку элемент отражается 180 градусов.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) образец поворачивает простой `BoxView` вертолетом при его вращение по центру экрана.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) вращается `BoxView` сектора по центру экрана, а затем поворачивает каждого типа «звезда» самого для создания интересных шаблонов:

[![Тройной снимок экрана: вращение сектора](images/ch22fg21-small.png "поворот сектора")](images/ch22fg21-large.png "поворот сектора")

Тем не менее, постепенно увеличить `Rotation` свойства элемента не может работать в долгосрочной перспективе как [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) демонстрирует образец.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) примере используются [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), и [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) стало очевидно, как если бы растровое изображение поворот в трехмерном пространстве.

### <a name="animating-the-bounds-property"></a>Анимация свойства bounds

Единственный метод расширения в `ViewExtensions` еще не показано — [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), который фактически анимируется доступной только для чтения [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) свойство путем вызова [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) метод. Этот метод обычно вызывается методом `Layout` производные продукты, как будет описано в [ **Глава 26. CustomLayouts**](chapter26.md).

`LayoutTo` Метод следует только в особых целей. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) программа использует его, разверните `BoxView` как он будет передаваться off сторонах страницы.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) примере используются `LayoutTo` перемещение плитки в реализацию классической головоломки 15-16, отображающий кодированное изображения, а не нумерованной плитки:

[![Снимок экрана тройной Xamarin Xuzzle](images/ch22fg26-small.png "игры головоломки Xuzzle")](images/ch22fg26-large.png "Xuzzle головоломки игры")

### <a name="your-own-awaitable-animations"></a>Поддерживающий ожидание объект анимации

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) образец создает поддерживающий ожидание объект анимации. Решающее значение класса, который может возвращать `Task` объект из метода и сигнал после завершения анимации [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/).

## <a name="deeper-into-animations"></a>Глубина анимации

Система анимации Xamarin.Forms, может быть несколько запутанным. В дополнение к `Easing` класса, состоит из системы анимации `ViewExtensions`, `Animation`, и `AnimationExtension` classses.

### <a name="viewextensions-class"></a>Класс ViewExtensions

Вы уже видели [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/). Он определяет девять методов, возвращающих `Task<bool>` и [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/). Семь целевой девять методы преобразования свойств. Два других — [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), целевых объектов [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) свойство, и [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), который вызывает [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) метод.

### <a name="animation-class"></a>Класс анимации

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Класс имеет [конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) пять аргументов для определения обратного вызова и методы завершения и параметры анимации.

Дочерних анимациях могут быть добавлены с классом [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), и и перегрузки [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

Объект анимации затем запускается с помощью вызова [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) метод.

### <a name="animationextensions-class"></a>Класс AnimationExtensions

[ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Класс содержит главным образом методы расширения. Существует несколько версий `Animate` метод и универсальный [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) , что это действительно функция только анимации, требуется универсальный метод.

## <a name="working-with-the-animation-class"></a>Работа с класс анимации

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) образец демонстрирует [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) класса несколько разных анимации.

### <a name="child-animations"></a>Дочерних анимациях

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) образец также демонстрирует использование анимации, делающие (весьма похожими) дочерние [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) и [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) методы.

### <a name="beyond-the-high-level-animation-methods"></a>За пределами методов высокоуровневые анимации

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) образце также показано, как для выполнения анимации, которые выходят за пределы целевым свойства проектом `ViewExtensions` методы. В примере один ряд периоды станет больше; в другом примере `BackgroundColor` анимации свойства.

### <a name="more-of-your-own-awaitable-methods"></a>Несколько методов типа awaitable

[ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод `ViewExtensions` не работает с [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) функции. Он останавливается при выводе плавности возвращает выше 1.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека содержит [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) класса [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) и [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) методы расширения, которые не являются эту проблему, а также [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) и [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) методы для отмены те анимации.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) демонстрирует `TranslateXTo` метод.

`MoreExtensions` Класс также содержит [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) метод расширения, который объединяет трансляции X и Y, и [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) метод.

### <a name="implementing-a-bezier-animation"></a>Реализация анимации Безье

Можно также разработать анимации, которая перемещает элемент пути сплайна Безье. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека содержит [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) структуры, который инкапсулирует сплайн Безье и [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) перечисления для ориентации элемента управления.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Класс содержит [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) метода расширения и [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) метод.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) образец демонстрирует элемент в пути Beizer анимации.

## <a name="working-with-animationextensions"></a>Работа с AnimationExtensions

Один тип анимации, отсутствующие на стандартную коллекцию является цветовой анимации. Проблема заключается не, как правильно выполнять интерполяцию между двумя `Color` значения. Можно выполнять интерполяцию отдельных значений RGB, но только как допустимый интерполяция значения HSL.

По этой причине [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека содержит два `Color` методы анимации: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)и [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Кроме того, существует два способа отмены: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) и [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Оба метода делают использование [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), который выполняет анимацию путем вызова универсального широко [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) метод в [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) образце показано использование этих двух типов цветовой анимации.

## <a name="structuring-your-animations"></a>Структурирование анимациями

Иногда бывает полезным express анимации в XAML и использовать их совместно с MVVM. Это рассматривается в следующей главе, [ **Глава 23. Триггеры и поведения**](chapter23.md).



## <a name="related-links"></a>Связанные ссылки

- [Полный текст главе 22 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Образцы главе 22](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Анимация](~/xamarin-forms/user-interface/animation/index.md)
