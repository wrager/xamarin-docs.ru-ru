---
title: Основные сведения о Xamarin.Forms XAML
description: В этом руководстве объясняется, как начало работы с кросс платформенных XAML для мобильных устройств. XAML позволяет разработчикам определение пользовательских интерфейсов в приложениях Xamarin.Forms, с помощью разметки, а не в коде.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 627267b95bb2d810a60f84c51e38bf5387fe1f99
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245967"
---
# <a name="xamarinforms-xaml-basics"></a>Основные сведения о Xamarin.Forms XAML

XAML (расширяемый язык разметки для приложений) позволяет разработчикам определить пользовательские интерфейсы приложений Xamarin.Forms с помощью разметки вместо кода. В программе Xamarin.Forms XAML никогда не требуется, но часто бывает краток и более визуальной согласованного, чем эквивалентный код и потенциально допускает использование средств разработки. XAML особенно хорошо подходит для использования с популярными архитектуры приложения MVVM (Model-View-ViewModel): код XAML определяет представление, связанного с кодом ViewModel через привязки данных на основе XAML.

## <a name="xaml-basics-contents"></a>Основы содержимого XAML

* [Обзор набора средств Visual Studio для Unity](#Overview)
* [Часть 1. Начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Часть 2. Основной синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Часть 3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Часть 5. Из привязка данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Помимо этих статей основы XAML можно загрузить главах книги [Создание мобильных приложений с помощью Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Обложки книги")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML рассматриваются более подробно в главах много книги, включая:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Глава 7. XAML vs. Код</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">Скачать PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Сводка</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Глава 8. Код и код XAML в согласованную</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">Скачать PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Сводка</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Глава 10. Расширения разметки XAML</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">Скачать PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Сводка</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Глава 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">Скачать PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Сводка</a></td></tr>
</table>

Эти разделы могут быть [для бесплатной загрузки](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Обзор

XAML — это язык на основе XML, созданным корпорацией Майкрософт в качестве альтернативы, чтобы программный код для создания экземпляра и инициализация объектов и упорядочение этих объектов в иерархии родители потомки. XAML внедрены в несколько технологий в .NET framework, однако он обнаружил его максимальной эффективности при определении макета пользовательского интерфейса в Windows Presentation Foundation (WPF), Silverlight, среда выполнения Windows и универсальных приложений Windows Платформы (UWP).

XAML также является частью Xamarin.Forms кросс платформенных изначально основе программный интерфейс для iOS, Android и UWP мобильных устройств. В файле XAML, а разработчик Xamarin.Forms можно определить пользовательские интерфейсы, с помощью всех Xamarin.Forms представления и макетов страниц, как классы, а также пользовательские. XAML-файл может быть скомпилирован или внедрен в исполняемый файл. В любом случае сведения о XAML разбирается во время построения, чтобы найти именованные объекты и еще раз во время выполнения для создания и инициализации объектов и для установления связей между этими объектами и программный код.

XAML имеет несколько преимуществ по сравнению с эквивалентный код:

-  XAML часто является более короткой и удобочитаемыми, чем эквивалентный код.
-  В иерархии родители потомки, присущих XML позволяет XAML имитировать с более высокую четкость visual иерархии "родители потомки" объектов пользовательского интерфейса.
-  XAML может быть легко рукописный программистами, но также решается быть оснащен инструментами и созданный средствами визуальной разработки.

Конечно существует также недостатки в основном относящихся к ограничения, которые являются внутренними для языка разметки.

-  XAML не может содержать код. Все обработчики событий должны быть определены в файле кода.
-  XAML не может содержать циклов для повторной обработки. (Однако несколько визуальных объектов Xamarin.Forms — особенно [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) — можно создать несколько дочерних элементов, в зависимости от объектов в его `ItemsSource` коллекции.)
-  XAML не может содержать условной обработки (тем не менее, привязка данных может ссылаться преобразователь привязки на основе кода, который фактически позволяет некоторые условной обработки).
-  XAML обычно не удается создать классы, которые не определяют конструктор без параметров. (Однако не существует иногда способ обойти это ограничение.)
-  Как правило, в XAML не может вызывать методы. (Опять же, это ограничение можно иногда преодолеть.)

Существует еще не визуальный конструктор для создания XAML в Xamarin.Forms приложений. Все XAML должен быть рукописный, но есть [средство предварительного просмотра XAML](~/xamarin-forms/xaml/xaml-previewer.md). Программистам, создающим новые XAML может возникнуть необходимость часто построение и запуск приложений, особенно после все, что может не подходить очевидно. Даже разработчики богатом опыте в XAML знают, что результативным экспериментов.

XAML по существу представляет собой XML, но некоторые функции уникального синтаксиса XAML. Наиболее важным являются:

- Свойства элементов
- Вложенные свойства
- Расширения разметки

Эти функции являются *не* расширения XML. XAML — полностью юридических XML. Однако эти функции проверки синтаксиса XAML использовать XML уникальным образом. Они подробно обсуждаются в статьях ниже, которые заканчиваются вводные сведения о языке XAML для реализации MVVM.

## <a name="requirements"></a>Требования

В этой статье предполагается ознакомиться с помощью Xamarin.Forms. Чтение [введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) настоятельно рекомендуется.

В этой статье также предполагается Знакомство с XML, а также понять использование объявления пространств имен XML и условия *элемент*, *тега*, и *атрибут*.

Если вы знакомы с Xamarin.Forms и XML, начинается считывание [часть 1. Начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Связанные ссылки

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Создание книги мобильные приложения](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
