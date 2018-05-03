---
title: Требования к Xamarin.Forms
description: Требования к платформе и системные требования к разработке Xamarin.Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/19/2018
ms.openlocfilehash: d2125c1ddaa3edc3e2ee76d8e03e384efdca42c6
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/20/2018
---
# <a name="xamarinforms-requirements"></a>Требования к Xamarin.Forms

_Требования к платформе и системные требования к разработке Xamarin.Forms._

Обзор рекомендаций по установке и настройке, действующих для различных платформ, см. в статье [Установка](~/cross-platform/get-started/installation/index.md).

## <a name="target-platforms"></a>Целевые платформы

Приложения Xamarin.Forms могут быть написаны для следующих операционных систем:

-  iOS 8 или более поздние версии;
-  Android 4.0.3 (API 15) или более поздние версии ([подробнее](#android));
-  универсальная платформа Windows для Windows 10 ([подробнее](#windows10));
-  Windows 8.1 или Windows Phone 8.1 WinRT ([подробнее](#windows));
-  *Windows Phone 8 Silverlight (не рекомендуется)*.

Предполагается, что разработчики знакомы с [переносимыми библиотеками классов](~/cross-platform/app-fundamentals/pcl.md) и [общими проектами](~/cross-platform/app-fundamentals/shared-projects.md).

<a name="android" />

### <a name="android"></a>Android

Необходимо установить последнюю версию Android SDK Tools и платформы API Android. Для обновления до последних версий можно воспользоваться [диспетчером пакетов SDK Android](~/android/get-started/installation/android-sdk.md).

Кроме того, в целевой версии или версии компиляции для проектов Android **должен** быть выбран параметр *Использовать самую новую установленную платформу*. Однако минимальной версией может быть API 15, что позволяет поддерживать устройства, использующие Android 4.0.3 и более поздние версии. Эти значения задаются в разделе **Параметры проекта**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Параметры проекта > Приложение > Свойства приложения**

![](installation-images/options-android-vs-sml.png "Параметры сборки Android в Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

**Сборка > Общие**

![](installation-images/options-general-sml.png "Сборка > Общие")

**Сборка > Приложение Android**

![](installation-images/options-android-sml.png "Сборка > Приложение Android")

-----


<a name="windows10" />

### <a name="universal-windows-platform"></a>Универсальная платформа Windows 

Если решение создается в macOS, проекты универсальной платформы Windows для Windows 10 не добавляются. Инструкции по добавлению этих проектов в существующее решение см. в статье [Добавление приложения универсальной платформы Windows (UWP)](~/xamarin-forms/platform/windows/installation/universal.md).


<a name="windows" />

### <a name="windows-81--windows-phone-81-winrt"></a>Windows 8.1 или Windows Phone 8.1 WinRT

Если решение создается в macOS, проекты Windows 8.1 или Windows Phone 8.1 WinRT не добавляются. Инструкции по добавлению этих проектов в существующее решение см. в статьях [Добавление приложения Windows Phone](~/xamarin-forms/platform/windows/installation/phone.md) и [Добавление приложения Windows](~/xamarin-forms/platform/windows/installation/tablet.md).


## <a name="development-system-requirements"></a>Требования к системе для разработки

Приложения Xamarin.Forms можно разрабатывать в ОС в macOS и Windows. Однако для создания Windows-версий приложения требуются Windows и Visual Studio.

## <a name="mac-system-requirements"></a>Требования к системе Mac

Visual Studio для Mac можно использовать для разработки приложений Xamarin.Forms в OS X El Capitan (10.11) или более поздних версиях. Для разработки приложений iOS рекомендуется установить по меньшей мере пакет SDK iOS 10 и Xcode 8.

> [!NOTE]
>  Приложения Windows нельзя разрабатывать в macOS.

## <a name="windows-system-requirements"></a>Требования к системе Windows

Приложения Xamarin.Forms для iOS и Android можно создавать в любой установке Windows, которая поддерживает возможности разработки Xamarin. Для этого в ОС Windows 7 и более поздних версиях следует установить Visual Studio 2017 или более позднюю версию. Для разработки в iOS требуется подключенный к сети компьютер Mac.

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

Для разработки приложений Xamarin.Forms для UWP требуются следующие компоненты:

* Windows 10 (рекомендуется Fall Creators Update)

* Visual Studio 2017

* [Пакет средств разработки Windows 10](https://dev.windows.com/downloads/windows-10-sdk)

Проекты UWP включены в решения Xamarin.Forms, созданные в Visual Studio 2015 и Visual Studio 2017.
[Приложение универсальной платформы Windows (UWP) можно добавить](~/xamarin-forms/platform/windows/installation/universal.md) в существующее решение Xamarin.Forms.

