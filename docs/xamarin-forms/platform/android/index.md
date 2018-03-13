---
title: "Компоненты платформы Android"
description: "Добавление функциональных возможностей, зависящих от Android Xamarin.Forms приложений"
ms.topic: article
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 9b5e9a4c449bc99bd88fc415f5ebb969d2c2a08a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-features"></a>Компоненты платформы Android

## <a name="platform-support"></a>Поддержка платформ

По умолчанию проект Xamarin.Forms Android использует старого стиля типы управления, общей до Android 5.0. Приложения, построенные с помощью шаблона имеют `FormsApplicationActivity` как базовый класс, из их основных действий.

## <a name="material-design-via-appcompat"></a>Существенные разработки через совместимости приложений

Xamarin.Forms также имеет дополнительный `FormsAppCompatActivity` , использующий **AppCompat** функций, предоставляемых Android для реализации материал разработчикам.

Для добавления в проект Xamarin.Forms Android материал разработчикам выполните [поддерживают инструкции по установке для совместимости приложений](appcompat.md)

Вот **Todo** образец со значением по умолчанию `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "Пример TODO приложения без AppCompat")](images/before-appcompat.png#lightbox "Todo образец приложения без совместимости приложений")

И это тот же код после обновления проектов для использования `FormsAppCompatActivity` (и добавление сведений о дополнительных темы):

[![](images/post-appcompat-sml.png "TODO образец приложения с AppCompat и темы")](images/post-appcompat.png#lightbox "Todo образец приложения с AppCompat и темы")

> [!NOTE]
> При использовании `FormsAppCompatActivity`, [базовые классы для некоторых Android пользовательских модулей подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) будут отличаться.


## <a name="related-links"></a>Связанные ссылки

- [Добавление поддержки материала разработки](appcompat.md)
