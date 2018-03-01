---
title: "Устранение неполадок"
description: "Известные проблемы с Xamarin Live Player и способы их устранения."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Устранение неполадок

![Функция предварительного просмотра](~/media/shared/preview.png)

В этой статье описываются некоторые общие проблемы и приводятся действия по их устранению.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Мобильное устройство не подключается после сканирования штрихкода (или написания кода)

Происходит, когда мобильное устройство под управлением Xamarin Live Player не в той же сети, что и компьютер под управлением интегрированной среды разработки. Просмотрите следующую информацию:

- Проверка компьютеров и устройств в одной и той же сети Wi-Fi.
  - Если компьютер также подключен к проводной сети, попробуйте отключить проводное подключение.
- Сети может быть тесно защищены (например, в некоторых сетях предприятия), блокирует порты, необходимые Live проигрывателем Xamarin.
- Закрытие приложения Xamarin Live Player и перезапустите ее.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>Сообщение «Ошибка при попытке развернуть» в интегрированной среде разработки

**«IOException: не удалось прочитать данные из транспортного соединения: операция на неблокирующих сокетах будет заблокирована»**

Эта ошибка часто возникают, когда мобильное устройство под управлением Xamarin Live Player не в той же сети, что компьютер под управлением интегрированной среды разработки; Это часто происходит при подключении к устройству, которое ранее было успешно пару.

* Убедитесь, что компьютер и устройства в той же сети Wi-Fi.
* Сети может быть тесно защищены (например, в некоторых сетях предприятия), блокирует порты, необходимые Live проигрывателем Xamarin. Для Xamarin Live Player требуются следующие порты:
  * 37847 — доступ к внутренней сети 
  * 8090 — внешнего сетевого доступа

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Сообщение «тип или пространство имен не найден» в интегрированной среде разработки

Убедитесь, что вы выбрали **запускаемый проект** , соответствующий типу вашего устройства (iOS или Android) и что конфигурация совпадает, тип устройства (например) **Отладка | iPhone симулятор** для iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Сообщение «Конструктор для типа «InterpretedXamarin.Forms.Button» не найден» в проигрывателе

Некоторые классы системы не может быть переопределен, например:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>«MainActivity.cs: «Resource.Layout» не содержит определение для «Main»»

Эта ошибка возникает для проектов Android с пользовательскими интерфейсами, определенные в файлах AXML.
Файлы AXML в Xamarin Live Player в настоящее время не поддерживаются.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android панель и вкладки отрисовки неправильного использования Xamarin.Forms

Проекты Xamarin.Forms Android необходимо использовать «Toolbar.axml» и «Tabbar.axml» для имен файлов, соответствующих макета. Шаблон по умолчанию использует эти имена. Переименование вызовет проблемы отрисовки.


Сообщите ли дополнительные проблемы на [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Связанные ссылки

- [Ограничения](~/tools/live-player/limitations.md)
- [Установка](~/tools/live-player/install.md)
