---
title: "Улучшенные возможности интеграции разделы"
ms.topic: article
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 5e51aa9ab9d4d63d16b3a68d24084c872d831975
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="external-integrations"></a>Внешние интеграции

Интеграция сборки должны ссылаться [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Извлечение нашей [Быстрая документация](~/tools/workbooks/sdk/index.md) Дополнительные сведения о начале работы с пакетом NuGet.

Клиент интеграции также поддерживаются и инициируются путем размещения в том же каталоге файлов JavaScript или CSS с тем же именем, что и сборка интеграции агента. Например, если агент сборки интеграции (на который ссылается NuGet) называется `SampleExternalIntegration.dll`, затем `SampleExternalIntegration.js` и `SampleExternalIntegration.css` будут встроены в клиент также, если они существуют. Интеграция клиента являются необязательными.

Самой внешней интеграции можно упаковать как NuGet, предоставленные и ссылаться непосредственно в приложение, которое размещается агент, или просто помещать рядом с `.workbook` файл, который хочет использовать его.

Пакет упоминается, согласно документации краткое при интеграции сборок, поставляемых вместе с книги требуется сослаться на него, поэтому будет автоматически загружается на внешних средств интеграции (агент и клиента) в пакетах NuGet:

```csharp
#r "SampleExternalIntegration.dll"
```

При ссылке на интеграцию таким образом, он не будет загружен клиентом сразу&mdash;вам потребуется вызова того же кода из интеграции для загрузки. Мы будем адресации этой ошибки в будущем.

`Xamarin.Interactive` PCL предоставляет несколько важных интеграции API-интерфейсы. Каждый интеграции по крайней мере должен предоставляют точку входа интеграции:

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

На этом этапе после ссылка на сборку интеграции, клиент неявно загружают интеграции файлы JavaScript и CSS.

## <a name="apis"></a>API - интерфейсы

Как с любой сборки ссылается книги или динамической проверки сеанса, любой из его открытого API-интерфейсы доступны в сеанс. Поэтому важно иметь безопасный и разумно область API для изучения.

Сборка интеграции фактически представляет собой мост между приложения или пакета SDK интерес и сеанса. Он может предоставить новые API, смысла в контексте книги или динамической проверки сеанса, или предоставить нет открытого API и просто выполнить «за кулисами» возвратил управление объектов [представления](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> API-интерфейсы, которые должны быть открытыми, но не следует подключаться через IntelliSense могут маркироваться обычные `[EditorBrowsable (EditorBrowsableState.Never)]` атрибута.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
