---
title: Устранение неполадок динамической проигрыватель Xamarin
description: В этом документе описаны известные проблемы с Xamarin Live Player и потенциальных ошибок. Он описывает проблемы с подключением, проблемы с конфигурацией и многое другое.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: 3db14db2c64e024ef1c04275661f610f9407dfb7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793759"
---
# <a name="troubleshooting-xamarin-live-player"></a>Устранение неполадок динамической проигрыватель Xamarin

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

Эта ошибка часто возникают, когда мобильное устройство под управлением Xamarin Live Player не в той же сети, что и компьютер под управлением Visual Studio. Это часто происходит при подключении к устройству, которое ранее было успешно пару.

* Убедитесь, что компьютер и устройства в той же сети Wi-Fi.
* Сети может быть тесно защищены (например, в некоторых сетях предприятия), блокирует порты, необходимые Live проигрывателем Xamarin. Для Xamarin Live Player требуются следующие порты:
  * 37847 — доступ к внутренней сети 
  * 8090 — внешнего сетевого доступа

## <a name="manually-configure-device"></a>Настройка устройства вручную

Если вы не можете подключиться к устройству по Wi-Fi можно попытаться вручную настроить на устройство с помощью файла конфигурации, с помощью следующих действий:

**Шаг 1: Откройте файл конфигурации**

Перейдите к папке данных приложения:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

В этой папке можно найти **PlayerDeviceList.xml** Если существует необходимо создать его.

**Шаг 2: Получение IP-адреса**

В приложении Xamarin Live Player, перейдите к **о > Проверка подключения > Начать тест подключения**.

Запишите IP-адреса, вам потребуется IP-адрес при настройке устройства.

**Шаг 3: Получение кодом связывания**

Внутри tap Xamarin Live Player **пары** или **пары снова**, нажмите клавишу **вручную введите**. Числовой код отображается, который необходимо обновить файл конфигурации.

**Шаг 4: Создать код GUID**

Последовательно выберите пункты: https://www.guidgenerator.com/online-guid-generator.aspx и сформировать новый идентификатор guid и убедитесь, что включен верхний регистр.

**Шаг 5: Настройка устройства**

Откройте **PlayerDeviceList.xml** вверх в редакторе, например Visual Studio или Visual Studio Code. Необходимо вручную настроить устройства в этом файле. По умолчанию, файл должен содержать следующий пустой `Devices` XML-элемента:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Добавьте устройства iOS:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Добавьте устройства Android:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Закройте и снова откройте Visual Studio.** Устройство должно отображаться в списке.

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
