---
title: "Сводка Глава 3. Глубина текста"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7dbcc093bc467e633f9333bb129adc25372832f3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Сводка Глава 3. Глубина текста

Более подробно рассматривается в этой главе [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) представление более подробно, включая цвета, шрифты и форматирование.

## <a name="wrapping-paragraphs"></a>Перенос абзацев

Когда [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойство `Label` содержит длинный текст `Label` автоматически переносятся в несколько строк, как показано в предыдущем [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) образца. Можно внедрять кодов Юникода, например «\u2014» тире или символы C#, такие как «\r» переключиться на новую строку.

При [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойства `Label` присваиваются `LayoutOptions.Fill`, общий размер `Label` регулируется пространство, контейнера делает доступными. `Label` Считается *ограниченного*. Размер `Label` размер контейнера.

Когда `HorizontalOptions` и `VerticalOptions` свойствам присваиваются значения, отличного от `LayoutOptions.Fill`, размер `Label` регулируется места, необходимого для отображения текста до размера, который делает доступными для контейнера `Label`. `Label` Считается *произвольные* и определяет собственный размер.

(Примечание: условия *ограниченного* и *произвольные* может быть counter-intuitive, так как представление произвольные обычно меньше, чем ограниченного представления. Кроме того эти условия не используются постоянно в первые главы книги.)

Представления, такие как `Label` в одном измерении ограниченное и неограниченное в другой. Объект `Label` только будет переносить текст на нескольких строках, если ограничен по горизонтали.

Если `Label` имеет ограниченное, его может занимать значительно больше места, чем требуется для текста. Текст может располагаться в пределах общей области `Label`. Задать [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) свойства члена [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) перечисления ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), или [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) для управления выравниванием все строки абзаца. Значение по умолчанию — `Start` и выравниваются по левому краю текста.

Задать [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) свойства члена `TextAlignment` перечисления для позиционирования текста на начало, по центру или по нижней части область, занимаемая `Label`.

Задать [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) свойства члена [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) перечисления ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), или [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)) для элемент управления, как несколько строк в между абзацами или усекаются.

## <a name="text-and-background-colors"></a>Цвет текста и фона

Задать [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) и [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) свойства `Label` для [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) значения для управления цвет текста и фона.

`BackgroundColor` Применяется к фону всю область, занимаемая `Label`. В зависимости от `HorizontalOptions` и `VerticalOptions` свойства, что размер может быть значительно больше места, чем область, необходимые для отображения текста. Чтобы поэкспериментировать с различными значениями можно использовать цвет `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, и `VerticalTextAlignment` чтобы увидеть, как они влияют на размер и положение `Label`и размер и положение текста в пределах `Label`.

## <a name="the-color-structure"></a>Структуры Color

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Структуры можно задать цвета в виде красного, зеленого, синего (RGB), либо значений цветового тона, насыщенности, яркость (HSL) или с именем цвета. Альфа-канал также доступна для указания прозрачности.

Используйте `Color` конструктор для задания:

- [оттенок серого цвета](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- [RGB-значение](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- [RGB-значение с прозрачностью](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Аргументы `double` значения в диапазоне от 0 до 1.

Можно также использовать несколько статических методов для создания `Color` значения:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) для `double` RGB-значения от 0 до 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) для значений RGB целое число от 0 до 255.
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) для `double` значений RGB с прозрачностью
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) для целых значений RGB с прозрачностью
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) для `double` значения HSL с прозрачностью
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) для `uint` значение вычисляется как (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) для `string` формат шестнадцатеричных цифр в формате «#AARRGGBB» или «#RRGGBB» или «#ARGB» или «#RGB», где каждая буква соответствует шестнадцатеричная цифра для альфа, красного, зеленого и синего каналов. Этот метод является первичным, используемый для преобразования цветов XAML, как описано в [Глава 7, XAML и код](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

После создания `Color` значение является неизменяемым. Характеристики цвета можно получить из следующих свойств:

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

Эти `double` значения в диапазоне от 0 до 1.

`Color` также определяет 240 открытого статического поля только для чтения для обычных цветов. На момент написания книги только 17 часто применяемые цвета были доступны.

Другое открытое статическое доступное только для чтения поле определяет цвет с помощью всех цветовых каналов, равен нулю:

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

Несколько методов экземпляра Разрешить изменение существующего цвета, чтобы создать новый цвет:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Наконец, два статических свойств только для чтения задать значение особые цвета:

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), все каналы, равным & #x 2013; 1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` предусмотрено для обеспечения платформы цветовой схемы и поэтому имеет разное значение в различных контекстах на разных платформах. По умолчанию платформа цветовые схемы являются:

- iOS: темный текст на фоне света
- Android: Светло-текст на темного фона (в книге) или темный текст на фоне светлой (для разработки материалы по совместимости приложений в **master** ветвь репозитория образца кода)
- UWP: Темно-текст на фоне света
- Windows 8.1: Светло-текста на темного фона
- Windows Phone 8.1: Светло-текста на темного фона

`Color.Accent` Значение результатов от платформы (и иногда выбираемых пользователем) цветом, видимый на Темная или Светлая фона.

## <a name="changing-the-application-color-scheme"></a>Изменение цветовой схемы приложения

Различные платформы имеют цветовую схему по умолчанию, как показано в приведенном выше списке.

При разработке для Android, можно переключиться на схему темной на свет, указав "светлой" темой в файле Android.Manifest.xml или [Добавление AppCompat и проектирования материал](~/xamarin-forms/platform/android/appcompat.md).

Для платформ Windows, цвет темы обычно выбранный пользователем, но можно добавить `RequestedTheme` атрибута значение `Light` или `Dark` в файле App.xaml платформы. По умолчанию содержит файл App.xaml в проекте UWP `RequestedTheme` атрибуту присвоено значение `Light`.

## <a name="font-sizes-and-attributes"></a>Размеры шрифтов и атрибуты

Задать [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) свойство `Label` в строку, например «Times Roman» выберите семейство шрифтов. Тем не менее необходимо указать семейство шрифтов, который поддерживается на определенной платформе и в этом отношении платформы не согласованы.

Задать [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) свойство `Label` для `double` для указания приблизительную высоту шрифта. В разделе [Глава 5, работе с размерами](chapter05.md), Дополнительные сведения о выборе размеров шрифта интеллектуально.

В качестве альтернативы можно получить одним из нескольких размеров стиль шрифта зависят от платформы. Статический [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) метод и [перегрузки](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) обе функции возвращают `double` значение размера шрифта, соответствующую платформы на основе членов [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)перечисления ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  и [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). Значение, возвращаемое из `Medium` элемент не обязательно совпадает с `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) пример выводит текст с этими с именем Size.

Задать [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) свойство `Label` на член [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) перечисления, [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), или [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/). Можно объединять `Bold` и `Italic` членов с помощью C# побитового оператора OR.

## <a name="formatted-text"></a>Форматированный текст

Во всех примерах в данный момент отображается весь текст `Label` переформатирования единообразно. Чтобы изменить форматирование в текстовую строку, не задано `Text` свойство `Label`. Вместо этого задайте [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) свойство для объекта типа [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` имеет [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) свойство, которое представляет собой коллекцию [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) объектов. Каждый `Span` объект имеет свой собственный [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), и [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) свойства.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) образце показано использование `FormattedText` свойство для отдельной строки текста, и [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) демонстрируется методика абзаца, как показано ниже:

[![Снимок экрана тройной переменной форматирования абзаца](images/ch03fg06-small.png "переменной форматированный текст метки")](images/ch03fg06-large.png#lightbox "переменной форматированный текст метки")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) программа использует один `Label` и `FormattedString` объекта, чтобы отобразить все размеры именованных шрифтов для каждой платформы.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 3 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Образцы раздел 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Образцы Глава 3 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [Работа с цветами](~/xamarin-forms/user-interface/colors.md)
