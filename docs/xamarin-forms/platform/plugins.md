---
title: "Подключаемые модули"
description: "Легко добавлять собственные функции в Xamarin.Forms приложений"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: ad338e655c1aeb475122c837ca5c805e491f84bc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="plugins"></a>Подключаемые модули

Имеется множество возможностей собственной платформы, которые существуют для всех платформ, но имеет немного другие API-интерфейсы. Разработчики создавать подключаемые модули для создания абстрактный интерфейс кросс платформенных для этих функций, они могут также совместно использовать с другими разделами.

Эти возможности включают: состояние аккумулятора, компас, датчиков движения, geolocation, преобразования текста в речь и многое другое. Подключаемые модули позволяют легко получать Xamarin.Forms приложения эти возможности.

## <a name="finding-and-adding-plugins"></a>Поиск и добавление подключаемых модулей

Xamarin сообщества создал многих подключаемых кросс платформенных совместим с Xamarin.Forms - большой коллекции можно найти в:

[**Подключаемые модули Xamarin**](https://github.com/xamarin/plugins)

Руководство для добавления пакетов NuGet в проект, см на нашем пошаговом руководстве [включая пакета NuGet в проекте](/visualstudio/mac/nuget-walkthrough/).


## <a name="creating-plugins"></a>Создание подключаемых модулей

Можно также создать и опубликовать собственных подключаемых модулей как пакетов Nuget (и компоненты Xamarin). Многие существующие подключаемые модули, открытым исходным кодом, поэтому вы сможете просматривать их код, чтобы понять, как они были writtern.

Например, список ниже подключаемых модулей являются все открытым исходным кодом, и они соответствуют примеров в [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) раздела:

- **Преобразования текста в речь** , Джеймсом Монтеманьо &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) и [NuGet  ](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **Состояние батареи** , Джеймсом Монтеманьо &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) и [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Эти проекты Github можно указать хорошей отправной точкой для создания собственных кросс платформенных подключаемых модулей, как выполнять эти инструкции для [Создание подключаемого модуля для Xamarin](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Структурирование подключаемого модуля кросс платформенные проекты

Несмотря на то, что определенный требования по разработке пакета NuGet отсутствуют, существуют некоторые рекомендации по созданию пакета для кросс платформенные приложения.

Подключаемый модуль кросс платформенных обычно должен включать следующие компоненты:

- PCL с интерфейс, представляющий API для подключаемого модуля
- iOS, Android и Windows класса библиотеки, используя реализацию интерфейса.

Чтение Джеймсом Монтеманьо [блога](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) описание процесса создания подключаемых модулей для Xamarin.

Рекомендуется избегать ссылок Xamarin.Forms непосредственно из подключаемого модуля.
Это может создать проблемы конфликта версий, при попытке использовать подключаемый модуль других разработчиков. Вместо этого попробуйте проектировать API так, чтобы он может использоваться любым приложением .NET или Xamarin.

### <a name="publishing-nuget-packages"></a>Публикация пакетов NuGet

Пакеты NuGet имеют **nuspec** файл, который представляет XML-файл, который определяет, какие части проекта публикуются в пакете. **Nuspec** файла также включает сведения о пакете, такие как идентификатор, заголовок и авторы.

В разделе [документации NuGet](http://docs.nuget.org/create/creating-and-publishing-a-package) Дополнительные сведения о создании и публикации пакеты NuGet.


## <a name="related-links"></a>Связанные ссылки

- [Создание многократно используемых подключаемых модулей для Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [С помощью & разработке подключаемых модулей для Xamarin (видео)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
