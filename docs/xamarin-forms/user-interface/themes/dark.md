---
title: "\"Темной\" теме"
ms.topic: article
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0b3a714e8295ed60f201c202fbe30e45dbdcc205
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="dark-theme"></a>"Темной" теме

![](~/media/shared/preview.png "Этот API в настоящее время находится в предварительной версии")

> [!NOTE]
> Темы используют предварительной версии Xamarin.Forms 2.3. Проверьте [советы по устранению неполадок](~/xamarin-forms/user-interface/themes/index.md) при возникновении ошибки.

Чтобы использовать темной темы:

## <a name="1-add-nuget-packages"></a>1. Добавление пакетов Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Добавить словарь ресурсов

В **App.xaml** файл добавления новых настраиваемых `xmlns` для темы, а затем убедитесь ресурсы темы объединяются со словарем ресурсов приложения.
Ниже приводится пример файла XAML.

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Загрузки классов темы

После этого использовать [Устранение неполадок шаг](~/xamarin-forms/user-interface/themes/index.md) и добавьте требуемый код в проектах приложения Android и iOS.

## <a name="4-use-styleclass"></a>4. Использовать StyleClass

Ниже приведен пример кнопок и метки в темной темы, вместе с разметку, которая создает их.

[ ![](dark-images/dark-theme-sml.png "Кнопки и метки в "темной" теме")](dark-images/dark-theme.png "кнопки и метки в "темной" теме")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

[Полный список встроенных классов](~/xamarin-forms/user-interface/themes/index.md) показывает, какие стили доступны для некоторых общих элементов управления.
