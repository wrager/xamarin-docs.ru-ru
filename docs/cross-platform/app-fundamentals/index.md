---
title: "Принципы работы приложения"
description: "Основные понятия приложения"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: c5b823370e5b65fbcf9ba366cb89c05e003b1a89
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="application-fundamentals"></a>Принципы работы приложения

В этом разделе содержится руководство для некоторых наиболее распространенных задач действия или концепции, разработчики должны иметь в виду при разработке приложений для мобильных устройств.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[Создание кроссплатформенных приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Выбор Xamarin и регулярно несколько вещей помнить при проектировании и разработке приложений для мобильных устройств, можно реализовать огромный объем кода, совместное использование на мобильных платформах, Сократите время выхода на рынок, использовать существующий talent, требований пользователей для мобильного доступа и уменьшить сложность между различными платформами. &nbsp;В этом документе перечислены ключевые рекомендации для понимания этих преимуществ для служебной программы и повышения производительности приложений.

## <a name="code-sharing-optionscode-sharingmd"></a>[Варианты общего доступа к коду](code-sharing.md)

Дополнительные сведения о различных кода, параметры, доступные для проектов Xamarin, включая переносимой библиотеки классов (PCLs), общих проектов и стандартные библиотеки .NET для управления доступом.


## <a name="accessibilityaccessibilitymd"></a>[Специальные возможности](accessibility.md)

Советы по созданию приложений со специальными возможностями.


## <a name="localizationlocalizationmd"></a>[Локализация](localization.md)

Рекомендации, обеспечивающие языковой стандарт приложений, которые могут быть переведены на несколько языков.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Переносимые библиотеки классов](~/cross-platform/app-fundamentals/pcl.md)

Проекты переносимой библиотеки классов позволяют создавать и распространять сборки, содержащие общий код на нескольких платформах. Для создания переносимой библиотеки классов (или «PCL») сначала выбрать какие платформы целевого, а затем написать код для подмножество .NET Framework, которая доступна в профиле, определенные для этих платформ. В этом документе описывается создание и использование PCLs с помощью Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Общие проекты](~/cross-platform/app-fundamentals/shared-projects.md)

Общие проекты позволяют писать общий код, который ссылается на несколько проектов различных приложений. Код компилируется как часть каждого ссылающийся проект и могут включать директивы компилятора для внедрения функциональных возможностей платформой базы общего кода. Здесь рассматривается работа общие проекты и как создать и использовать их с проектами Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

Стандартная .NET — это новая возможность для совместное использование кода несколькими платформами. Он работает сходным образом в переносимой библиотеки классов; Код построена на основе конкретной версии (в настоящее время 1.0 до версии 1.6) и может быть использовано в других проектах, которые поддерживают этот уровень или более поздней версии. Проекты .NET standard, поддерживаются в Xamarin Studio 6.2, Visual Studio для Windows и Visual Studio для Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Проекты NuGet: Многоплатформенного библиотек для совместного использования кода](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Пакеты NuGet можно создавать автоматически из проектов PCL или .NET standard. и общие проекты, которые могут быть упакованы в пакеты NuGet «заманить и подменить», с помощью отдельный тип проекта NuGet. В этом разделе описывается создание пакетов NuGet для каждого сценария совместного использования кода.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Создание вручную пакетов NuGet для Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Советы по созданию пакеты NuGet, которые работают с платформе Xamarin.

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[Междоменный доступ платформы данных](~/xamarin-forms/data-cloud/index.md)

Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных элементарно мал, это обычно требуется базы данных и уровнем данных в приложении для управления доступом к базе данных. iOS и Android установлен компонент database engine SQLite, «встроенные» и доступа для хранения и извлечения данных стало проще благодаря платформе Xamarin. [Доступа к данным Android](~/android/data-cloud/data-access/index.md), [доступа к данным для операций ввода-вывода](~/ios/data-cloud/data/index.md), и [доступа к данным Xamarin.Forms](~/xamarin-forms/data-cloud/index.md) руководства содержат примеры того, как получить доступ к SQLite для каждой платформы.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[Протокол TLS](transport-layer-security.md)

Сведения о selectingthe правильной реализации SSL/TLS для защиты приложения сетевое подключение.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[Уведомления](~/xamarin-forms/data-cloud/push-notifications/index.md)

Мобильные приложения используют уведомления ненавязчивого способ информирования пользователя, который был выполнен некоторые события конкретного приложения. Обычно уведомления используются для уведомления пользователя о состоянии процесса приложения, на котором выполняется в фоновом режиме. Примером этого может загружать большой файл. Этот файл может занять много времени для загрузки, поэтому это действие должно выполняться в фоновом режиме. Когда загрузка завершена, пользователь уведомляется факта по уведомление.
Кроме того уведомления ares не только к локальным приложениям. Можно также для серверных приложений для публикации уведомлений для приложений для мобильных устройств. В этой статье рассматривается использование уведомлений в iOS и Android.
