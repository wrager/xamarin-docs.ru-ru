---
title: Задание стиля Xamarin.Forms приложения, использующие XAML стили
description: В этом руководстве объясняется, как настроить внешний вид приложения Xamarin.Forms с помощью стилей XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f439e3ba888b67ac1752eae95149adcf55055943
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245876"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Задание стиля Xamarin.Forms приложения, использующие XAML стили

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

Xamarin.Forms приложения часто содержат несколько элементов управления, имеющих идентичным внешним видом. Настройка внешнего вида каждого отдельного элемента управления может быть повторяющихся и подвержены ошибкам. Вместо этого стили можно создать, настроить внешний вид элемента управления путем группирования и параметры свойств, доступных на типе элемента управления.

## <a name="explicit-stylesexplicitmd"></a>[Явные стили](explicit.md)

*Явных* стиль — это сервер, выборочно применять к элементам управления, установив их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства.

## <a name="implicit-stylesimplicitmd"></a>[Неявные стили](implicit.md)

*Неявное* стиль является тот, который используется для всех элементов управления в одной и той же [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), без необходимости каждый элемент управления для ссылки на стиль.

## <a name="global-stylesapplicationmd"></a>[Глобальные стили](application.md)

Стили можно сделать доступными глобально, добавив их в приложение [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Это помогает избежать дублирования стилей всех страниц или элементов управления.

## <a name="style-inheritanceinheritancemd"></a>[Наследование стилей](inheritance.md)

Стили можно наследовать другие стили, чтобы сократить дублирование и включить повторное использование.

## <a name="dynamic-stylesdynamicmd"></a>[Динамические стили](dynamic.md)

Стили не реагировать на изменения свойств и остаются неизменными в течение всего приложения. Тем не менее приложения можно реагировать на изменения стиля динамически во время выполнения с помощью динамического ресурсов.

## <a name="device-stylesdevicemd"></a>[Стили устройства](device.md)

Xamarin.Forms включает шесть *динамическое* стили, известный как *устройства* стили в [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) класса. Все шесть стили могут применяться к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) только экземпляр.
