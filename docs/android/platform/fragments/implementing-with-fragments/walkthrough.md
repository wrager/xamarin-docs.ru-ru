---
title: Пошаговое руководство
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: cc5c05fce9b9c3dd974afe027cc7cbb60085c621
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough"></a>Пошаговое руководство

В следующих шагах базовое приложение создается с помощью фрагментов. Первым шагом является создание нового Xamarin.Android для проекта Android.

## <a name="1-create-a-project"></a>1. Создание проекта

Создайте новый проект с именем Xamarin.Android **FragmentSample**. **Android минимум** версия должна быть задана, для Android 3.1 или более поздней версии, как показано на рисунке ниже:

[![Установка Android Минимальная версия](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2. Создание MainActivity

Теперь нам нужны для создания `MainActivity` класса. Это действие при запуске приложения. Это действие будет размещаться одного или обоих фрагментов, в зависимости от размера экрана. `MainActivity` Это необходимо сделать по загрузке файла макет, который подходит для устройства.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` Вложенные классы фрагмент должен иметь открытый стандартный конструктор без аргументов.

## <a name="3-create-the-layout-files"></a>3. Создавать файлы макета

Два разных размеров требуется два файла другой макет. Поэтому давайте создадим новую папку **ресурсы или макет-большого**и создайте новый макет **activity_main.axml**. Мы также будем переименовать файл макета по умолчанию как **Resources/Layout/activity_main.axml**. После внесения этих изменений макета папки должен быть похож на следующем снимке экрана.

[![Снимок экрана: макет папки в Интегрированной среде разработки](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


Все устройства будет загрузить и использовать файл макета в **ресурсы и макет**.
Это очень простой макет, который просто отображает `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Для устройств с большим экраном, Android будет загружать файл макета в **ресурсы или макет-большого**. Содержимое макета для планшетных ПК выглядит следующим образом:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

Файл макета для больших экранов немного отличается. Не только `TitlesFragment` отображаются в этом файле макета, но `FrameLayout` добавляется рядом с фрагмента. На больших экранах `DetailsFragment` программным образом добавляется `MainActivity` при выборе пользователем воспроизведения. Позднее будут описаны более подробно как это сделать.

Android 3.2 появился новый способ указания макеты экрана. Эти новые квалификаторы укажите объем пространства, должен макета, а не размера экрана. Если это приложение был предназначен для работы только в Android 3.2 или более поздней версии, мы создадим **ресурсов, макет sw600dp** папку (вместо папке **ресурсов или макет-большого**) для файла макета  **activity_main.AXML**. Этот файл ресурсов будет загружаться все устройства, которые имеют ширину экрана 600 зависящий от плотности пикселей. Тем не менее, как это приложение использует только задать целевой объект Android 3.1 или более поздней версии, более старые квалификатором ресурса.

## <a name="4-create-the-titlesfragment"></a>4. Создание TitlesFragment

`TitlesFragment` будет отображать названия различных играет, так что давайте Добавление новый фрагмент в проект с именем `TitlesFragment`:

[![Добавление в проект TitlesFragment новый фрагмент](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

После `TitlesFragment` была добавлена, нам нужно изменить класс, наследующий от `Android.App.ListFragment`. `ListFragment` — Это тип специализированные фрагмент, включающий список функциональные возможности.
`TitlesFragment` также переопределяют `OnActivityCreated` (другой метод фрагмент жизненного цикла) и предоставить `Adapter` , `ListFragment` будет использоваться для заполнения списка:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

Как упоминалось ранее, наше приложение имеет два макеты для `MainActivity`. Код в `OnActivityCreated` определяет наличие `FrameLayout` и определяет, какой файл макета был загружен. Если `FrameLayout` существует в макете, то `_isDualPane` флаг имеет значение `true`. `_isDualPane` Флаг используется в другом месте в действии, в частности `ShowDetails` метод. `ShowDetails` Метод рассматриваются более подробно ниже.

`TitlesFragment` приведен список и должна реагировать на выборе пользователя в списке. Чтобы сделать это, `TitlesFragment` переопределит метод `OnListItemClick`. Внутри `OnListItemClick`, новый `DetailsFragment` будет создан и отображен в `FrameLayout`, программными средствами. Соответствующий код внутри `TitlesFragment` является:

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

Код с устройства определяет способ форматирования и отображения выбранного play с кавычками. В случае планшетных ПК `_isDualPane` флагу будет присвоено `true`, и поэтому квоты будет отображаться рядом с `TitlesFragment`. Если выбранного play `id` еще не отображается, выберите новый `DetailsFragment` создается и затем загружается в `FrameLayout` в действии. Для других устройств, не имеющих большом мониторе &ndash; телефонов, например &ndash; `isDualPane` будет присвоено `false` , новый `DetailsActivity` будет запущена.


## <a name="5-create-the-detailsactivity"></a>5. Создание DetailsActivity

`DetailsActivity` Отображает `DetailsFragment` для более мелких устройств. Чтобы увидеть это, сначала мы добавим новое действие в проект с именем `DetailsActivity`. `DetailsActivity` — Это очень простое действие. Создает и затем разместить новый `DetailsFragment` для воспроизведения `id` , был отправлен:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Обратите внимание, что файл разметки не загружаются для `DetailsActivity`. Вместо этого `DetailsFragment` загружается в представлении корневого действия. В этом представлении корневой имеет специальный идентификатор `Android.Resource.Id.Content`. Новый `DetailFragment` , которая затем добавляется к этому представлению корневой внутри `FragmentTransaction` , создаваемый с помощью этого действия `FragmentManager`.


## <a name="6-create-the-detailsfragment"></a>6. Создание DetailsFragment

Теперь добавим в приложение с именем другой фрагмент `DetailsFragment`. Этот фрагмент выдаст сообщение предложения из выбранного play. В следующем коде показано полное `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

Чтобы `DetailsFragment` для правильной работы функций, они должны иметь индекс, игры, который выбран в `TitlesFragment`. Существует много способов предоставить это значение, чтобы `DetailsFragment`; в этом примере воспроизведение `Id` помещаются в пакет и которое пакет хранится свойство Arguments экземпляра `DetailsFragment`. Свойство `ShownPlayId` предоставляется для удобства &ndash; будет использоваться для экземпляров `DetailsFragment` для извлечения значения из пакета.

`OnCreateView` вызывается, когда этот фрагмент необходимо нарисовать свой пользовательский интерфейс и должен возвращать `Android.Views.View` объекта. В большинстве случаев это `View` завышенными из существующего файла разметки. В случае приведенном выше примере фрагмент будет программного построения представление, которое будет использоваться для отображения.

Поздравляем! Теперь вы создали приложение, использующее фрагментов для упрощения разработки через форм-факторов.

В [следующий раздел](supporting-pre-honeycomb.md), мы собираемся расширить это приложение, чтобы он будет работать на устройствах Android перед 3.0.

