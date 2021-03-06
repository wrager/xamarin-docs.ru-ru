---
title: Введение в расширения функциональности
description: Это поведение позволяет добавлять функциональные возможности к элементам пользовательского интерфейса без необходимости подкласс их. Вместо этого функциональность реализуется в классе поведение и присоединенных к элементу управления, как если бы он был частью самого элемента управления. В этой статье содержатся вводные поведения.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: dc6d8396c2908d251290e4540dbb3cec3344542f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732792"
---
# <a name="introduction-to-behaviors"></a>Введение в расширения функциональности

_Это поведение позволяет добавлять функциональные возможности к элементам пользовательского интерфейса без необходимости подкласс их. Вместо этого функциональность реализуется в классе поведение и присоединенных к элементу управления, как если бы он был частью самого элемента управления. В этой статье содержатся вводные поведения._

Поведения позволяют реализовать код, который вы обычно имеет для записи в виде кода программной части, так как он взаимодействует напрямую с помощью API управления таким образом, что его можно кратко присоединенных к элементу управления и упакованные для повторного использования в нескольких приложениях. Они могут использоваться для обеспечения полный спектр функциональные возможности для элементов управления, таких как:

- Добавление электронной почты проверяющего элемента управления для [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/).
- Создание элемента управления оценка с помощью распознавателя tap.
- Управление анимацией.
- Добавление эффекта в элемент управления.

Поведение также позволяют более сложных сценариев. В контексте *заставляя*, поведения — это полезна для подключения элемента управления к команде. Кроме того их можно использовать для связи команд с помощью элементов управления, которые не были предназначены для взаимодействия с командами. Например их можно использовать для вызова команды в ответ на события.

Xamarin.Forms поддерживает два различных стилей поведений:

- **Xamarin.Forms поведения** — классы, производные от [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) или [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) класса, где `T` тип элемента управления, к которому поведение необходимо применить. Дополнительные сведения о Xamarin.Forms поведения см. в разделе [поведения Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) и [для повторного использования поведения](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Вложенные поведения** — `static` классы с одного или нескольких вложенных свойств. Дополнительные сведения о вложенные поведения см. в разделе [присоединенного поведения](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Это руководство посвящено Xamarin.Forms поведения, так как они являются рекомендуемым способом создания поведение.



## <a name="related-links"></a>Связанные ссылки

- [Поведение](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Поведение&lt;T&gt;](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
