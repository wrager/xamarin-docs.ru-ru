---
title: "Пользовательской анимации"
description: "Класс анимации ― это средство, все анимации Xamarin.Forms, с помощью методов расширения в классе ViewExtensions, создание одного или нескольких объектов анимации. В этой статье показано, как класс анимации для создания и отменить анимации, синхронизация нескольких анимаций и создание анимации, анимация свойства, которые не являются анимированы с существующие методы анимации."
ms.topic: article
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 42ef3e6c82763831b5114f3de7603bba8f59eac6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="custom-animations"></a>Пользовательской анимации

_Класс анимации ― это средство, все анимации Xamarin.Forms, с помощью методов расширения в классе ViewExtensions, создание одного или нескольких объектов анимации. В этой статье показано, как класс анимации для создания и отменить анимации, синхронизация нескольких анимаций и создание анимации, анимация свойства, которые не являются анимированы с существующие методы анимации._


Число параметров должно быть указано при создании `Animation` объекта, включая значения начала и окончания анимируемого свойства и обратный вызов, который изменяет значение свойства. `Animation` Объекта можно также поддерживать коллекцию дочерних анимациях, можно выполнить и синхронизированы. Дополнительные сведения см. в разделе [дочерних анимациях](#child).

Запуск анимации, созданной с помощью [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) класс, который может включать или не включать дочерних анимациях, производится путем вызова [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) метод. Этот метод указывает продолжительность анимации и среди других элементов, обратный вызов, который определяет, следует ли повторять анимацию.

## <a name="creating-an-animation"></a>Создание анимации

При создании [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) объекта, как правило, как минимум три параметра являются обязательными, как показано в следующем примере кода:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Этот код определяет анимацию [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляр из значения 1 значение 2. Анимированное значение, которое определяется с помощью Xamarin.Forms, передается обратный вызов, указанный в качестве первого аргумента, где он используется для изменения параметра `Scale` свойство.

Анимация начинается с вызова [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) метода, как показано в следующем примере кода:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Обратите внимание, что [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) метод не возвращает `Task` объекта. Вместо этого уведомления предоставляются через методы обратного вызова.

Можно указать следующие аргументы в `Commit` метод:

- Первый аргумент (*владельца*) определяет владельца анимации. Это может быть визуальный элемент, к которому применяется анимация или другой визуальный элемент, например страницы.
- Второй аргумент (*имя*) определяет анимацию с именем. Имя вместе с владельцем для уникальной идентификации анимации. Этот уникальный код может затем использоваться для определения, выполняется ли анимация ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), или отменить его ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- Третий аргумент (*скорость*) указывает время в миллисекундах между вызовами метода обратного вызова, определенный в [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) конструктор
- Четвертый аргумент (*длина*) указывает длительность анимации в миллисекундах.
- Пятый аргумент (*плавности*) определяет функцию реалистичной анимации для использования в анимации. Кроме того, можно указать функцию реалистичной анимации как аргумент [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) конструктор. Дополнительные сведения о функции плавности см. в разделе [функции плавности](~/xamarin-forms/user-interface/animation/easing.md).
- Шестой аргумент (*завершения*) является обратным вызовом, который будет выполняться после завершения анимации. Этот обратный вызов принимает два аргумента, с первым аргументом, указывающее, конечное значение, а второй аргумент, `bool` , имеющим значение `true` если анимация была отменена. Кроме того *завершения* обратного вызова можно задать аргумент для [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) конструктор. Тем не менее, с помощью одной анимации Если *завершения* обратные вызовы указаны в обеих `Animation` конструктор и `Commit` только обратный вызов, указанный в методе `Commit` метод будет выполняться.
- Седьмой аргумент (*повторите*) является обратным вызовом, позволяющий анимации должно повторяться. Он вызывается в конце анимации и возвращая `true` указывает, что анимация должна повторяться.

Общий эффект является создание анимации, которая увеличивает [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) от 1 до 2, более 2 секунд (2000 мс), с помощью [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) функции реалистичной анимации входа. Каждый раз при завершении анимации, его `Scale` свойства начинается с 1 и повторе анимации.

> [!NOTE]
> **Примечание**: одновременных анимации, выполняются независимо друг от друга могут создаваться путем создания `Animation` объекта для каждого анимации и последующего вызова `Commit` метод для каждой анимации.

<a name="child" />

### <a name="child-animations"></a>Дочерних анимациях

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Класс также поддерживает дочерних анимациях, который включает в себя создание `Animation` объектов для других `Animation` добавляются объекты. Это позволяет ряд анимации должен быть запущен и синхронизированы. В следующем примере кода показано создание и запуск дочерних анимациях:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Кроме того в примере кода можно написать более кратко, как демонстрируется в следующем примере кода:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

В обоих примерах кода родительского [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) создается объект, к которому дополнительных `Animation` затем добавляются объекты. Первые два аргумента, чтобы [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) метод укажите время начала и окончания анимации дочерних. Значения аргументов должно быть между 0 и 1 и представлять относительный периода в родительской анимации анимации указанного дочернего элемента будет активным. Таким образом, в этом примере `scaleUpAnimation` будет активна в первой половине анимации, `scaleDownAnimation` будут активными для анимации, вторая половина и `rotateAnimation` будет активна в течение всего периода.

Общий эффект —, что анимация возникает более чем 4 секунды (4000 миллисекунд). `scaleUpAnimation` Анимирует [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойства от 1 до 2, более 2 секунды. `scaleDownAnimation` Затем анимирует `Scale` свойство из 2 1, более 2 секунды. Хотя оба шкалы анимации, `rotateAnimation` анимирует [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойства от 0 до 360, более чем 4 секунды. Обратите внимание, что анимация масштабирования также использовать функции плавности. [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Функции реалистичной анимации входа вызывает [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) изначально сжатие перед получением большего размера и [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) функции реалистичной анимации входа вызывает `Image` станет меньше фактического размера ближе к концу завершения анимации.

Существует ряд различий между [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) объект, который использует дочерних анимациях и которая не:

- При использовании дочерних анимациях *завершения* обратного вызова на анимации дочерних указывает после завершения дочернего и *завершения* обратного вызова, передаваемый `Commit` метод указывает, когда анимацию полностью завершена.
- При использовании дочерних анимациях, возвращая `true` из *повторите* обратный вызов `Commit` повтор анимации не вызовет метод, но анимации продолжит работу без новых значений.
- При включении функции плавности в `Commit` метода и функции для реалистичной анимации возвращает значение больше 1, анимация завершается. Если функция плавности возвращает значение меньше 0, значение присваивается значение 0. Чтобы использовать функцию реалистичной анимации, которая возвращает значение меньше 0 или больше 1, он указан в одном из дочерних анимациях, а не в `Commit` метод.

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Класс также содержит [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) методы, которые можно использовать для добавления к родительскому элементу дочерних анимациях `Animation` объекта. Тем не менее их *начать* и *Готово* значения аргументов не ограничиваются 0, 1, но только эту часть дочерних анимации, которая соответствует диапазону от 0 до 1 будет активным. Например если `WithConcurrent` вызова метода определяет дочерние анимации, которая обращается [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойства от 1 до 6, но с *начать* и *Готово* значения -2 и 3, *начать* значение -2 соответствует `Scale` значение 1 и *Готово* значение 3 соответствует `Scale` значение 6. Поскольку значения вне диапазона от 0 до 1 не принимают участия в анимации, `Scale` свойство будет только анимировать от 3 до 6.

## <a name="canceling-an-animation"></a>Отмена анимации

Приложение может отменить анимации с помощью вызова [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) метод расширения, как показано в следующем примере кода:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Обратите внимание, что анимации однозначно идентифицируются сочетанием анимации владельца и имя анимации. Таким образом имя и владельца указан при выполнения анимации необходимо указать для отмены анимации. Таким образом, в примере кода будет немедленно отменить анимации с именем `SimpleAnimation` , владельцем которой является страница.

## <a name="creating-a-custom-animation"></a>Создание пользовательской анимации

Примеры, показанные здесь показали анимации, столь же можно достичь с помощью методов в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) класса. Однако преимущество [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) класса является наличие доступа к методу обратного вызова, который выполняется при изменении анимированные значения. Это позволяет обратного вызова для реализации любой требуемой анимации. Например, в следующем примере кода анимирует [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) свойства страницы, присвоив ему значение [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) значениями, созданными [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)метод оттенка значениями от 0 до 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Лучших результатов предоставляет внешний вид перемещения через радуги цвета фона страницы.

Дополнительные примеры создания сложных анимации, включая анимации кривой Безье, в разделе [главе 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) из [Создание мобильных приложений с помощью Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Создание метода расширения пользовательской анимации

Методы расширения в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) класс анимировать свойство с его текущим значением к указанному значению. Это усложняет задачу создания, например, `ColorTo` метод анимации, который может использоваться анимация цвета из одного значения на другой, так как:

- Единственным [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) свойством, которое определяется [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) класс [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), не всегда нужного `Color` свойство для анимации.
- Часто текущее значение [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) свойство [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), не реальными цвет, так и которого не может использоваться в вычислениях интерполяции.

Решением этой проблемы является имеет `ColorTo` конкретной целевой метод [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) свойство. Вместо этого оно может быть указано с методом обратного вызова, который передает интерполированные `Color` обратно в вызывающий объект. Кроме того, метод будет принимать начала и окончания `Color` аргументы.

`ColorTo` Метод может быть реализован как метод расширения, который использует [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) метод в [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) класса для выполнения его функций. Это вызвано `Animate` метод можно использовать для свойства целевого объекта, которые не являются типа `double`, как показано в следующем примере кода:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) Метод требует *преобразования* аргументом, который представляет метод обратного вызова. Входные данные для этого обратного вызова — всегда `double` от 0 до 1. Таким образом `ColorTo` метод определяет свои собственные преобразования `Func` , принимающий `double` от 0 до 1, а два возвращает [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) значение, соответствующее этому значению. `Color` Значение вычисляется путем интерполяции [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), и [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) значения двух предоставленный `Color` аргументы. `Color` Значение затем передается в метод обратного вызова для приложения для конкретного свойства.

Такой подход позволяет `ColorTo` метод анимировать любое [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) свойства, как показано в следующем примере кода:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

В этом примере кода `ColorTo` анимирует метод [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) и [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) свойства [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), `BackgroundColor`свойства страницы и [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) свойство [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/).

## <a name="summary"></a>Сводка

В этой статье показано, как использовать [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) класс для создания и Отмена анимации, синхронизация нескольких анимаций и создание анимации, анимация свойства, которые не являются анимированы с существующие анимации методы. `Animation` Класс является блоком построения языка все Xamarin.Forms анимации.


## <a name="related-links"></a>Связанные ссылки

- [Пользовательской анимации (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Анимация](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
