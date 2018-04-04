---
title: Общие сведения о единой API
description: Новый стиль API упрощает его могут совместно использовать код Mac и iOS, а также позволяя для поддержки 32- и 64 разрядных приложений с тем же двоичный.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c36682ba038c18dfb872e76f338ea1d9881cca10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="unified-api-overview"></a>Общие сведения о единой API

_Новый стиль API упрощает его могут совместно использовать код Mac и iOS, а также позволяя для поддержки 32- и 64 разрядных приложений с тем же двоичный._

Чтобы оптимизировать совместное использование кода несколькими iOS и Mac и разработчики могут иметь единая база кода, которая работает на 32- и 64-бит, в ранней 2015 мы представили новый интерфейс API в продуктах Xamarin.Mac и Xamarin.iOS вызывается API единой.

> [!IMPORTANT]
> **Классический устаревания профиля:** по мере добавления новых платформ в Xamarin.iOS начинаем постепенно число нерекомендуемых функций из классической профиля (monotouch.dll). Например параметр не NRC (новый ref-count) был удален. NRC всегда включена для всех единой приложения (т. е. не NRC никогда не была альтернативы) и известные проблемы отсутствуют. Возможность использования Боэм как сборщик мусора приведет к удалению будущих выпусках. Она также параметр никогда не доступно единое приложениям. Полное удаление поддержки классический запланирована Далее попадают в выпуске Xamarin.iOS 10.0.

## <a name="ios"></a>iOS

`Xamarin.iOS.dll` Сборки, в состав Xamarin.iOS 8.6 наших **первой версии стабильной и поддерживаемых** единой API для операций ввода-вывода.
Предыдущие версии preview универсальный API, можно закрыть, но не полностью совместимы.

## <a name="mac"></a>Mac

`Xamarin.Mac.dll` Сборки в стабильной канала Xamarin.Mac наших **первой версии стабильной и поддерживаемых** единой API для Mac.
Предыдущие версии preview универсальный API, можно закрыть, но не полностью совместимы.

## <a name="runtime-defaults"></a>Значения по умолчанию среды выполнения

По умолчанию использует API единой **SGen** сборщик мусора и [новый подсчет ссылок](~/ios/internals/newrefcount.md) система отслеживания владельца объектов. Эта же функция, которое было перенесено Xamarin.Mac.

Это позволяет решить несколько проблем, которые разработчики встают старую систему и также облегчают [управление памятью](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Обратите внимание, что можно включить новый счетчика ссылок даже для классический API, но по умолчанию консервативной и не требует от пользователей внести изменения. С помощью единого интерфейса API, мы воспользовались возможностью изменения значение по умолчанию и предоставить разработчикам все улучшения в то же время они рефакторинг и повторного тестирования кода.

<a name="namespace-changes" />

## <a name="library-split"></a>Разбиение библиотеки

С этого момента нашими API, будет отображена двумя способами:

-  **Классический API:** ограничено 32 бита (только) и без поддержки в `monotouch.dll` и `XamMac.dll` сборки.
-  **Универсальный интерфейс API:** поддерживает 32- и 64 разработки-разрядных приложений в единый интерфейс API `Xamarin.iOS.dll` и `Xamarin.Mac.dll` сборки.

Это означает, что для предприятия разработчиками (не для различных версий магазина приложений), можно продолжать использовать существующие классический API, мы сохранить подсчитывается их навсегда. Кроме того можно обновить на новые API.

### <a name="namespace-changes"></a>Изменения пространства имен

Чтобы уменьшить трения могут совместно использовать код наших продуктов iOS и Mac, выполняется изменение пространства имен для API-интерфейсов в продуктах.

Мы пропускают префикс «MonoTouch» из наших продуктов iOS и «MonoMac» из наших продуктов Mac для типов данных.

Это упрощает совместное использование кода платформы iOS и Mac, не прибегая к условной компиляции и будет уменьшения уровня шума в верхней части файлов исходного кода.

-  **Классический API:** пространств имен используйте `MonoTouch.` или `MonoMac.` префикс.
-  **Универсальный интерфейс API:** отсутствует префикс пространства имен

### <a name="api-changes"></a>Изменения в API

API единой удаляет устаревшие методы и существует несколько экземпляров где было опечаток в именах API, когда они были привязаны к исходной пространства имен MonoTouch и MonoMac в классический API. Эти экземпляры будут исправлены в новых API единой и необходимо обновить в компонент, iOS и Mac-приложений. Ниже приведен список наиболее распространенные из них, вы можете столкнуться.

|Имя метода классический API|Имя метода единый интерфейс API|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Полный список изменений при переключении из классической в единой API, см. в разделе нашей [классический (monotouch.dll) vs отличия API в единой (Xamarin.iOS.dll)](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) документации.

### <a name="updating-to-unified"></a>Обновление с единой

Несколько старого или неработающие или устаревшие API в **классический** недоступны в **единой** API. Его можно легко исправить `CS0616` обновления предупреждения перед запуском вашей (автоматически или вручную), так как у вас `[Obsolete]` поможет правой API атрибут сообщения (часть предупреждения).

Обратите внимание, что мы публикуем [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) в классическом Visual Studio единой изменения API, которые можно использовать до или после обновления проекта. По-прежнему исправления старые вызовы в классическом будут часто быть экономия времени (менее поиска документации).

Следуйте этим инструкциям, чтобы [обновление существующих приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md), или [приложений Mac](~/cross-platform/macios/unified/updating-mac-apps.md) в единой API.
Просмотрите в оставшейся части этой страницы и [эти советы](~/cross-platform/macios/unified/updating-tips.md) Дополнительные сведения о переносе кода.

## <a name="components-and-nuget"></a>Компоненты и NuGet

Большинство существующих компонентов и пакетов NuGet необходимо обновить для поддержки единого API.
Компоненты, разработанные для классический API не могут ссылаться на проект единого API.

Существующие компоненты, не имеющие все ссылки на `monotouch.dll` (или `XamMac.dll`) не требуют обновления.

### <a name="components"></a>Компоненты

компоненты операций ввода-вывода в [хранилище компонентов Xamarin](https://components.xamarin.com/) необходимо обновить для работы с проектами единой API. Для обновления компонентов мы публикации и предлагать другие авторы сделать то же самое работает Xamarin.

### <a name="nuget"></a>NuGet

Пакеты NuGet, которые ранее поддерживается Xamarin.iOS через классический API опубликованные их сборок с помощью **Monotouch10** моникер платформы.

API единой появился новый идентификатор платформы для совместимых пакетов - **Xamarin.iOS10**. Существующие пакеты NuGet потребуется обновить для добавления поддержки для этой платформы, создав для единой API.

> [!IMPORTANT]
> При наличии ошибки в виде _«ошибка 3 не может включать в том же проекте Xamarin.iOS «monotouch.dll» и «Xamarin.iOS.dll» — «Xamarin.iOS.dll» упоминается явно, пока ссылается «monotouch.dll» "xxx, версия = 0.0.000, Язык и региональные параметры = neutral, PublicKeyToken = null'»_ после преобразования приложения единой API-интерфейсам, это обычно из-за наличия компонента или пакета NuGet в проект, который не был обновлен для API единой. Необходимо удалить существующий компонент/NuGet, обновите версию, которая поддерживает API-интерфейсы единой и выполните чистую сборку.

<a name="deprecated-apis" />

## <a name="arrays-and-systemcollectionsgeneric"></a>Массивы и System.Collections.Generic

Поскольку C# индексаторов ожидает тип `int`, необходимо явно привести `nint` значения `int` для доступа к элементам в коллекции или массиве. Пример:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Это ожидаемое поведение, поскольку приведение из `int` для `nint` является потерей данных на 64-разрядная версия, неявное преобразование не выполняется.

## <a name="converting-datetime-to-nsdate"></a>Преобразование типа DateTime в NSDate

При использовании API единой, неявное преобразование `DateTime` для `NSDate` значения больше не выполняется. Эти значения необходимо явно преобразовываться из одного типа в другой. Следующие методы расширения можно использовать для автоматизации этого процесса:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

## <a name="deprecated-apis-and-typos"></a>Устаревшие интерфейсы API и опечаток

Классический API внутри Xamarin.iOS (monotouch.dll) `[Obsolete]` атрибут использовался двумя разными способами:

-  **Устаревшие iOS API:** это когда Apple подсказок можно остановить с помощью API-Интерфейс, так как он выполняется заменяется более новой версии. Классический API по-прежнему нормально и часто требуется (если требуется поддержка более раннюю версию iOS).
 Такие API (и `[Obsolete]` атрибут), включаются в новые Xamarin.iOS сборки.
-  **Неверный API** некоторые API бы опечаток по их именам.

Для исходной сборки (monotouch.dll и XamMac.dll), мы сохранится старого кода для совместимости, но они были удалены из единой API сборок (Xamarin.iOS.dll и Xamarin.Mac)

<a name="NSObject_ctor" />

## <a name="nsobject-subclasses-ctorintptr"></a>Подклассы .ctor(IntPtr) NSObject

Каждый `NSObject` подкласс имеет конструктор, принимающий `IntPtr`. Это, как можно создать новый управляемый экземпляр из собственного дескриптора ObjC.

В классическом был `public` конструктор. Однако было легко неправильно использовать эту функцию в пользовательском коде, например, создание нескольких управляемые экземпляры для одного экземпляра ObjC *или* создание на управляемом экземпляре, будет недоставать ожидаемое состояние управляемых (для подклассов).

Чтобы избежать этих вида проблем `IntPtr` конструкторы являются теперь `protected` в **единой** API, используемый только для создания подкласса. Это позволит гарантировать, что исправить/safe API используется для создания управляемого экземпляра из дескрипторов, т. е.

    var label = Runtime.GetNSObject<UILabel> (handle);

Этот API возвращает управляемый экземпляр (если он существует) или создаст новый (при необходимости). Он уже находится в классический и единый интерфейс API.

Обратите внимание, что `.ctor(NSObjectFlag)` теперь также является `protected` , но такой редко было использовано за пределами создания подкласса.

<a name="NSAction" />

## <a name="nsaction-replaced-with-action"></a>Заменить действие NSAction

С помощью API-интерфейсы единой `NSAction` был удален в пользу Стандартная .NET `Action`. Это вызвано значительным усовершенствованием по `Action` является тип .NET, тогда как `NSAction` был относящиеся к Xamarin.iOS. В обоих случаях то же, но они были отличающимися и несовместимые типы и привело к необходимости записываться для достижения такого же результата больше кода.

Например, если в существующее приложение Xamarin включен следующий код:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Теперь его можно заменить простое лямбда-выражение:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Ранее, должно быть ошибку компилятора, потому что `Action` не может быть назначен `NSAction`, но поскольку `UITapGestureRecognizer` теперь принимает `Action` вместо `NSAction` допустимо в единой API.

## <a name="custom-delegates-replaced-with-actiont"></a>Заменить действие пользовательских делегатов<T>

В **единой** несколько простых (например, один параметр) были заменены делегаты .net `Action<T>`. Например,

    public delegate void NSNotificationHandler (NSNotification notification);

Теперь можно использовать как `Action<NSNotification>`. Этот код promote повторного использования и снижения дублирования кода внутри Xamarin.iOS и собственных приложений.

## <a name="taskbool-replaced-with-taskbooleannserror"></a>Задача<bool> заменены задачи < логическое значение, NSError >>

В **классический** возникли некоторые асинхронные API-интерфейсы возврат `Task<bool>`. Однако некоторые из них где можно использовать, когда `NSError` был частью сигнатуры, т. е. `bool` уже был `true` и было необходимо перехватывать исключения, чтобы получить `NSError`.

Поскольку некоторые ошибки являются очень часто и возвращаемое значение не может быть полезным этот шаблон был изменен в **единой** для возврата `Task<Tuple<Boolean,NSError>>`. Это позволяет проверить успешность и любые ошибки, может произойти во время асинхронного вызова.

## <a name="nsstring-vs-string"></a>Строка NSString vs

В некоторых случаях была изменена из некоторых констант `string` для `NSString`, например `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

В целом мы предпочтительно использовать .NET `System.String` типа. Тем не менее, несмотря на рекомендации Apple некоторые API-Интерфейсы собственного сравнение константы указателей (но не саму строку) и это работают, только когда мы предоставляем константы как `NSString`.

 <a name="protocols" />

## <a name="objective-c-protocols"></a>Протоколы Objective c.

Исходный MonoTouch не имеет полную поддержку для протоколов ObjC и некоторых, не является оптимальным, API-интерфейса были добавлены для поддержки наиболее распространенным сценарием. Это ограничение больше не существует, но для обеспечения обратной совместимости несколько интерфейсов API хранятся вокруг внутри `monotouch.dll` и `XamMac.dll`.

Эти ограничения были удалены и очистить на единой API-интерфейсы. Большинство изменений будут выглядеть следующим образом:

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I` Префикса означает **единой** предоставления интерфейса, вместо конкретного типа, для протокола ObjC. Это облегчит не место подкласс конкретного типа, предоставленный Xamarin.iOS вариантов.

Его также можно использовать некоторые API для более точного и простые в использовании, например:

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Такие API, теперь проще (США), без refering документации и дополнение кода в интегрированной среде разработки, вы получите более полезным предложения на основании протокола или интерфейс.

### <a name="nscoding-protocol"></a>Протокол NSCoding

Наш исходное привязки включены .ctor(NSCoder) для каждого типа -, даже если он не поддерживает `NSCoding` протокола.  Один `Encode(NSCoder)` метод присутствовал в `NSObject` кодируемый объект.
Однако этот метод будет работать, только если экземпляр согласование протокола NSCoding.

API-интерфейсы единой были устранены это.  Новые сборки будут только `.ctor(NSCoder)` Если соответствует тип `NSCoding`. Теперь есть такие типы `Encode(NSCoder)` метод, который соответствует `INSCoding` интерфейса.

Слабо влияет: В большинстве случаев это изменение не влияет на приложения как старые, удалено, конструкторы не может быть использована.

## <a name="further-tips"></a>Дополнительные советы

Дополнительные изменения, которые следует учитывать, перечислены в [советы для обновления приложения для API единой](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Пример кода

Начиная с 31 июля Корпорация Майкрософт опубликовала порты примеров операций ввода-вывода, которые этот новый API на `magic-types` ветвление в [monotouch образцы](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Для Mac, выполняется проверка образцов в обоих [образцы mac](https://github.com/xamarin/mac-samples) репозитория (отображение новых API в Mavericks или Yosemite), а также примеры 32 или 64-разрядных ветви magic типы [образцы mac](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Обновление приложения Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Обновление приложений Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Обновление привязки](~/cross-platform/macios/unified/update-binding.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Обновление советы](~/cross-platform/macios/unified/updating-tips.md)
- [Классический vs отличия единой API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
