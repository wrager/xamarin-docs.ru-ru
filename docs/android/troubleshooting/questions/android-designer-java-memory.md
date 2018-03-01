---
title: "Настройка параметров памяти Java для конструктора Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 354a750fc57fd28754aea902d293e75b65d63997
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Настройка параметров памяти Java для конструктора Android

Параметры памяти по умолчанию, которые будут использоваться при запуске `java` обработать конструктора Android могут быть несовместимы с некоторых конфигураций системы.

Начиная с Xamarin Studio 5.7.2.7 (и более поздней версии, Visual Studio для Mac) и Xamarin для Visual Studio 3.9.344 эти параметры можно настроить отдельно для каждого проекта.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Новые свойства Android конструктора и соответствующие параметры Java

Следующие имена свойств соответствуют указанной java [параметр командной строки](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Откройте решение в Visual Studio.

2.  В обозревателе решений выберите проект Android каждого по одному, а затем нажмите кнопку [Показать все файлы](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) дважды для каждого проекта. Можно пропустить, проекты, которые не содержат `.axml` файлы макета. Этот шаг будет гарантировать, что каждый каталог проекта содержит `.csproj.user` файла.

3.  Выйдите из Visual Studio.

4.  Найдите `.csproj.user` файл для каждого проекта из шага 2.

5.  Изменить каждый `.csproj.user` файл в текстовом редакторе.

6.  Добавьте одно или все свойства Android конструктора памяти в пределах `<PropertyGroup>` элемента. Можно использовать существующий `<PropertyGroup>` или создайте новую. Ниже приведен полный пример `.csproj.user` файл, который содержит все атрибуты 3 установить значения по умолчанию:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  Сохраните и закройте все обновленного `.csproj.user` файлов.

8.  Перезапустите Visual Studio и повторно откройте решение.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1.  Откройте решение в Visual Studio для Mac, чтобы убедиться в нужном каталоге решений содержит `.userprefs` файла.

2.  Закройте Visual Studio для Mac.

3.  Найдите `.userprefs` файл в каталоге решения.

4.  Изменить `.userprefs` файл в текстовом редакторе.

5.  Найдите элемент XML в следующем формате. Последняя часть имени этого элемента будут соответствовать имени проекта: «AndroidApplication1» в этом примере:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Если `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` элемент не существует, создайте его в любом месте во включающем `<Properties>` элемента. Обязательно замените «AndroidApplication1» с именем проекта.

7.  Добавьте одно или все новые свойства Android конструктора памяти как атрибуты элемента. Ниже приведен полный пример `.userprefs` файл, который содержит все атрибуты 3 установить значения по умолчанию:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  Повторите шаги 5 – 7 для каждого проекта Android в решении, которое содержит любые `.axml` файлы макета. (То есть, добавьте один `<MonoDevelop.Ide.ItemProperties.ProjectName>` элемент для каждого проекта.)

9.  Сохраните и закройте `.userprefs` файла.

10. Перезапустите Visual Studio для Mac и повторно откройте решение.

-----

