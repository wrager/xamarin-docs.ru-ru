---
title: Редактирование метаданных NuGet
description: Параметры проекта используются для изменения метаданных NuGet многоплатформенного библиотек
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fa526d33758afb73965e315c8e471d960d84e781
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="editing-nuget-metadata"></a>Редактирование метаданных NuGet

_Параметры проекта используются для изменения метаданных NuGet многоплатформенного библиотек_

Типы проекта библиотеки (например PCL .NET Standard или новый тип проекта NuGet) имеют **пакет NuGet** статьи **параметры проекта** окна.

**Метаданные** раздел настраивает значения, используемые в [ **.nuspec** файл манифеста пакета NuGet](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Необходимые сведения

**Общие** вкладка содержит четыре поля, которые должны вводиться для создания пакета NuGet:

[![](metadata-images/metadata-general-sml.png "Окна необходимые метаданные пакета NuGet.")](metadata-images/metadata-general.png#lightbox)

- **Идентификатор** — идентификатор пакета, который должен быть уникальным в пределах Nuget.org (или везде, где этот пакет будет распространяться). После этого использовать [руководство](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) и использовать только символы, допустимые в URL-АДРЕСЕ (без пробелов и избежать большинство специальных символов).
- **Версия** — выберите номер версии, согласуется с [правилами управления версиями NuGet](https://docs.microsoft.com/en-us/nuget/create-packages/dependency-versions).
- **Авторы** — список имен, разделенных запятыми.
- **Описание** — Обзор возможностей пакета, который отображается, когда пользователь выбирает пакет.

> [!NOTE]
> Не забудьте увеличить номер версии при создании новых версий для распространения среди NuGet или других пользователей.

Дополнительные сведения см. в разделе [необходимые ссылки на элементы](https://docs.microsoft.com/en-us/nuget/schema/nuspec#required-metadata-elements) Дополнительные сведения, а также как эти подробные инструкции по [выбора идентификатора уникальных пакета и настройки номер версии](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) и [ Тип пакета параметра](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Необходимо ввести все поля на этой вкладке; в противном случае появится сообщение об ошибке: _«проект не содержит NuGet метаданные, не будет создан пакет NuGet. В разделе метаданных в параметрах проекта можно указать метаданные пакета NuGet»_

## <a name="optional-metadata"></a>Необязательные метаданные

**Сведения** вкладка содержит дополнительные поля для включения в файле манифеста пакета NuGet.

[![](metadata-images/metadata-detail-sml.png "Окна необязательные метаданные пакета NuGet.")](metadata-images/metadata-detail.png#lightbox)

Ссылаться на [Справочник по дополнительным элементам](https://docs.microsoft.com/en-us/nuget/schema/nuspec#optional-metadata-elements) Дополнительные сведения о обязательные и необязательные поля.

> [!NOTE]
> Если пакет NuGet распространяется на [NuGet.org](https://www.nuget.org) рекомендуется указать столько информации.


## <a name="related-links"></a>Связанные ссылки

- [Справочник по файлу NUSPEC](https://docs.microsoft.com/en-us/nuget/schema/nuspec#general-form-and-schema)
