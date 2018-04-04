---
title: Изменения архитектуры
description: Обзор новых возможностей iOS 11
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 6dd874fd803d2b3de21b0b604cbc2bafa5bf1aec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="architecture-changes"></a>Изменения архитектуры

_Обзор новых возможностей iOS 11_

Одна из крупнейших изменений, которые следует учитывать с iOS 11 — поддержки 32-разрядных приложений, как описано в [Apple](https://developer.apple.com/news/?id=06282017b) пресс-релизе. Все новые приложения и обновления для существующих приложений должен поддерживать 64-разрядной. 32-разрядных приложений **не запускаются** в iOS 11.

Чтобы обновить приложение в Visual Studio для Mac, выполните следующие действия:

1. Щелкните правой кнопкой мыши имя проекта, чтобы открыть **параметры проекта**.
2. Перейдите к **сборка iOS** вкладки.
3. Задать **Поддерживаемые архитектуры** раскрывающийся список для **x86_64** для **отладка | iPhoneSimulator** и **выпуска | iPhoneSimulator**:

    ![Изменения архитектуры симулятор раскрывающегося списка](architecture-changes-images/image1.png)

4. Для устройств iOS, измените конфигурацию на **отладка | iPhone** и задайте **Поддерживаемые архитектуры** раскрывающийся список для **ARM64**:

    ![Изменение устройства архитектур раскрывающегося списка](architecture-changes-images/image2.png)

5. Измените конфигурацию на **выпуска | iPhone** и задайте **Поддерживаемые архитектуры** раскрывающийся список для **ARM64**.

Дополнительные сведения о 32-разрядных и 64-разрядной архитектуры в разделе [анализ 32 или 64-разрядной платформе](~/cross-platform/macios/32-and-64/index.md#ios) руководства.

## <a name="related-links"></a>Связанные ссылки

- [Новые возможности iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Страница продукта обновленные App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Обновление приложения для iOS 11 (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/204/)
