---
redirect_url: /xamarin/tools/live-player/
title: XAML Live предварительного просмотра
description: В этом документе описывается использование Xamarin Live Player live предварительного просмотра XAML-страницы, внести изменения в XAML и увидеть изменения мгновенно отображаются на устройстве.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: cc68044342fca84e62e3b17770170e1d7a23f677
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793706"
---
# <a name="xaml-live-previewing"></a>XAML Live предварительного просмотра

Одно из преимуществ Xamarin Live Player является возможность live предварительного просмотра XAML-страницы, внести изменения в код в Visual Studio и увидеть изменения сразу же отображаться на устройстве. Интерактивный просмотр можно сделать на устройства iOS или Android или на симуляторе или эмуляторе. В этом руководстве показано, как использовать функцию интерактивного просмотра для просмотра отдельных экранов XAML.

## <a name="requirements"></a>Требования

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Компьютер под управлением Windows 7 или более поздней версии.
2. Visual Studio 2017 г. версия 15.4 или выше, **Разработка мобильных приложений в .NET Framework** установлен рабочей нагрузки.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

1. Mac с OS X 10.11 macOS 10.12 или более поздней версии.
2. Visual Studio для Mac 7.2 или более поздней версии. Мы рекомендуем последнюю версию.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>Развертывание на устройстве

Прежде чем использовать проигрыватель Xamarin Live с устройства iOS или Android, вам потребуется скачать приложение Xamarin Live Player и свяжите его в Visual Studio, как описано в [установить](~/tools/live-player/install.md) руководства. После успешного иметь пару устройства для Visual Studio, можно начать интерактивный просмотр XAML-страницы. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Откройте страницу XAML для интерактивного просмотра в редакторе Visual Studio 2017 г.:

    ![](live-view-images/vs-image1.png)

2. Задайте для конфигурации устройства **отладка | iPhone** для операций ввода-вывода или **отладки** для Android, а затем выберите устройство Live Player в списке:

    ![](live-view-images/vs-image2.png)

3. Чтобы запустить этот XAML-страницы как в реальном времени на устройстве, выберите **Сервис > Xamarin Live Player > Live запуска текущее представление** из строки меню:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте страницу XAML, требуется интерактивный просмотр в Visual Studio для Mac редактора:

    ![](live-view-images/image1.png)

2. Задайте для конфигурации устройства **отладка | iPhone** для операций ввода-вывода или **отладки** для Android, а затем выберите устройство Live Player в списке:

    ![](live-view-images/image2.png)

3. Чтобы запустить этот XAML-страницы как в реальном времени на устройстве, выберите **запуска > Live запуска текущее представление** из строки меню:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Развертывание в эмуляторе Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Откройте страницу XAML для интерактивного просмотра в редакторе Visual Studio 2017 г.:

    ![](live-view-images/vs-image1.png)

2. Задайте для конфигурации устройства **отладки** для Android и выберите устройство Live Player в списке:

    ![](live-view-images/vs-image4.png)

3. Чтобы запустить этот XAML-страницы как в реальном времени в эмуляторе Android, выберите **Сервис > Xamarin Live Player > Live запуска текущее представление** из строки меню:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Откройте страницу XAML, требуется интерактивный просмотр в Visual Studio для Mac редактора:

    ![](live-view-images/image7.png)

2. Задайте для конфигурации устройства **отладки** для Android и выберите устройство Live Player в списке:

    ![](live-view-images/image6.png)

3. Чтобы запустить этот XAML-страницы как в реальном времени на устройстве, выберите запуска > Live запуска текущее представление на панели меню:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-ios-simulator"></a>Развертывание на эмуляторе iOS

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В настоящее время не поддерживается для использования динамической XAML, предварительный просмотр в симуляторе iOS удаленное взаимодействие в Windows. Вместо этого следует [развертывания на устройстве](#deploydevice).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Откройте страницу XAML, требуется интерактивный просмотр в Visual Studio для Mac редактора:

    ![](live-view-images/image1.png)

2. Задайте для конфигурации устройства **отладка | iPhoneSimulator** для операций ввода-вывода и выберите из списка имитаторе iOS:

    ![](live-view-images/image2.png)

3. Выберите **запуска > Live запуска текущее представление** в строке меню для запуска симулятора и отображения XAML-страницы:

    ![](live-view-images/image4.png)

4. После запуска симулятора, можно начать редактирование XAML и отображается динамическая предварительного просмотра:

    ![](live-view-images/image5.png)  

-----

## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о динамической проигрыватель Xamarin](https://xamarin.com/live)
- [запись блога](https://blog.xamarin.com/live-player/)
- [Образцы динамической проигрыватель Xamarin](~/tools/live-player/samples.md)
