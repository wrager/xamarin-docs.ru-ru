---
title: Изменения платформы дополнительных watchOS 3
description: В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для watchOS 3.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 572aaff9d010ec1ec1f48db2878859e445e2fb20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="additional-watchos-3-frameworks-changes"></a>Изменения платформы дополнительных watchOS 3

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для watchOS 3._

Помимо основных изменений для операций ввода-вывода Apple внес изменения и усовершенствования несколько существующих инфраструктур в watchOS 3.


## <a name="core-data"></a>Основные данные

Следующие улучшения были внесены в платформе основных данных для наблюдения за ОС 3:

- Корневой [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объектов поддерживает параллельных сбой и выборка без сериализации.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) класс поддерживается пул хранилищ данных SQLite.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объектов с хранилищами данных SQLite в режиме с упреждающей Записью журнала поддержки создания нового запроса компонентов, где управляемого объекта контексты Майкрософт можно прикрепить к версии конкретной базы данных для следующей выборки и Ошибка транзакции.
- С помощью высокоуровневые `NSPersistenceContainer` ссылка `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) и других ресурсов настройки основных данных.
- Были добавлены несколько новых удобных методов `NSManagedObject` упрощая процесс для выполнения выборки и создавать подклассы.

Дополнительные сведения см. в разделе Apple [Framework справочные сведения об основных данных](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Основные движения

Следующие улучшения были внесены в framework Core движения для наблюдения за ОС 3:

- Новое событие движения устройство использует акселерометр и гироскопа для предоставления обновлений движения и ориентацию. для этого обновления (по ставкам до 100 Гц) можно зарегистрировать приложение приложений.
- Новое событие Pedometer обеспечивает быстрый и уведомления в режиме реального времени, когда пользователь задерживает и возобновляет выполнение. Используйте [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) для регистрации событий pedometer основной или фоновой.


## <a name="foundation"></a>Foundation

Следующие улучшения были внесены в платформе Foundation для Контрольные значения 3 ОС:

- Используйте новую [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) класса, чтобы дата и время вычисления интервал, например длительность, для сравнения интервалов и тестирование для пересечения интервал.
- Были добавлены несколько новых свойств [NSLocal](https://developer.apple.com/reference/foundation/nslocale) класса для получения информации и отображение доступных форматов.
- Используйте новую [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) класса для преобразования между различных единиц из мер (единица Измерения) или выполняют вычисления над значениями в разных UOMs.
- Используйте новую [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) класс форматирование локализованных измерения для отображения для конечного пользователя.
- Используйте новую [NSUnit](https://developer.apple.com/reference/foundation/nsunit) и [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) классы для представления определенных UOMs.


## <a name="healthkit"></a>HealthKit

Следующие улучшения были внесены в платформе HealthKit для Контрольные значения 3 ОС:

- Используйте новую [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) класс, чтобы указать `ActivityType` и `LocationType` для тренировки.
- Новый [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) и `WheelchairUse` метод [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) класс были добавлены для работы с инвалидных связанных данных о работоспособности.
- Были добавлены новые разделы метаданных для типов погоды (такие как `HKWeatherConditionClear` и `HKWeatherConditionCloudy`) и тренировки типы (такие как `HKWorkoutActivityTypeFlexibility` и `HKWorkoutActivityTypeWheelchairRunPace`) были добавлены.


## <a name="homekit"></a>HomeKit

Следующие улучшения были внесены в платформе HomeKit для Контрольные значения 3 ОС:

- Добавлена возможность просматривать и взаимодействовать с HomeKit подключен IP камеры.
- Добавлено несколько новых служб и их характеристики.
- Добавлены дополнительные контекста и конфигурации аксессуаров основные службы и связи служб.


## <a name="passkit"></a>PassKit

Следующие улучшения были внесены в платформе PassKit для Контрольные значения 3 ОС:

- При развертывании среды для поддержки безопасного выплат в приложении на Apple Watch физические товары и служб.
- Теперь доступны следующие классы: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) и [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Следующие улучшения были внесены в платформе UIKit для Контрольные значения 3 ОС:

- Для поддержки динамического типа подписей для текстовых полей и текстовых полей используйте новый `PreferredFontForTextStyle` метод `UIFont` класса.
- `ColorWithDisplayP3` Был добавлен метод поддерживает широкую цветовую.


## <a name="related-links"></a>Связанные ссылки

- [Приступая к работе (пример)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [Новые возможности watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
