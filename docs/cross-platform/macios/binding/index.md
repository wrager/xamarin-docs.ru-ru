---
title: Привязка Objective-C
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 721036993061d08dadf8b279e13981caaa51f91f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="binding-objective-c"></a>Привязка Objective-C

Этот раздел содержит различные документы, которые охватывают Создание привязки к библиотекам Objective-C, поэтому их можно вызывать из приложения C#, созданные с помощью Xamarin.iOS или Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Обзор набора средств Visual Studio для Unity](~/cross-platform/macios/binding/overview.md)

Этот документ содержит некоторые из внутренних принцип осуществления привязки. Это документ дополнительные технические сведения.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Привязка библиотек Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

В этом документе описан процесс, который используется для создания привязок C# Objective-C API и сопоставление идиом в Objective-C идиом, используемых в .NET.
При связывании просто C API-интерфейсы для этого платформа P/Invoke следует использовать стандартный механизм .NET.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Руководство по определению привязки](~/cross-platform/macios/binding/binding-types-reference.md)

Это руководство, которая описывает все атрибуты, доступные для авторов привязки для управления процессом создания привязки.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цели Sharpie — это средство командной строки для начальной загрузки на первом этапе привязки. Он работает путем синтаксического анализа заголовочные файлы для сопоставления открытого API-интерфейса в собственной библиотеки [определения привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (этот процесс, можно также вручную).

## <a name="ios"></a>iOS

[Страницы привязки iOS](~/ios/platform/binding-objective-c/index.md) ссылается на общие ресурсы привязки, кроме в примерах ниже.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Пошаговое руководство: Привязка библиотеку Objective c.](~/ios/platform/binding-objective-c/walkthrough.md)

В этой статье приводятся пошаговые указания создания привязки проекта, используя открытый исходный код [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C проекта в качестве примера. Библиотека InfColorPicker предоставляет контроллер для повторного использования представления, разрешить пользователю выбирать цвета в его представление HSB выбора цвета более удобной для пользователей. Цели Sharpie будет использоваться для помощи в процессе привязки.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Образцы привязки](https://github.com/mono/monotouch-bindings)

Коллекцию привязок сторонних разработчиков, которые могут быть использовать ссылку, при создании новых проектов привязки.

## <a name="mac"></a>Mac

Исторически [привязки Mac](~/mac/platform/binding.md) была очень ручной процесс. В данный момент [загружаемые preview](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) поддержки Mac привязки проекта в будущих выпусках Visual Studio для Mac.



## <a name="related-links"></a>Связанные ссылки

- [iOS привязки](~/ios/platform/binding-objective-c/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
