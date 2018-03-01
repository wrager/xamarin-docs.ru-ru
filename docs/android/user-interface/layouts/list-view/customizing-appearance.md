---
title: "Настройка внешнего вида ListView"
ms.topic: article
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 18c53ed6428eff911420c696d45b341d8e0fa5c1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="customizing-a-listviews-appearance"></a>Настройка внешнего вида ListView

<a name="overview" />

## <a name="overview"></a>Обзор

Макет отображаемых строк зависит от вида ListView. Чтобы изменить внешний вид `ListView`, макет другую строку.

<a name="Built-in_Row_Views" />

## <a name="built-in-row-views"></a>Встроенные строки представления

Существует 12 встроенных представлений, которые можно ссылаться с помощью **Android.Resource.Layout**:

- **TestListItem** &ndash; одна строка текста с минимальным форматированием.

- **SimpleListItem1** &ndash; одна строка текста.

- **SimpleListItem2** &ndash; двух строк текста.

- **SimpleSelectableListItem** &ndash; одна строка текста, которая поддерживает выбор одного или нескольких элементов (добавляется в уровень API 11).

- **SimpleListItemActivated1** &ndash; аналогична SimpleListItem1, но цвет фона указывает при выборе строки (добавляется в уровень API 11).

- **SimpleListItemActivated2** &ndash; аналогична SimpleListItem2, но цвет фона указывает при выборе строки (добавляется в уровень API 11).

- **SimpleListItemChecked** &ndash; отображает флажки для указания выбора.

- **SimpleListItemMultipleChoice** &ndash; отображает флажки для указания нескольких вариантов выбора.

- **SimpleListItemSingleChoice** &ndash; отображает переключатели указания выбора взаимоисключающими.

- **TwoLineListItem** &ndash; двух строк текста.

- **ActivityListItem** &ndash; одна строка текста с изображением.

- **SimpleExpandableListItem** &ndash; можно разворачивать и сворачивать группы строк по категориям и каждой группы.

Каждое представление встроенные строки имеет встроенный стиль, связанные с ним. Эти снимки экрана показывают, как выглядит каждого представления:

[![Снимки экрана TestListItem, SimpleSelectableListItem, SimpleListitem1 и SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png)

[![Снимки экрана SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked и SimpleListItemMultipleChecked](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png)

[![Снимки экрана SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem и SimpleExpandableListItem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png)

**BuiltInViews/HomeScreenAdapter.cs** образец файла (в **BuiltInViews** решения) содержит код для создания экранов элемента-раскрывающийся список. Представление установлено `GetView` метод следующим образом:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Свойства представления затем могут быть заданы с помощью ссылки на идентификаторы стандартных элементов управления `Text1`, `Text2` и `Icon` под `Android.Resource.Id` (не заданы свойства, которые представление не содержит или будет вызвано исключение):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs** образец файла (в **BuiltInViews** решения) содержит код для создания экрана SimpleExpandableListItem. Представление "Группа" задается в `GetGroupView` метод следующим образом:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Дочернее представление задается в `GetChildView` метод следующим образом:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Свойства в представлении группы и дочернего представления затем устанавливаются с помощью ссылки на стандартные `Text1` и `Text2` управления идентификаторами, как показано выше. Снимок экрана SimpleExpandableListItem (как показано выше) пример группы одной строки представления (SimpleExpandableListItem1) и две строки дочернее представление (SimpleExpandableListItem2). Кроме того представление "Группа" можно настроить для двух строк (SimpleExpandableListItem2) и дочернего представления можно настроить для одной строки (SimpleExpandableListItem1) или обе группы представление и дочернее представление могут иметь одинаковое число строк. 


<a name="Accessories" />

## <a name="accessories"></a>Стандартные

Строки могут иметь стандартные добавлен справа представления, чтобы указать состояние выбора:

- **SimpleListItemChecked** &ndash; создает список единственного выбора с помощью проверки в качестве индикатора.

- **SimpleListItemSingleChoice** &ndash; создает переключатель Тип кнопки списки, где возможно только один вариант.

- **SimpleListItemMultipleChoice** &ndash; создает списки типа checkbox, где возможны несколько вариантов.

Стандартные упомянутой выше представлены в следующих диалоговых окнах в порядке их соответствующие:

[![Снимки экрана SimpleListItemChecked, SimpleListItemSingleChoice и SimpleListItemMultipleChoice с стандартные](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png)

Для отображения одного этапа эти стандартные макета требуется идентификатор ресурса к адаптеру вручную установите состояние выбора для нужные строки. Эта строка кода показано, как создать и назначить `Adapter` с помощью одного из этих макетов:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` Сам поддерживает режимы другой вариант, независимо от того, будет отображаться метод. Чтобы избежать путаницы, используйте `Single` режим выбора с `Checked` и `SingleChoice` стандартные и `Multiple` режим с `MultipleChoice` стиля. Режим выбора управляется `ChoiceMode` свойство `ListView`.

<a name="Handling_API_Level" />

### <a name="handling-api-level"></a>Уровень обработки API

Более ранних версиях Xamarin.Android реализованы перечисления в виде свойств целое число со знаком. Последнюю версию был представлен соответствующие типы перечисления .NET, который облегчает процесс для обнаружения возможных вариантов.

В зависимости от того, какой уровень API вы обращаетесь, `ChoiceMode` — целое число или перечисление. Образец файла **AccessoryViews/HomeScreen.cs** имеет блок в комментарий, если требуется выбрать Gingerbread API:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```

<a name="Selecting_Items_Programmatically" />

### <a name="selecting-items-programmatically"></a>Выбор элементов программным способом

Ручная настройка элементов «выбрано» выполняется с помощью `SetItemChecked` метод (он может вызываться несколько раз для выбора нескольких):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Код должен обнаруживать выделение отличается от множественный выбор. Чтобы определить, какая строка была выбрана в `Single` используйте режим `CheckedItemPosition` целочисленное свойство:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Чтобы определить, какие строки были выбраны в `Multiple` режиме, необходимо использовать для перебора в цикле `CheckedItemPositions` `SparseBooleanArray`. Разреженный массив является как словарь, который содержит только записи где значение было изменено, поэтому необходимо пройти весь массив поиск `true` значений, чтобы знать, что был выбран в списке как показано в следующем фрагменте кода:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```

<a name="Creating_Custom_Row_Layouts" />

## <a name="creating-custom-row-layouts"></a>Создание настраиваемой строки макетов

Четыре встроенные строки представления очень просты. Для отображения более сложными структурами (например, список сообщений электронной почты, или твиты или контактных данных) требуется пользовательское представление. Настраиваемые представления обычно объявляются как AXML файлы в **ресурсы и макет** каталог и затем загрузить с помощью их ресурсов идентификатор настраиваемого адаптера. Представление может содержать любое количество классов отображаемое (например, TextViews ImageViews и других элементов управления) с помощью пользовательских цветов, шрифтов и макета.

В этом примере отличается от предыдущих примерах несколькими способами:

-  Наследует от `Activity` , а не `ListActivity` . Можно настроить для любой строки `ListView` , однако другие элементы управления также могут быть включены в `Activity` макета (например, заголовок, кнопки или другие элементы пользовательского интерфейса). В этом примере добавляется заголовок выше `ListView` для иллюстрации.

-  Требуется файл макета AXML для экрана. в предыдущих примерах `ListActivity` файл макета не требуется. Содержит этот AXML `ListView` управления объявления.

-  Требуется файл AXML макет для отображения каждой строки. Этот файл AXML содержит текстовых и графических элементов управления с помощью пользовательского шрифта и цветов.

-  Использует необязательный настраиваемого селектора XML-файл, чтобы задать внешний вид строки, при этом.

-  `Adapter` Реализация возвращает пользовательский макет из `GetView` переопределения.

-  `ItemClick` необходимо объявить иначе (присоединяется обработчик событий к `ListView.ItemClick` вместо переопределения `OnListItemClick` в `ListActivity`).


Эти изменения описаны ниже, начиная с созданием действия представление и представление пользовательских строк и затем охватывающие изменений адаптер и действия для их отображения.

<a name="Adding_a_ListView_to_an_Activity_Layout" />

### <a name="adding-a-listview-to-an-activity-layout"></a>Добавление действия макета ListView

Поскольку `HomeScreen` больше не наследуют `ListActivity` он не имеет представление по умолчанию, поэтому необходимо создать файл AXML макета для представления HomeScreen. Например, представление будет иметь заголовок (с помощью `TextView`) и `ListView` для отображения данных. Макет определяется в **Resources/Layout/HomeScreen.axml** файла, как показано ниже:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

Преимущество использования `Activity` с пользовательский макет (вместо `ListActivity`) заключается в возможность добавлять дополнительные элементы управления на экран, например заголовок `TextView` в этом примере.

<a name="Creating_a_Custom_Row_Layout" />

### <a name="creating-a-custom-row-layout"></a>Создание настраиваемой строки разметки

Другой файл макета AXML требуется для размещения пользовательский макет для каждой строки, которая будет отображаться в представлении списка. В этом примере строка будет иметь зеленым фоном, коричневая текст и изображения по правому краю. Описывается Android XML-разметку для объявления этой структуры в **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Но макета пользовательских строк может содержать множество различных элементов управления, прокрутка производительности могут быть затронуты сложных конструкций и с помощью изображения (в особенности, если они имеют для загрузки по сети). В статье Google Дополнительные сведения об адресации прокрутки проблем с производительностью.

<a name="Referencing_a_Custom_Row_View" />

### <a name="referencing-a-custom-row-view"></a>Ссылающаяся на представление настраиваемой строки

Пример настраиваемого адаптера реализуется на `HomeScreenAdapter.cs`. Основной метод — `GetView` где он загружает пользовательский AXML, используя идентификатор ресурса `Resource.Layout.CustomView`, а затем устанавливает свойства для каждого элемента управления в представлении перед возвращением. Класс адаптера завершения отображается:

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```

<a name="Referencing_the_Custom_ListView_in_the_Activity" />

### <a name="referencing-the-custom-listview-in-the-activity"></a>Создание ссылок на пользовательские ListView в действии

Поскольку `HomeScreen` класс теперь наследует от `Activity`, `ListView` поле объявляется в классе для хранения ссылки на элемент, объявленный в AXML:

```csharp
ListView listView;
```

Класс затем необходимо загрузить пользовательский макет действия AXML с помощью `SetContentView` метод. Он может найти `ListView` элемента управления в макете создает и назначает адаптера и назначает обработчик щелчка. Ниже показан код для метода OnCreate:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Наконец `ItemClick` обработчик должен быть определен; в этом случае просто отображает `Toast` сообщение:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Полученный экран выглядит следующим образом:

[![Снимок экрана: результирующий CustomRowView](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png)


<a name="Customizing_the_Row_Selector_Color" />

### <a name="customizing-the-row-selector-color"></a>Настройка цвета селектор строк

Если входящая строка должны быть выделены для отзывов пользователей. Когда пользовательское представление указывает, как цвет фона, что **CustomView.axml** он также переопределяет метод выделения. Эта строка кода в **CustomView.axml** наборов фона светло-зеленый, но он также есть визуальных признаков при затронутых строк:

```xml
android:background="#FFDAFF7F"
```

Чтобы снова включить режим выделения, а также чтобы настроить цвет, используемый фона атрибуту присваивается настраиваемого селектора вместо него. Селектор будет объявлять как цвет фона по умолчанию так и цвет выделения. Файл **Resources/Drawable/CustomSelector.xml** содержит следующее объявление:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Для ссылки на пользовательский селектор, измените атрибут фона в **CustomView.axml** для:

```xml
android:background="@drawable/CustomSelector"
```

Выбранные строки и соответствующий `Toast` сообщение сейчас выглядит следующим образом:

[![Выбранной строки в оранжевый с всплывающее сообщение, отображающее имя выбранной строки](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png)


<a name="Preventing_Flickering_on_Custom_Layouts" />

### <a name="preventing-flickering-on-custom-layouts"></a>Предотвращение мерцание пользовательские макеты

Android пытается повысить производительность `ListView` прокрутка, кэширование информации о макете. При наличии долго прокрутка списков данных также следует задать `android:cacheColorHint` свойство `ListView` объявление в определении AXML действия (в то же значение цвета фона макет пользовательских строк). Чтобы включить это указание может привести к «мерцание» как пользователь перемещается по списку фоновыми цветами пользовательских строк.



## <a name="related-links"></a>Связанные ссылки

- [BuiltInViews (пример)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (пример)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (пример)](https://developer.xamarin.com/samples/CustomRowView/)
