---
title: Сводка главе 9. Вызовы API конкретной платформы
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка главе 9. Вызовы API конкретной платформы'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 719f075ada576f87d4533697209deedcbb7003c4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239933"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Сводка главе 9. Вызовы API конкретной платформы

Иногда бывает необходимо запускать код, который зависит от платформы. В этой главе рассматриваются методы.

## <a name="preprocessing-in-the-shared-asset-project"></a>Предварительная обработка в проекта общих ресурсов

Общий Xamarin.Forms активов проект может выполняться другой код для каждой платформы, с помощью директивы препроцессора C# `#if`, `#elif`, и `endif`. Это показано в [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Снимок экрана тройной переменной форматирования абзаца](images/ch09fg01-small.png "модель устройства и операционной системы")](images/ch09fg01-large.png#lightbox "модель устройства и операционной системы")

Однако результирующий код может быть сложный и сложной для чтения.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Параллельные классов в проекта общих ресурсов

Демонстрируется более структурированный способ выполнения кода для конкретной платформы в SAP в [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) образца. Проекты каждой из платформ содержит класс с таким же именем и методы, но реализованы для данной конкретной платформы. SAP просто создает экземпляр класса и вызывает метод.

## <a name="dependencyservice-and-the-portable-class-library"></a>Помощью DependencyService и переносимой библиотеки классов

Библиотеки не может получить доступ к обычно классы в проектах приложений. Вероятно, это ограничение для предотвращения метод, описанный в **PlatInfoSap2** не могут использоваться в PCL. Однако Xamarin.Forms содержит класс с именем [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) , использует отражение .NET к открытых классов в проекте приложения со переносимой библиотеке Классов.

Необходимо определить PCL `interface` с элементами, которые нужны для работы в каждой платформы. Затем в каждой из платформ содержит реализацию этого интерфейса. Класс, реализующий интерфейс должен быть идентифицирован с [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) на уровне сборки.

PCL затем использует универсальный [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) метод `DependencyService` для получения экземпляра класса платформы, который реализует интерфейс.

Это показано в [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) образца.

## <a name="platform-specific-sound-generation"></a>Создание звуковой платформой

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) пример добавляет сигналы **MonkeyTap** программы путем обращения к функции создания звук в каждой платформы.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст главе 9 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Образцы главе 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Службы зависимостей](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
