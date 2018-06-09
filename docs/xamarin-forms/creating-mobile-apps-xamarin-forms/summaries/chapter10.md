---
title: Сводка Глава 10. Расширения разметки XAML
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка Глава 10. Расширения разметки XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cc6c3154b7e6535fa7528032fb7a91ad90a0a7f8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241104"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Сводка Глава 10. Расширения разметки XAML

Как правило, средство синтаксического анализа XAML преобразует любую строку, заданную в качестве значения атрибута к типу свойство, основанное на стандартные преобразования для базовых типов данных .NET, или [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) производный класс присоединенное свойство или его тип [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

Однако иногда бывает удобно для задания атрибута из другого источника, например, элемент в словарь, или значение статического свойства или поля, или из расчета определенного рода.

Это задание из *расширение разметки XAML*. Несмотря на название, расширения разметки XAML — *не* расширение XML. XAML всегда является допустимым XML.

## <a name="the-code-infrastructure"></a>Код инфраструктуры

Расширение разметки XAML — это класс, реализующий [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) интерфейса. Такой класс часто имеет слово `Extension` в конце имени но обычно отображается в XAML без этого суффикса.

Все реализации XAML поддерживает следующие расширения разметки XAML.

- `x:Static` Поддерживается [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` Поддерживается [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` Поддерживается [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` Поддерживается [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` Поддерживается [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Эти четыре расширения разметки XAML, поддерживаются многие реализации XAML, включая Xamarin.Forms:

- `StaticResource` Поддерживается [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` Поддерживается [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` поддерживаемые [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;подробно [Глава 16. Привязка данных](#chapter16)
- `TemplateBinding` поддерживаемые [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;Непокрытые в книге

Дополнительные расширения разметки XAML включается в Xamarin.Forms в связи с [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;Непокрытые в книге

## <a name="accessing-static-members"></a>Доступ к статическим членам.

Используйте [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) элемент атрибута присваивается значение открытого статического члена свойства, поля или перечисления. Задать [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) свойство статический член. Обычно проще указать `x:Static` и имя элемента в фигурные скобки. Имя `Member` свойства необязательно должен быть включен, сам элемент. Этот общий синтаксис показан в [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) образца. Статические поля сами определяются в [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) класса. Этот метод позволяет устанавливать константы, используемые в программе.

С помощью дополнительных XML-декларацию пространства имен, можно ссылаться открытые статические свойства, поля или перечисления элементов, определенных в .NET framework, как показано в [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) образца .

## <a name="resource-dictionaries"></a>Словари ресурсов

`VisualElement` Класс определяет свойство с именем [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) , можно задать для объекта типа [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). В XAML, можно сохранять элементы в этом словаре и идентифицировать их с `x:Key` атрибута. Элементы, хранящиеся в словаре ресурсов являются общими для всех ссылок на элемент.

### <a name="staticresource-for-most-purposes"></a>StaticResource для большинства целей

В большинстве случаев вы будете использовать [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) расширения разметки для ссылки на элемент из словаря ресурсов, как показано в предыдущем [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) образца . Можно использовать `StaticResourceExtension` элемент или `StaticResource` заключены в фигурные скобки:

[![Снимок экрана: три общий доступ к ресурсам](images/ch10fg03-small.png "общий доступ к ресурсам")](images/ch10fg03-large.png#lightbox "общий доступ к ресурсам")

Не следует путать `x:Static` расширения разметки и `StaticResource` расширения разметки.

### <a name="a-tree-of-dictionaries"></a>Дерево словарей

Когда средство синтаксического анализа XAML обнаруживает `StaticResource`, он начинает поиск вверх по визуальному дереву ключ на соответствие и затем просматривает `ResourceDictionary` в приложении `App` класса. Это позволяет элементов в словаре ресурсов глубже в визуальном дереве для переопределения в визуальном дереве выше словарь ресурсов. Это показано в [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) образца.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource для особых целей

`StaticResource` Расширение разметки приводит к тому элемента должно быть извлечено из словаря, когда визуальное дерево строится во время `InitializeComponent` вызова. Это альтернатива `StaticResource` — [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), который хранит ссылку на ключ словаря и обновляет целевой объект, когда элемент ссылается ключевых изменений.

Разница между `StaticResource` и `DynamicResource` демонстрируется [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) образца.

Свойства, заданные `DynamicResource` должны быть основаны на привязываемые свойства, как описано в [главе 11, привязываемых инфраструктуры](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Расширения разметки, используется меньшее

Используйте [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) расширение разметки, чтобы задать свойство `null`.

Используйте [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) расширение разметки, чтобы задать для свойства .NET `Type` объекта.

Используйте [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) определить массив. Укажите тип элементов массива, задав [`Type`] свойства `x:Type` расширения разметки.

## <a name="a-custom-markup-extension"></a>Пользовательское расширение разметки

Можно создать собственные расширения разметки XAML, написав класс, реализующий [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) взаимодействовать с [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) метод.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Класса соответствует их требованиям. Она создает значение типа `Color` на основе значений свойств с именем `H`, `S`, `L`, и `A`. Этот класс является первым в библиотеку Xamarin.Forms [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) , создавать и использовать в ходе этой книги.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) образце показано, как ссылаться на эту библиотеку и использовать пользовательское расширение разметки.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 10 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Образцы Глава 10](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
