---
title: Удаление Xamarin
description: Удаление продуктов Xamarin с компьютера
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 98c10aae9e281600201570ea1fc8bb023286164d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="uninstalling-xamarin"></a>Удаление Xamarin

> [!IMPORTANT]
> В этой статье описывается удаление Xamarin Studio или других продуктов Xamarin с компьютера Mac или Windows. Сведения об удалении Visual Studio для Mac см. в руководстве по [удалению](https://docs.microsoft.com/visualstudio/mac/uninstall) на веб-сайте docs.microsoft.com.

Существует несколько продуктов Xamarin, обеспечивающих разработку кроссплатформенных приложений, включая автономные приложения, такие как Xamarin Studio, и расширения для других приложений, такие как поддержка Xamarin в Visual Studio.

В этом руководстве содержатся сведения об удалении Xamarin из macOS или из Visual Studio в Windows:

1.  [Удаление Xamarin Studio](#uninstallxamarinstudio)
  1.  [Удаление Mono](#uninstallmono)
  1.  [Удаление Xamarin.Android](#uninstallandroid)
  1.  [Удаление Xamarin.iOS](#uninstallios)
  1.  [Удаление Xamarin.Mac](#uninstallmac)
  2.  [Удаление Inspector и Workbooks](#uninstallworkbooks)
1.  [Удаление Xamarin из Windows](#uninstallwindows)
  1.  [Удаление Xamarin из Visual Studio 2015 и более ранних версий](#uninstallvs2015)
  1.  [Удаление Xamarin из Visual Studio 2017](#uninstallvs2017)
1.  [Удаление Visual Studio для Mac](#uninstallvsmac)

Если необходимо переустановить Xamarin с помощью универсального установщика, всегда рекомендуется сначала перезагрузить компьютер.

## <a name="uninstalling-xamarin-on-mac"></a>Удаление Xamarin из Mac

Это руководство можно использовать при раздельном удалении продуктов, перейдя в соответствующий раздел. Чтобы удалить весь набор инструментов Xamarin, выполните это руководство от начала и до конца.

Сведения об использовании [скрипта удаления](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) см. в разделе [Использование скрипта удаления](#uninstallscript) в нижней части этого руководства.

<a name="uninstallxamarinstudio" />

### <a name="uninstall-xamarin-studio"></a>Удаление Xamarin Studio

Первый шаг в удалении Xamarin Studio из Mac заключается в поиске файла **Xamarin Studio.app** в каталоге **/Applications** и перетаскивании его в **корзину**. Можно также щелкнуть правой кнопкой мыши и выбрать пункт **Переместить в удаленные**, как показано ниже:

 [![](uninstalling-xamarin-images/image1.png "Можно также щелкнуть правой кнопкой мыши и выбрать пункт "Переместить в удаленные", как показано ниже")](uninstalling-xamarin-images/image1.png#lightbox)

Удаление этого набора приложений приведет к удалению Xamarin Studio, однако в файловой системе останутся другие файлы, связанные с Xamarin.

Чтобы удалить все следы Xamarin Studio, нужно выполнить в терминале следующие команды:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Удаление Mono SDK (MDK)

Mono представляет собой реализацию .NET Framework Майкрософт с открытым исходным кодом и используется всеми продуктами Xamarin (Xamarin.iOS, Xamarin.Android и Xamarin.Mac), чтобы обеспечить разработку на C# в рамках этих платформ.

> [!IMPORTANT]
> Есть и другие приложения, не входящие в Xamarin, которые также используют Mono, например Unity. Перед удалением Mono убедитесь в отсутствии зависимостей от нее.

Чтобы удалить платформу Mono с компьютера, выполните следующие команды в терминале:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Удаление Xamarin.Android

Существует несколько элементов, необходимых для установки и использования Xamarin.Android, таких как пакет SDK для Android и пакет SDK для Java. Дополнительные сведения об этих необходимых компонентах см. в руководстве об [установке вручную](https://docs.microsoft.com/visualstudio/mac/installation/).

Для удаления Xamarin.Android используйте следующие команды:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Удаление пакета SDK для Android и пакета SDK для Java

Пакет SDK для Android требуется для разработки приложений Android. Чтобы полностью удалить все части пакета SDK для Android, найдите файл в папке **~/Library/Developer/Xamarin/** и переместите его в **корзину**, как показано ниже:

 [![](uninstalling-xamarin-images/image2.png "Чтобы полностью удалить все части пакета SDK для Android, найдите файл и переместите его в корзину, как показано ниже")](uninstalling-xamarin-images/image2.png#lightbox)

Пакет SDK для Java (JDK) удалять не требуется, так как он уже входит в состав Mac OS X.

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Удаление Xamarin.iOS

Xamarin.iOS позволяет разрабатывать приложения iOS на языках C# и F # в Xamarin Studio для Mac.
Узел сборки Xamarin также автоматически устанавливался вместе с более ранними версиями Xamarin.iOS, чтобы обеспечить разработку приложений iOS в Visual Studio. Чтобы удалить оба этих компонента с компьютера, сделайте следующее:

Выполните следующие команды в терминале для удаления всех файлов Xamarin.iOS из файловой системы:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

#### <a name="uninstall-the-mac-build-host"></a>Удаление узла сборки Mac

Примечание. Узел может быть уже удален, если уже выполнено обновление до Xamarin 4. Чтобы удалить приложение узла сборки, запустите в терминале следующую команду:

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Процесс узла сборки или задание `launchd` все еще может выполняться или прослушивать определенные порты.
Чтобы проверить его состояние, выполните команду `launchctl list | grep com.xamarin.mtvs.buildserver` в терминале.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Удаление Xamarin.Mac

После успешного удаления Xamarin Studio можно удалить продукт Xamarin.Mac и его лицензию с компьютера Mac с помощью двух соответствующих команд:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Удаление Workbooks и Inspector

Следующая команда Bash позволяет удалить Xamarin Inspector и Workbooks версии 1.2.2 и более поздние версии:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Сведения о более ранних версиях см. в руководстве по удалению [Workbooks](~/tools/workbooks/install.md#uninstall-macos).

### <a name="uninstall-the-xamarin-installer"></a>Удаление установщика Xamarin

Чтобы удалить все следы универсального установщика Xamarin, используйте следующие команды:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Использование скрипта удаления

[Скрипт удаления](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) удаляет Xamarin с компьютера. Чтобы использовать скрипт удаления, выполните следующие действия:

1.  Скачайте скрипт удаления и запомните расположение скачанного файла. По умолчанию это каталог **/Downloads**.
1.  Откройте **терминал** и измените рабочий каталог на папку, куда был скачан скрипт:

        $ cd /location/of/file

1. Сделайте скрипт исполняемым и запустите его с помощью **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. Наконец, удалите этот скрипт удаления.

На этом этапе продукт Xamarin должен быть удален с компьютера.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Удаление Xamarin из Windows

<a name="uninstallvs2015" />

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 и более ранние версии

Xamarin можно удалить с компьютера Windows через **панель управления**. Перейдите к элементу **Программы и компоненты** или **Программы > Удаление программы**, как показано ниже:

 [![](uninstalling-xamarin-images/image3.png "Переход к элементу "Программы и компоненты" или "Программы > Удаление программы", как показано здесь")](uninstalling-xamarin-images/image3.png#lightbox) [![](uninstalling-xamarin-images/image3.png "Переход к элементу "Программы и компоненты" или "Программы > Удаление программы", как показано здесь")](uninstalling-xamarin-images/image3.png#lightbox)

Чтобы удалить Xamarin Studio, выберите **Xamarin Studio 5.x.x** в списке программ и щелкните **Удалить**. Чтобы удалить расширение Xamarin для Visual Studio и пакеты SDK, выберите **Xamarin** в списке программ и щелкните **Удалить**. Они показаны на снимке экрана ниже:

 [![](uninstalling-xamarin-images/image4a.png "Они показаны на снимке экрана ниже")](uninstalling-xamarin-images/image4a.png#lightbox)

Эти программы также могут быть удалены полностью со всеми компонентами Xamarin:

-  Android SDK


  [![](uninstalling-xamarin-images/image5.png "Эти программы также могут быть удалены полностью со всеми компонентами Xamarin")](uninstalling-xamarin-images/image5.png#lightbox)
-  GTK#


  [![](uninstalling-xamarin-images/image6.png "Эти программы также могут быть удалены полностью со всеми компонентами Xamarin")](uninstalling-xamarin-images/image6.png#lightbox)
-  Универсальный установщик Xamarin


 [![](uninstalling-xamarin-images/image7.png "Эти программы также могут быть удалены полностью со всеми компонентами Xamarin")](uninstalling-xamarin-images/image7.png#lightbox)
-  Пакет SDK для Java (будьте внимательны при удалении этого пакета, так как могут существовать другие зависимости от него)


 [![](uninstalling-xamarin-images/image8.png "Будьте внимательны при удалении пакета SDK для Java, так как могут существовать другие зависимости от него")](uninstalling-xamarin-images/image8.png#lightbox)

Чтобы полностью удалить Visual Studio, следуйте [инструкциям корпорации Майкрософт](https://msdn.microsoft.com/library/mt720585.aspx).


<a name="uninstallvs2017" />

## <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin можно удалить из Visual Studio 2017 с помощью приложения установщика:

1. Откройте **установщик Visual Studio** из меню **Пуск**.

  [![](uninstalling-xamarin-images/vs2017-01-sml.png "Запуск установщика Visual Studio")](uninstalling-xamarin-images/vs2017-01.png#lightbox)

1. Нажмите кнопку **Изменить**, предварительно выбрав изменяемый экземпляр.

  [![](uninstalling-xamarin-images/vs2017-02-sml.png "Нажатие кнопки "Изменить"")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

1. На вкладке **Рабочие нагрузки** отмените выбор параметра **Разработка мобильных приложений на платформе .NET** (в разделе **Мобильные приложения и игры**).

  [![](uninstalling-xamarin-images/vs2017-03-sml.png "Отмена выбора рабочей нагрузки "Разработка мобильных приложений"")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

1. Нажмите кнопку **Изменить** в правом нижнем углу окна.
1. Установщик удалит компоненты, выбор которых отменен (прежде чем установщик сможет вносить изменения, необходимо закрыть Visual Studio 2017).

  [![](uninstalling-xamarin-images/vs2017-04-sml.png "Нажатие кнопки "Изменить"")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Чтобы удалить отдельные компоненты Xamarin (например, Profiler или Profiler), на шаге 3 перейдите на вкладку **Отдельные компоненты** и отмените их выбор:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Удаление отдельных компонентов")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Чтобы удалить Visual Studio 2017 полностью, в трехстрочном меню рядом с кнопкой **Запустить** выберите пункт **Удалить**.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Удаление Visual Studio полностью")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> При наличии двух (или более) параллельно установленных (SxS) экземпляров Visual Studio (например, версии выпуска и предварительной версии) удаление одного экземпляра может привести к удалению некоторых компонентов Xamarin из других экземпляров Visual Studio, включая:
>
> - Xamarin Profiler
> - Xamarin Workbooks или Inspector
> - Симулятор iOS удаленной работы для Xamarin
> - Пакет SDK для Apple Bonjour
>
> При определенных условиях удаление одного из параллельно установленных экземпляров может привести к некорректному удалению этих компонентов. В результате может быть снижена производительность платформы Xamarin в экземплярах Visual Studio, оставшихся в системе после удаления экземпляра SxS.
>
>Эту проблему можно разрешить, запустив в установщике Visual Studio возможность **восстановления** для повторной установки отсутствующих компонентов.

<a name="uninstallvsmac" />

## <a name="uninstalling-visual-studio-for-mac"></a>Удаление Visual Studio для Mac

Чтобы удалить Visual Studio для Mac, но продолжать использовать Xamarin Studio, найдите файл **Visual Studio.app** в каталоге **/Applications** и перетащите его в корзину. Можно также щелкнуть правой кнопкой мыши и выбрать пункт **Переместить в удаленные**, как показано ниже:

 [![](uninstalling-xamarin-images/image9.png "Щелчок правой кнопки на значке Visual Studio и выбор команды "Переместить в удаленные"")](uninstalling-xamarin-images/image9.png#lightbox)

Чтобы полностью удалить Xamarin с компьютера, сначала удалите Visual Studio для Mac, а затем выполните действия, приведенные в разделе [Удаление Xamarin Studio](#uninstallxamarinstudio).

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели процедуру полного удаления Xamarin с компьютера MAC с помощью команд терминала, а также удаление Xamarin с компьютера Windows с помощью элемента **Программы и компоненты** (для Visual Studio 2015 и более ранних версий) и с помощью **установщика Visual Studio** для Visual Studio 2017.


## <a name="related-links"></a>Связанные ссылки

- [Скрипт удаления (пример](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
