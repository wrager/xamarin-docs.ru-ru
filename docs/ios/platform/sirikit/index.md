---
title: SiriKit
description: В этой статье показано, как использовать SiriKit в приложении Xamarin.iOS для предоставления служб, которые доступны пользователю, с помощью Siri на устройстве iOS.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac68f249361fd5bee8af102a1418c9d5f1d2c0ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="sirikit"></a>SiriKit

_В этой статье показано, как использовать SiriKit в приложении Xamarin.iOS для предоставления служб, которые доступны пользователю, с помощью Siri на устройстве iOS._

Новые для iOS 10 SiriKit позволяет приложению iOS предоставляют службы, которые доступны пользователю, с помощью Siri и Maps приложения на устройстве iOS с помощью расширений приложения и новых **целей** и **пользовательского интерфейса целей** платформ.

Siri работает с понятием **домены**, групп знать действия для связанных задач. Каждый диалог, приложение будет установлено с Siri необходимо относятся к одной известной службы доменов следующим образом:

- Аудио или видео вызова.
- Резервирования расстояния.
- Управление тренировки.
- Обмен сообщениями.
- Поиск фотографий.
- Отправки или приема платежей.

Когда пользователь выполняет запрос Siri, связанных с одним расширения приложения служб, SiriKit отправляет расширение **намерение** , описывающий запроса пользователя, а также все дополнительные данные. Расширение приложения создает соответствующий **ответ** объекта для заданного **намерение**, с подробными сведениями о том, как расширение сможет обработать запрос.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[Основные сведения о понятиях SiriKit](~/ios/platform/sirikit/understanding-sirikit.md)

В этой статье рассматриваются основные понятия, которые будут необходимы для работы с SiriKit приложения Xamarin.iOS. Она охватывает новый целей и целей точки расширения пользовательского интерфейса и как они работают в приложение и знания, пользователь, для открытия приложения для Siri.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[Реализация SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

В этой статье рассматриваются действия, необходимые для реализации поддержки SiriKit в приложениях Xamarin.iOS. Разработчик рекомендуется ознакомиться с руководством основные понятия SiriKit выше перед попыткой добавить поддержку SiriKit к приложению, в качестве ключа, которые рассматриваются основные понятия, необходимых для успешной реализации.





## <a name="related-links"></a>Связанные ссылки

- [Образец ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Руководство по программированию SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Справочник по Framework целей](https://developer.apple.com/reference/intents)
- [Способы ссылке платформы пользовательского интерфейса](https://developer.apple.com/reference/intentsui)
