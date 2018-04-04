---
title: Сложности
description: watchOS позволяет разработчикам создавать пользовательские сложности для гарнитуры Контрольное значение
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: c058922f97183359122a1bfa2b0e162e4ce344f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="complications"></a>Сложности

_watchOS позволяет разработчикам создавать пользовательские сложности для гарнитуры Контрольное значение_

Эта страница демонстрирует различные типы доступных сложности и добавление сложностью приложения watchOS 3.

Обратите внимание, что каждое приложение watchOS может быть только один усложнения.

Прочитайте [документации компании Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) определить, подходит ли приложение для сложностью. Существует 5 `CLKComplicationFamily` типа отображения для выбора:

[![](complications-images/all-complications-sml.png "5 доступных типов CLKComplicationFamily: циклическая небольшой, модульную небольшой, модульную больших, практической небольшой, практической больших")](complications-images/all-complications.png#lightbox)

Приложения могут реализовывать только один стиль или все пять, в зависимости от отображаемых данных.
Можно также поддерживают переход во времени, указав значения для прошлых или будущих времени, как пользователь включает цифровой Главная.

<a name="adding" />

## <a name="adding-a-complication"></a>Добавление усложнения

### <a name="configuration"></a>Конфигурация

Сложностей можно добавить приложение watch во время создания, или вручную добавить в существующее решение.

### <a name="add-new-project"></a>Добавьте новый проект...

**Добавить новый проект...**  мастер содержит флажок, который будет автоматически создавать класс контроллера усложнения и настраивать **Info.plist** файла:

![](complications-images/file-new-project-sml.png "Флажок Включить усложнения")

### <a name="existing-projects"></a>Существующие проекты

Чтобы добавить существующий проект осложнение:

1. Создайте новый **ComplicationController.cs** файл класса и реализуйте `CLKComplicationDataSource`.
2. Настройка приложения на **Info.plist** для предоставления усложнения и какие семейства усложнения поддерживаются удостоверения.

Эти действия описаны более подробно ниже.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>Класс CLKComplicationDataSource

Следующий шаблон C# включает минимальные требуемые методы для реализации `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

Выполните [записи осложнение](#writing) инструкции по добавлению кода к этому классу.

### <a name="infoplist"></a>Info.plist

Модуль наблюдения за **Info.plist** файла следует указать имя `CLKComplicationDataSource` и какие семейства усложнения, вы хотите поддерживать:

[![](complications-images/complications-config-sml.png "Типы семейства усложнения")](complications-images/complications-config.png#lightbox)

**Класс источника данных** запись в списке будут показаны имена классов, подкласс `CLKComplicationDataSource` подкласс, в котором включает логику усложнения.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Все функции усложнения реализуются в один класс, переопределение методов из `CLKComplicationDataSource` абстрактного класса (который реализует `ICLKComplicationDataSource` интерфейс).

### <a name="required-methods"></a>Требуемые методы

Необходимо реализовать следующие методы для усложнения для запуска:

- `GetPlaceholderTemplate` – Должен возвращать статического отображения, используемые во время настройки или когда приложение не может предоставить значение.
- `GetCurrentTimelineEntry` -Вычисления отображать при запуске усложнения.
- `GetSupportedTimeTravelDirections` — Возвращает параметры из `CLKComplicationTimeTravelDirections` например `None`, `Forward`, `Backward`, или `Forward | Backward`.

### <a name="privacy"></a>Конфиденциальность

Сложностей, которые отображают персональные данные

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` или `HideOnLockScreen`

Если этот метод возвращает `HideOnLockScreen` усложнения будет показано, как значок или имя приложения (и не все данные) при блокировке часами.

### <a name="updates"></a>Обновления

- `GetNextRequestedUpdateDate` -Возвращают время при операционной системы следует далее запроса приложения для обновленной усложнения отображения данных.

Чтобы принудительно обновить из приложения iOS.

### <a name="supporting-time-travel"></a>Поддержка Переход во времени

Время движения поддержки является необязательным и управляется объектом `GetSupportedTimeTravelDirections` метод. Если он возвращает `Forward`, `Backward`, или `Forward | Backward` , то необходимо реализовать следующие методы

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Запись осложнение

Сложности в диапазоне от простых данных отображать сложные изображения и визуализации данных с поддержкой Переход во времени. В следующем примере кода показано, как построить простое, один шаблон усложнения.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Пример кода

В этом примере поддерживает только `UtilitarianLarge` шаблона, поэтому может быть выбран только от конкретных Контрольное значение сторон, которые поддерживает данный тип усложнения. При *выбора* сложностей контрольного значения, он отображает **Мои УСЛОЖНЕНИЯ** и когда *под управлением* отображается текст **МИНУТУ _час_**   (с составляющей времени).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Шаблоны усложнения

Для каждого стиля усложнения доступны несколько разных шаблонов.
**Кольцо** шаблоны позволяют отображать ход выполнения стиль кольцо вокруг усложнения, который может использоваться для графического отображения хода выполнения или любое другое значение.

[Apple CLKComplicationTemplate документы](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Циклическая малых

Эти имена классов шаблона начинаются с `CLKComplicationTemplateCircularSmall`:

- **RingImage** -отображать один образ, с помощью кольца хода выполнения вокруг него.
- **RingText** -отображения одной строки текста с кольца хода выполнения вокруг него.
- **В образце SimpleImage** -просто отображают небольшое изображение.
- **Программа Simple Text** -просто отображают небольшого фрагмента текста.
- **StackImage** -отображать изображения и строки текста, друг с другом
- **StackText** -отображение двух строк текста.

### <a name="modular-small"></a>Модульная малых

Эти имена классов шаблона начинаются с `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -отображать Мелкая сетка текстовых значений (2 строк и 2 столбцов).
- **RingImage** -отображать один образ, с помощью кольца хода выполнения вокруг него.
- **RingText** -отображения одной строки текста с кольца хода выполнения вокруг него.
- **В образце SimpleImage** -просто отображают небольшое изображение.
- **Программа Simple Text** -просто отображают небольшого фрагмента текста.
- **StackImage** -отображать изображения и строки текста, друг с другом
- **StackText** -отображение двух строк текста.

### <a name="modular-large"></a>Модульная большой

Эти имена классов шаблона начинаются с `CLKComplicationTemplateModularLarge`:

- **Столбцы** -отобразить сетку, 3 строк с двумя столбцами, при необходимости включая изображение слева от каждой строки.
- **StandardBody** -Отображение полужирным заголовка строки, с двумя строками открытого текста. Заголовок при необходимости можно отобразить изображение в левой части экрана.
- **Таблица** -Отображение полужирным заголовка строки, с сеткой 2 x 2 текста под ней. Заголовок при необходимости можно отобразить изображение в левой части экрана.
- **TallBody** -отображать строку полужирным заголовка с размером шрифта одну строку текста под.

### <a name="utilitarian-small"></a>Практической малых

Эти имена классов шаблона начинаются с `CLKComplicationTemplateUtilitarianSmall`:

- **Плоский** -отображает изображение и текст в одной строке (текст должен быть коротким).
- **RingImage** -отображать один образ, с помощью кольца хода выполнения вокруг него.
- **RingText** -отображения одной строки текста с кольца хода выполнения вокруг него.
- **Квадратный** -квадратный изображения (40px или 44px квадрат для 38 мм или 42 мм Apple Watch соответственно).

### <a name="utilitarian-large"></a>Практической большой

Имеется только один шаблон для этого стиля усложнения: `CLKComplicationTemplateUtilitarianLargeFlat`.
Отображает одно изображение и текст, все в одной строке.



## <a name="related-links"></a>Связанные ссылки

- [Документы Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC видео](https://developer.apple.com/videos/play/wwdc2015-209/)
