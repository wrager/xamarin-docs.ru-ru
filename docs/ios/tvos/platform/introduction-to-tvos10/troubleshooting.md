---
title: Устранение неполадок
description: В этой статье предоставляет несколько советов по устранению неполадок для работы с tvOS 10 в Xamarin.tvOS приложениях.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8875e658ead17820655a2401079627875c14958b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Устранение неполадок

_В этой статье предоставляет несколько советов по устранению неполадок для работы с tvOS 10 в Xamarin.tvOS приложениях._

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании tvOS 10 с Xamarin.tvOS и решения этих проблем:

- [Магазин приложений](#App-Store)
- [Двоичная совместимость](#Binary-Compatibility)
- [Протокол HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>Магазин приложений

Известные проблемы:

 - При тестировании покупки из приложений в среде "песочницы", диалоговое окно проверки подлинности, появляются дважды.
 - При тестировании покупки из приложений с содержимым, размещенной в среде "песочницы", диалоговое окно пароля будет отображаться каждый раз, чтобы приложение переводится на передний план, до завершения загрузки содержимого.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Двоичная совместимость

Известные проблемы:

 - Вызов `NSObject.ValueForKey` будет `null` ключа приведет к возникновению исключения.
 - Ссылка на шрифт по имени, при вызове `UIFont.WithName` приведет к сбою.
 - Оба `NSURLSession` и NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' URL-адреса.
 - Приложения может зависнуть, если они изменяют Обзор от geometry в любом `ViewWillLayoutSubviews` или `LayoutSubviews` методы.
 - Для всех подключений SSL/TLS симметричного шифрования RC4 теперь отключено по умолчанию. Кроме того API защиты транспорта больше не поддерживает SSLv3 и рекомендуется, приложение остановить использование шифрования SHA-1 и 3DES как можно быстрее.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>Протокол HTTP CFNetwork

`HTTPBodyStream` Свойство `NSMutableURLRequest` класса должно быть присвоено поток Неоткрытое с момента `NSURLConnection` и `NSURLSession` теперь строго соблюсти это требование.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Длительные операции вернет _«Нет разрешения на сохранение файла.»_ Произошла ошибка.

<a name="CoreImage" />

## <a name="coreimage"></a>CoreImage

`CIImageProcessor` API теперь поддерживает числа произвольных входного образа. `CIImageProcessor` API, который был включен в tvOS 10 бета-версия 1 будут удалены.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

После выполнения операции передачи `UserInfo` свойство `NSUserActivity` объект может быть пустым. Явным образом вызвать `BecomeCurrent` NSUserActivity "объект как текущее решение.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Известные проблемы:

 - Изменения вида фона `UINavigationBar`, `UITabBar` или `UIToolBar` может привести к передаче макета для решения новый внешний вид. Предпринимается попытка изменить эти вхождений внутри `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` или `DidUpdateSubviews` события может привести в макет бесконечный цикл.
 - В вызове tvOS 10, `RemoveGestureRecognizer` метод `UIView` объекта явно отменяет все выполняющиеся распознаватель жестов.
 - Просмотр контроллеров, представленному может повлиять на внешний вид в строке состояния.
 - tvOS 10 случае разработчику необходимо вызвать `base.AwakeFromNib` для подклассов `UIViewController` и переопределение `AwakeFromNib` метод.
 - Приложения с пользовательским `UIView` подклассов, которые переопределяют `LayoutSubviews` и измененные макет перед вызовом `base.LayoutSubviews` может запускать макета бесконечный цикл в tvOS 10.
 - Средства конкретного направление или flippable изображений — зеркального отображения при назначении `UIButton` объектов.





## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Новые возможности tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
