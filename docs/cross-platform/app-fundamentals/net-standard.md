---
title: .NET Standard
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: c70a1cb1aa05426ba6d54d8af3787f014883bfa1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>С помощью стандартных проекты библиотеки .NET для совместного использования кода

Библиотека .NET Standard представляет собой формальную спецификацию интерфейсов API .NET, которые должны быть доступны во всех средах выполнения .NET. Целью введения библиотеки Standard является повышение уровня согласованности экосистемы .NET.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) продолжает обеспечивать единообразие для среды выполнения .NET, однако аналогичные спецификации для библиотек базовых классов (BCL) .NET для реализаций библиотек .NET отсутствуют.

Можно представить его как упрощенная, нового поколения [переносимой библиотеки классов](https://msdn.microsoft.com/library/gg597391.aspx).
Он представляет собой отдельную библиотеку универсальный интерфейс API для всех платформ .NET, включая .NET Core. Просто создайте одну стандартную библиотеку .NET и использовать его в любой среде выполнения, которая поддерживает платформу .NET Standard.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

В этом разделе рассматриваются способы создания и использования стандартной библиотеки .NET с помощью Visual Studio для Mac. Обратитесь к разделу стандартный пример библиотеки .NET для полной реализации.

### <a name="creating-a-net-standard-library"></a>Создание стандартной библиотеки .NET

Добавление стандартной библиотеки .NET в решение является достаточно прост.

1. В диалоговом окне Добавление нового проекта выберите `.NET Core` категории, а затем выберите `Class Library(.NET Core)`.

  **Примечание:** этот шаблон будет переименован в `.NET Standard` в будущих версиях Visual Studio для Mac.

  ![Создание библиотеки классов .NET Core](net-standard-images/vsm01.png)

2. Стандартная библиотека .NET проект будет отображаться, как показано в обозревателе решений. Узел зависимости покажет, что библиотека использует [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Указывает .NET Standard узел зависимостей в решении](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Изменение параметров стандартной библиотеки .NET

Параметры стандартной библиотеки .NET можно просмотреть и, щелкнув правой кнопкой мыши проект и выбрав команду `Options` как показано на этом снимке экрана:

![Изменить .NET Standard требуемая версия .NET framework в параметрах проекта](net-standard-images/vsm03.png)

Внутри вашей версии можно изменить `netstandard` , изменив `Target Framework` значение раскрывающегося списка.

**Кроме того:** можно изменить `.csproj` непосредственно для изменения этого значения.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 г. (Windows)

В этом разделе рассматриваются способы создания и использования стандартной библиотеки .NET с помощью Visual Studio. Обратитесь к разделу стандартный пример библиотеки .NET для полной реализации.

### <a name="creating-a-net-standard-library"></a>Создание стандартной библиотеки .NET

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Добавление стандартной библиотеки .NET в решение является достаточно прост.

1. В диалоговом окне Добавление нового проекта выберите `.NET Standard` категории, а затем выберите `Class Library(.NET Standard)`.

  ![](net-standard-images/vs01.png "Создайте новую библиотеку классов для .NET Standard")

2. Стандартная библиотека .NET проект будет отображаться, как показано в обозревателе решений. Узел зависимости покажет, что библиотека использует [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![](net-standard-images/vs02.png ".NET стандартного проекта в решении")

#### <a name="editing-net-standard-library-settings"></a>Изменение параметров стандартной библиотеки .NET

Параметры стандартной библиотеки .NET можно просмотреть и, щелкнув правой кнопкой мыши проект и выбрав команду `Properties` как показано на этом снимке экрана:

![](net-standard-images/vs03.png "Ссылки на .NET Standard библиотеки так же, как другие проекты")

Внутри вашей версии можно изменить `netstandard` , изменив `Target Framework` значение раскрывающегося списка.

**Кроме того:** можно изменить `.csproj` непосредственно для изменения этого значения.

#### <a name="using-net-standard-library"></a>С помощью стандартной библиотеки .NET

После создания стандартной библиотеки .NET, можно добавить ссылку на него из любого совместимого проекта приложения или библиотеки таким же образом, обычно добавляются ссылки. В Visual Studio, щелкните правой кнопкой мыши узел "ссылки" и выберите `Add Reference...` переключитесь `Solution : Projects` вкладки, как показано:

![](net-standard-images/vs04.png "В Visual Studio щелкните правой кнопкой мыши узел "ссылки" и выберите команду Добавить ссылку..., затем перейдите на вкладку проекты решения, как показано")

-----

