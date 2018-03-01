---
title: "Возможности платформы"
description: "Использовать преимущества платформ с помощью Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
ms.openlocfilehash: 950cc4a8611b05c22825ef89a85827fa0d3e5f7b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Возможности платформы

Xamarin.Forms является расширяемым и позволяет объединять компонентов платформой, с использованием [эффекты](~/xamarin-forms/app-fundamentals/effects/index.md), [пользовательские модули подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [помощью DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)и многое другое.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

В этом руководстве описывается реализация конструктора материал путем обновления существующего приложения Xamarin.Forms Android.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[Индексирование приложения и глубокое связывание](deep-linking.md)

Индексирование приложения позволяет приложениям, в противном случае — забывается после использует несколько оставаться применимо, в результатах поиска. Глубокое связывание позволяет приложениям реагировать на результат поиска, который содержит данные приложений, как правило, перейдя на страницу, на которые ссылается прямая ссылка.

## <a name="device-classdevicemd"></a>[Класс устройства](device.md)

Как использовать `Device` класса, чтобы создать поведение конкретной платформы в общий код и пользовательский интерфейс (в том числе с помощью XAML). Также описывается `BeginInvokeOnMainThread` что является необходимым при изменении элементов управления пользовательского интерфейса из фоновых потоков.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

Некоторые стили операций ввода-вывода выполняемые с помощью **Info.plist** и `UIAppearance` API. Данное руководство содержит примеры использования для включения в приложения iOS решения Xamarin.Forms, включая поиску Spotlight основные функции iOS 9.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms теперь имеет поддержку macOS приложений в предварительной версии.

## <a name="native-formsnative-formsmd"></a>[Собственный форм](native-forms.md)

Собственный формы позволяют Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страниц для использования собственных проектов Xamarin.iOS, Xamarin.Android и универсальной платформы Windows (UWP).

## <a name="native-viewsnative-viewsindexmd"></a>[Собственные представления](native-views/index.md)

Собственные представления из iOS, Android и универсальная платформа Windows могут существовать прямые ссылки из Xamarin.Forms. Свойства и обработчики событий могут быть установлены для собственного представления, и они могут взаимодействовать с Xamarin.Forms представления.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Особенности платформы](platform-specifics/index.md)

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, не требуя пользовательские модули подготовки отчетов или эффекты.

## <a name="pluginspluginsmd"></a>[Plugins](plugins.md)

В Github, Nuget и хранилище компонентов Xamarin, чтобы расширить Xamarin.Forms приложений доступны самые разнообразные подключаемые модули открытым исходным кодом.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms реализована поддержка четыре различных типа проекта Windows:

* Windows Phone 8 Silverlight (исходный платформы Windows поддерживается Xamarin.Forms)
* Windows Phone 8.1 (WinRT)
* Windows 8.1 (WinRT) и
* Универсальная платформа Windows (Windows 10).

В этом разделе описаны различия между ними и их добавлении в существующее решение Xamarin.Forms.
