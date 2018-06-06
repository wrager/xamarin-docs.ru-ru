---
title: Создание вручную пакетов NuGet для Xamarin
description: Этот документ содержит советы помогут вам создавать пакеты NuGet, в качестве целевой платформы Xamarin. Здесь содержатся профили Xamarin пакета NuGet, NuGets PCL с зависимостями платформы и ссылки на различные образцы с открытым исходным кодом.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: cc39ade2ccc1192461bcfa19c98b7f9925b667a0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781426"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Создание вручную пакетов NuGet для Xamarin

_Эта страница содержит некоторые рекомендации, которые помогают создавать пакеты NuGet, в качестве целевой платформы Xamarin._

> [!NOTE]
> Xamarin Studio 6.2 (и Visual Studio для Mac) включает в себя возможность _автоматически_ создают пакеты NuGet PCL, .NET Standard или общие проекты. Ссылаться на [многоплатформенных библиотек для совместного использования кода](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) руководство для получения дополнительных сведений.

## <a name="nuget-package-xamarin-profiles"></a>Профили Xamarin пакета NuGet

Веб-сайте NuGet [поддержки нескольких .NET Framework версий и профили](https://docs.nuget.org/create/enforced-package-conventions) рассматриваются как обеспечить поддержку различных платформ Microsoft и профили, но не содержит имен целевой платформы, используемые Xamarin.

Основной Xamarin целевые платформы, используемых сегодня являются:

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [единой API](~/cross-platform/macios/unified/index.md) (поддерживает 64-разрядная)
* **Xamarin.Mac** -Xamarin.Mac мобильных профиля, который эквивалентен поверхности Xamarin.iOS и Xamarin.Android API.

Имеется также целевым объектом для более старых iOS [классический API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS классический API

Объект **.nuspec** файл, который нацелен на все эти будет выглядеть следующим образом:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Выше игнорирует все переносимые библиотеки классов.

Большинство **.nuspec** файлы укажите номер версии целевой платформы, но является обязательным, если сборка работает со всеми версиями, требуемая версия .NET framework. Таким образом, если на целевом объекте был **lib\MonoAndroid** это будет означать она работает с любой версией Xamarin.Android.

Укажите версию набора чисел без десятичного разделителя или его можно указать с помощью десятичные точки. Без десятичного разделителя NuGet просто принимает каждое число и включить его в версию, вставив "." между каждая цифра.

В приведенном выше «MonoAndroid10» означает «Android 1.0". Это означает проекта [требуемой версии .NET framework](~/android/app-fundamentals/android-api-levels.md) должен быть MonoAndroid 1.0 или более поздней версии. Версия, указанная в `<TargetFrameworkVersion>` элемент в файле проекта.

Для уточнения:

- **MonoAndroid403** соответствует Android 4.0.3 и более поздних (ie API уровня 15)
- **Xamarin.iOS10** соответствует Xamarin.iOS 1.0 и новее
- **Xamarin.iOS1.0** также соответствует Xamarin.iOS 1.0 и новее

## <a name="pcl-nugets-with-platform-dependencies"></a>NuGets PCL с зависимостями платформы

Профили PCL ограничены какие .NET framework API-интерфейсов, они могут получить доступ и несомненно запрете доступа к кода под конкретную платформу. Эти ссылки сторонних поставщиков рассматриваются различные подходы к созданию пакеты NuGet, которые используют PCL и собственный API для обеспечения совместимости Xamarin и других платформ:

- [Как заставить библиотеки переносимых классов работать для вас](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Прием PCL заманить и подменить](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Создание NuGet PCL, который работает с Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Этот внешний [список профилей PCL с их именами NuGet целевой](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) можно использовать для справки.

## <a name="examples"></a>Примеры

Некоторые примеры открытым исходным кодом, которые можно ссылаться на:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) — создавать приложения с помощью System.Net.Http, но удалить эту библиотеку в и осуществляется значительно быстрее (представление [источника](https://github.com/paulcbetts/ModernHttpClient)).
- [**Сплаттинга** ](https://www.nuget.org/packages/Splat/) — библиотеку, чтобы упростить кросс платформенных, которые должны быть (представление [источника](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -кроссплатформенная библиотека для отрисовки векторной графики на платформе .NET (представление [источника](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## <a name="related-links"></a>Связанные ссылки

- [Nugetizer 3000 автоматическое создание Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Обновление NuGets для 64-разрядных iOS](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Включая NuGet в проекте](/visualstudio/mac/nuget-walkthrough/index.md)
