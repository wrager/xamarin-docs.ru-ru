---
title: "Сводка Глава 5. Работа с размерами"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0c61727e90a03d618a7423e5b865a7fcc9e0b399
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Сводка Глава 5. Работа с размерами

Пока обнаружены несколько размеров в Xamarin.Forms:

- Высота строки состояния операций ввода-вывода — 20
- `BoxView` Имеет стандартную ширину и высоту 40
- Значение по умолчанию `Padding` в `Frame` — 20
- Значение по умолчанию `Spacing` на `StackLayout` — 6
- `Device.GetNamedSize` Метод возвращает числовой размер

Эти размеры не пикселей. Вместо этого они являются аппаратно независимых единицах, которые распознаются каждой платформы независимо друг от друга.

## <a name="pixels-points-dps-dips-and-dius"></a>Пикселей, точки, диагностики, частные интерфейсы и DIUs

В начале историю Apple Mac и Microsoft Windows программисты работал в единицах пикселей. Тем не менее с появлением отображает более высоким разрешением требуется более виртуализированной и абстрактный способ экранные координаты. В мире Mac, программисты работал в единицах *точки*, традиционно 1/72 дюйма при разработчиков Windows используется *аппаратно независимых единицах* (DIUs) на основании 1/96 дюйма.

Мобильных устройств, однако обычно содержат гораздо ближе к лицевой стороны и иметь более высоким разрешением desktop экраны, которые подразумевает можно, которое должно пройти больше плотности пикселей.

Программисты, предназначенных для устройства Apple iPhone и iPad продолжить работу в единицах *точки*, но есть 160 точек на дюйм. В зависимости от устройства может быть 1, 2 или 3 пикселей в точку.

Аналогично Android. Программисты работают в единицах *плотность независимых пикселях* (диагностики), и связь между диагностики и пикселей на основании 160 диагностики на дюйм.

Коэффициенты масштабирования, которые подразумевают, что-то близко к 160 аппаратно независимых единицах на дюйм также создала среды выполнения Windows.

Таким образом Xamarin.Forms программиста, нацеленного на телефоны и планшетные ПК можно предположить, что все единицы измерения основаны на следующих критериев:

- 160 единиц дюйм эквивалентно
- 64 единиц сантиметр

Только для чтения [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) и [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) свойства, определенные `VisualElement` «макета» значения по умолчанию &ndash;1. Только в том случае, если элемент размера и размещено в макет будет эти свойства отражают фактический размер элемента в аппаратно независимых единицах. Этот размер включает какой-либо `Padding` задать в элементе, но не `Margin`.

Визуальный элемент срабатывает [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) событий при его `Width` или `Height` был изменен. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) образец использует это событие для отображения размера экрана программы.

## <a name="metrical-sizes"></a>Размеры metrical

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) использует [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) и [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) для отображения `BoxView` один дюйм высоту и один Расширенный сантиметры.

## <a name="estimated-font-sizes"></a>Размеры шрифтов, оценка

[ **Размеры шрифта** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) образце показано, как с помощью правила 160-единицы дюймов указать размеры шрифтов в единицах точек. Visual согласованности между платформами, при использовании этого метода лучше, чем `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Изменение размера текста по доступный размер

Это возможно в соответствии с вычисленным блок текста в прямоугольнике, определенной `FontSize` из `Label` с помощью следующих условий:

- Межстрочный интервал — 120% от размера шрифта (130% на платформах Windows).
- Средняя ширина символа составляет 50% от размера шрифта.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) этот метод продемонстрирован в примере. Эта программа написан до [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) было предусмотрено, поэтому он использует [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) с [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) параметр, чтобы имитировать поле.

[![Снимок экрана тройной предполагаемый размер](images/ch05fg07-small.png "текста по размеру доступный размер")](images/ch05fg07-large.png#lightbox "текста по размеру доступный размер")

## <a name="a-fit-to-size-clock"></a>Часы размеру размер

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) образце показано использование [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) запустить таймер, который периодически уведомляет приложение, когда все готово для обновления часы. Размер шрифта равным шестая ширина страницы Чтобы сделать изображение как можно большего размера.

## <a name="accessibility-issues"></a>Проблемы доступа

**EstimatedFontSize** программы и **FitToSizeClock** обе программы содержат небольшие недостаток: Если пользователь изменяет параметры специальных возможностей телефона на Android или Windows 10 Mobile, программа больше не можно оценить размер текст отображается на основе размера шрифта. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) пример демонстрирует эту проблему.

## <a name="empirically-fitting-text"></a>Эмпирически размещением текста

Другой способ размещения текста в прямоугольник — эмпирически вычислить размер отображаемого текста и настраивать его вверх или вниз. Программа в вызовах книги [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) на визуальный элемент, чтобы получить нужный размер элемента. Метод является устаревшим, что вместо этого необходимо вызвать программы [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/).

Для `Label`, первый аргумент должен быть Ширина контейнера (Чтобы обтекание), а второй аргумент должен иметь значение для `Double.PositiveInfinity` чтобы высота неограниченное. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) этот метод продемонстрирован в примере.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 5 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Образцы Глава 5.](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Образцы Глава 5 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
