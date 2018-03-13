---
title: "Развертывание и тестирование"
description: "Руководства по стабилизации и развертыванию"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2DBF3BF9-79E7-4E24-AF26-E34C972B0169
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f051e855c9d32f5c9667dc70c7cb9cf9ba8105fb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="deployment-and-testing"></a>Развертывание и тестирование

В разделах этого документа рассматриваются вопросы тестирования и распространения приложений. В них описываются инструменты, используемые для отладки, содержатся сведения о развертывании для инженеров-испытателей, а также приводятся инструкции о публикации приложений в Магазин приложений.


##  <a name="app-distributioniosdeploy-testapp-distributionindexmd"></a>[Распространение приложений](~/ios/deploy-test/app-distribution/index.md)

В этой статье демонстрируется настройка, сборка и публикация приложения Xamarin.iOS для распространения по различным каналам, включая следующие:

- [Распространение через Магазин приложений](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Собственное (корпоративное) распространение](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Прямое распространение](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

##  <a name="ipa-deploymentiosdeploy-testapp-distributionipa-supportmd"></a>[Развертывание IPA](~/ios/deploy-test/app-distribution/ipa-support.md)

В рамках прямых и корпоративных развертываний разработчики могут создавать пакеты, распространяемые в целях тестирования или использования внутри организации. В этом документе описывается создание пакета IPA, который можно синхронизировать с устройством iOS с помощью iTunes.

## <a name="provisioningprovisioningindexmd"></a>[Code Signing and Provisioning](provisioning/index.md) (Подписывание кода и подготовка)

В этой серии руководств описано, как подписывать код и подготавливать такие основные возможности, как работа со списков свойств, а также приложения для служб приложений. 

## <a name="wireless-deploymentwireless-deploymentmd"></a>[Wireless Deployment](wireless-deployment.md) (Развертывание по беспроводному доступу)

 Xcode 9 позволяет выполнять развертывание на устройстве iOS или Apple TV по сети. Так вам не нужно будет каждый раз использовать физическое подключение для развертывания и отладки приложения. Эта возможность сейчас доступна в режиме предварительной версии.

##  <a name="testflightiosdeploy-testtestflightmd"></a>[TestFlight](~/ios/deploy-test/testflight.md)

Теперь сервис TestFlight принадлежит компании Apple и является основным способом бета-тестирования приложений Xamarin.iOS. В этой статье приводятся все этапы процесса TestFlight — от отправки приложения до работы с iTunes Connect.

##  <a name="debugging-in-xamariniosiosdeploy-testdebugging-in-xamarin-iosmd"></a>[Отладка в Xamarin.iOS](~/ios/deploy-test/debugging-in-xamarin-ios.md)

В Visual Studio и Visual Studio для Mac реализована поддержка для отладки приложений Xamarin.iOS в симуляторе iOS и на устройствах iOS. В этой статье показано, как использовать отладчик, а также как настраивать различные поддерживаемые им параметры.


##  <a name="touchunitiosdeploy-testtouchunitmd"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

В этом документе описывается создание модульных тестов для проектов Xamarin.iOS.
Модульное тестирование с помощью Xamarin.iOS осуществляется с помощью платформы Touch.Unit, в состав которой входит средство запуска тестов iOS и измененная версия платформы[NUnitLite](http://www.nunitlite.com/), предоставляющая знакомый набор API-интерфейсов для написания модульных тестов.



##  <a name="using-instruments-to-detect-native-leaks-using-markheapiosdeploy-testusing-instruments-to-detect-native-leaks-using-markheapmd"></a>[Использование средства Instruments для обнаружения собственных утечек с помощью MarkHeap](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

В этой статье описывается использование средства Instruments на любом устройстве iOS и в любом приложении Xamarin.iOS. В ней также рассматривается профилирование приложений в симуляторе.



##  <a name="walkthrough---using-apples-instrument-tooliosdeploy-testwalkthrough-apples-instrumentmd"></a>[Пошаговое руководство. Использование средства Apple Instruments](~/ios/deploy-test/walkthrough-apples-instrument.md)

В этой статье последовательно описываются этапы использования средства Apple Instruments для диагностики проблем с памятью в приложении iOS, созданном с помощью Xamarin. В ней описывается запуск Instruments, создание мгновенного снимка кучи и анализ роста памяти. В статье также показано, как использовать Instruments для отображения и определения именно тех строк кода, которые приводят к возникновению проблем с памятью.

##  <a name="linking-on-ioslinkermd"></a>[Компоновка в iOS](linker.md)

В статье описано, как компоновщик обеспечивает минимальный пакет приложений, а также приведены рекомендации по его использованию и изменению параметров.

##  <a name="xamarinios-performanceperformancemd"></a>[Производительность Xamarin.iOS](performance.md)

Существует множество методов повышения производительности приложений, созданных с помощью Xamarin.iOS. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением.

##  <a name="mtouchmtouchmd"></a>[mtouch](mtouch.md)

Сведения о средстве командной строки mtouch.exe, которое преобразует проект в приложение, которое можно использовать в iOS.

## <a name="ios-build-mechanicsios-build-mechanicsmd"></a>[iOS Build Mechanics](ios-build-mechanics.md) (Механизм сборки iOS)

В этом руководстве описано, как планировать приложения и использовать методы, которые можно применять для ускоренного создания сборок для всех конфигураций.