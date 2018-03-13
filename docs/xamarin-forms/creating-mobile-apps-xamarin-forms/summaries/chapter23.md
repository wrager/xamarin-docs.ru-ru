---
title: "Сводка по 23 главы. Триггеры и поведения"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9a53cddcf216efd2bb86c838e280d599ff26c191
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Сводка по 23 главы. Триггеры и поведения

Триггеры и поведения похожи, в том, что оба они предназначены для использования в XAML-файлов для упрощения взаимодействия элемент за пределами использование привязок данных и для расширения функциональности элементов XAML. С объектами пользовательского интерфейса visual почти всегда используются триггеры и поведения.

Для поддержки триггеры и поведение, как `VisualElement` и `Style` поддерживает два свойства коллекции:

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) и [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) типа `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) и [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) типа `IList<Behavior>`

## <a name="triggers"></a>Триггеры

Триггер — это условие (изменения свойства или вызов события, события), приведет к появлению ответа (другой изменения свойства или выполнение некоторых кода). `Triggers` Свойство `VisualElement` и `Style` относится к типу `IList<TriggersBase>`. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) — Это абстрактный класс, от которого наследуют четыре запечатанных классов:

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) для ответов на основе изменений свойств
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) для ответов в зависимости от обработки событий
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) для ответов в зависимости от привязок данных
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) для ответов на основе нескольких триггеров

Триггер всегда задается в элементе, свойство которого изменяется триггером.

### <a name="the-simplest-trigger"></a>Простейший триггера

[ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) Класс проверяет при изменении значения свойства и отвечает, устанавливая другого свойства этого элемента.

`Trigger` определяет три свойства:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) типа `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) типа `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) Тип `IList<SetterBase>`, свойство содержимого `Trigger`

Кроме того `Trigger` требует, чтобы следующее свойство наследуется от `TriggerBase` задать:

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) Чтобы указать тип элемента, на котором `Trigger` присоединен

`Property` И `Value` составляют условие и `Setters` коллекции представляет собой ответ. При указанной строке `Property` имеет значением, полученным путем `Value`, то `Setter` объекты в `Setters` применяются коллекции. Когда `Property` имеет другое значение, переключатели, удаляются. `Setter` Определяет два свойства, которые являются одинаковыми как первые два свойства `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) типа `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) типа `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) образце показано, как `Trigger` применены к `Entry` можно увеличить размер `Entry` через `Scale` свойство при `IsFocused`свойство `Entry` — `true`.

Несмотря на то, что не так часто, `Trigger` можно задать в коде, как [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) демонстрирует образец.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) образце показано, как `Trigger` можно задать в `Style` для применения к нескольким `Entry` элементов.

### <a name="trigger-actions-and-animations"></a>Действия триггера и анимации

Можно также запустить небольшой код, основанный на триггер. Этот код может быть анимации, которая обращается к свойству. Обычным способом является использование [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), который определяет два свойства:

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) Тип `string`, имя события
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) Тип `IList<TriggerAction>`, список действий, которое запускается в ответ.

Для этого необходимо создать класс, производный от [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), обычно `TriggerAction<VisualElement>`. В этом классе можно определить свойства. Они простых свойств среды CLR, а не свойства связывания поскольку `TriggerAction` не является производным от `BindableObject`. Необходимо переопределить [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) метод, вызываемый при вызове действия. Аргумент является целевой элемент.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки приведен пример. Он вызывает `ScaleTo` анимируемое свойство `Scale` свойства элемента. Так как одна из его свойств типа `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) класс позволяет использовать стандартные `Easing` статических полей в XAML.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) образец демонстрирует способ вызова `ScaleAction` из `EventTrigger` объектов, которое ведет наблюдение за `Focused` и `Unfocused` события.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) образце показан способ определения пользовательской функции для реалистичной анимации `ScaleAction` в файле кода.

Также можно вызывать действия с помощью `Trigger` (как distinguished из `EventTrigger`). Это требуется, если известно, `TriggerBase` определяет две коллекции:

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) типа `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) типа `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) образце демонстрируется использование этих коллекций.

### <a name="more-event-triggers"></a>Несколько триггеров событий

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) вызовы библиотеки `ScaleTo` дважды для масштабирования вверх и вниз. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) образец использует это в элемент `EventTrigger` для предоставления пользователю при `Button` нажата. Двойные анимации можно также с помощью двух действий в коллекции с типом [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Класса в **Xamarin.FormsBook.Toolkit** библиотеки задаются shiver настраиваемые действия. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) образец демонстрирует его.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Класса в **Xamarin.FormsBook.Toolkit** библиотеки будет ограничен `Entry` элементы и наборы `TextColor` свойства красным, если `Text` свойство не является `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) образец демонстрирует его.

### <a name="data-triggers"></a>Триггеры данных

[ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) Аналогичен `Trigger` за исключением того, что вместо мониторинг свойство для изменения значения, он отслеживает привязки данных. Таким образом, свойство в один элемент для изменения свойства в другой элемент.

`DataTrigger` определяет три свойства:

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) типа `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) типа `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) типа `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) образец требует [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) библиотеки и наборы цвета имена учащихся на синий или на основе розовый `Sex` свойство:

[![Тройной экрана цветов Пол](images/ch23fg04-small.png "цвета Пол")](images/ch23fg04-large.png#lightbox "Пол цветов")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) образцы наборов `IsEnabled` свойство `Entry` для `False` Если `Length` свойство `Text` свойство `Entry`равен 0. Обратите внимание, что `Text` свойство инициализируется в пустую строку, по умолчанию — `null`и `DataTrigger` не будет работать неправильно.

### <a name="combining-conditions-in-the-multitrigger"></a>Объединение условий в MultiTrigger

[ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) — Это набор условий. Когда они находятся все `true`, то применяются задания. Этот класс определяет два свойства:

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) типа `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) типа `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) класс является абстрактным и имеет два дочерних классов:

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/), который имеет [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) и [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) такие свойства, как `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/), который имеет [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) и [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) такие свойства, как `DataTrigger`

В [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) образце `BoxView` окрашивается только в том случае, если четыре `Switch` элементы включены.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) образце показано, как сделать `BoxView` цвет при *любой* из четырех `Switch` элементы включены. Это требует закон De Morgan и обращения всю логику приложения.

Объединение и и или логику не так просто и обычно требует невидимой `Switch` элементы для промежуточных результатов. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) образце показано, как `Button` можно включить, если любой из двух `Entry` элементы имеют некоторые текста, введенного в, но не в том случае, если обе они некоторые при вводе текста на.

## <a name="behaviors"></a>поведения

Все действия триггера, это также можно сделать с поведением, но поведения всегда требуется класс, производный от [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) и переопределяет следующие два метода:

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

Аргумент является элементом, применяется поведение. Как правило `OnAttachedTo` метод присоединяет обработчики событий, и `OnDetachingFrom` отсоединяет их. Так как такой класс обычно сохраняет какое-либо состояние, он обычно не может передаваться в `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) пример аналогичен **TriggerEntryValidation** за исключением того, что он использует поведение & #x 2014; [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки.

### <a name="behaviors-with-properties"></a>Поведение с помощью свойства

`Behavior<T>` является производным от `Behavior`, который является производным от `BindableObject`, поэтому привязываемые свойства могут быть определены в поведении. Эти свойства могут быть активны в привязки данных.

Это показано в [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) использование программа, которая предоставляет [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) класса в [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки. `ValidEmailBehavior` Содержит привязываемые свойства только для чтения и служит в качестве источника в привязки данных.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) образце используется таким же образом для отображения индикатора, чтобы подтвердить правильность адреса электронной почты другого типа.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) пример представлен вариант предыдущего примера. Использует ButtonGlide `DataTrigger` в сочетании с этого поведения.

### <a name="toggles-and-check-boxes"></a>Переключатели и флажки

Можно инкапсулировать поведение выключатель в классе, такие как [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки, а затем определите все визуальные элементы для переключателя полностью в XAML.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) примере используются `ToggleBehavior` с `DataTrigger` использовать `Label` две текстовые строки для переключателя.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) Образец реализует эту концепцию путем переключения между двумя `FormattedString` объектов.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Класса в **Xamarin.FormsBook.Toolkit** библиотека является производным от `ContentView`, определяет `IsToggled` свойство и включает в себя `ToggleBehavior` для переключателя логика. Это упрощает определение выключателя в XAML, как показано в предыдущем [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) образца.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) включает [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) класс, производный от `ToggleBase` и использует [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)класс, чтобы создать выключатель, который напоминает Xamarin.Forms `Switch`.

Объект [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) в **Xamarin.FormsBook.Toolkit** предоставляет анимации для предоставления анимированных уровень в [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)образца.

### <a name="responding-to-taps"></a>Реакция на касания

Один недостаток `EventTrigger` является невозможно присоединить его к `TapGestureRecognizer` реагировать на нажатия. Получение решения этой проблемы является назначение [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) в [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) примере используются `TapBehavior` использовать раннюю `ShiverAction` для касании `BoxView` элементов.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) образце показано, как можно избавиться от разметку, инкапсулируя `ShiverView` класса.

### <a name="radio-buttons"></a>Переключатели

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека также имеет [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) класс для создания переключателей, по которому группируются `string` имя группы.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) программа использует текстовые строки для его переключателя. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) примере используются `Style` разницу в виде между checked и unchecked кнопок. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) образец использует процессор изображения для его переключатели:

[![Тройной экрана образов переключатель](images/ch23fg17-small.png "изображения кнопки переключатель")](images/ch23fg17-large.png#lightbox "изображения кнопок переключателей")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) образец рисует традиционных отображения объектов переключателей с точкой внутри окружности.

### <a name="fades-and-orientation"></a>Переходы и ориентации

Окончательный образца [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) позволяет переключаться между представлениями другой цвет, с помощью переключателей. Три представления затухание и выхода из системы с помощью [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки.

Программа также реагирует на изменения в ориентации между книжной и альбомной с помощью [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) в **Xamarin.FormsBook.Toolkit** библиотеки.



## <a name="related-links"></a>Связанные ссылки

- [Глава 23 полный текст (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Образцы Глава 23](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Работа с триггерами](~/xamarin-forms/app-fundamentals/triggers.md)
- [Реакции на событие Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
