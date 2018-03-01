---
title: "Почему не Мое приложение iOS 9 с: System.Exception: не удалось упаковать объект Objective-C?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: baba2526eefa1b69d47da47b73ea0bd417ecdc57
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Почему не Мое приложение iOS 9 с: System.Exception: не удалось упаковать объект Objective-C?

Может появиться ошибка этой формы:

> System.Exception: Не удалось упаковать объект Objective-C... Не удалось найти существующий управляемый экземпляр этого объекта...

Изменения API в iOS 9 потребовать использования конструктора обратного вызова при вызов неуправляемого кода, в качестве базового интерфейса API теперь ожидает ее. Добавление обратного вызова конструктора класса, используйте следующую строку: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Следующие шаги

Для получения дополнительной помощи, свяжитесь с нами, или если эта проблема остается даже после использования указанные выше сведения см. в разделе [какие варианты поддержки доступны для Xamarin?](~/cross-platform/troubleshooting/support-options.md) сведения о параметрах контактов, предложения, а также как файл новую ошибку, при необходимости. 
