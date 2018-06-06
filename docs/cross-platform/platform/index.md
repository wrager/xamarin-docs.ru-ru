---
title: Программирование языковой поддержке в Xamarin
description: 'В этом документе описываются разные языки программирования, поддерживаемые Xamarin. Он описывает C#, F #, переносимый Visual Basic.NET и шаблонов Razor.'
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781680"
---
# <a name="programming-language-support-in-xamarin"></a>Программирование языковой поддержке в Xamarin

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)

Версия 5 C# появились два новых ключевых слов для выражения асинхронных операций: async и await. Эти ключевые слова можно написать простой код, который использует библиотеку параллельных задач для выполнения длительных операций (например, доступ к сети) в другом потоке и легко получить доступ к результатам по завершении. Последние версии Xamarin.iOS и Xamarin.Android поддерживают async и await — в этом документе приводятся разъяснения и пример использования нового синтаксиса с помощью Xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[6 возможностей языка C#](~/cross-platform/platform/csharp-six.md)

Последнюю версию языка C# — версия 6 – постоянно развивается языка меньше стандартных действий, улучшен и согласованность. Сжимаемая синтаксис инициализации, возможность использования `await` в `catch/finally` блоки и условием null `?` оператор особенно полезны.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Создание мобильных приложений с помощью F # и Xamarin.

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Переносимый Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio поддерживает создание переносимой библиотеки классов, с помощью Visual Basic.NET, который можно включить в приложения Xamarin. В этой статье показано, как создать новый PCL Visual Basic, а затем использовать его в образце приложения Xamarin.iOS, Xamarin.Android и Windows Phone.

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Построение HTML представления с помощью шаблонов Razor](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin позволяет разработчикам использовать подсистемы шаблонов Razor, изначально появившуюся в ASP.NET MVC, а также легко объединение данных с HTML, Javascript и CSS, не загружая вручную построения строки HTML в коде C#.
В этой статье демонстрируется использование шаблонов Razor с помощью Xamarin iOS и Android.
