---
title: Настройка ViewCell
description: Xamarin.Forms ViewCell — это ячейка, который может быть добавлен в ListView или TableView, которая содержит представления, определяемые разработчиком. В этой статье показано, как создать пользовательское средство отрисовки для ViewCell, который размещается в элементе управления Xamarin.Forms ListView. Это предотвратит вычисления макета Xamarin.Forms не вызывается несколько раз во время прокрутки ListView.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: ec7e8ef619ba065c0e9d81b71f267eb70a68bd14
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847710"
---
# <a name="customizing-a-viewcell"></a>Настройка ViewCell

_Xamarin.Forms ViewCell — это ячейка, который может быть добавлен в ListView или TableView, которая содержит представления, определяемые разработчиком. В этой статье показано, как создать пользовательское средство отрисовки для ViewCell, который размещается в элементе управления Xamarin.Forms ListView. Это предотвратит вычисления макета Xamarin.Forms не вызывается несколько раз во время прокрутки ListView._

Каждая ячейка Xamarin.Forms имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. Когда [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) отрисовывается приложением Xamarin.Forms в iOS `ViewCellRenderer` создается экземпляр класса, который в свою очередь создает собственный `UITableViewCell` элемента управления. На платформе Android `ViewCellRenderer` класс создает экземпляр собственного `View` элемента управления. В универсальной платформы Windows (UWP), `ViewCellRenderer` класс создает экземпляр собственного `DataTemplate`. Дополнительные сведения о модуль подготовки отчетов и классы собственный элемент управления, которые сопоставляются с элементами управления Xamarin.Forms см [подготовки базовых классов и собственные элементы управления](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) и соответствующие собственные элементы управления, которые реализуют:

![](viewcell-images/viewcell-classes.png "Связь между элементом управления ViewCell и реализации собственные элементы управления")

Процесс подготовки отчета можно считать преимущества реализации настроек платформой путем создания пользовательского средства визуализации для [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Cell) пользовательских ячеек Xamarin.Forms.
1. [Использовать](#Consuming_the_Custom_Cell) пользовательских ячеек с помощью Xamarin.Forms.
1. [Создание](#Creating_the_Custom_Renderer_on_each_Platform) пользовательское средство отрисовки для ячейки для каждой платформы.

Каждый элемент теперь обсуждаются в свою очередь, чтобы реализовать `NativeCell` подготовки отчетов, который использует преимущества макета платформой для каждой ячейки, размещенные внутри Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) управления. Эта команда останавливает вычисления макета Xamarin.Forms из многократно вызывается во время `ListView` прокрутки.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Создание пользовательских ячеек

Ячейки пользовательского элемента управления можно создать путем создания подкласса [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) класса, как показано в следующем примере кода:

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
`NativeCell` Класса в проекте библиотеки .NET Standard и определяет API-Интерфейс для пользовательских ячеек. Предоставляет ячейки пользовательского `Name`, `Category`, и `ImageFilename` свойства, которые могут быть отображены через привязку данных. Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Использование пользовательских ячеек

`NativeCell` Пользовательских ячеек может ссылаться на языке Xaml в проекте библиотеки .NET Standard объявление пространства имен для его расположения и с помощью префикса пространства имен для пользовательских ячеек элемента. В следующем примере кода показан способ `NativeCell` пользовательские ячейки могут быть использованы XAML-страницы:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <StackLayout>
              <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
              <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                  <ListView.ItemTemplate>
                      <DataTemplate>
                          <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                      </DataTemplate>
                  </ListView.ItemTemplate>
              </ListView>
          </StackLayout>
      </ContentPage.Content>
</ContentPage>
```

`local` Префикс пространства имен можно присвоить любое имя. Тем не менее `clr-namespace` и `assembly` значения должны соответствовать сведений пользовательского элемента управления. После объявления пространства имен, префикс используется для ссылки на пользовательские ячейки.

В следующем примере кода показан способ `NativeCell` пользовательские ячейки могут быть использованы на странице C#:

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) управления используется для отображения списка данных, который заполняется по [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) свойство. [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Стратегию кэширования пытается свести к минимуму `ListView` объем памяти, выполнения, скорость путем перезапуска ячейки списка. Дополнительные сведения см. в разделе [стратегии кэширования](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Каждая строка в списке содержит три элемента данных: имя, категорию и имя файла изображения. Макет каждая строка в списке определяется `DataTemplate` , осуществляется с помощью [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) привязываемые свойства. `DataTemplate` Определяет, что каждая строка данных в списке будут `NativeCell` , отображающая ее `Name`, `Category`, и `ImageFilename` свойства посредством привязки данных. Дополнительные сведения о `ListView` управления см. в разделе [ListView](~/xamarin-forms/user-interface/listview/index.md).

Возможность добавления пользовательского средства отрисовки в каждый проект приложения для настройки макета платформой для каждой ячейки.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского средства визуализации для каждой платформы

Создание класса пользовательского средства отрисовки выполняется следующим образом:

1. Создать подкласс `ViewCellRenderer` класс, который подготавливает к просмотру пользовательских ячеек.
1. Переопределить метод платформой, который подготавливает к просмотру пользовательских ячеек и напишите логику для ее настройки.
1. Добавить `ExportRenderer` атрибут класс пользовательского средства визуализации, чтобы указать, что он будет использоваться для отображения пользовательских ячеек Xamarin.Forms. Этот атрибут используется для регистрации пользовательского средства визуализации с помощью Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms является необязательным для предоставления пользовательского средства отрисовки в каждом проекте платформы. Если пользовательское средство отрисовки не зарегистрирован, будет использоваться модуль подготовки отчетов по умолчанию для базового класса элемента управления. Однако пользовательские модули подготовки отчетов, необходимы в каждом проекте платформы при подготовке к просмотру [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) элемента.

На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](viewcell-images/solution-structure.png "Обязанности проекта NativeCell пользовательского модуля подготовки отчетов")

`NativeCell` Пользовательских ячеек визуализируется классов отрисовки специфический для платформы, которые являются производными от `ViewCellRenderer` класса для каждой платформы. Это приведет к каждой `NativeCell` ячейки пользовательского готовится к просмотру с макетом специфический для платформы, как показано на следующем снимке экрана:

![](viewcell-images/screenshots.png "NativeCell на каждой платформе")

`ViewCellRenderer` Класс предоставляет методы платформой для отрисовки пользовательских ячеек. Это `GetCell` метод на платформе iOS `GetCellCore` метод на платформе Android и `GetTemplate` метод на UWP.

Каждый класс пользовательского средства отрисовки снабжен `ExportRenderer` атрибут, который регистрирует модуль подготовки отчетов с помощью Xamarin.Forms. Атрибут принимает два параметра — имя типа ячейки Xamarin.Forms готовится к просмотру и имя типа пользовательского средства отрисовки. `assembly` Префикс в атрибут указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматриваются реализации каждого пользовательского средства отрисовки платформой класса.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

В следующем примере кода показано пользовательское средство отрисовки для платформы iOS.

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCell` Метод вызывается для создания каждой ячейки для отображения. Каждая ячейка является `NativeiOSCell` экземпляр, который определяет макет ячейки и ее данных. Работу `GetCell` метод зависит от [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования:

- Когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования — [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCell` метод будет вызван для каждой ячейки. Объект `NativeiOSCell` будет создан экземпляр для каждого `NativeCell` экземпляр, который отображается на экране. Как пользователь прокручивает `ListView`, `NativeiOSCell` экземпляры будут использовать повторно. Дополнительные сведения о ячейке iOS повторного использования см. в разделе [повторно использовать ячейки](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Этот код пользовательского средства отрисовки выполняет некоторые ячейки повторно использовать, даже когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) равно сохранить ячейки.

  Данные, отображаемые на каждое `NativeiOSCell` экземпляра, созданных или повторно использовать, будут ли обновлены с данными из каждого `NativeCell` экземпляра, `UpdateCell` метод.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Метод никогда не будет вызываться при [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования равно сохранить ячейки.

- Когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования — [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCell` метод будет вызван для каждой ячейки, которое изначально отображается на экране. Объект `NativeiOSCell` будет создан экземпляр для каждого `NativeCell` экземпляр, который отображается на экране. Данные, отображаемые на каждое `NativeiOSCell` экземпляры будут обновляться данными из `NativeCell` экземпляра, `UpdateCell` метод. Тем не менее `GetCell` не будет вызван метод прокрутке пользователем через `ListView`. Вместо этого `NativeiOSCell` экземпляры будут использовать повторно. `PropertyChanged` будет ли вызываться события на `NativeCell` экземпляра об изменении данных и `OnNativeCellPropertyChanged` обработчик событий будет обновлять данные в каждом повторно используется `NativeiOSCell` экземпляра.

В следующем примере кода показан `OnNativeCellPropertyChanged` метода, который вызван при `PropertyChanged` события:

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Этот метод обновляет данные будут отображаться в повторно используемый `NativeiOSCell` экземпляров. Для свойства, которое изменяется проверяется, как метод может вызываться несколько раз.

`NativeiOSCell` Класс определяет макет для каждой ячейки и показано в следующем примере кода:

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Этот класс определяет элементы управления, используемые для отображения содержимого ячейки и их размещение. Этот класс реализует [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) интерфейс, который является обязательным, если [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) использует [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) стратегию кэширования. Этот интерфейс указывает, что класс должен реализовывать [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) свойство, которое должен вернуть данным пользовательских ячеек для очистки ячеек.

`NativeiOSCell` Конструктор инициализирует вида `HeadingLabel`, `SubheadingLabel`, и `CellImageView` свойства. Эти свойства используются для отображения данных, хранящихся в `NativeCell` экземпляра, с `UpdateCell` метода требуется задать значение каждого свойства. Кроме того, когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) использует [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) стратегию, данные, отображаемые элементом кэширования `HeadingLabel`, `SubheadingLabel`, и `CellImageView` свойства могут быть обновление с `OnNativeCellPropertyChanged` метод в пользовательское средство отрисовки.

Макет ячеек осуществляется `LayoutSubviews` переопределения, который задает координаты `HeadingLabel`, `SubheadingLabel`, и `CellImageView` в ячейке.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

В следующем примере кода показано пользовательское средство отрисовки для платформы Android.

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCellCore` Метод вызывается для создания каждой ячейки для отображения. Каждая ячейка является `NativeAndroidCell` экземпляр, который определяет макет ячейки и ее данных. Работу `GetCellCore` метод зависит от [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования:

- Когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования — [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCellCore` метод будет вызван для каждой ячейки. Объект `NativeAndroidCell` создается для каждого `NativeCell` экземпляр, который отображается на экране. Как пользователь прокручивает `ListView`, `NativeAndroidCell` экземпляры будут использовать повторно. Дополнительные сведения о Android ячейки повторного использования в разделе [строки представления повторно использовать](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Обратите внимание, что этот код пользовательского средства отрисовки выполнит некоторые ячейки повторно использовать, даже когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) равно сохранить ячейки.

  Данные, отображаемые на каждое `NativeAndroidCell` экземпляра, созданных или повторно использовать, будут ли обновлены с данными из каждого `NativeCell` экземпляра, `UpdateCell` метод.

  > [!NOTE]
  > Обратите внимание, что, хотя `OnNativeCellPropertyChanged` метод будет вызываться при [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) является значение сохранить ячеек, не будет обновлен `NativeAndroidCell` значения свойств.

- Когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) стратегию кэширования — [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCellCore` метод будет вызван для каждой ячейки, которое изначально отображается на экране. Объект `NativeAndroidCell` будет создан экземпляр для каждого `NativeCell` экземпляр, который отображается на экране. Данные, отображаемые на каждое `NativeAndroidCell` экземпляры будут обновляться данными из `NativeCell` экземпляра, `UpdateCell` метод. Тем не менее `GetCellCore` не будет вызван метод прокрутке пользователем через `ListView`. Вместо этого `NativeAndroidCell` экземпляры будут использовать повторно.  `PropertyChanged` будет ли вызываться события на `NativeCell` экземпляра об изменении данных и `OnNativeCellPropertyChanged` обработчик событий будет обновлять данные в каждом повторно используется `NativeAndroidCell` экземпляра.

В следующем примере кода показан `OnNativeCellPropertyChanged` метода, который вызван при `PropertyChanged` события:

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Этот метод обновляет данные будут отображаться в повторно используемый `NativeAndroidCell` экземпляров. Для свойства, которое изменяется проверяется, как метод может вызываться несколько раз.

`NativeAndroidCell` Класс определяет макет для каждой ячейки и показано в следующем примере кода:

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Этот класс определяет элементы управления, используемые для отображения содержимого ячейки и их размещение. Этот класс реализует [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) интерфейс, который является обязательным, если [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) использует [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) стратегию кэширования. Этот интерфейс указывает, что класс должен реализовывать [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) свойство, которое должен вернуть данным пользовательских ячеек для очистки ячеек.

`NativeAndroidCell` Конструктор увеличивает `NativeAndroidCell` макета и инициализирует `HeadingTextView`, `SubheadingTextView`, и `ImageView` свойства к элементам управления в увеличенную макета. Эти свойства используются для отображения данных, хранящихся в `NativeCell` экземпляра, с `UpdateCell` метода требуется задать значение каждого свойства. Кроме того, когда [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) использует [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) стратегию, данные, отображаемые элементом кэширования `HeadingTextView`, `SubheadingTextView`, и `ImageView` свойства могут быть обновление с `OnNativeCellPropertyChanged` метод в пользовательское средство отрисовки.

В следующем примере кода показано определение макета для `NativeAndroidCell.axml` файл макета:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Для этой структуры указывает два `TextView` элементов управления и `ImageView` управления используются для отображения содержимого ячейки. Два `TextView` элементы управления являются вертикально в `LinearLayout` управления со всеми элементами, которые включены в `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского средства визуализации на UWP

В следующем примере кода показано пользовательское средство отрисовки для UWP.

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` Метод вызывается для возвращения ячейки для визуализации для каждой строки данных в списке. Он создает `DataTemplate` для каждого `NativeCell` экземпляр, который будет отображаться на экране с `DataTemplate` определения внешнего вида и содержимым ячейки.

`DataTemplate` Хранятся в словаре ресурса на уровне приложения и показано в следующем примере кода:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate` Определяет элементы управления, используемый для отображения содержимого ячейки и их макет и внешний вид. Два `TextBlock` элементов управления и `Image` управления используются для отображения содержимого ячейки через привязку данных. Кроме того, экземпляр `ConcatImageExtensionConverter` используется для сцепления `.jpg` расширение для каждого имя файла изображения. Это гарантирует, что `Image` управления можно загружать и отобразить изображение, когда это `Source` свойству.

## <a name="summary"></a>Сводка

В этой статье продемонстрировано, как создать пользовательское средство отрисовки для [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) , размещенного в Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) управления. Эта команда останавливает вычисления макета Xamarin.Forms из многократно вызывается во время `ListView` прокрутки.


## <a name="related-links"></a>Связанные ссылки

- [Производительность элемента управления ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
