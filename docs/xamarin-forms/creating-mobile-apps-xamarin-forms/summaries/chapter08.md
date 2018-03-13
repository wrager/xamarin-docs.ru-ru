---
title: "Сводка Глава 8. Код и код XAML в согласованную"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 452a7835bcb54501edffe7a2467544c6677616ba
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Сводка Глава 8. Код и код XAML в согласованную

В этой главе более глубоко исследует XAML, и особенно кода XAML взаимодействия и.

## <a name="passing-arguments"></a>Передача аргументов

В общем случае класс, созданный в XAML должен иметь открытый конструктор без параметров; результирующий объект инициализируется через параметры свойств. Тем не менее существует два других способа можно создавать экземпляры и инициализации объектов.

Несмотря на то, что это методы общего назначения, они в основном используется в связи с MVVM Просмотр моделей.

### <a name="constructors-with-arguments"></a>Конструкторы с аргументами

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) образце демонстрируется использование `x:Arguments` тег, чтобы задать аргументы конструктора. Эти аргументы должны быть заключены в теги элементов, указывающее тип аргумента. Для базовых типов данных .NET доступны следующие теги:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>Может вызывать методы из XAML

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) образце демонстрируется использование `x:FactoryMethod` элемента, чтобы указать фабричный метод, вызываемый для создания объекта. Такой метод фабрики должен быть открытым и статическим, и его необходимо создать объект типа, в котором он определен. (Например [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) определяет метод, так как он является открытым и статическим и возвращает значение типа `Color`.) Фабричный метод аргументы задаются в `x:Arguments` тегов.

## <a name="the-xname-attribute"></a>Атрибут x: Name

`x:Name` Атрибут позволяет создать его экземпляр в XAML, чтобы задать имя объекта. Правила для этих имен, идентичны представлениям имена переменных в C#. После возврата `InitializeComponent` вызовите в конструкторе, файл кода может ссылаться на эти имена для доступа к соответствующий элемент XAML. Имена, фактически преобразуются средством синтаксического анализа XAML в закрытых полей в созданный разделяемый класс.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) образце показано использование `x:Name` позволяет создать файл кода для сохранения два `Label` элементы, определенные в XAML, обновление с текущей датой и временем.

То же имя не может использоваться для нескольких элементов на одной странице. Это определенной проблемы при использовании `OnPlatform` создания параллельных именованные объекты для каждой платформы. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) демонстрирует более эффективный способ сделать что-нибудь подобное.

## <a name="custom-xaml-based-views"></a>Представления XAML

Существует несколько способов избежать повтора разметки в XAML. Один из распространенных способов является создание нового класса на основе XAML, производный от [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Этот метод показан в [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) образца. `ColorView` Класс является производным от `ContentView` для отображения определенного цвета и его имя, а `ColorViewListPage` класс является производным от `ContentPage` в обычном режиме и явно создает 17 экземпляры `ColorView`.

Доступ к `ColorView` класса в языке XAML требуется другое объявление пространства имен XML, обычно с именем `local` для классов в той же сборке.

## <a name="events-and-handlers"></a>События и обработчики

События могут быть назначены обработчиков событий в XAML, но сам обработчик событий должен быть реализован в файле кода. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) демонстрирует построение пользовательского интерфейса клавиатуры в XAML и реализовать `Clicked` обработчики в файле кода.

## <a name="tap-gestures"></a>Коснитесь жесты

Любой `View` объекта можно получить сенсорный ввод и создают события на основе этих данных. `View` Класс определяет [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) свойство коллекции, который может содержать один или несколько экземпляров классов, производных от [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/).

[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Приводит к возникновению ошибки [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) события. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) программы показано, как присоединить `TapGestureRecognizer` объектов до четырех `BoxView` элементы должны быть созданы имитация игры:

[![Снимок экрана тройной monkey отвода](images/ch08fg07-small.png "игры имитации")](images/ch08fg07-large.png#lightbox "имитации игры")

Но **MonkeyTap** звук действительно необходимы программе. (См. [следующей главе](chapter09.md).)



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 8 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Образцы Глава 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Образец главы 8 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
