---
title: Azure Active Directory
description: Регистрация приложения для использования Azure Active Directory
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: decc069bd2fe31d54c886793ae4c94935b23ad02
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Регистрация приложения для использования Azure Active Directory_

Azure Active Directory позволяет разработчикам защищенным ресурсам, таких как файлы, ссылки и веб-API, с помощью одной учетной записи организации, сотрудники используют для входа в их системах или проверить свои сообщения электронной почты.

Разработка мобильных приложений, которые могут проходить проверку подлинности в Azure Active Directory состоит из трех этапов.
Первые два шага, обычно относятся к тем же, независимо от того, какие службы вы планируете использовать. Третий шаг отличается для каждого типа службы.

  1. [Регистрация в Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) на *windowsazure.com* портала, нажмите
  2. [Настройка служб](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Разработка мобильных приложений с использованием служб.

Примеры различных служб, которые можно открыть.

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Веб-интерфейс API
- Office 365


## <a name="conclusion"></a>Заключение

С помощью шагов выше может проверить подлинность мобильных приложений с Azure Active Directory. Библиотека проверки подлинности Active Directory (ADAL) облегчает процесс с меньшим количеством строк кода, сохраняя большая часть кода совпадают и тем самым создавая совместного использования разных платформах.



## <a name="related-links"></a>Связанные ссылки

- [Образец Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
