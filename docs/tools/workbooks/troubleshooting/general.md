---
title: "Известные проблемы и обходные пути"
ms.topic: article
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: d8d419dd5eeb44f9b86ec8668888325e5dfde5bf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="known-issues--workarounds"></a>Известные проблемы и обходные пути

## <a name="persistence-of-cultureinfo-across-cells"></a>Сохраняемость CultureInfo для ячеек

Установка `System.Threading.CurrentThread.CurrentCulture` или `System.Globalization.CultureInfo.CurrentCulture` не сохраняется по ячейкам книги на основе моно книг целей (Mac, iOS и Android) из-за [ошибку в его моно `AppContext.SetSwitch` ] [ appcontext-bug] реализации .

### <a name="workarounds"></a>Методы обхода проблемы

* Установите приложение домена local `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Или обновление до книг 1.2.1 или более поздней версии, которой будет переписать назначений, `System.Threading.CurrentThread.CurrentCulture` и `System.Globalization.CultureInfo.CurrentCulture` для обеспечения требуемого поведения (обходные пути моно ошибки).

## <a name="unable-to-use-newtonsoftjson"></a>Не удается использовать Newtonsoft.Json

### <a name="workaround"></a>Обходной путь

* Обновление до книг 1.2.1, которые будут устанавливать Newtonsoft.Json 9.0.1.
  Книги 1.3, в настоящее время альфа-канал поддерживает версии 10 и более поздних.

### <a name="details"></a>Подробные сведения

Newtonsoft.Json 10 был выпущен которой увеличить на один зависимый от нее на Microsoft.CSharp, который конфликтует с книгами версии поставляется для поддержки `dynamic`. Эта проблема устранена в предварительной версии 1.3 книг, но сейчас мы разработали вокруг это путем закрепления Newtonsoft.Json специально для версии 9.0.1.

Пакеты NuGet явным образом в зависимости от Newtonsoft.Json 10 или более поздней версии поддерживаются только в версии 1.3 книг, в настоящее время в альфа-канала.

## <a name="code-tooltips-are-blank"></a>Всплывающие подсказки кода, пусто

Отсутствует [ошибку в редакторе Монако] [ monaco-bug] в Safari и WebKit, который используется в приложении Mac книг, результатов во время отрисовки подсказок кода без текста.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Обходной путь

* Щелкнув всплывающей подсказки, после его появления приведет к текст для отображения.

* Или обновление до книг 1.2.1 или более поздней версии

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>В версии 1.3 книг отсутствуют SkiaSharp модулей подготовки отчетов

Начиная с версии 1.3 книг, мы удалили SkiaSharp модулей подготовки отчетов, которые мы поставляются в книгах 0.99.0, пользу SkiaSharp предоставления модулей подготовки отчетов, сам, с помощью наших [SDK] [/ направляющие и кросс платформы или книги/sdk /].

### <a name="workaround"></a>Обходной путь

* Обновите SkiaSharp до последней версии в NuGet. Во время записи это 1.57.1.

## <a name="related-links"></a>Связанные ссылки

- [Регистрации ошибок](~/tools/workbooks/install.md#reporting-bugs)
