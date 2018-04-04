---
title: Привязка библиотеки iOS
description: Том, как делать собственные библиотеки iOS (и CocoaPods) доступны в приложениях Xamarin.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 268900c7ab7b317b0b20f4c1ead2360fd6f9bbf0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="binding-ios-libraries"></a>Привязка библиотеки iOS

_Том, как делать собственные библиотеки iOS (и CocoaPods) доступны в приложениях Xamarin._

Выполните эти ссылки на дополнительные сведения о привязке библиотеки Objective-C и CocoaPods Xamarin.iOS и Xamarin.Mac.

- [**Общие сведения о** ](~/cross-platform/macios/binding/overview.md) -
  описано, как работает привязка.
- [**Привязка библиотеки Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  инструкции о том, как привязывать библиотеки Objective-C для использования в проектах Xamarin.
- [**Руководство по определению типа** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  описывает все атрибуты, доступные для авторов привязки для управления процессом создания привязки.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цели Sharpie — это средство командной строки для начальной загрузки на первом этапе привязки.
Он работает путем синтаксического анализа заголовочные файлы для сопоставления открытого API-интерфейса в собственной библиотеки [определения привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (этот процесс, в противном случае выполняется вручную). Цели Sharpie не создает привязку сам по себе, но помогут приступить к работе!

Цели Sharpie 3.0 появилась возможность непосредственно привязать Cocoapods!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Пошаговое руководство. привязка библиотеки Objective-C iOS](walkthrough.md)

Эта страница содержит пошаговое руководство по использованию создается проект iOS привязки, используя открытый исходный код [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) Objective-C проекта в качестве примера. **InfColorPicker** библиотека предоставляет контроллер для повторного использования представления, разрешить пользователю выбирать цвета в его представление HSB выбора цвета более удобной для пользователей.
Цели Sharpie будет использоваться для помощи в процессе привязки.

## <a name="xamarin-university-lightning-lecture"></a>Лекции Молния университета Xamarin

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS привязок в C/C++ с [университета Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Связанные ссылки

- [Привязка Objective-C](~/cross-platform/macios/binding/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
