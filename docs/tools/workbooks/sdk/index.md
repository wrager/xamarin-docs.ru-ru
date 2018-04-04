---
title: Приступая к работе с книгами Xamarin SDK
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: bc8ae0304e5b044cc1a898820d0ac33e33dfec0d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Приступая к работе с книгами Xamarin SDK

Этот документ содержит краткое руководство по началу работы с разработки интеграции для Xamarin книг. Большая часть этой будет работать с книгами, стабильный Xamarin, но **загрузке интеграции через пакеты NuGet поддерживается только в версии 1.3 книг**, альфа-канала на момент написания статьи.

# <a name="general-overview"></a>«Общие»

Xamarin книг интеграций являются небольшой библиотек, которые используются [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] пакета SDK для интеграции с Xamarin книги и инспектора агентов предоставляют расширенные возможности.

Существует 3 основных шагов по началу работы с разработкой интеграцию — мы будем структуры их здесь.

## <a name="creating-the-integration-project"></a>Создание проекта интеграции

Лучше всего интеграции библиотеки разрабатываются в виде многоплатформенный библиотеки. Так как вы хотите предоставить рекомендации интеграции на всех доступных агентов, прошлых и будущих, имеет смысл выбрать широко поддерживаемые набор библиотек. Рекомендуется использовать шаблон «Переносимой библиотеки» расширить поддержку:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Переносимая библиотека шаблонов Visual Studio для Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio шаблона переносимой библиотеки](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

В Visual Studio имеет смысл убедитесь в том, что выбраны следующие целевой платформы для переносимой библиотеки:

[![Переносимая библиотека платформы Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

После создания проекта библиотеки, добавьте ссылку на нашем `Xamarin.Workbooks.Integration` библиотеки NuGet через диспетчер пакетов NuGet.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![NuGet Visual Studio для Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Может потребоваться удалить пустым классом, который создается автоматически в рамках проекта, вы не необходимости использовать его для этого. После выполнения этих шагов вы готовы начать построение интеграции.

## <a name="building-an-integration"></a>Построение интеграции

Мы создадим простую интеграцию. Мы действительно популярностью цвет зеленый, так как представление для каждого объекта, мы добавим зеленый цвет. Во-первых, создайте новый класс с именем `SampleIntegration`и сделать его реализации нашей [ `IAgentIntegration` ] [ integration-type] интерфейс:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Мы хотим этого делать — добавить [представление](~/tools/workbooks/sdk/representations.md) для каждого объекта, являющегося зеленым цветом. Мы сделаем это с помощью поставщика представление. Поставщики наследовать [ `RepresentationProvider` ] [ reppr] класса — для нашего, нам нужно переопределить [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Мы возвращая [ `Color` ] [ color], предварительно созданного типа представления в нашем SDK.
Вы заметите, что возвращаемый тип является `IEnumerable<object>` &mdash;один поставщик представление может возвращать несколько представлений для объекта. Все поставщики представление вызываются для каждого объекта, поэтому очень важно не делать никаких предположений о том, какие объекты передаются вам.

Последним шагом является фактически регистрация наш поставщик в агенте и указывающие расположение нашей тип интеграции книг. Для регистрации поставщика, добавьте следующий код в `IntegrateWith` метод `SampleIntegration` класса, созданную ранее:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Тип интеграции параметра осуществляется через атрибут уровня сборки. Это можно поместить в вашей AssemblyInfo.cs или в том же классе вашему типу интеграции для удобства:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Во время разработки, может оказаться более удобным способом [ `AddProvider` перегрузки] [ addprovider] на `RepresentationManager` , которые позволяют зарегистрировать обратный вызов простой обеспечивают представления в книге а затем переместите этот код в вашей `RepresentationProvider` реализацию после завершения работы. Пример для подготовки к просмотру [ `OxyPlot` ] [ oxyplot] `PlotModel` может выглядеть следующим образом:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Эти API-интерфейсы позволяют быстро приступить к работе, но не рекомендуется доставки всю интеграцию только с их помощью&mdash;они предоставляют очень мало контроль над обработкой типы клиентом.

Зарегистрированные представлением интеграцию готов к отправке!

## <a name="shipping-your-integration"></a>Доставка интеграции

Отправка интеграции, необходимо добавить его в пакет NuGet.
Можно переслать его с помощью NuGet существующую библиотеку или при создании нового пакета этот файл .nuspec шаблон можно использовать в качестве отправной точки.
Необходимо будет заполнить разделы, относящиеся к интеграции. Наиболее важной частью является, все файлы для интеграции находились в `xamarin.interactive` каталог в корне пакета. Это позволяет легко находить все значимые файлы для интеграции, независимо от того, является ли использовать существующий пакет или создать новую.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

После создания файла .nuspec можно упаковать NuGet следующим образом:

```csharp
nuget pack MyIntegration.nuspec
```

а затем опубликовать его в [NuGet][nugetorg]. Как только он существует, можно будет ссылаться на него из любой книги и посмотрите в действии. На снимке экрана ниже мы упакован образец интеграции мы построен в этом документе и установить пакет NuGet в книге:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Книги с интеграцией](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Книги с интеграцией](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Обратите внимание, что не отображаются `#r` директивы или каком-либо для инициализации интеграции — книг Разобравшись все это для вас в фоновом!

## <a name="next-steps"></a>Следующие шаги

Ознакомьтесь с нашей документации, Дополнительные сведения о скользящего элементы, входящие в состав пакета SDK, и мы [пример интеграции](~/tools/workbooks/samples/index.md) для выполнения дополнительных задач, можно сделать из интеграции, например, пользовательским кодом JavaScript, выполняемого в Клиент книг.

[integration-type]: https://developer.xamarin.com/api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[xir]: https://developer.xamarin.com/api/namespace/Xamarin.Interactive.Representations/
[reppr]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
