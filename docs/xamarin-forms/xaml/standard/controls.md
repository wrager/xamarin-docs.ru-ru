---
title: Элементы управления XAML Standard (Предварительная версия)
description: В этой статье исследуются XAML стандартных элементов управления, доступных в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245576"
---
# <a name="xaml-standard-preview-controls"></a>Элементы управления XAML Standard (Предварительная версия)

![Предварительный просмотр](~/media/shared/preview.png)

На этой странице перечислены XAML стандартные элементы управления в области предварительного просмотра вместе с их эквивалент Xamarin.Forms.

Также приведен список элементов управления, которые имеют имена, новые свойства и перечисления в стандартных XAML.

## <a name="controls"></a>Элементы управления

|Xamarin.Forms|Стандарт XAML|
|--- |--- |
|Frame|Border|
|Средство выбора|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Метка|TextBlock|
|Ввод|TextBox|
|Параметр|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Свойства и перечисления

|Элементы управления Xamarin.Forms с обновленные свойства|Свойство Xamarin.Forms или enum|Стандартная XAML эквивалент|
|--- |--- |--- |
|Кнопка, запись, метки, DatePicker, редактор, SearchBar, TimePicker|textColor|Foreground|
|VisualElement|BackgroundColor|Фон *|
|Кнопка выбора|Цвет границы, OutlineColor|BorderBrush|
|Кнопка|BorderWidth|BorderThickness|
|ProgressBar|Ход выполнения|Значение|
|Кнопка, запись, метки, редактор, SearchBar, диапазон, шрифт|FontAttributesBold, курсив, нет|FontStyleItalic-норм.|
|Кнопка, запись, метки, редактор, SearchBar, диапазон, шрифт|FontAttributes|FontWeights * полужирный, обычный|
|InputView|KeyboardDefault, URL-адрес, номер, телефон, текст, чат, отправка по электронной почте|InputScopeNameValue * по умолчанию, URL-адрес, номер, TelephoneNumber, текст, чат, EmailNameOrAddress|
|StackPanel|StackOrientation|Ориентация *|

> [!IMPORTANT]
> Элементы, отмеченные * являются неполными в текущей предварительной версии

## <a name="related-links"></a>Связанные ссылки

- [Предварительная версия NuGet](https://aka.ms/xf-xamlstandard-nuget)
