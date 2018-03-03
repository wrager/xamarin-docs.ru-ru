---
title: "Обновление существующих приложений Xamarin.Forms"
description: "Выполните следующие действия для обновления существующего приложения Xamarin.Forms использование API единой и обновление до версии 1.3.1"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 52b53618e23a47884bee6cb821d85b15d759968c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Обновление существующих приложений Xamarin.Forms

_Выполните следующие действия для обновления существующего приложения Xamarin.Forms использование API единой и обновление до версии 1.3.1_


> [!IMPORTANT]
> Поскольку Xamarin.Forms 1.3.1 первый выпуск, поддерживающий API единой, всего решения должен быть обновлен для использования последней версии в то же время, как миграция приложения iOS в единой. Это означает, что наряду с обновлением для поддержки единого проект iOS, также необходимо изменить код в _всех_ проектов в решении.



Обновление выполняется в два этапа:

1. Перенос приложения iOS единой API, с помощью Visual Studio для построения Mac в средство миграции.

    - Используйте средство миграции автоматически обновлять проект.

    - Обновление iOS собственного API-интерфейсы, как описано в инструкциях для [обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md) (в частности в коде службы пользовательского модуля подготовки отчетов или зависимостей).

2. Обновите все решение Xamarin.Forms версии 1.3.

    1. Установите пакет NuGet Xamarin.Forms 1.3.1.

    2. Обновление `App` класса в общий код.

    3. Обновление `AppDelegate` в проекте iOS.

    4. Обновление `MainActivity` в проекте Android.

    5. Обновление `MainPage` в проекте Windows Phone.


# <a name="1-ios-app-unified-migration"></a>1. iOS приложения (единой миграции)

Миграции, требуется обновление до версии 1.3, который поддерживает API единой Xamarin.Forms. Чтобы правильные ссылки на сборки должен быть создан необходимо сначала обновить проект iOS для использования API единой.

## <a name="migration-tool"></a>Средство миграции

Если щелкнуть проект iOS, чтобы он установлен, нажмите кнопку **проекта > Миграция в единой API Xamarin.iOS...**  и принимайте предупреждающего сообщения.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Выберите проект >... перенести API единой Xamarin.iOS и соглашаетесь предупреждающего сообщения")

Это будет автоматически:

 - Измените тип проекта для поддержки API единой 64-разрядной.
 - Изменить ссылку на framework **Xamarin.iOS** (заменив старые **monotouch** ссылки).
 - Изменение ссылки на пространства имен в коде, чтобы удалить `MonoTouch` префикс.
 - Обновление **csproj** файла для использования API единой правильное построение целевых объектов.

**Очистить** и **построения** проекта, чтобы нет ошибок для исправления. Никаких дополнительных действий не требуется. Далее эти шаги рассматриваются более подробно в [docs единой API](~/cross-platform/macios/unified/updating-ios-apps.md).

## <a name="update-native-ios-apis-if-required"></a>Обновить машинным кодом iOS API-интерфейсы (при необходимости)

При добавлении дополнительных операций ввода-вывода машинный код (например, пользовательские модули подготовки отчетов или служб зависимостей) может потребоваться внесите исправления дополнительный код вручную. Повторно скомпилируйте приложение и ссылаться на [iOS обновление существующего приложения инструкции](~/cross-platform/macios/unified/updating-ios-apps.md) Дополнительные сведения об изменениях, которые могут потребоваться. [Эти советы](~/cross-platform/macios/unified/updating-tips.md) также поможет выявить изменения, которые необходимы.


# <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 обновления

После обновления приложения iOS в единой API, остальная часть решения необходимо обновить до версии 1.3.1 Xamarin.Forms. В том числе следующее:

 - Обновление пакета Xamarin.Forms NuGet в каждом проекте.
 - Изменение кода, чтобы использовать новый Xamarin.Forms `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android), и `FormsApplicationPage` классы (Windows Phone).

Эти действия описаны ниже:


## <a name="21-update-nuget-in-all-projects"></a>2.1 обновление NuGet во всех проектах

Обновление Xamarin.Forms 1.3.1 предварительной версии с помощью диспетчера пакетов NuGet для всех проектов в решении: PCL (при его наличии), iOS, Android и Windows Phone. Рекомендуется, которую **удалить и снова добавить** пакет Xamarin.Forms NuGet для обновления до версии 1.3.

**Примечание:** Xamarin.Forms версии 1.3.1 в настоящее время *предварительной версии*. Это означает, что необходимо выбрать **предварительного выпуска** в диалоговом окне NuGet через (деления — окно в Visual Studio для Mac) или раскрывающегося списка списка в Visual Studio для просмотра последней предварительной версии.


> [!IMPORTANT]
> Если вы используете Visual Studio, убедитесь, что установлена последняя версия диспетчера пакетов NuGet. Более старые версии NuGet в Visual Studio не устанавливается правильно единой версии Xamarin.Forms 1.3.1. Последовательно выберите пункты **Сервис > расширения и обновления...**  и выберите команду **установленные** список, чтобы убедиться, что **диспетчера пакетов NuGet для Visual Studio** является как минимум версия 2.8.5. Если она была создана ранее, щелкните **обновления** списка, чтобы загрузить последнюю версию.



После обновления пакета NuGet Xamarin.Forms 1.3.1 внести следующие изменения в каждом проекте для обновления до нового `Xamarin.Forms.Application` класса.

## <a name="22-portable-class-library-or-shared-project"></a>2.2 Переносимая библиотека классов (или в другом проекте)

Изменение **App.cs** файл, чтобы:

 - `App` Класс теперь наследует от `Application`.
 - `MainPage` Свойству первой страницы содержимого, которые следует отобразить.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Мы удалили полностью `GetMainPage` метода и вместо этого задайте `MainPage` *свойство* на `Application` подкласс.

Этот новый `Application` базовый класс также поддерживает `OnStart`, `OnSleep`, и `OnResume` переопределений, которые помогают управлять жизненным циклом приложения.

`App` Класса передается в новый `LoadApplication` метод в каждом проекте приложения, как описано ниже:


## <a name="23-ios-app"></a>2.3 приложение iOS


Изменение **AppDelegate.cs** файл, чтобы:

 - Этот класс наследует от `FormsApplicationDelegate` (а не `UIApplicationDelegate` ранее).
 - `LoadApplication` вызывается для нового экземпляра `App`.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```


## <a name="23-android-app"></a>2.3 приложение android

Изменение **MainActivity.cs** файл, чтобы:

 - Этот класс наследует от `FormsApplicationActivity` (а не `FormsActivity` ранее).
 - `LoadApplication` вызывается для нового экземпляра `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```


## <a name="24-windows-phone-app"></a>2.4 приложения Windows Phone

Необходимо обновить **MainPage** -XAML и код поддержки.

Изменение **MainPage.xaml** файл, чтобы:

 - Корневой элемент XAML должен быть `winPhone:FormsApplicationPage`.
 - `xmlns:phone` Атрибут должен иметь *изменить* для `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Ниже приведен пример обновленные — необходимо только изменить эти действия (остальные атрибуты должны остаться прежними):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```


Изменение **MainPage.xaml.cs** файл, чтобы:

 - Этот класс наследует от `FormsApplicationPage` (а не `PhoneApplicationPage` ранее).
 - `LoadApplication` вызывается для нового экземпляра Xamarin.Forms `App` класса. Необходимо полностью определить эту ссылку, так как Windows Phone собственный `App` класс уже определен.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

## <a name="troubleshooting"></a>Устранение неполадок

Иногда появится сообщение об ошибке следующего вида после обновления пакета Xamarin.Forms NuGet. Это происходит, когда средство обновления NuGet не полностью удаляет ссылки на более старых версий из вашего **csproj** файлов.

>ВАШ\_PROJECT.csproj: ошибка: данный проект ссылается на пакеты NuGet, отсутствующие на этом компьютере. Включите восстановление пакетов NuGet скачать их.  Дополнительные сведения см. в разделе http://go.microsoft.com/fwlink/?LinkID=322105. Отсутствующий файл является... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (ВАШ\_ПРОЕКТА)

Чтобы устранить эти ошибки, откройте **csproj** файл в текстовом редакторе и поищите `<Target` элементы, которые ссылаются на более старые версии Xamarin.Forms, такие как элемент, показанный ниже. Следует вручную удалить этот весь элемент из **csproj** файл и сохраните изменения.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

После удаления этих старые ссылки проекта будет выполнена успешно.

# <a name="considerations"></a>Особенности

Следующие факторы следует принимать во внимание при преобразовании существующего проекта Xamarin.Forms классический API в новый интерфейс API единой Если приложение зависит от компонента или пакета NuGet.

## <a name="components"></a>Компоненты

Любой компонент, который включен в приложении также необходимо обновить в единой API или при компиляции возникнет конфликт. Для любого компонента включено заменить текущую версию с новой версией в магазине Xamarin компонента, поддерживающий API единой и выполните чистую сборку. Любой компонент, который еще не был преобразован автором, будут отображаться только предупреждение в хранилище компонентов 32-разрядная.


## <a name="nuget-support"></a>Поддержка NuGet

Пока мы публиковать изменения NuGet для работы с поддержкой единого API, не нового выпуска NuGet, поэтому мы оценке способы получения NuGet распознавать новые интерфейсы API.

До этого времени, так же, как компоненты необходимо для переключения любой пакет NuGet, включенных в версию, которая поддерживает API-интерфейсы единого проект и затем выполните чистую сборку.

> [!IMPORTANT]
> **Примечание:** при наличии ошибки в виде _«ошибка 3 не может включать в том же проекте Xamarin.iOS «monotouch.dll» и «Xamarin.iOS.dll» — «Xamarin.iOS.dll» упоминается явно, пока ссылается «monotouch.dll» "xxx Версия = 0.0.000, язык и региональные параметры = neutral, PublicKeyToken = null'»_ после преобразования приложения единой API-интерфейсам, это обычно из-за наличия компонента или пакета NuGet в проект, который не был обновлен для API единой. Необходимо удалить существующий компонент/NuGet, обновите версию, которая поддерживает API-интерфейсы единой и выполните чистую сборку.




# <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Включение 64-разрядных сборок приложения Xamarin.iOS

Для мобильного приложения Xamarin.iOS, преобразованное в единой API разработчику по-прежнему необходимо позволяют строить приложения для 64-разрядных компьютерах из параметров приложения. См. в разделе **Включение 64 разрядных сборок из Xamarin.iOS приложений** из [анализ 32 или 64-разрядной платформе](~/cross-platform/macios/32-and-64.md#enable-64) документа подробные инструкции по включению 64-разрядных сборок.

# <a name="summary"></a>Сводка

Xamarin.Forms приложения теперь должны быть обновлены до версии 1.3.1 и переносом приложения iOS единой API (который поддерживает 64-разрядной архитектуры на платформе iOS).

Как отмечалось выше, если приложение Xamarin.Forms включает машинного кода, такие как пользовательские модули подготовки отчетов или зависимостей служб, а затем они также может потребоваться обновление для использования новых типов [появилась в API единой](~/cross-platform/macios/index.md).


## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-apps.md)
- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Обновление советы](~/cross-platform/macios/unified/updating-tips.md)
- [Классический vs отличия единой API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
