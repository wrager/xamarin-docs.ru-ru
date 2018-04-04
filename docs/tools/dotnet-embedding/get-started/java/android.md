---
title: Приступая к работе с Android
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: ed3d6ae3f8537bfae456c6919743e8c9fbfb2009
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-android"></a>Приступая к работе с Android


Помимо требований к из наших [Приступая к работе с Java](~/tools/dotnet-embedding/get-started/java/index.md) руководства, необходимо:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) или более поздней версии
* [Android Studio 3.x](https://developer.android.com/studio/index.html) с Java 1.8

Как обзор произойдет следующее.

* Создание проекта библиотеки Android C#
* Установка .NET внедрения с помощью NuGet
* Запустите Embeddinator на сборку библиотеки Android
* Используйте созданный файл AAR в проекте Java в Android Studio

## <a name="create-an-android-library-project"></a>Создайте проект библиотеки Android

Откройте Visual Studio для Windows или Mac, создайте новый проект библиотеки классов Android, назовите его `hello-from-csharp`и сохраните файл в `~/Projects/hello-from-csharp` или `%USERPROFILE%\Projects\hello-from-csharp`.

Добавить новое действие Android с именем `HelloActivity.cs`, а затем макета, Android `Resource/layout/hello.axml`.

Добавьте новый `TextView` макет и изменение текста на что-нибудь приятной.

Источник макет должен выглядеть примерно следующим образом:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

В действии, убедитесь, что вы вызываете `SetContentView` с новому макету:
```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```
*Примечание: не забудьте `[Register]` см. в разделе сведения в разделе ограничения*

Выполните построение проекта, будут сохранены в результирующую сборку `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Установка .NET внедрение из NuGet

Выберите **Добавить > Добавление пакетов NuGet...**  и установить `Embeddinator-4000` из диспетчера пакетов NuGet: ![диспетчера пакетов NuGet](android-images/visualstudionuget.png)

Будет выполнена установка `Embeddinator-4000.exe` в `packages/Embeddinator-4000/tools` каталога.

## <a name="run-embeddinator-4000"></a>Запустите Embeddinator 4000

Мы добавим на этапе после построения для запуска Embeddinator и создать собственный файл AAR для сборки проекта библиотеки Android.

В Visual Studio для Mac перейдите к _параметры проекта > сборки > команд Custom_ и добавьте _после построения_ шаг.

Настройка следующих commnd:
```
mono '${SolutionDir}/packages/Embeddinator-4000.0.2.0.80/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

> [!NOTE]
> Примечание: Убедитесь, что для использования номера версии, установленные из NuGet

Если вы собираетесь выполнять текущей работы над проектом C#, можно также добавить пользовательскую команду для очистки `output` каталог до выполнения Embeddinator:
```
rm -Rf '${SolutionDir}/output/'
```

![Действие настраиваемого построения](android-images/visualstudiocustombuild.png)

Будут помещены в файл Android AAR `~/Projects/hello-from-csharp/output/hello_from_csharp.aar`. _Примечание: дефисы заменяются, так как Java не поддерживается в именах пакетов._

### <a name="running-net-embedding-on-windows"></a>Под управлением .NET встраивание в Windows

Фактически мы настроит то же самое, но меню в Visual Studio несколько отличается в Windows. Команды оболочки также могут немного отличаться.

Последовательно выберите пункты **параметры проекта > события построения** и введите следующий текст в **Командная строка события после построения** поле:
```
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

Например, следующий снимок экрана:

![Embeddinator в Windows](android-images/visualstudiowindows.png)

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Использовать созданные выходные данные в проекте Android Studio

1. Откройте Android Studio и создайте новый проект с **пустое действие**.
2. Щелкните правой кнопкой мыши ваш `app` модуль и выберите **Создать > модуль**.
3. Выберите **импорта. JAR. Пакет AAR**.
4. Используйте обозреватель каталогов для поиска `~/Projects/hello-from-csharp/output/hello_from_csharp.aar` и нажмите кнопку **Готово**.

![Импорт AAR в Android Studio](android-images/androidstudioimport.png)

Файл AAR будут скопированы в новый модуль с именем `hello_from_csharp`.

![Проект Android Studio](android-images/androidstudioproject.png)

Чтобы использовать новый модуль из вашего `app`, щелкните правой кнопкой мыши и выберите **открыть параметры модуля**. На **зависимости** добавьте новый **зависимостей модуля** и выберите `:hello_from_csharp`.

![Android Studio зависимости](android-images/androidstudiodependencies.png)

В действии, добавьте новый `onResume` метод и используйте следующий код, чтобы запустить наши действия C#:

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Сжатие сборки *внимание!*

Один дальнейших изменений не требуется для внедрения .NET в проекте Android Studio.

Откройте ваше приложение `build.gradle` и добавьте следующее изменение:
```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```
Xamarin.Android в данный момент загружает сборки .NET напрямую из APK, но он требует сборки, не сжимаются.

Если вы не настроен, приложения приводит к сбою при запуске и на консоль выводятся примерно следующим образом:

```csharp
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Запуск приложения

При запуске приложения:

![Hello из примера для C# в эмуляторе](android-images/hello-from-csharp-android.png)

Обратите внимание, что произошло здесь:

* У нас есть класс C#, `HelloActivity`, что подклассы Java
* У нас есть файлы ресурсов Android
* Мы использовали из Java в Android Studio

Чтобы этот образец работал, выполняются все следующие настройки в окончательный APK:

* Xamarin.Android настраивается на запуск приложения
* Сборки .NET, включенные в `assets/assemblies`
* `AndroidManifest.xml` изменения для действия C#, и т. д.
* Android ресурсы и ресурсам с помощью библиотеки .NET
* [Android с помощью вызываемых оболочек](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/android_callable_wrappers/) для любого `Java.Lang.Object` подкласс

Если вам нужны дополнительные Пошаговое руководство, ознакомьтесь в этом видео, внедрение Чарльз Петцольд [FingerPaint Демонстрация](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) в Android Studio проектов здесь:

[![Embeddinator-4000 для Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>С помощью Java 1.8

На момент написания этой лучше всего использовать Android Studio 3.0 ([Загрузите здесь](https://developer.android.com/studio/index.html)).

Чтобы включить Java 1.8 в модуле приложения `build.gradle` файла:
```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
Вы также можете взглянуть на нашем тестовый проект Android Studio [здесь](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) для получения дополнительных сведений.

Если желающие использовать стабильная 2.3.x Android Studio, необходимо включить устаревшие гнездо инструментов:
```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Текущие ограничения на Android

В данный момент в том случае, если вы подкласс `Java.Lang.Object`, Xamarin.Android создаст заглушку Java (Android вызываемую оболочку) вместо Embeddinator.

Поэтому необходимо выполнить экспорт C# в Java как Xamarin.Android тем же правилам.

Так, например в C#:
```csharp
    [Register("mono.embeddinator.android.ViewSubclass")]
    public class ViewSubclass : TextView
    {
        public ViewSubclass(Context context) : base(context) { }

        [Export("apply")]
        public void Apply(string text)
        {
            Text = text;
        }
    }
```

* `[Register]` требуется для сопоставления с требуемое имя пакета Java
* `[Export]` необходимо, чтобы сделать метод видимым для Java

Мы можем использовать `ViewSubclass` в Java следующим образом:
```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Дополнительные сведения о [Java интеграция с Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Несколько сборок

Внедрение в одну сборку понятным. Однако это гораздо более вероятно, будет иметь более одного сборки C#. Сколько раз будет иметь зависимости от пакетов NuGet, таких как поддержка Android библиотеки или службы Google Play, дальнейшей усложнить вещи.

В результате дилемму, так как Embeddinator должен включать множество типов файлов в окончательный AAR, такие как:
* Android активы
* Android ресурсы
* Android собственные библиотеки
* Источник Android java

Скорее всего не требуется включить эти файлы из библиотеки поддержки Android или службы Google Play в вашей AAR, но в Android Studio предпочитаете использовать официальной версии от Google.

Ниже приведен рекомендуемый подход.
* Передайте Embeddinator любую сборку, вы являетесь владельцем (иметь источник) и требуется вызвать из Java
* Передайте Embeddinator любую сборку, необходимо Android, собственные библиотеки и ресурсы из
* Добавить поддержку Java зависимости, такие как Android библиотеки или службы Google Play в Android Studio

Поэтому может иметь команду:
```
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```
Никаких действий следует исключить из NuGet, если можно определить в ней Android активы, ресурсы, т. д., которые понадобятся в проекте Android Studio. Можно также опустить зависимости, которые необходимо вызывать из Java и компоновщик _следует_ включают части библиотеки, которые необходимы.

Чтобы добавить все зависимости Java, необходимые в Android Studio вашей `build.gradle` файл может выглядеть так:
```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Дополнительные сведения

* [Обратные вызовы на Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Предварительное исследование Android](~/tools/dotnet-embedding/android/index.md)
* [Ограничения Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)


## <a name="related-links"></a>Связанные ссылки

- [Пример Weather (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
