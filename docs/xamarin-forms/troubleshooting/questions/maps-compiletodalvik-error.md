---
title: "Почему Мой проект Xamarin.Forms.Maps Android не с НЕПРЕДВИДЕННОЙ ОШИБКОЙ верхнего уровня COMPILETODALVIK?"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 41cb955ba22a83d0ae2a11ac5c5a114b465ca9b2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Почему Мой проект Xamarin.Forms.Maps Android не с НЕПРЕДВИДЕННОЙ ОШИБКОЙ верхнего уровня COMPILETODALVIK?

Эта ошибка может появиться в панели ошибок Visual Studio для Mac или в окне вывода построения Visual Studio; в Android проектов, с помощью Xamarin.Forms.Maps.

Чаще всего разрешается путем увеличения размера кучи Java для проекта Xamarin.Android. Выполните следующие действия, чтобы увеличить размер кучи.

## <a name="visual-studio"></a>Visual Studio

1. Щелкните правой кнопкой мыши проект Android и откройте свойства проекта.
2. Последовательно выберите пункты **Android параметры -> Дополнительно**
3. В текстовом поле Размер кучи Java введите 1 ГБ.
4. Перестройте проект.

![Снимок экрана параметров проекта Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Android параметров в Visual Studio сборки")

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1.  Щелкните правой кнопкой мыши проект Android и откройте свойства проекта.
2.  Последовательно выберите пункты **построение -> сборки Android -> Дополнительно**
3.  В текстовом поле Размер кучи Java введите 1 ГБ.
4.  Перестройте проект.  

![Снимок экрана: Visual Studio для Mac параметров проекта](maps-compiletodalvik-error-images/xsjavaheap.png "Android построения параметры в Visual Studio для Mac")

