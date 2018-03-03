---
title: "iOS локализации"
description: "В этом документе рассматриваются возможности локализации с помощью пакета SDK и способах доступа к ним с помощью Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>iOS локализации

_В этом документе рассматриваются возможности локализации с помощью пакета SDK и способах доступа к ним с помощью Xamarin._

Ссылаться на [кодировки интернационализации](encodings.md) инструкции о включении наборов и кодовых страниц, символ в приложениях, которые нужно обработать данные не в Юникоде.

## <a name="ios-platform-features"></a>Возможности платформы iOS

В этом разделе описываются некоторые возможности локализации в iOS. Перейдите к [разделу](#basics) определенный код и примеры.

### <a name="language"></a>Язык

Пользователям выбрать язык в **параметры** приложения. Этот параметр влияет на язык строк и изображений, отображаемых операционной системы, а также приложения, обнаруживающие языковые параметры.

Это происходит, где пользователи будут выбирать ли разделе английский, испанский, японский, французский или другой язык, который отображается в своих приложениях.

Фактический список языков, поддерживаемых iOS 7: английский (США), английский (Великобритания), китайский (упрощенное письмо), китайский (традиционный), французский, немецкий, итальянский, японский, корейский, испанский, арабский, каталанский, хорватский, чешский, датский, голландский, финский, греческий, иврит, Венгерский, индонезийский, малайский, норвежский, польский, португальский, португальский (Бразилия), румынский, русский, словацкий, шведский, тайский, турецкий, украинский, вьетнамский, английский (Австралия), испанский (Мексика).

Текущий язык, может запросить доступ к первому элементу `PreferredLanguages` массива:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Это значение будет равно код языка `en` для английского языка, `es` для испанского языка, `ja` для японского языка, и т. д. _Возвращаемое значение будет ограничен одним локализации, поддерживаемые приложением (с использованием резервных правил для определения оптимального совпадения)._

Код приложения не всегда необходимо выполнять проверку этого значения - Xamarin и оба iOS предоставляют функции, помогающие автоматически предоставлять правильный строку или ресурсов для языка пользователя. Эти возможности описаны в остальной части этого документа.

> [!NOTE]
> **Примечание:** до iOS 9, не рекомендуемые кода `var lang = NSLocale.PreferredLanguages [0];`.
>
> Результаты, возвращенные этот код, изменения в iOS 9. в разделе [Технические заметки TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) для получения дополнительной информации.
>
> Можно продолжать использовать `NSLocale.PreferredLanguages [0]` для определения фактических значений, выбранных пользователем (независимо от локализации поддерживает приложения).

### <a name="locale"></a>Языковой стандарт

Пользователи выбирают их языкового стандарта **параметры** приложения. Этот параметр влияет на способ форматирования дат, времени, чисел и валюты.

Это позволяет пользователям выбирать ли они видят 12 часов или 24-часовой форматы времени, является ли их десятичного разделителя запятую или точку и порядок день, месяц и год в отображения даты.

С помощью Xamarin у вас есть доступ к классам обоих Apple iOS (`NSNumberFormatter`) и классы .NET в System.Globalization. Разработчики должны оценить которого лучше подходит для их потребностям, как различные функции, доступные в каждом. В частности Если извлечение и отображение цен в покупку приложения с помощью StoreKit определенно следует использовать классы форматирования Apple для возвращаемых сведений цены.

Текущий языковой стандарт, можно запросить с помощью:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Первое значение могут кэшироваться операционной системой и поэтому может не всегда отражают языкового стандарта текущего выбранного пользователя. Второе значение используйте для получения текущего выбранного языка.


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

Создает iOS `NSCurrentLocaleDidChangeNotification` когда пользователь обновляет свой языковой стандарт. Приложения можно ожидать передачи данных для этого уведомления пока они выполняются и внести соответствующие изменения в пользовательский Интерфейс.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Основные сведения о локализации в iOS

Следующие функции iOS легко будет применить в Xamarin, чтобы предоставлять локализованные ресурсы для отображения пользователю. Ссылаться на [TaskyL10n пример](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) чтобы узнать, как реализовать эти идеи.

### <a name="infoplist"></a>Info.plist

Прежде чем начать, настройте **Info.plist** файл со следующими ключами:

- `CFBundleDevelopmentRegion` -язык по умолчанию для приложения (обычно языка произносятся разработчиками и используемые в раскадровках и строковые и графические ресурсы). В следующем примере **en** указано (для английского языка).
- `CFBundleLocalizations` -Массив других локализации поддерживается этим приложением, также с помощью кодов языка, как **es** (испанский) и **pt-PT** (португальский в Португалия).

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>Метод LocalizedString

`NSBundle.MainBundle.LocalizedString` Метод выполняет поиск локализованного текста, который будет сохранено в **.strings** файлы в проекте. Эти файлы организованы в специальную каталоги с языком **.lproj** суффикс.

#### <a name="strings-file-locations"></a>расположения файлов .strings

- **Base.lproj** — это каталог, содержащий ресурсы для языка по умолчанию.
  Часто находится в корневом каталоге проекта (но также может быть помещен в **ресурсов** папки).
- **<language>.lproj** каталоги создаются для каждого поддерживаемого языка, обычно в **ресурсов** папки.

Может существовать несколько разных **.strings** файлы в каталоге каждого языка:

- **Localizable.strings** -основного списка локализованный текст.
- **InfoPlist.strings** — некоторые специальные клавиши разрешены в этом файле для перевода таких вещей, как имя приложения.
- **< имя storyboard > .strings** -необязательный файл, который содержит переводы для элементов пользовательского интерфейса в раскадровку.

**Действие при построении** для этих файлов должна быть **пакета ресурсов**.

#### <a name="strings-file-format"></a>формат файла .strings

Для значения локализованных строк используется синтаксис:

```csharp
/* comment */
"key"="localized-value";
```

Следует экранировать следующие символы в строках:

* `\"`  кавычки
* `\\`  Обратная косая черта
* `\n`  Новая строка

Ниже приведен пример **es/Localizable.strings** (т. е. Испанский) файл из примера:

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>Изображения

Для локализации изображения в iOS:

1. Обратитесь к изображению в коде, например:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Поместите файл изображения по умолчанию **flag.png** в **Base.lproj** (каталог CLR собственной разработки).

3. При необходимости установите локализованные версии изображения в **.lproj** папки для каждого языка (например) **es.lproj**, **ja.lproj**). Используйте это имя **flag.png** в каждом каталоге языка.

Если изображение не отображается для определенного языка, iOS переключиться на родном языке папку по умолчанию и загрузить изображение из него.


#### <a name="launch-images"></a>Запустите изображений

Использовать стандартные соглашения об именовании для запуска образов (XIB и раскадровки для моделей iPhone 6) при помещении их в **.lproj** каталоги для каждого языка.

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Имя приложения

Размещение **InfoPlist.strings** файла в **.lproj** каталог позволяет переопределить некоторые значения в приложении **Info.plist**, включая имя приложения:

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

Другие ключи, которые можно использовать для [локализовать строки для конкретного приложения](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) являются:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>Регион собственной разработки локализации

Строки по умолчанию (расположенный в **Base.lproj** папки) будет считаться «язык». Это означает, что если перевод запрашивается в коде, а не найден для текущего языка **Base.lproj** папки будет выполнен поиск строку, используемую по умолчанию (Если совпадение не найдено, сама строка идентификатора преобразования является отображается).

Разработчики могут выбрать другой язык для этот резервный механизм, задав ключ plist `CFBundleDevelopmentRegionKey`. Его должно быть значение в код языка для разработки собственного языка. На этом снимке экрана показаны plist в Xamarin Studio в области разработки собственного наборов испанский (es):

![](images/cfbundledevelopmentregion.png "Свойство InfoPList.strings")

Если язык по умолчанию, используемый в собственных раскадровок и во всем коде не английский, это значение в соответствии с собственного языка, используемого в коде приложения.

### <a name="dates-and-times"></a>Значения даты и времени

Несмотря на то, что можно использовать встроенные функции даты и времени .NET (вместе с текущим `CultureInfo`) для форматирования даты и времени для языкового стандарта, это приводит к пропуску языковому стандарту пользовательских параметров (которые могут быть настроены отдельно от языка).

Используйте iOS `NSDateFormatter` для вывода, который соответствует предпочтениями пользователя языкового стандарта. В следующем примере кода показаны основные Дата и время параметры форматирования:

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Результаты для английского языка в США:

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Результаты для Испанский (Испания):

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Ссылаться на Apple [модули форматирования даты](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Дополнительные сведения см. При тестировании форматирование зависящие от языкового стандарта даты и времени, установите флажки **iPhone язык** и **область** параметры.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Макет справа налево (RTL)

iOS предоставляет ряд возможностей для помощи в создании приложения с поддержкой справа НАЛЕВО:

* Используйте **автоматический макет** `leading` и `trailing` атрибуты для выравнивание элемента управления (который соответствует *левой* и *правой* для английского языка, например, но он отменить для языков с направлением письма справа НАЛЕВО).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Управления особенно полезна для размещение элементов управления, которые следует учитывать справа НАЛЕВО.
* Используйте `TextAlignment = UITextAlignment.Natural` для выравнивания текста (который может быть *левой* для большинства языков, но *правой* для RTL).
* `UINavigationController` автоматически отражает кнопки "Назад" и меняет направление считывание.

В следующих снимках экрана показано [локализованные **Tasky** пример](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) арабском языке и иврите (несмотря на то, что в полях было введено английский):

[ ![](images/rtl-ar-sml.png "Локализация на арабском языке")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "Локализация на иврите")](images/rtl-he.png "Hebrew")

автоматически меняет iOS `UINavigationController`, и другие элементы управления размещаются внутри `UIStackView` или выровнена с автоматическим макетом.
Справа НАЛЕВО текст локализуется с помощью **.strings** файлы в так же, как текст слева Направо.


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Локализация пользовательского интерфейса в коде

[Tasky (локализованное в коде)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) образце показан способ локализации приложения, где пользовательского интерфейса создается в коде (а не XIBs или раскадровки).

### <a name="project-structure"></a>Структура проекта



![](images/solution-code.png "Дерево ресурсов")

### <a name="localizablestrings-file"></a>Файл Localizable.strings

Как описано выше, **Localizable.strings** формат файла состоит из пары "ключ значение", где ключ — строка, указывающая, выбранные пользователем

Испанский (**es**) локализации для примера приведены ниже:

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Выполнение локализации

В коде приложения, там, где это имеет значение отображаемого текста пользовательского интерфейса (будь то текст метки, или заполнитель входных данных и т. д) в коде используется iOS `LocalizedString` функции для получения правильное преобразование для отображения:

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Локализация пользовательских интерфейсов раскадровки

Образец [Tasky (локализованные раскадровки)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) показан способ локализации текста в элементе управления, в раскадровку.

### <a name="project-structure"></a>Структура проекта

**Base.lproj** каталог содержит раскадровки, а также должен содержать никаких изображений, используемых в приложении.

Содержать другие каталоги языка **Localizable.strings** файл все строковые ресурсы, в коде, а также **MainStoryboard.strings** файл, который содержит переводы для текста в Раскадровка.

![](images/solution-storyboard.png "Дерево ресурсов")

Каталоги языка должен содержать копию все образы, локализованные, чтобы переопределить имеющемуся в **Base.lproj**.

### <a name="object-id--localization-id"></a>Идентификатор объекта или ИД локализации

При создании и изменении элементов управления в раскадровку, выделите каждый элемент управления и проверить идентификатор, используемый для локализации:

* в Visual Studio для Mac, она находятся в области свойств и **ИД локализации**.
* в Xcode, называется **идентификатор объекта**.

Это строковое значение, часто имеют форму, как **«xmR NF3 h8»**:

![](images/xs-designer-localization-id.png "Представление Xcode локализации раскадровки")

Это значение используется в **.strings** файл, чтобы автоматически назначить переведенный текст каждого элемента управления.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Похож на формат файла перевода раскадровки **Localizable.strings** файла, за исключением того, что ключ (значение слева) не может быть задано пользователем, но вместо этого должен иметь особый формат: `ObjectID.property`.

В примере **Mainstoryboard.strings** ниже можно видеть `UITextField`имеют `placeholder` свойство text, которое можно локализовать; `UILabel`имеют `text` свойство; и `UIButton`s текст по умолчанию задается с помощью `normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **С помощью раскадровки с классами размер** может привести к переводы не отображаются. Возможно, это относится к [эту проблему](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder) где отображается надпись документации компании Apple: локализации раскадровки или XIB будет не локализовано правильно, если выполняются все из следующих трех условий: раскадровки или XIB использует классы размер. Базовый локализации и целевой объект построения задаются в универсальную. Сборка обращается iOS 7.0.
Исправление предназначено для дублирования свой файл раскадровки, строки в два идентичных файла: **MainStoryboard~iphone.strings** и **MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "Файлы строк")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>Листинг App Store

Соответствует Apple вопросы и ответы на [локализации приложения магазина](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) ввести переводы для каждой страны, ваше приложение находится на продажи. Обратите внимание, их предупреждение, переводы будут отображаться только в том случае, если приложение содержит локализованный **.lproj** каталог для языка.

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>Сводка

В этой статье описываются основы работы локализация приложений iOS с помощью функции обработки и раскадровки встроенных ресурсов.

Можно узнать больше о i18n и L10n для iOS, Android и кросс платформенных приложений (включая Xamarin.Forms) в [это руководство кросс платформенных](~/cross-platform/app-fundamentals/localization.md).


## <a name="related-links"></a>Связанные ссылки

- [Tasky (локализованное в коде) (пример)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (локализованные раскадровки) (пример)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Руководство по локализации Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Общие сведения о локализации](~/cross-platform/app-fundamentals/localization.md)
- [Локализация Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization.md)
- [Android локализации](~/android/app-fundamentals/localization.md)
