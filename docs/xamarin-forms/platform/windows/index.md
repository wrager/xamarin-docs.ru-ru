---
title: "Компоненты платформы Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: 4385534a6e2ecfc9c908648fa267a543c2313ce0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="windows-platform-features"></a>Компоненты платформы Windows

Разработка приложений Xamarin.Forms для платформ Windows требуется Visual Studio. [Странице требований](~/xamarin-forms/get-started/installation.md) содержит дополнительные сведения о необходимых компонентов.

![](images/allhanselman.png "Xamarin.Forms приложений, работающих в Windows")

## <a name="platform-support"></a>Поддержка платформ

Xamarin.Forms шаблоны, доступные в Visual Studio содержат один проект Windows по умолчанию:

* **Приложениях универсальной платформы Windows** -Xamarin.Forms приложения также могут быть оптимизированы для Windows 10. Универсальные приложения (UWP) можно запускать на телефон, планшетных и настольных компьютеров.

Если вы установили параметры правильные разработки в Visual Studio, его можно также [добавить](installation/index.md) проектов этих типов для поддержки более старых версий Windows:

* **Windows 8.1** — вы можете развертывать приложения Xamarin.Forms для планшетных и настольных конструктивных приложение Windows 8.1 как проект с помощью элементов управления WinRT.
* **Windows Phone 8.1** -Xamarin.Forms имеет полную поддержку для платформы Windows Phone 8.1 с помощью WinRT. Внешний вид приложений с помощью поддержки Windows Phone 8.1 могут быть различными в более ранних приложения Windows Phone Xamarin.Forms, которые были основаны на Silverlight.


> [!NOTE]
> **Примечание:** поддержки 1.x и 2.x Xamarin.Forms _Windows Phone 8 Silverlight_ разработки приложений, однако рекомендуется к использованию этого типа проекта.


## <a name="getting-started"></a>Начало работы

Последовательно выберите пункты **файл > Создать > проект** в Visual Studio и выберите один из **кросс-платформенных > пустое приложение (Xamarin.Forms)** шаблоны, чтобы приступить к работе.

Старые решения Xamarin.Forms, так и созданных на macOS, не будет иметь все проекты Windows, перечисленных выше (но они должны быть добавлены вручную).
Если в вашем решении, посетите еще на платформу Windows, требуется выбрать [инструкции по установке](installation/index.md) добавить нужные Windows проектов типа в секунду.


## <a name="samples"></a>Примеры

[Все образцы](https://github.com/xamarin/xamarin-forms-book-preview-2) Чарльз Петцольд книги [ *Создание мобильных приложений с помощью Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) включают Windows Phone 8.1, Windows 8.1 и проектах универсальной платформы Windows (для Windows 10).

[«Скотт Хансельман» демонстрационных приложений](https://github.com/jamesmontemagno/Hanselman.Forms) доступна отдельно, а также включает проекты Apple Watch и Android с (с помощью Xamarin.iOS и Xamarin.Android соответственно, Xamarin.Forms не выполняется на этих платформах).


## <a name="related-links"></a>Связанные ссылки

- [Проекты установки Windows](~/xamarin-forms/platform/windows/installation/index.md)
