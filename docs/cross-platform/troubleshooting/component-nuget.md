---
title: Обновление ссылок на компоненты для NuGet
description: Замените обращается Ваш компонент пакеты NuGet, чтобы в будущем приложений.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: a76adab41e9f7de5abb391e69a5b27783e0c3a63
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="updating-component-references-to-nuget"></a>Обновление ссылок на компоненты для NuGet

_Замените обращается Ваш компонент пакеты NuGet, чтобы в будущем приложений._

В этом руководстве объясняется, как обновить существующие решения Xamarin для изменения компонента ссылки на пакеты NuGet.

- [Компоненты, которые содержат пакеты NuGet](#contain)
- [Компоненты с замен NuGet](#replace)

Большинство компонентов относятся к одному из этих категорий.
Если вы используете это компонент, который не включает пакет NuGet эквивалент, ознакомьтесь со статьей [компоненты без пути миграции NuGet](#require-update) разделе ниже.

Ссылаться на эти страницы более подробные инструкции по добавлению на пакеты NuGet [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) или [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

## <a name="opening-a-project-containing-a-component"></a>Открытие проекта, содержащего компонент

В ноябре 2017 г., было [было объявлено](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) , хранилище компонентов Xamarin будет прекращена. С целью обеспечения sunsetting компонентов 15.6 выпуск Visual Studio и 7.4 выпуск Visual Studio для Mac больше не поддерживают компоненты в проекте. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Если загрузить проект в Visual Studio, отображается следующее диалоговое окно, о том, что необходимо удалить все компоненты из проекта вручную.

![Диалоговое окно о том, что компонент находится в проекте и должен быть удален на предупреждения](component-nuget-images/component-alert-vs.png)

Чтобы удалить компонент из проекта:

1. Откройте CSPROJ-файл. Для этого щелкните правой кнопкой мыши имя проекта и выберите **выгрузить проект**. 

2. Снова щелкните правой кнопкой мыши выгрузить проект и выберите **изменить CSPROJ {ваш проект name}**.

3. Найти все ссылки в файле `XamarinComponentReference`. Он должен выглядеть примерно следующим образом:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Удалите ссылки на `XamarinComponentReference` и сохраните файл. В приведенном выше примере можно безопасно удалить весь `ItemGroup`.

5. После сохранения файла, щелкните правой кнопкой мыши имя проекта и выберите **перезагрузить проект**.

6. Повторите предыдущие шаги для каждого проекта в решении.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Если загрузить проект в Visual Studio для Mac, отображается следующее диалоговое окно, о том, что необходимо удалить все компоненты из проекта вручную.

![Диалоговое окно о том, что компонент находится в проекте и должен быть удален на предупреждения](component-nuget-images/component-alert.png)

Чтобы удалить компонент из проекта:

1. Откройте CSPROJ-файл. Для этого щелкните правой кнопкой мыши имя проекта и выберите **Сервис > изменить файл**.

2. Найти все ссылки в файле `XamarinComponentReference`. Он должен выглядеть примерно следующим образом:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Удалите ссылки на `XamarinComponentReference` и сохраните файл. В приведенном выше примере можно безопасно удалить весь `ItemGroup`

4. Повторите предыдущие шаги для каждого проекта в решении. 

-----

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Компоненты, которые содержат пакеты NuGet

Многие компоненты уже содержат пакеты NuGet и пути миграции, достаточно удалить ссылки на компонент.

Можно определить, включает ли компонент уже пакет NuGet, дважды щелкните компонент в решении.

![Развернуть узел компонентов](component-nuget-images/solution-sml.png)

**Пакетов** вкладке будут перечислены все пакеты NuGet, входящих в компонент:

![Вкладка «пакеты» содержит NuGet](component-nuget-images/packages-tab-sml.png)

Обратите внимание, что **сборки** вкладка будет пустым:

![Вкладка "сборки" пуст.](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Обновление решения

Чтобы обновить решение, удалите **компонент** входа из решения:

![Удаление компонента](component-nuget-images/delete-component-sml.png)

Пакет NuGet останется в **пакетов** узла и приложения Скомпилируйте и запустите обычным образом. В будущем обновления для этого пакета будет выполняться через **Nuget** функцию обновления:

![Обновление пакета NuGet](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Компоненты с замен NuGet

Если страница сведений о компоненте **сборки** вкладка содержит записи, как показано ниже, вам потребуется найти эквивалентное пакет NuGet вручную.

![Содержит сборки](component-nuget-images/assemblies-tab-sml.png)

Обратите внимание, что **пакетов** вкладку, скорее всего, будет пустым:

![](component-nuget-images/packages-tab-empty-sml.png)

_Он может содержать зависимостей NuGet, но их можно проигнорировать._


Чтобы подтвердить замену существует пакет NuGet, найдите на [NuGet.org](https://www.nuget.org/packages), с помощью имени компонента, или в качестве альтернативы автором.

Например, можно найти популярный **sqlite-net-pcl** пакета путем поиска:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) — Имя продукта.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) — имя автора профиля.


### <a name="updating-the-solution"></a>Обновление решения

Когда вы убедились, что компонент доступен в NuGet, выполните следующие действия.

#### <a name="delete-the-component"></a>Удаление компонента

Щелкните правой кнопкой мыши компонент в решении и выберите **удалить**:

![Удаление компонента](component-nuget-images/remove-component-sml.png)

Компонент и все ссылки будут удалены. Это приведет к разрыву построения, пока вы не добавите эквивалентные пакет NuGet заменить его.

#### <a name="add-the-nuget-package"></a>Добавьте пакет NuGet

1. Щелкните правой кнопкой мыши **пакетов** узел и выберите **Добавление пакетов...** .
2. Поиск для замены имени или автором NuGet:

  ![](component-nuget-images/nuget-search-sml.png)

3. Нажмите клавишу **добавить пакет**.

Пакет NuGet будет добавлен в проект, а также любые зависимости.
Это должно исправить сборки. Если построение сохранится, проанализируйте каждой ошибки, чтобы увидеть, если бы различиях API компонента и пакета NuGet.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Компоненты без пути миграции NuGet

Не стоит беспокоиться в том случае, если вы не нашли немедленно замены для компонентов, используемых в приложении. Существующие компоненты будут работать в Visual Studio 15,5 и **компоненты** узел будет отображаться в вашем решении, как обычно.

Тем не менее, в будущих выпусках Visual Studio будет _не_ восстановления или обновления компонентов.
Это означает, если открыть решение на новом компьютере, компонент не будет загружено и установлено; и автор не смогут предоставить все обновления. Необходимо запланировать:

* Извлеките сборок из компонента и ссылаться на них непосредственно в проект.
* Обратитесь к автору компонента и попросите о планах для миграции в NuGet.
* Изучить альтернативные пакетов NuGet или поиск исходного кода, если компонент является открытым исходным кодом.

Многие поставщики компонент по-прежнему работаете миграции для NuGet, и другим пользователям (включая продаже продуктов) может исследование параметры альтернативного доставки.


## <a name="related-links"></a>Связанные ссылки

- [Установка и использование пакета NuGet (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Включая пакет NuGet (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
