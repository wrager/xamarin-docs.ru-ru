---
title: "Сводка главе 21. Transform"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 378ce3fb39cfb5c42d5ec7611415f5420146a9cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-21-transforms"></a>Сводка главе 21. Transform

Xamarin.Forms отображается на экране, расположение и размер определяется своим родительским элементом, который обычно является `Layout` или `Layout<View>` производный класс. *Преобразования* — это функция Xamarin.Forms, можно изменить, расположение, размер или даже ориентации.

Xamarin.Forms поддерживает три основных типа преобразований:

- *Перевод* & #x 2014; сдвинуть элемент по горизонтали или по вертикали
- *Масштаб* & #x 2014; изменение размера элемента
- *Поворот* & #x 2014; включить элемент вокруг точки или оси

В Xamarin.Forms масштабирование мощности; она влияет на ширину и высоту равномерно. Поворот поддерживается как на двумерной поверхности экрана и в трехмерном пространстве. Имеется преобразование не наклона (или большое) и без обобщенный матрица преобразования.

Поддерживаемые преобразования с восемью свойства типа `double` определяется `VisualElement` класса:

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

Все эти свойства, поддерживаемых привязываемые свойства. Они могут являться целевыми для привязки данных и стилем. [**Глава 22. Анимация** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) показано, как эти свойства могут быть анимированы, но некоторые примеры в этой главе Показать, как их с помощью Xamarin.Forms анимировать [таймера](~/xamarin-forms/platform/device.md#Device_StartTimer).

Свойства определяют, как только элемент подготавливается к просмотру и выполните преобразование *не* влияют на то, как ей элемента в макете.

## <a name="the-translation-transform"></a>Преобразованию переноса

Ненулевое значение, равное [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) и [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) свойства сдвинуть элемент по горизонтали или вертикали.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) программы позволяет экспериментировать с этими свойствами с двумя `Slider` элементы, которые управляют `TranslationX` и `TranslationY` свойства `Frame`. Преобразование также влияет на все дочерние элементы, `Frame`.

### <a name="text-effects"></a>Эффекты для текста

Один общих свойствах преобразования используется немного смещения отрисовки текста. Это показано в [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) образца:

[![Снимок экрана: три смещает текст](images/ch21fg03-small.png "смещает текст")](images/ch21fg03-large.png "смещает текст")

Другим эффектом является для подготовки к просмотру нескольких копий `Label` выглядеть 3D блока, таких, как показано в [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) образца.

### <a name="jumps-and-animations"></a>Переходы и анимации

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) образец использует преобразования для перемещения `Button` всякий раз, когда оно выполняется отвод, но основной призван продемонстрировать, что `Button` получает входные данные пользователя там, где кнопки подготавливается к просмотру.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) пример аналогичен, но использует таймер для анимации `Button` из одной точки в другую.

## <a name="the-scale-transform"></a>Преобразование изменения масштаба

[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Преобразования можно увеличить или уменьшить отображаемый размер элемента. Значение по умолчанию — 1. Значение 0 приводит элемент быть скрытым. Отрицательные значения вызывают элемент будет выглядеть 180 градусов. `Scale` Свойства не влияет на `Width` или `Height` свойства элемента. Эти значения не изменяются.

Можно поэкспериментировать с `Scale` свойства с помощью [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) образца.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) образец иллюстрирует различие между анимации `Scale` свойство `Button` и анимации `FontSize` свойство. `FontSize` Влияет на свойство как `Button` воспринимается в макете; `Scale` не поддерживает свойство.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) пример вычисляет `Scale` свойство, которое применяется к `Label` чтобы сделать его как можно большего размера при по-прежнему соответствует текущей страницы.

### <a name="anchoring-the-scale"></a>Закрепление шкалы

Элементы в предыдущих трех примерах масштабировании все увеличилось или уменьшилось размер относительно центра элемента. Другими словами элемент увеличивается или уменьшается в размерах одинаковым во всех направлениях. Только точка в центре элемента остается в том же расположении во время масштабирования.

Вы можете изменить центра масштабирования, задав [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) и [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) свойства. Эти свойства определяются относительно самого элемента. Для `AnchorX`, значение 0 означает левого края элемента, а справа от 1. Аналогичным образом, для `AnchorY`, 0 находится в верхней и нижней является 1. Оба свойства имеют значения по умолчанию 0,5, которого является центр.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) пример позволяет экспериментировать с `AnchorX` и `AnchorY` свойства а также `Scale` свойство.

На iOS, используя значения по умолчанию из `AnchorX` и `AnchorY` свойства обычно несовместим с телефона изменение ориентации.

## <a name="the-rotation-transform"></a>Преобразование поворота

[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Свойства указываются в градусах и указывает поворот по часовой стрелке вокруг точки элементом, определенным в `AnchorX` и `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) позволяет экспериментировать с этими тремя свойствами.

### <a name="rotated-text-effects"></a>Эффекты повернутый текст

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) образец демонстрирует математических операций, необходимые для Рисование окружности с помощью 64 очень мала, повернуто `BoxView` элементов.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) образца отображается несколько `Label` поворачивать элементы с ту же текстовую строку как сектора.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) пример отображает текстовую строку, отображается программы-оболочки в кружке.

### <a name="an-analog-clock"></a>Аналогично часам со стрелками

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека содержит [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) класс, который вычисляет углы для стрелки часов. Чтобы избежать зависимостей платформы в ViewModel, использует класс `Task.Delay` вместо таймера для поиска нового `DateTime` значение.

Также включены в **Xamarin.FormsBook.Toolkit** — [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) класс, реализующий `IValueConverter` и служит для округления второй угол до ближайшей секунды.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) использует три Поворот `BoxView` элементы для рисования аналогично часам со стрелками.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) использует `BoxView` для более сложных графики, включая деления помечает вокруг циферблата часов и передает то поворот мало расстояние от своей стороны:

[![Снимок экрана тройной BoxView часов](images/ch21fg17-small.png "аналогом циферблат")](images/ch21fg17-large.png "аналогом циферблат")

В дополнение к этому [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) класса в **Xamarin.FormsBook.Toolkit** вызывает секундной стрелки будет выглядеть немного смещают до перехода вперед, а затем переместите обратно в правильное положение.

### <a name="vertical-sliders"></a>Вертикальные ползунки?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) образец демонстрирует, что `Slider` элементы могут быть повернут на 90 градусов и по-прежнему работают. Однако это трудности при размещении этих поворачивать `Slider` элементов так, как в макете они по-прежнему могут быть горизонтальной.

## <a name="3d-ish-rotations"></a>3D-ИШ вращения

[ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) Свойство, по-видимому, поворот элемента вокруг 3D по оси x, чтобы в верхней и нижней части элемента показаться для перемещения по направлению к или от него. Аналогичным образом [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) кажется поворот элемента вокруг оси y, чтобы сделать левую и правую части элемента показаться для перемещения по направлению к или от него.

`AnchorX` Влияет на свойство `RotationY` , но не `RotationX`. `AnchorY` Влияет на свойство `RotationX` , но не `RotationY`. Можно поэкспериментировать с [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) образец для изучения взаимодействия из этих свойств.

Трехмерная система координат, содержится в Xamarin.Forms для левши. При наведении указателя мыши указательным левой руки по оси X возрастающей координаты (справа) и палец среднего по оси Y возрастающей координаты (вниз), затем точек thumb в направлении возрастания координаты Z (за пределы экрана).

Кроме того для любого из трех оси, при наведении указателя мыши на левом thumb в направлениями увеличения значения, то кривой пальцы указывает направление поворота положительные значения угла поворота.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст главе 21 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Образцы главе 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
