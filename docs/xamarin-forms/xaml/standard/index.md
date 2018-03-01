---
title: "Стандарт XAML (Предварительная версия)"
description: "Как начать изучение Предварительная версия Standard XAML в Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 3214a76ee3f976f5dd2afb28edde07100db5a7a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview"></a>Стандарт XAML (Предварительная версия)

![Предварительный просмотр](~/media/shared/preview.png)

Выполните следующие действия, чтобы поэкспериментировать со стандартными XAML в Xamarin.Forms.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Загрузить [предварительного просмотра здесь пакет NuGet](https://aka.ms/xf-xamlstandard-nuget).
2. Добавить **Xamarin.Forms.Alias** пакета NuGet в проект Xamarin.Forms PCL .NET Standard и платформы.
3. Инициализировать пакет с `Alias.Init()`
4. Добавить `xmlns:a` ссылки `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Использовать типы в XAML - в разделе [Справочник элементов управления](controls.md) для получения дополнительной информации.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Загрузить [предварительного просмотра здесь пакет NuGet](https://aka.ms/xf-xamlstandard-nuget).
2. Добавить **Xamarin.Forms.Alias** пакета NuGet в проект Xamarin.Forms PCL .NET Standard и платформы.
3. Инициализировать пакет с `Alias.Init()`
4. Добавить `xmlns:a` ссылки `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Использовать типы в XAML - в разделе [Справочник элементов управления](controls.md) для получения дополнительной информации.

-----

Следующий код XAML демонстрирует некоторые XAML стандартных элементов управления, используемых в Xamarin.Forms `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> **Примечание:** требует xmlns `a:` префикс имени XAML стандартные элементы управления является ограничением текущей предварительной версии.


## <a name="related-links"></a>Связанные ссылки

- [Предварительная версия NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Справочник по элементам управления](controls.md)
