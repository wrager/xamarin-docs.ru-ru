---
title: Специальные возможности в Xamarin.Forms
description: Построение доступности приложения гарантирует, что приложение может использоваться подход пользовательский интерфейс в диапазоне от потребностей и опыт, специалистами.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 813100acad03684fa9db5343534aee7ca13400c1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241859"
---
# <a name="xamarinforms-accessibility"></a>Специальные возможности в Xamarin.Forms

_Построение доступности приложения гарантирует, что приложение может использоваться подход пользовательский интерфейс в диапазоне от потребностей и опыт, специалистами._

Предоставление доступа к приложению Xamarin.Forms означает думать о макете и конструктора многие элементы пользовательского интерфейса. Рекомендации, которые следует учитывать см. в разделе [контрольный список специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md). Многих связанных со специальными возможностями, таких как крупные шрифты и подходящие настройки цвета и контрастности уже можно работать посредством API-интерфейсы Xamarin.Forms.

[Android специальных возможностей](~/android/app-fundamentals/accessibility.md) и [специальных операций ввода-вывода](~/ios/app-fundamentals/accessibility.md) руководства содержат сведения о собственных API, предоставляемые Xamarin и [UWP руководство специальных возможностей на сайте MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) объясняет собственный подход на этой платформе. Эти API используются для полной реализации приложений со специальными возможностями для каждой платформы.

Xamarin.Forms в данный момент нет *встроенные* поддержки для всех специальных возможностей API-интерфейса на каждом из базовой платформы. Тем не менее он поддерживает значения параметров специальных возможностей на элементы пользовательского интерфейса для поддержки средств помощь по экрана чтения и навигации, которое является одним из наиболее важные части построение приложений со специальными возможностями. Дополнительные сведения см. в разделе [значения параметров специальных возможностей на элементы пользовательского интерфейса](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md).

Другие специальные возможности API-интерфейсы (такие как [PostNotification на iOS](~/ios/app-fundamentals/accessibility.md)) может быть лучше подходят для [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) или [пользовательского средства визуализации](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) реализации. Они не описаны в данном руководстве.

## <a name="testing-accessibility"></a>Тестирование специальных возможностей

Xamarin.Forms приложений обычно предназначены для нескольких платформ, это означает, что проверка специальных возможностей в зависимости от используемой платформы. Выполните эти ссылки на дополнительные сведения о тестировании специальных возможностей на каждой платформе.

- [**iOS тестирования**](~/ios/app-fundamentals/accessibility.md)
- [**Тестирование Android**](~/android/app-fundamentals/accessibility.md)
- [**AccScope Windows (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>Связанные ссылки

- [Кросс платформенных специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md)
- [Установка значений специальных возможностей в элементах пользовательского интерфейса](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
