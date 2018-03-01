---
title: "Элементы управления XAML Standard (Предварительная версия)"
description: "Как начать изучение Предварительная версия Standard XAML в Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>Элементы управления XAML Standard (Предварительная версия)

![Предварительный просмотр](~/media/shared/preview.png)

На этой странице перечислены XAML стандартные элементы управления в области предварительного просмотра вместе с их эквивалент Xamarin.Forms.

Также приведен список элементов управления, которые имеют имена, новые свойства и перечисления в стандартных XAML.

## <a name="controls"></a>Элементы управления

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>Стандарт XAML</th></tr>
  <tr><td>Frame</td><td>Border</td></tr>
  <tr><td>Средство выбора</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Метка</td><td>TextBlock</td></tr>
  <tr><td>Ввод</td><td>TextBox</td></tr>
  <tr><td>Параметр</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>Свойства и перечисления

<table>
  <tr><th>Xamarin.Forms<br/>Элементы управления с обновленные свойства</th><th>Xamarin.Forms<br/>Свойство или Enum</th><th>Стандарт XAML<br/>Эквивалент</th></tr>
  <tr><td>Кнопка, запись, метки, DatePicker, редактор, SearchBar, TimePicker</td><td>TextColor</td><td>Foreground</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Фон *</i></td></tr>
  <tr><td>Кнопка выбора</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Кнопка</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>Ход выполнения</td><td>Значение</td></tr>
  <tr><td>Кнопка, запись, метки, редактор, SearchBar, диапазон, шрифт</td><td>FontAttributes<br/>Полужирный, курсив, нет</td><td>FontStyle<br/>Курсив, обычный</td></tr>
  <tr><td>Кнопка, запись, метки, редактор, SearchBar, диапазон, шрифт</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>Полужирным шрифтом, обычная</td></tr>
  <tr><td>InputView</td><td>Клавиатура<br/>По умолчанию URL-адрес, номер, телефон, текст, чат, электронной почты</td><td><i>InputScopeNameValue *</i><br/>По умолчанию URL-адрес, номер, TelephoneNumber, текст, чат, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Ориентация *</i></td></tr>
</table>

> [!IMPORTANT]
> Элементы, отмеченные * являются неполными в текущей предварительной версии


## <a name="related-links"></a>Связанные ссылки

- [Предварительная версия NuGet](https://aka.ms/xf-xamlstandard-nuget)
