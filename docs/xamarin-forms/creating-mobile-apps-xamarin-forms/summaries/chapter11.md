---
title: "Сводка главе 11. Инфраструктура связывания"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ccae97021e86eb1375f948c5ad126253c6088037
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Сводка главе 11. Инфраструктура связывания

Каждый программированию на C# знакомы с C# *свойства*. Свойства содержат *задать* доступа и/или *получить* метода доступа. Они часто называются *свойства CLR* для среды CLR.

Xamarin.Forms определяет определение расширенные свойства с именем *привязываемые свойства* инкапсулируется [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) класса и поддерживается [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)класса. Эти классы являются связанных, но довольно разные: `BindableProperty` используется для задания свойства. `BindableObject` подобно `object` в том, что он является базовым классом для классов, которые определяют свойства для привязки.

## <a name="the-xamarinforms-class-hierarchy"></a>Иерархия классов Xamarin.Forms

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) образец использует отражение для отображения иерархии класса Xamarin.Forms и продемонстрируйте, ключевую роль, которую играют `BindableObject` в этой иерархии. `BindableObject` является производным от `Object` и является родительским классом для [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) откуда [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) является производным. Это является родительским классом для [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) и [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), который является родительским классом для [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![Тройной экрана иерархию классов для управления доступом](images/ch11fg01-small.png "Иерархия класса для управления доступом")](images/ch11fg01-large.png#lightbox "Иерархия класса для управления доступом")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Считывания в BindableObject и BindableProperty

В классах, производных от `BindableObject` многие свойства CLR, называются «поддерживаться» привязываемые свойства. Например [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойство `Label` класс является свойство CLR, но `Label` класс также определяет открытое статическое доступное только для чтения поле с именем [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) из Тип `BindableProperty`.

Приложение может задать или получить `Text` свойство `Label` как правило, или приложение может задать `Text` путем вызова [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метода, определенного `BindableObject` с `Label.TextProperty` аргумент. Аналогичным образом приложение может получить значение `Text` свойство путем вызова [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) метод, с `Label.TextProperty` аргумент. Это продемонстрировано на [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) образца.

На самом деле `Text` свойство CLR полностью реализуется с использованием `SetValue` и `GetValue` методов, определенных `BindableObject` в сочетании с `Label.TextProperty` статическое свойство.

`BindableObject` и `BindableProperty` обеспечивают поддержку:

- Предоставление значения свойств по умолчанию
- Хранение их текущие значения
- Предоставляет механизмы для проверки значений свойств
- Обеспечение согласованности между связанные свойства в одном классе
- Реагирование на изменения свойств
- Запуска уведомления, когда свойство является изменение или был изменен
- Поддержка привязки данных
- Поддержка стили
- Динамические ресурсы поддержки

При изменении свойства, поддерживаемый изменении привязываемые свойства `BindableObject` активируется [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) событий, определяющий свойство, которая была изменена. Это событие не возникает, когда задано то же значение.

Некоторые свойства не поддерживаются некоторые Xamarin.Forms классы и свойства для привязки &mdash; например `Span` &mdash; не являются производными от `BindableObject`. Только класс, производный от `BindableObject` может поддерживать привязываемыми свойствами, так как `BindableObject` определяет `SetValue` и `GetValue` методы.

Поскольку `Span` не является производным от `BindableObject`, ни один из его свойств &mdash; например `Text` &mdash; , поддерживаемых привязываемые свойства. Именно поэтому `DynamicResource` на `Text` свойство `Span` вызывает исключение в [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) примера в предыдущем разделе. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) образце показано, как задать динамические ресурсы в коде с помощью [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) метода, определенного `Element`. Первым аргументом является объектом типа `BindableProperty`.

Аналогичным образом [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) метода, определенного `BindableObject` первый аргумент типа `BindableProperty`.

## <a name="defining-bindable-properties"></a>Определение свойства для привязки

Можно определить собственные привязываемые свойства с помощью статического [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) метода или немного более короткой [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) перегрузку, чтобы создать статического поля только для чтения типа `BindableProperty`.

Это показано в [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки. Класс является производным от `Label` и позволяет указать размер шрифта в пунктах. Демонстрируется в [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) образца.

Четыре аргументы `BindableProperty.Create` метод требуются:

- `propertyName`: имя текстового свойства (то же, как и имя свойства CLR)
- `returnType`: тип свойства среды CLR
- `declaringType`: тип класса, объявление свойства
- `defaultValue`: значение свойства по умолчанию

Поскольку `defaultValue` относится к типу `object`, компилятор должен уметь определить тип значения по умолчанию. Например если `returnType` — `double`, `defaultValue` должно быть равно примерно 0,0, а не только 0 или несоответствие типов будет вызвано исключение во время выполнения.

Это очень часто привязываемые свойства включают:

- `propertyChanged`: статический метод, вызываемый при изменении значения свойства. Первый аргумент — это экземпляр класса, свойство которого было изменено.

Аргументы для `BindableProperty.Create` не так часто:

- `defaultBindingMode`: используется при привязке данных (как описано в [ **Глава 16. Привязка данных**](chapter16.md))
- `validateValue`: обратный вызов для проверки допустимое значение
- `propertyChanging`: обратный вызов для указания, если свойство не будет изменена
- `coerceValue`: обратный вызов для присвоения значения набора с другим значением
- `defaultValueCreate`: обратный вызов для создания значения по умолчанию, не может совместно использоваться экземплярами класса (например, коллекция)

### <a name="the-read-only-bindable-property"></a>Привязываемые свойства только для чтения

Привязываемые свойства может быть только для чтения. Создание связываемой свойство только для чтения требует вызова статического метода [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) или более коротким [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) для определения частного статического поля только для чтения типа [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

Затем определите свойство CLR `set` accesor как `private` для вызова [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) перегрузка с `BindablePropertyKey` объекта. Это предотвращает свойство устанавливается за пределами класса.

Это показано в [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) класс, используемый в [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) образца.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст главе 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Образцы главе 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
