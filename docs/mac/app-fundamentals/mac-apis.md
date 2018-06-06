---
title: интерфейсы API для разработчиков Xamarin.Mac macOS
description: В этом документе описывается порядок чтения селекторы Objective-C и как найти соответствующие методы C# в приложении Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: cceaa2f6e89b712be5929f7e978663d8c47f18c5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791554"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>интерфейсы API для разработчиков Xamarin.Mac macOS

## <a name="overview"></a>Обзор

Для большой объем времени разработки с использованием Xamarin.Mac вы думаете, чтение и запись в C# не столь важно, базовые интерфейсы API Objective-C. Тем не менее иногда последующего необходимо прочитать документацию по API из Apple, преобразовать ответа от переполнения стека в решение для проблемы либо сравнения существующего образца.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Чтение достаточно Objective-C опасными

Иногда он будет необходимо прочитать определение Objective-C или метод вызова и преобразовать этот эквивалентный метод C#. Давайте взгляните на определение функции Objective-C и разбить на части. Этот метод ( *селектор* в Objective-C) можно найти на `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Объявления можно прочитать слева направо.

- `-` Префикс означает метода экземпляра (не статического). + означает это метод класса (статический)
- `(BOOL)` возвращаемый тип (bool в C#)
- `canDragRowsWithIndexes` — Первая часть имени.
- `(NSIndexSet *)rowIndexes` Представляет тип первого параметра и с ним имеет тип. Первый параметр имеет формат: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` — Тип второго параметра и его тип. После первого каждый параметр имеет формат: `selectorPart:(Type) pararmName`
- Полное имя селектора этого сообщения: `canDragRowsWithIndexes:atPoint:`. Примечание `:` в конце - важно.
- Фактическая привязка Xamarin.Mac C# является: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Этот вызов селектор могут считываться так же, как:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Экземпляр `v` испытывает его `canDragRowsWithIndexes:atPoint` селектор вызывается с двумя параметрами `set` и `point`, переданное в.
- В C# вызов метода выглядит следующим образом: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Поиск члена C# для заданного селектора

Теперь, когда вы нашли селектор Objective-C, необходимые для вызова неуправляемого кода, следующим шагом является сопоставление, эквивалентный член C#. Существует четыре подхода, можно попробовать (продолжением `NSTableView CanDragRows` пример):

1. Используйте список автоматического завершения можно быстро находить примерно с таким же именем. Поскольку мы знаем, что он является экземпляром класса `NSTableView` можно ввести:

    - `NSTableView x;`
    - `x.` [ctrl + пробел, если список не отображается).
    - `CanDrag` [Введите]
    - Щелкните правой кнопкой мыши метод, перейти к объявлению, чтобы открыть обозреватель сборки, где можно сравнить `Export` для рассматриваемой селектор атрибута

2. Поиск привязки весь класс. Поскольку мы знаем, что он является экземпляром класса `NSTableView` можно ввести:

    - `NSTableView x;`
    - Щелкните правой кнопкой мыши `NSTableView`, перейти к объявлению обозреватель сборки
    - Поиск в вопросе селектор

3. Можно использовать [электронной документации по Xamarin.Mac API](https://developer.xamarin.com/api/root/monomac-lib/) .

4. Мигель содержит представление «Розеттском Фиксировано» API-интерфейсов Xamarin.Mac [здесь](http://tirania.org/tmp/rosetta.html) , можно найти через данного API. Если API не AppKit или конкретных macOS, может оказаться существует.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
