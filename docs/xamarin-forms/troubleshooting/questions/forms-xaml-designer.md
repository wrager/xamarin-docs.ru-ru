---
title: Почему не работает в конструкторе Visual Studio XAML для Xamarin.Forms XAML-файлов
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 1f82f16429ca23a4ba6806f775310dd90126096e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему не работает в конструкторе Visual Studio XAML для Xamarin.Forms XAML-файлов

Xamarin.Forms не в настоящее время поддерживает визуальные конструкторы для XAML-файлов. По этой причине при попытке открыть файл XAML формы в среде Visual Studio *конструктора пользовательского интерфейса XAML* или *пользовательского интерфейса конструктора XAML с кодировкой*, выдается следующее сообщение об ошибке:

> «Невозможно открыть файл с помощью выбранного редактора. Выберите другой редактор.»

Это ограничение описан в [Обзор](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) раздел [основы XAML Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md) руководства:

> «Не существует, пока визуальный конструктор для создания XAML в Xamarin.Forms приложений, поэтому вся XAML должен быть рукописный».
