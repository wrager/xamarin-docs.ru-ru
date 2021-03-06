---
title: Анимация в Xamarin.Forms
description: Xamarin.Forms включает собственную инфраструктуру анимации, прост для создания простой анимации, при этом гибким, для создания сложных анимаций.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 5bc04f638168a10266c20e278481fc0c513afe48
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245030"
---
# <a name="animation-in-xamarinforms"></a>Анимация в Xamarin.Forms

_Xamarin.Forms включает собственную инфраструктуру анимации, прост для создания простой анимации, при этом гибким, для создания сложных анимаций._

Классы анимации Xamarin.Forms целевых различных свойств визуальных элементов, обычно анимации, все изменения свойства от одного значения на другой за период времени. Обратите внимание, что интерфейс не XAML для классов анимации Xamarin.Forms. Тем не менее, могут найти анимации в [поведения](~/xamarin-forms/app-fundamentals/behaviors/index.md) и затем ссылаться на нее из XAML.

## <a name="simple-animationssimplemd"></a>[Простые анимации](simple.md)

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Класс предоставляет методы расширения, которые могут использоваться для создания простой анимации, поворот, масштабирование, преобразования и затухание [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) экземпляров. В этой статье демонстрируется создание и Отмена анимации с помощью `ViewExtensions` класса.

## <a name="easing-functionseasingmd"></a>[Функции плавности](easing.md)

Включает Xamarin.Forms [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) класс, который позволяет указать функцию передачи, которая определяет способ ускорить анимации или замедлять работу, как они работают. В этой статье показано, как использовать предварительно определенные функции плавности и создание функции плавности.

## <a name="custom-animationscustommd"></a>[Пользовательские анимации](custom.md)

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Класса ― это средство, все анимации Xamarin.Forms, с помощью методов расширения в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) создание одного или нескольких классов `Animation` объектов. В этой статье демонстрируется использование `Animation` класс для создания и Отмена анимации, синхронизация нескольких анимаций и создание анимации, анимация свойства, которые не являются анимированы с существующие методы анимации.
