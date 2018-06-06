---
title: Пример с использованием проекта Xcode
description: В этом документе описывается используют в качестве прямого ввода для Sharpie цели, что упрощает процесс создания привязки C# для кода Objective-C проект Xcode.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 850c351f91c9a09a6654c876167e035f751e9504
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781121"
---
# <a name="real-world-example-using-an-xcode-project"></a>Пример с использованием проекта Xcode


**В этом примере используется [библиотеки POP от Facebook](https://github.com/facebook/pop).**

Новое в версии 3.0 Sharpie цель поддерживает проекты Xcode в качестве входного. Эти проекты укажите правильный заголовок файлы и флаги компилятора, необходимые для компиляции собственной библиотеки и таким образом необходимость привязать его слишком. Выбирает первый цели Sharpie _целевой_ и его конфигурация по умолчанию проекта, если не выдал вам иные инструкции.

Прежде чем Sharpie цель пытается выполнить синтаксический анализ файлов проекта и заголовок, его необходимо построить. Проекты часто имеют этапов построения, которые правильно структуры заголовочные файлы для внешнего использования и интеграции, поэтому лучше всегда построения полного проекта перед попыткой привязать его.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

