---
title: Советы по обновлению кода до унифицированный интерфейс API
description: В этом документе рассматриваются наиболее распространенных ошибок и различные подсказки полезны при обновлении приложения для использования API единой Xamarin.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
author: asb3993
ms.author: amburns
ms.openlocfilehash: cab27d5dc38eeab65728f242c6f11fd445601a88
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782122"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Советы по обновлению кода до унифицированный интерфейс API

С помощью средства автоматической миграции в Visual Studio для iOS и Mac проекты для ссылки единой API (Xamarin.iOS.dll или Xamarin.Mac.dll) и внести много изменений необходимый код автоматически преобразует Mac (в разделе [Xamarin Studio выпуска заметки статьи «Классический средству миграции кода единой API»](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) конкретные сведения).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Не удалось найти NSInvalidArgumentException раскадровки ошибки

Отсутствует [ошибки](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) в текущей версии Visual Studio для Mac, которая может возникнуть после использования средства автоматической миграции для преобразования проекта в единой API-интерфейсы. После обновления, если вы получаете сообщение об ошибке в форме:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

Необходимо выполнить следующую команду, чтобы решить эту проблему, найдите этот целевой файл сборки действий.

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

В этом файле требуется найти в следующем объявлении цели:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

И добавьте `DependsOnTargets="_CollectBundleResources"` к нему атрибут. Пример:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Сохраните файл, перезагрузить Visual Studio для Mac и выполнить чистую & перестроения проекта. Решением этой проблемы необходимо освободить вскоре с Xamarin.

## <a name="useful-tips"></a>Полезные советы

После использования средства миграции, по-прежнему возможны некоторые ошибки компилятора ручного вмешательства.
Некоторые элементы, которые, возможно, потребуется вручную исправить включают:

* Сравнение `enum`s может потребоваться `(int)` приведения.

* `NSDictionary.IntValue` Теперь возвращает `nint`, имеется `Int32Value` может использоваться вместо этого.

* `nfloat` и `nint` типы не могут быть помечены `const`;   `static readonly nint` является альтернативой разумного.

* Действия, которые ранее были непосредственно в `MonoTouch.` стали обычно в пространстве имен `ObjCRuntime.` пространства имен (например: `MonoTouch.Constants.Version` теперь `ObjCRuntime.Constants.Version`).

* Код, который выполняет сериализацию объектов может прерываться при попытке сериализации `nint` и `nfloat` типов. Обязательно убедитесь, что код сериализации работает должным образом после миграции.

* Иногда Автоматизированное средство промахов код внутри `#if #else` директивы условной компиляции. В этом случае необходимо вручную внести исправления (см. ниже распространенные ошибки).

* Вручную экспортированных методов, с помощью `[Export]` может не быть устранена автоматически с помощью средства миграции, например в snippert этот код, необходимо вручную обновить тип возвращаемого значения для `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Так как преобразование без потери данных не API единой не предоставляет неявное преобразование между NSDate и даты и времени .NET. Чтобы избежать ошибок, связанных с `DateTimeKind.Unspecified` преобразования .NET `DateTime` для локальных или в формате UTC до приведения к `NSDate`.

 * Objective-C категории методов теперь будут создаваться как методы расширения в единой API. Например, код, который ранее использовал `UIView.DrawString` теперь ссылка `NSString.DrawString` в единой API.

 * Код с использованием классов AVFoundation `VideoSettings` следует изменить для использования `WeakVideoSettings` свойство. Для этого необходимо `Dictionary`, который доступен как свойство классы параметров, например:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject `.ctor(IntPtr)` конструктор был изменен с открытым для защищенного ([чтобы предотвратить неправильное использование](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

 * `NSAction` было [заменить](~/cross-platform/macios/unified/overview.md#NSAction) с starndard .NET `Action`. Некоторые делегаты простой (один параметр) также были заменены `Action<T>`.

Наконец, см. [отличия API единой классический v](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) для поиска изменения для API-интерфейсов в коде. Поиск [эту страницу](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) поможет найти классический API и что они были обновлены, чтобы.

**Примечание:** `MonoTouch.Dialog` пространство имен остается неизменным после миграции. Если ваш код использует **MonoTouch.Dialog** следует по-прежнему использовать это пространство имен — сделать *не* изменить `MonoTouch.Dialog` для `Dialog`!

## <a name="common-compiler-errors"></a>Типичные ошибки компилятора

Ниже перечислены другие примеры распространенных ошибок и решение:

**Ошибки CS0012: Тип «MonoTouch.UIKit.UIView» определен в сборке, которая не используется.**

Исправление: Это обычно означает, что проект ссылается на компонент или пакет NuGet, не была построена с помощью единого интерфейса API. Следует удалить и повторно добавить все компоненты и NuGet пакетов. Если это не устранит ошибку, внешние библиотеки может пока не поддерживает единый интерфейс API.

**Ошибка MT0034: Не может содержать «monotouch.dll» и «Xamarin.iOS.dll» в том же проекте Xamarin.iOS - «Xamarin.iOS.dll» упоминается явно, пока ссылается «monotouch.dll» "Xamarin.Mobile, версия = 0.6.3.0, язык и региональные параметры = neutral, PublicKeyToken = null ".**

Исправление: Удалить компонент, который вызывает эту ошибку, а затем добавьте в проект.

**Ошибка CS0234: Имя типа или пространства имен «Foundation» не существует в пространстве имен «MonoTouch». Возможно, отсутствует ссылка на сборку?**

Исправление: Автоматизированные средства миграции в Visual Studio для Mac *следует* обновить все `MonoTouch.Foundation` ссылки на `Foundation`, однако в некоторых случаях их потребуется обновить вручную. Для других пространств ранее в аналогичную ошибки могут отображаться `MonoTouch`, такие как `UIKit`.

**Ошибка CS0266: Не удается неявно преобразовать тип «double» для «System.float»**

Исправление: изменение типа и приведения `nfloat`. Эта ошибка также может возникать для других типов с 64-разрядные эквиваленты (такие как `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Ошибка CS0266: Не удается неявно преобразовать тип «CoreGraphics.CGRect» в «System.Drawing.RectangleF». Существует явное преобразование (возможно, отсутствует приведение)**

Исправление: Изменение экземпляров для `RectangleF` для `CGRect`, `SizeF` для `CGSize`, и `PointF` для `CGPoint`. Пространство имен `using System.Drawing;` необходимо заменить `using CoreGraphics;` (если она еще не существует).

**Ошибка CS1502: наиболее подходящий перегруженный метод для "CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])" имеет некоторые недопустимые аргументы**

Исправление: Измените тип массива, чтобы `nfloat[]` и явного приведения `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Ошибка CS0115: «WordsTableSource.RowsInSection (UIKit.UITableView, int)» отмечен как переопределение, но не найден подходящий метод для переопределения**

Исправление: Изменить возвращаемое значение и параметр типы, которые должны `nint`. Это обычно происходит в переопределения методов, например на `UITableViewSource`, в том числе `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`и т. д.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Ошибка CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView) "**

Исправление: Если тип возвращаемого значения меняется на `nint`, привести возвращаемое значение `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Ошибка CS1061: Тип «CoreGraphics.CGPath» не содержит определение для «AddElipseInRect»**

Исправление: Проверка орфографии для `AddEllipseInRect`. Другие изменения имени включают:

* Измените «Color.Black» `NSColor.Black`.
* Измените MapKit «AddAnnotation» `AddAnnotations`.
* Измените AVFoundation «DataUsingEncoding» `Encode`.
* Измените AVFoundation «AVMetadataObject.TypeQRCode» `AVMetadataObjectType.QRCode`.
* Измените AVFoundation «VideoSettings» `WeakVideoSettings`.
* Изменить PopViewControllerAnimated для `PopViewController`.
* Измените CoreGraphics «CGBitmapContext.SetRGBFillColor» `SetFillColor`.

**Ошибка CS0546: невозможно переопределить, поскольку «MapKit.MKAnnotation.Coordinate» не имеет функции доступа set (CS0546)**

При создании пользовательской заметкой путем создания подклассов MKAnnotation координат поле имеет нет метода задания только метод считывания.

[Исправьте](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Добавление поля для отслеживания координаты
* возвращать это поле в методе считывания свойства координат
* Переопределите метод SetCoordinate и задайте полю
* Вызов SetCoordinate в вашей ctor с параметром переданный координат

Он должен выглядеть следующим образом:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений](~/cross-platform/macios/unified/updating-apps.md)
- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Обновление приложения Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Обновление приложений Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Обновление привязки](~/cross-platform/macios/unified/update-binding.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Классический vs отличия единой API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
