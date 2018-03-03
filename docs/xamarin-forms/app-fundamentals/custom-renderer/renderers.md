---
title: "Модуль подготовки отчетов базовых классов и собственные элементы управления"
description: "Каждый элемент управления Xamarin.Forms имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. В этой статье перечислены модуль подготовки отчетов и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms страницы, макет, представления и ячейки."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 95518d9b23db68cc972549c730deeea968512444
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Модуль подготовки отчетов базовых классов и собственные элементы управления

_Каждый элемент управления Xamarin.Forms имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. В этой статье перечислены модуль подготовки отчетов и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms страницы, макет, представления и ячейки._

За исключением элемента `MapRenderer` платформой модулей подготовки отчетов класса, можно найти в следующих пространств имен:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Windows Phone 8** – Xamarin.Forms.Platform.WinPhone
- **WinRT** — Xamarin.Forms.Platform.WinRT
- **Универсальная платформа Windows (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` Класс можно найти в следующих пространств имен:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** — Xamarin.Forms.Maps.Android
- **Windows Phone 8** – Xamarin.Forms.Maps.WP8
- **WinRT** — Xamarin.Forms.Maps.WinRT
- **Универсальная платформа Windows (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Число страниц

В следующей таблице перечислены средства визуализации и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms [страницы](~/xamarin-forms/user-interface/controls/pages.md) типа:

<table>
 <thead>
   <tr>
     <td><strong>страница</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8 < / strong</td>
     <td><strong>WinRT или UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md">PageRenderer</a></td>
     <td>UIViewController</td>
     <td>Группе ViewGroup</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a></td>
     <td><p>PhoneMasterDetailRenderer (iOS — Phone)</p><p>TabletMasterDetailPageRenderer (iOS — планшете)</p><p>MasterDetailRenderer (Android)</p><p>MasterDetailPageRenderer (Android AppCompat)</p><p>MasterDetailRenderer (Windows Phone)</p><p>MasterDetailPageRenderer (WinRT)</p></td>
     <td><p>UIViewController (Phone)</p><p>UISplitViewController (планшетный компьютер)</p></td>
     <td>DrawerLayout (версия 4)</td>
     <td>DrawerLayout (версия 4)</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (пользовательский элемент управления)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a></td>
     <td><p>NavigationRenderer (iOS и Android)</p><p>NavigationPageRenderer (Android AppCompat)</p><p>NavigationPageRenderer (Windows Phone 8 и WinRT)</p></td>
     <td>UIToolbar</td>
     <td>Группе ViewGroup</td>
     <td>Группе ViewGroup</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (пользовательский элемент управления)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a></td>
     <td><p>TabbedRenderer (iOS и Android)</p><p>TabbedPageRenderer (Android AppCompat)</p><p>TabbedPageRenderer (Windows Phone 8 и WinRT)</p></td>
     <td>UIView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>Элементов пользовательского интерфейса (Pivot)</td>
     <td>FrameworkElement (Pivot)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a></td>
     <td>PageRenderer</td>
     <td>UIViewController</td>
     <td>Группе ViewGroup</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage<a/></td>
     <td>CarouselPageRenderer</td>
     <td>UIScrollView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>Элементов пользовательского интерфейса ("Панорама")</td>
     <td>FrameworkElement (FlipView)</td>
   </tr>
 </tbody>
</table>

## <a name="layouts"></a>Макеты

В следующей таблице перечислены средства визуализации и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms [макета](~/xamarin-forms/user-interface/controls/layouts.md) типа:

<table>
 <thead>
   <tr>
     <td><strong>Макет</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT или UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Frame</a></td>
     <td>FrameRenderer</td>
     <td>UIView</td>
     <td>Группе ViewGroup</td>
     <td>Border</td>
     <td>Border</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a></td>
     <td>ScrollViewRenderer</td>
     <td>UIScrollView</td>
     <td>ScrollView</td>
     <td>ScrollViewer</td>
     <td>ScrollViewer</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Сетка</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Просмотр</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

## <a name="views"></a>Представления

В следующей таблице перечислены средства визуализации и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms [представление](~/xamarin-forms/user-interface/controls/views.md) типа:

<table>
 <thead>
   <tr>
     <td><strong>Представления</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT или UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a></td>
     <td>ActivityIndicatorRenderer</td>
     <td>UIActivityIndicator</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a></td>
     <td><p>BoxRenderer (iOS и Android)</p><p>BoxViewRenderer (Windows Phone и WinRT)</p></td>
     <td>UIView</td>
     <td>Группе ViewGroup</td>
     <td></td>
     <td>Прямоугольник</td>
     <td>Прямоугольник</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Button</a></td>
     <td>ButtonRenderer</td>
     <td>UIButton</td>
     <td>Кнопка</td>
     <td>AppCompatButton</td>
     <td>Кнопка</td>
     <td>Кнопка</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/">CarouselView</a></td>
     <td>CarouselViewRenderer</td>
     <td>UIScrollView</td>
     <td>RecyclerView</td>
     <td></td>
     <td>FlipView</td>
     <td>FlipView</td>
   </tr>   
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></td>
     <td>DatePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>DatePicker</td>
     <td>DatePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Редактор</a></td>
     <td>EditorRenderer</td>
     <td>UITextView</td>
     <td>EditText</td>
     <td></td>
     <td>TextBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Ввод</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/entry.md">EntryRenderer</a></td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>PhoneTextBox/PasswordBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Изображение</a></td>
     <td>ImageRenderer</td>
     <td>UIImageView</td>
     <td>ImageView</td>
     <td></td>
     <td>Изображение</td>
     <td>Изображение</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a></td>
     <td>LabelRenderer</td>
     <td>UILabel</td>
     <td>TextView</td>
     <td></td>
     <td>TextBlock</td>
     <td>TextBlock</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/listview.md">ListViewRenderer</a></td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>LongListSelector</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/">Map</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md">MapRenderer</a></td>
     <td>MKMapView</td>
     <td>MapView</td>
     <td></td>
     <td>Карта</td>
     <td>MapControl</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Средство выбора</a></td>
     <td>PickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td>EditText</td>
     <td>FrameworkElement</td>
     <td>ComboBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a></td>
     <td>ProgressBarRenderer</td>
     <td>UIProgressView</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></td>
     <td>SearchBarRenderer</td>
     <td>UISearchBar</td>
     <td>SearchView</td>
     <td></td>
     <td>PhoneTextBox</td>
     <td><p>Окна поиска (WinRT)</p><p>AutoSuggestBox (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a></td>
     <td>SliderRenderer</td>
     <td>UISlider</td>
     <td>SeekBar</td>
     <td></td>
     <td>Slider</td>
     <td>Slider</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a></td>
     <td>StepperRenderer</td>
     <td>UIStepper</td>
     <td>LinearLayout</td>
     <td></td>
     <td>Граница с StackPanel и две кнопки</td>
     <td><p>UserControl (WinRT)</p><p>Элемент управления (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Коммутатор</a></td>
     <td>SwitchRenderer</td>
     <td>UISwitch</td>
     <td>Параметр</td>
     <td>SwitchCompat</td>
     <td>Граница с ToggleSwitchButton</td>
     <td>ToggleSwitch</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a></td>
     <td>TableViewRenderer</td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>Сетка с TextBlock и ListBox</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></td>
     <td>TimePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>TimePicker</td>
     <td>TimePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a></td>
     <td>WebViewRenderer</td>
     <td>UIWebView</td>
     <td>Веб-представление</td>
     <td></td>
     <td>Веб-браузер</td>
     <td>Веб-представление</td>
   </tr>
 </tbody>
</table>

## <a name="cells"></a>Ячейки

В следующей таблице перечислены средства визуализации и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms [ячейки](~/xamarin-forms/user-interface/controls/cells.md) типа:

<table>
 <thead>
   <tr>
     <td><strong>Ячейки</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT или UWP</strong></td>
   </tr>
 </thead>
 <tbody>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a></td>
   <td>EntryCellRenderer</td>
   <td>UITableViewCell с UITextField</td>
   <td>LinearLayout с TextView и EditText</td>
   <td>DataTemplate с сетку, содержащую TextBlock и PhoneTextBox</td>
   <td>DataTemplate с текстовым полем</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a></td>
   <td>SwitchCellRenderer</td>
   <td>UITableViewCell с UISwitch</td>
   <td>Параметр</td>
   <td>DataTemplate с ToggleSwitch</td>
   <td>DataTemplate с сетку, содержащую TextBlock и ToggleSwitch</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a></td>
   <td>TextCellRenderer</td>
   <td>UITableViewCell</td>
   <td>LinearLayout с двумя TextViews</td>
   <td>DataTemplate с кнопкой, содержащий StackPanel с двумя TextBlocks</td>
   <td>DataTemplate с StackPanel, содержащий два TextBlocks</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a></td>
   <td>ImageCellRenderer</td>
   <td>UITableViewCell с UIImage</td>
   <td>LinearLayout с двумя TextViews и ImageView</td>
   <td>DataTemplate с кнопкой, содержащий таблицы с изображения и StackPanel, содержащий два TextBlocks</td>
   <td>DataTemplate с сетку, содержащую два TextBlocks и изображения</td>  
   </td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/">ViewCell</a></td>
   <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md">ViewCellRenderer</a></td>
   <td>UITableViewCell</td>
   <td>Просмотр</td>
   <td>DataTemplate с ContentPresenter</td>
   <td>DataTemplate с ContentPresenter</td>  
   </td>
 </tr>
 </tbody>
</table>

## <a name="summary"></a>Сводка

В этой статье перечислил модуль подготовки отчетов и классы собственного элемента управления, которые реализуют каждого Xamarin.Forms страницы, макет, представления и ячейки. Каждый элемент управления Xamarin.Forms имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления.



## <a name="related-links"></a>Связанные ссылки

- [Пользовательские модули подготовки отчетов (университет Xamarin видео)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
