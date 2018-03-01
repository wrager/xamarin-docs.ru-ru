---
title: "Почему не работает в конструкторе Visual Studio XAML для Xamarin.Forms XAML-файлов"
ms.topic: article
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fae0792b6db940bb8b4aa4772eb0b5bb42b78da1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему не работает в конструкторе Visual Studio XAML для Xamarin.Forms XAML-файлов

Xamarin.Forms не в настоящее время поддерживает визуальные конструкторы для XAML-файлов. По этой причине при попытке открыть файл XAML формы в среде Visual Studio *конструктора пользовательского интерфейса XAML* или *пользовательского интерфейса конструктора XAML с кодировкой*, выдается следующее сообщение об ошибке:

> «Невозможно открыть файл с помощью выбранного редактора. Выберите другой редактор.»

Это ограничение описан в [Обзор](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) раздел [основы XAML Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md) руководства:

> «Не существует, пока визуальный конструктор для создания XAML в Xamarin.Forms приложений, поэтому вся XAML должен быть рукописный».
