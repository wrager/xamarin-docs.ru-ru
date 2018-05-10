---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF и. Xamarin.Forms: Сходства и различия'
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 21ffca65ee72308d1340a1db43471228b2adbe91
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF и. Xamarin.Forms: Сходства и различия

## <a name="control-templates"></a>Шаблоны элементов управления

WPF поддерживает концепцию *шаблонов элементов управления* обеспечена визуализации для элемента управления (`Button`, `ListBox`и т. д.). Как упоминалось выше, Xamarin.Forms использует конкретный _отрисовки_ это классы, которые взаимодействуют с платформой собственного (iOS, Android, т. д.), для визуализации элемента управления.

Тем не менее Xamarin.Forms _does_ имеют `ControlTemplate` тип — используется для использования тем `Page` объектов. Он предоставляет определения для `Page` , предоставляет согласованный содержимое, но позволяет пользователю страницы изменить цвета, шрифты, т. д. и даже добавлять элементы, чтобы сделать его уникальным для приложения.

Общие способы использования для этого являются действия, такие как диалоговые окна проверки подлинности, запросы, а также для предоставления типовых, но тем страницы внешний вид и поведение, можно настроить в приложении. В рамках этой поддержки используются многие знакомые с именем WPF элементы управления.

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Однако важно знать, что они _не_ обслуживании той же цели в Xamarin.Forms. Дополнительные сведения о данной функции см. [страницы документации](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML используется как декларативный язык разметки для WPF и Xamarin.Forms. В большинстве случаев синтаксис совпадает - основное различие заключается в объекты, определенные или создан на графиках XAML.

- Поддерживает Xamarin.Forms [спецификации XAML 2009](/dotnet/framework/xaml-services/xaml-2009-language-features/); это упрощает определение данных, таких как `string`s, `int`s, т. д. так как определение универсальных типов и передача аргументов в конструкторы.

- Сейчас нет способа загрузки dyanmically XAML как WPF с `XamlReader`. Можно получить те же функциональные с [пакет NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) на то, что.

### <a name="markup-extensions"></a>Расширения разметки

Xamarin.Forms поддерживает расширение XAML посредством расширения разметки, так же как WPF. По умолчанию он имеет одинаковые основные стандартные блоки:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Кроме того, он включает `{x:Reference}` из спецификации XAML 2009 и `{TemplateBinding}` расширения разметки, используемой для специальной версией оператора `ControlTemplate` поддерживается Xamarin.Forms.

> [!WARNING]
> `ControlTemplate` Поддержки отличается - даже если он имеет то же имя.

Xamarin.Forms поддерживает также - расширений собственной разметки, но реализация немного отличается. В WPF, должен быть производным от `MarkupExtension` -абстрактный базовый класс. В Xamarin.Forms, который будет заменен интерфейсом `IMarkupExtension` или `IMarkupExtension<T>` которого является более гибкой.

Так же, как WPF, имеет один обязательный метод `ProvideValue` метод для возврата значения из расширения разметки.

## <a name="binding-infrastructure"></a>Инфраструктура привязок

Одно из основных понятий переносятся — это инфраструктура привязки данных для подключения визуальных свойств для свойств данных .NET. Это позволяет MVVM архитектурные шаблоны. Основные принципы проектирования идентичен — иметь привязываемых базовый класс [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), в WPF, это [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) класса. Этот базовый класс используется как корневой предка для всех объектов, которые будут участвовать в качестве целевых объектов в привязке данных. Производные классы, то предоставлять [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) объектов, которые действуют как резервного хранилища для значений свойств (они определяются как [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) объектов в WPF).

### <a name="defining-bindable-properties"></a>Определение свойства для привязки

Определение для связывания свойства в Xamarin.Forms не WPF.
1. Объект должен быть производным от `BindableObject`.
2. Должен быть открытым статическим полем типа `BindableProperty` объявить, чтобы определить ключ резервного хранилища для свойства.
3. Должно быть оболочку свойств открытого экземпляра, которая использует `GetValue` и `SetValue` для извлечения и измените значение свойства.

Полный пример см. в разделе [свойства связывания в Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Вложенные свойства

Вложенные свойства являются подмножеством привязываемые свойства, и они работают так же, как в WPF. Основное различие заключается в что оболочки свойства не задано в этом случае и заменены набор статических get и set методы в класс-владелец. В разделе [вложенные свойства в Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) для получения дополнительной информации.

### <a name="using-the-binding-engine"></a>С помощью механизма привязки

Процесс использования механизм привязки одинаково как в WPF. Он может использоваться в коде путем создания `Binding` объектов, связанных с исходным объектом (любой тип .NET) и значение необязательного свойства (если не указано, он считает исходного объекта самого - свойства так же, как WPF). Затем можно использовать `SetBinding` для какого-либо `BindableObject` для создания привязки к `BindableProperty`.

Кроме того, можно определить связь привязки в XAML с помощью `BindingExtension`. Он имеет те же основные значения, что расширение в WPF.

Поддержку привязки и механизм для реализации Silverlight большее сходство, чем WPF. Существует несколько отсутствующих компонентов, которые не были реализованы в Xamarin.Forms:

- Некоторые свойства привязки не поддерживается:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - ValidationRules коллекции
    - XPath
    - Класс XmlNamespaceManager
- `Binding.Mode` не поддерживает `OneTime`, вместо этого использовать `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Не поддерживается для `RelativeSource` привязки. В WPF они позволяют выполнять привязку для других визуальных элементов, определенных в XAML. В Xamarin.Forms, этот же возможность достигается с помощью `{x:Reference}` расширения разметки. Например предположим, что у нас есть элемент управления с именем «otherControl» имеет свойство Text, можно выполнить привязку к ее следующим образом:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Такая же функция может использоваться для `{RelativeSource Self}` компонентов. Тем не менее не поддерживается для нахождения предков по типу (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Контекст привязки

В WPF, можно определить `DataContext` значение какие reprents свойства источника привязки по умолчанию. Если источник привязки, не определен, используется значение этого свойства. Значение наследуется вниз визуального дерева, что позволяет определить на более высоком уровне и затем используются дочерние элементы.

В Xamarin.Forms, эта же функция по, но имя свойства `BindingContext`.

#### <a name="value-converters"></a>преобразователи значений;

Преобразователи значений, полностью поддерживаются в Xamarin.Forms - так же, как WPF. Используется интерфейс фигуру, но Xamarin.Forms содержит интерфейс, определенный в `Xamarin.Forms` пространства имен.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM полностью поддерживается WPF и Xamarin.Forms.

WPF включает встроенную в `RoutedCommand` иногда используемый; Xamarin.Forms не имеет встроенных команд поддержки за пределами `ICommand` определением интерфейса. Может содержать различные MVVM платформ, чтобы добавить необходимые базовые классы для реализации MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>Интерфейс INotifyPropertyChanged и INotifyCollectionChanged

Оба интерфейса полностью поддерживаются в привязках Xamarin.Forms. В отличие от множество платформ на основе XAML уведомления об изменении свойств, которые могут возникать в фоновых потоках в Xamarin.Forms (как WPF) и механизм привязки будет правильно переход в потоке пользовательского интерфейса.

Кроме того, поддерживает обе эти среды `SynchronziationContext` и `async` / `await` делать маршалинг в правильном потоке. WPF включает `Dispatcher` класса на все визуальные элементы, Xamarin.Forms имеет статический метод `Device.BeginInvokeOnMainThread` может также использоваться (несмотря на то что `SynchronizationContext` является предпочтительным для кросс платформенной кодирования).

- Включает Xamarin.Forms `ObservableCollection<T>` поддерживающей уведомления об изменении коллекции.
- Можно использовать `BindingBase.EnableCollectionSynchronization` для включения обновлений между потоками для коллекции. API-интерфейса несколько отличается от варианта WPF [Проверьте документы сведения об использовании](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/).

## <a name="data-templates"></a>Шаблоны данных

Шаблоны данных поддерживаются в Xamarin.Forms для настройки обработки `ListView` строк (элементов). В отличие от WPF, который можно использовать `DataTemplate`s для любого ориентированного содержимое элемента управления, Xamarin.Forms в настоящее время используются только для `ListView`. Определение шаблона может быть определен как встроенный (назначенный `ItemTemplate` свойства), или как ресурс в `ResourceDictionary`.

Кроме того они не являются достаточно гибким, как их аналоги WPF.

1. Корневой элемент `DataTemplate` должен _всегда_ быть `ViewCell` объекта.
2. Триггеры данных полностью поддерживается в шаблон данных, но должно включать `DataType` свойство, указывающее тип свойства, связанного с триггером.
3. `DataTemplateSelector` также поддерживается, но является производным от `DataTemplate` и поэтому просто присваивается непосредственно `ItemTemplate` свойства (или `ItemTemplateSelector` в WPF).

## <a name="itemscontrol"></a>Элемент управления ItemsControl

Нет нет встроенной equivelent для `ItemsControl` в Xamarin.Forms; но [пользовательские один для Xamarin.Forms вы можете найти здесь](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Пользовательские элементы управления

В WPF `UserControl`s используются для предоставления раздел пользовательского интерфейса, в которой имеется связанная поведение. В Xamarin.Forms, мы используем `ContentView` для той же цели. Они поддерживают привязку и включения в XAML.

## <a name="navigation"></a>Навигация

WPF включает редко используемые `NavigationService` которого может использоваться для предоставления функции навигации «браузер по принципу». Большинство приложений не использоваться с этим тем не менее и вместо него используется другой `Window` элементов или разные части окна для отображения данных.

На разных устройствах phone _экраны_ часто решения и поэтому Xamarin.Forms поддерживает различные формы навигации:

| Стиль навигации | Тип страницы |
|--- |--- |
|На основе стека (отправку/извлечение)|NavigationPage|
|Основной/подробности|MasterDetailPage|
|Вкладки|TabbedPage|
|Проведите пальцем влево или вправо|CarouselView|

`NavigationPage` Является наиболее распространенным подходом, и каждая страница имеет `Navigation` свойство, которое может использоваться для передаче или страницы в и из стека навигации. Это ближайший equivelent для `NavigationService` в WPF.

### <a name="url-navigation"></a>URL-адрес перехода

WPF — это технология для рабочей среды и могут принимать параметры командной строки, чтобы указать поведение при запуске. Можно использовать Xamarin.Forms [глубокое связывание URL-адрес](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) для перехода на страницу при запуске.
