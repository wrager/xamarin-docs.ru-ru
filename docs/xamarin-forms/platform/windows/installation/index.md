---
title: "Проекты установки Windows"
description: "Добавление новых проектов Windows в существующее решение Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 2acabc68c595bfb3bbd5c3516f68cd777ba10327
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="setup-windows-projects"></a>Проекты установки Windows

_Добавление новых проектов Windows в существующее решение Xamarin.Forms_

Старые проекты Xamarin.Forms (или тех, которые созданы на Mac OS&nbsp;X) не будет иметь следующие настройки проектов Windows.

Это означает, что вам потребуется вручную добавить этих типов проектов для создания приложений Windows 8.1, Windows Phone 8.1 и Windows 10 (UWP).

## <a name="add-a-windows-81-app"></a>Добавление приложения Windows 8.1

* Если используется шаблон PCL [обновить профиль](#pcl), затем
* [Добавление приложения Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) для планшета или рабочего стола форм факторов.

## <a name="add-a-windows-phone-81-app"></a>Добавление Windows Phone 8.1 приложения

* Если используется шаблон PCL [обновить профиль](#pcl), затем
* [Добавление Windows Phone 8.1 приложения](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>Добавить универсальную платформу Windows приложения платформы (UWP)

* Построение [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) приложений требуется Visual Studio 2015, под управлением Windows 10.
* Если используется шаблон PCL [обновить профиль](#pcl), затем
* [Добавить универсальную платформу Windows приложения платформы](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>Обновление профиля PCL

Если существующие приложения Xamarin.Forms шаблона переносимой библиотеки классов (PCL), необходимо обновить его профиль.

1. **Щелкните правой кнопкой мыши > свойства** (существующие параметры могут отличаться)

  ![](images/targets.png "Целевые объекты PCL")

2. Щелкните **изменений...**  кнопки

3. Убедитесь, **Windows 8** и **Windows Phone 8.1** выбраны параметры (и **Silveright Windows Phone** — *свертывание*):

  ![](images/pcl.png "PCL Target-параметры")

4. Нажмите клавишу **ОК** и сохраните изменения.

Это равнозначно **111 профиль** при настройке вашего PCL в Visual Studio для Mac с помощью раскрывающегося списка.

  ![](images/pcl-xs.png "Профиль PCL 111")

**Примечание:** Если решение по-прежнему содержит проект Windows Phone 8 Silverlight, PCL должно быть присвоено профиль 259 знаков. Поддержка Windows Phone 8 Silverlight устарела, поэтому рекомендуется заменить его в типах проектов, отображаемые на этой странице.
