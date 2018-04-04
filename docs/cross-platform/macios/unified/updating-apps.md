---
title: Обновление существующих приложений в единое API
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ca14d35bcc167bd35cf1ec9e822e86421579b7b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Обновление существующих приложений в единое API

> [!IMPORTANT]
> **Классический устаревания профиля:** по мере добавления новых платформ в Xamarin.iOS начинаем постепенно число нерекомендуемых функций из классической профиля (monotouch.dll). Например параметр не NRC (новый ref-count) был удален. NRC всегда включена для всех единой приложения (т. е. не NRC никогда не была альтернативы) и известные проблемы отсутствуют. Возможность использования Боэм как сборщик мусора приведет к удалению будущих выпусках. Она также параметр никогда не доступно единое приложениям. Полное удаление поддержки классический запланирована Далее попадают в выпуске Xamarin.iOS 10.0.




## <a name="how-to-update-your-apps"></a>Обновление приложений

Университет Xamarin имеет свободным видео на **обновление до iOS единый API**. Посетите [Xamarin университета Молния лекции](http://university.xamarin.com/lightninglectures) для отслеживания!

[ ![](updating-apps-images/xamu-video-sml.png "Университет Xamarin")](http://university.xamarin.com/lightninglectures)

Существует три действия для обновления приложения.

1. Исправьте все возникшие предупреждения компилятора существующего кода, особенно тех, относящиеся к устаревшие интерфейсы API.

2. Используйте средство миграции, встроенной в Visual Studio для Mac, чтобы обновить файлы проекта и пространства имен.

3. Устраните остальные ошибки компилятора, относящиеся к новому [64 типы](~/cross-platform/macios/nativetypes.md) и [другие интерфейсы API](~/cross-platform/macios/unified/index.md#deprecated-typos) , были изменены. Извлечение [эти советы](~/cross-platform/macios/unified/updating-tips.md) Дополнительные сведения о обновления вручную, которые могут быть необходимы.

Для каждого продукта для обновления в единой API и поддержка 64-разрядных приложений доступны определенной руководства:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Приложения Xamarin.iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Можно обновить существующие приложения Xamarin.iOS единой API, с помощью автоматической миграции средства, встроенные Visual Studio для Mac. Некоторые дополнительные исправления может затем потребоваться, как описано в статье [эти инструкции](~/cross-platform/macios/unified/updating-ios-apps.md) и [советы](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac приложений](~/cross-platform/macios/unified/updating-mac-apps.md)

Можно обновить существующие приложения Xamarin.Mac единой API, с помощью автоматической миграции средства, встроенные Visual Studio для Mac. Некоторые дополнительные исправления может затем потребоваться, как описано в статье [эти инструкции](~/cross-platform/macios/unified/updating-mac-apps.md) и [советы](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms приложений](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Выполните следующие действия для обновления существующего решения Xamarin.Forms использовать API-Интерфейс единого проект iOS. Унифицированный поддержка API-интерфейса только в Xamarin.Forms 1.3 и более поздней версии, поэтому [инструкции](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) также объясняется, как обновить приложение Xamarin.Forms до версии 1.3. Эти [советы](~/cross-platform/macios/unified/updating-tips.md) может помочь обновление любой машинный код для iOS в пользовательские модули подготовки отчетов или служб зависимостей.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/nativetypes.md)

В этой статье описывается использование новых iOS единой API, собственные типы (nint, nuint, nfloat) в приложении кросс платформенных, где код используется совместно с устройства без iOS, таким как Android или ОС Windows Phone. Он позволяет контролировать, когда должен использоваться собственных типов и обеспечивает несколько возможных решений для случаев, где новый тип должен использоваться с кросс платформенного кода.

## <a name="update-bindings-to-the-unified-api"></a>Обновить привязки в единый интерфейс API

Пользователям, которые создали привязки к библиотекам Objective-C будет необходимо обновить проект привязки для отражения изменений в базовый API (где некоторые типы теперь будет 64-разрядной).
Следуйте этим инструкциям, чтобы [обновите существующий проект привязки для поддержки API единой](~/cross-platform/macios/unified/update-binding.md).




## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Обновление приложения Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Обновление приложений Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Обновление привязки](~/cross-platform/macios/unified/update-binding.md)
- [Обновление советы](~/cross-platform/macios/unified/updating-tips.md)
- [Классический vs отличия единой API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
