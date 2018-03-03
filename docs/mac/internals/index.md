---
title: "Взгляд изнутри"
description: "Взглянуть на суть Xamarin.Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 2ba3ffb421dc64bba7df1e10a40125f14365f29e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="under-the-hood"></a>Взгляд изнутри

_Взглянуть на суть Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Преимущества времени компиляции (AOT)](aot.md)

Вперед (AOT) времени компиляции методика мощные оптимизации по повышению производительности при запуске. Тем не менее оно также влияет на время построения, размера приложения и выполнение программы в эффективных средств, поэтому стоит основные сведения о работе.

## <a name="mac-architecturearchitecturemd"></a>[Архитектура Mac](architecture.md)

Связь по Xamarin.Mac Objective-C, включая такие концепции, как компиляции, селекторы, регистраторов, запустить приложение и генератора.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac регистратора](registrar.md)

Xamarin.Mac устраняет разрыв между управляемой среде и выполнения его Cocoa, позволяя управляемых классов для вызова неуправляемые классы Objective-C и вызываться при возникновении событий. Регистратор обрабатывается работ, необходимый для выполнения «magic», но общее представление о происходящем «за кулисами» иногда может быть полезно.
