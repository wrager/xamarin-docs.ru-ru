---
title: "Xamarin.Forms представлений"
description: "Xamarin.Forms представления являются составными частями кросс платформенных мобильных пользовательских интерфейсов."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms представлений

_Xamarin.Forms представления являются составными частями кросс платформенных мобильных пользовательских интерфейсов._

<style>.tableimg {Максимальная ширина: нет! важные;}</style>

## <a name="views"></a>Представления

Xamarin.Forms использует слово *представление* ссылалась на визуальных объектов, таких как кнопки, метки или текста для полей ввода — которых может быть более широко известен как элементы управления мини-приложений.

Эти элементы пользовательского интерфейса, обычно являются подклассами [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

Поддерживает Xamarin.Forms:

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Тип</strong>
    </th>
    <th>
      <strong>Описание</strong>
    </th>
    <th style="min-width:400px">
      <strong>Screenshot</strong>
    </th>

  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
Элемент управления visual используется для указания, что-то является текущей. Этот элемент управления предоставляет визуальные признаки пользователю, что-то происходит без информации о ходе его выполнения.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="Пример ActivityIndicator" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> используется для рисования сплошной цветной прямоугольник. BoxView является полезным замены для изображений или пользовательские элементы при начальной создание прототипов. BoxView имеет 40 x 40 запрос на размер по умолчанию. Если требуется другой размер назначить <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> и <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="Пример BoxView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Button</a>
    </td>
    <td align="center" valign="top">
Кнопка <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , реагирует на события касания.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Пример кнопки" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , позволяющий Выбор даты. Визуальное представление DatePicker очень похожа на одном из <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">входа</a>, за исключением того, что вместо клавиатуры отображается специальный элемент управления для выбора даты </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="Пример DatePicker" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Редактор</a>
    </td>
    <td valign="top">
Элемент управления, можно изменить несколько строк текста. Записей в одну строку, в разделе <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">входа</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Пример редактора" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Запись</a>
    </td>
    <td valign="top">
Элемент управления, который можно редактировать одну строку текста. Запись является записью одну строку текста. Лучше всего он используется для сбора небольших отдельных частей сведения, например имена пользователей и пароли.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Пример записи" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , содержащий изображение.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Изображения API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">Работа с образами</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Демонстрация источника</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Пример изображения" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , отображается текст в формате только чтения. Метка используется для отображения элементов одну строку текста, а также блоков нескольких строк текста.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Пример метки" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">ItemView</a> , отображающая набор данных в виде вертикального списка.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">Документация ListView</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="Пример ListView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , отображает контент OpenGL.
    <ul>
      <li>Работает только для iOS и Android проектов (без поддержки Windows Phone).
      <li>Требуется ссылка на <b>OpenTK 1.0</b> сборки в проекты iOS и Android.</li>
      <li>Лучше всего подходит для использования в общих проектов; При использовании в переносимую библиотеку Классов, а затем помощью DependencyService также может потребоваться.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="Пример OpenGlView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Средство выбора</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> элемента управления для выбора элемента в списке. Визуальное представление элемент выбора аналогичен <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">запись</a>, однако появляется элемент управления средства выбора вместо клавиатуры.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Пример выбора" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , указывающий индикатора выполнения.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="Пример элемента управления ProgressBar класса ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> управления, предоставляющий поле поиска.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="Пример SearchBar" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> элемент управления, который принимает значение линейной.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Пример ползунка" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Пошаговым</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> элемент управления, который получает на входе дискретные значения, ограниченный диапазон.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Пример с пошаговым" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Коммутатор</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> элемента управления, обеспечивающего переключаемых значение.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Пример коммутатора" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> , содержащий строки <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">ячейки</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">TableView документации</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Демонстрация источника</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="Пример TableView" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> элемента управления, обеспечивающего комплектации времени. Визуальное представление TimePicker очень похожа на одном из <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">входа</a>, за исключением того, что отображается специальный элемент управления для выбора времени вместо клавиатуры.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="Пример TimePicker" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a> представляет содержимое HTML.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">API веб-представление</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">Документация по веб-представление</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Демонстрация источника</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="Пример веб-представление" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Коллекция Xamarin.Forms (пример)](https://developer.xamarin.com/samples/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
