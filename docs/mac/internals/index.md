---
title: За кулисами Xamarin.Mac
description: Документ содержит ссылки на различные руководства, описывающие суть Xamarin.Mac. Связанные документы рассматриваются опережает время компиляции, архитектура Xamarin.Mac и Xamarin.Mac регистратора.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792493"
---
# <a name="under-the-hood-in-xamarinmac"></a>За кулисами Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Преимущества времени компиляции (AOT)](aot.md)

Вперед (AOT) времени компиляции методика мощные оптимизации по повышению производительности при запуске. Тем не менее оно также влияет на время построения, размера приложения и выполнение программы в эффективных средств, поэтому стоит основные сведения о работе.

## <a name="mac-architecturearchitecturemd"></a>[Архитектура Mac](architecture.md)

Связь по Xamarin.Mac Objective-C, включая такие концепции, как компиляции, селекторы, регистраторов, запустить приложение и генератора.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac регистратора](registrar.md)

Xamarin.Mac устраняет разрыв между управляемой среде и выполнения его Cocoa, позволяя управляемых классов для вызова неуправляемые классы Objective-C и вызываться при возникновении событий. Регистратор обрабатывается работ, необходимый для выполнения «magic», но общее представление о происходящем «за кулисами» иногда может быть полезно.
