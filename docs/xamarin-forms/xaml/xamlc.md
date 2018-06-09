---
title: Компиляция XAML в Xamarin.Forms
description: В этой статье объясняется, как XAML можно при необходимости компилируется непосредственно в промежуточный язык (IL), с помощью компилятора Xamarin.Forms XAML (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 0128fecebe9f6ba8f55e965a8fa65787d03d9ded
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245749"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Компиляция XAML в Xamarin.Forms

_XAML можно скомпилировать непосредственно в промежуточный язык (IL), с помощью компилятора XAML (XAMLC) при необходимости._

Компилятор XAMLC предлагает ряд преимуществ, которые перечислены далее:

- Он проверяет XAML во время компиляции, уведомляя пользователя об ошибках.
- Он сокращает часть времени загрузки и создания элементов XAML.
- Он позволяет сократить размер файла окончательной сборки, так как больше не добавляет XAML-файлы.

XAMLC отключен по умолчанию для обеспечения обратной совместимости. Включить на уровне сборки и класса, добавив `XamlCompilation` атрибута.

В следующем примере кода показано включение XAMLC на уровне сборки:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

В этом примере все XAML-кода проверки во время компиляции внутри `PhotoApp` выполняется пространства имен, с ошибками XAML, сообщаемый во время компиляции, а не во время выполнения.
`assembly` Префикс для `XamlCompilation` атрибут указывает, что атрибут применяется ко всей сборке.

В следующем примере кода показано включение XAMLC на уровне класса:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

В этом примере проверка кода XAML для компиляции `HomePage` класса будет выполняться, и выводятся сообщения об ошибках в процессе компиляции.

> [!NOTE]
> `XamlCompilation` Атрибута и `XamlCompilationOptions` перечисления находятся в `Xamarin.Forms.Xaml` пространства имен, которое необходимо импортировать их использования.


## <a name="related-links"></a>Связанные ссылки

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
