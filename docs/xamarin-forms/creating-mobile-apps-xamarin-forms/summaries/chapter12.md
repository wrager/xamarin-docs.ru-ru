---
title: Сводка Глава 12. Стили
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bb26c5d93bc9945ad43ed62d7feba2bc851e510e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-12-styles"></a>Сводка Глава 12. Стили

В Xamarin.Forms стили позволяют несколько представлений для совместного использования набор свойств. Это уменьшает разметки и обеспечивает обслуживание согласованное визуальные темы.

Стили почти всегда определяются и используются в разметке. Объект типа [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) создается в словаре ресурсов и затем установите значение [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) визуальных элементов с помощью свойства `StaticResource` или `DyanamicResource` разметки расширение.

## <a name="the-basic-style"></a>Основной стиль

Объект `Style` требует его [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) быть задан тип, он применяется для визуального объекта. Когда `Style` создается в словаре ресурсов (как это часто бывает) необходимо также `x:Key` атрибута.

`Style` Имеет свойство содержимого типа [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/), которое является коллекцией из [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) объектов. Каждый `Setter` связывает [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) с [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/).

В языке XAML `Property` имя свойства среды CLR (например, `Text` свойство `Button`), но со стилем свойства должны быть основаны на привязываемые свойства. Кроме того, должно быть определено свойство в классе, обозначенном `TargetType` Установка или наследуемые этим классом.

Можно указать `Value` с помощью элемента свойства `<Setter.Value>`. Это позволяет задать `Value` на объект, не могут быть выражены в текстовой строке, а также к `OnPlatform` объект или на объект создан с помощью `x:Arguments` или `x:FactoryMethod`. `Value` Может также быть установлено с `StaticResource` выражения в другой элемент в словаре.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) программы показан основной синтаксис и показано, как ссылаться на `Style` с `StaticResource` расширения разметки:

[![Снимок экрана тройной основной стиль](images/ch12fg01-small.png "основные стили")](images/ch12fg01-large.png#lightbox "основные стили")

`Style` Объект и все объекты, созданные в `Style` объекта в виде `Value` параметр являются общими для всех представлений, ссылаясь на него `Style`. `Style` Не может содержать никаких действий, который нельзя использовать совместно, такие как `View` производный класс.

Невозможно задать обработчики событий `Style`. `GestureRecognizers` Свойство не может быть установлено в `Style` , так как он не поддерживается привязываемые свойства.

## <a name="styles-in-code"></a>Стили в коде

Несмотря на то, что не так часто, можно создать и инициализировать `Style` объектов в коде. Это продемонстрировано на [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) образца.

## <a name="style-inheritance"></a>Наследование стилей

`Style` имеет [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) , можно задать для свойства `StaticResource` расширения разметки, ссылающиеся на другой стиль. Это позволяет стили, которые наследуют от предыдущего стили при добавлении или удалении параметров свойств. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) примере демонстрируется это.

Если `Style2` на основе `Style1`, `TargetType` из `Style2` должен быть таким же, как `Style1` или производный от `Style1`. Словарь ресурсов, в котором `Style1` хранится должен быть того же словарь ресурсов, как `Style2` или более поздней версии в визуальном дереве словарь ресурсов.

## <a name="implicit-styles"></a>Неявные стили

Если `Style` в ресурсе словаря имеет `x:Key` значениями атрибута, ей назначается ключ словаря автоматически и `Style` объект становится *неявный стиль*. Вид без `Style` параметр, тип которых соответствует `TargetType` точно найти этот стиль как [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) образец демонстрирует.

Неявный стиль могут быть производными от `Style` с `x:Key` параметр, но не наоборот. Неявный стиль нельзя сослаться явно.

Можно реализовать три типа иерархии со стилями и `BasedOn`:

- Из стили, определенные на `Application` и `Page` до стили, определенные на макете более низкого уровня в визуальном дереве.
- Из стили, определенные для базовых классов, таких как `VisualElement` и `View` для стили, определенные для конкретных классов.
- Из стили, явную словарь ключей неявных стилей.

Демонстрируются эти иерархии в [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) образца.

## <a name="dynamic-styles"></a>Динамические стили

Позволяет ссылаться на стиль в словаре ресурсов `DynamicResource` вместо `StaticResource`. Это делает стиль *динамического стиля*. Если этот стиль заменяется в словаре ресурсов другой стиль с тем же ключом, представления, ссылающиеся на этот стиль с `DynamicResource` автоматического изменения. Кроме того, отсутствие записи с указанным ключом в словаре приведет к `StaticResource` для вызова исключения, но не `DynamicResource`.

Этот метод можно использовать динамически изменять стили и темы как [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) демонстрирует образец.

Однако нельзя задать `BasedOn` свойства `DynamicResource` состава расширения из-за `BasedOn` не поддерживается с помощью привязки свойства. Для динамического формирования стиля, не устанавливайте `BasedOn`. Вместо этого задайте [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) свойства ключ словаря требуется наследовать стиль. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) этот метод продемонстрирован в примере.

## <a name="device-styles"></a>Стили устройства

[ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Вложенный класс определяет двенадцать статические поля только для чтения для шести стили с `TargetType` из `Label` , можно использовать для распространенных типов использований текста.

Шесть эти поля относятся к типу `Style` , можно задать непосредственно в `Style` свойства в коде:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

Шесть полей имеют тип `string` и может использоваться в качестве ключей словаря для динамические стили:

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) равно «BodyStyle»
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) равно «TitleStyle»
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) равно «SubtitleStyle»
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) равно «CaptionStyle»
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) равно «ListItemTextStyle»
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) равно «ListItemDetailTextStyle»

Эти стили проиллюстрированы [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) образца.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 12 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Образцы Глава 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Стили](~/xamarin-forms/user-interface/styles/index.md)
