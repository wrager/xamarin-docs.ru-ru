---
title: Диспетчер визуальных состояний Xamarin.Forms
description: Используйте Диспетчер визуальных состояний вносить изменения в элементы XAML на основании визуальных состояний задать в коде.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 3c0330d8d6d07112350db007d0500d57c236dc24
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848028"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Диспетчер визуальных состояний Xamarin.Forms

_Используйте Диспетчер визуальных состояний вносить изменения в элементы XAML на основании визуальных состояний задать в коде._

Диспетчер визуального состояния (VSM) впервые появилось в версии 3.0 Xamarin.Forms. VSM структурированных позволяет вносить визуальные изменения в пользовательском интерфейсе из кода. В большинстве случаев пользовательском интерфейсе приложения определяется на языке XAML, и этот код XAML содержит разметку, описывающий, как Диспетчер визуальных состояний влияет на визуальные элементы пользовательского интерфейса.

VSM введено понятие _визуальных состояний_. Xamarin.Forms представления, такие как `Button` может иметь несколько различное визуальное представление в зависимости от состояния базовой &mdash; ли он отключен, нажата или фокус ввода. Это состояний кнопки.

Визуальные состояния собираются в _визуальное состояние группы_. Визуальные состояния в группе визуальных состояний являются взаимоисключающими. Визуальные состояния и визуальное состояние группы определяются по простые текстовые строки.

Диспетчер визуальных состояний Xamarin.Forms определяет одну группу визуального состояния, с именем «CommonStates» и три визуальных состояний:

- "Normal"
- «Отключено»
- «С фокусом ввода»

Эта группа визуального состояния поддерживается для всех классов, производных от [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), который является базовым классом для [ `View` ](xref:Xamarin.Forms.View) и [ `Page` ](xref:Xamarin.Forms.Page). 

Можно также определить собственные визуальное состояние группы и визуальных состояний в этой статье описывается.

> [!NOTE]
> Xamarin.Forms разработчики, знакомые с [триггеры](~/xamarin-forms/app-fundamentals/triggers.md) известно, что триггеры также можно внести изменения в визуальные элементы в пользовательском интерфейсе на основе изменений в свойства представления или срабатывание события. Однако использование триггеров для работы с различными сочетаниями этих изменений может стать довольно сложными. Исторически Диспетчер визуальных состояний впервые появился в среды на основе Windows XAML, чтобы компенсировать путаницы, возникающие в результате сочетания визуальных состояний. В VSM визуальных состояний в группе визуальных состояний всегда являются взаимоисключающими. В любой момент только один в каждой группе вариант, текущее состояние.

## <a name="the-common-states"></a>Общие состояния

Диспетчер визуальных состояний можно включать в себя разделы в свой файл XAML, можно изменить внешний вид представления, если представление обычного или отключен или имеет фокус ввода. Эти функции известны как _общие состояния_.

Например, предположим, что у вас есть `Entry` представления на странице, и нужно, чтобы внешний вид `Entry` изменение одним из следующих способов:

- `Entry` Должны иметь розовый цвет фона при `Entry` отключена.
- `Entry` Следует обычно имеют Салатовый фона.
- `Entry` Должен развернуть дважды обычный высоту, когда фокус ввода.

Можно присоединить разметки VSM в отдельном окне, или если он применяется несколько представлений можно определить в стиле. В следующих двух разделах этих подходов.

### <a name="vsm-markup-on-a-view"></a>VSM разметку в представлении

Чтобы присоединить VSM разметку для `Entry` Просмотр, сначала разделите `Entry` в открывающий и закрывающий теги:

```xaml
<Entry FontSize="18">

</Entry>
```

Предоставил размер шрифта явные, так как одно из состояний будет использовать `FontSize` свойство удвоить размер текста в `Entry`.

Затем вставьте `VisualStateManager.VisualStateGroups` теги между этими тегами:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) является присоединенным свойством привязываемых определяется [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) класса. (Дополнительные сведения о вложенных привязываемые свойства см. в статье [вложенные свойства](~/xamarin-forms/xaml/attached-properties.md).) Это как `VisualStateGroups` свойство присоединено к `Entry` объекта.

`VisualStateGroups` Свойство относится к типу [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), которое является коллекцией из [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) объектов. В пределах `VisualStateManager.VisualStateGroups` тегов, вставить пару `VisualStateGroup` теги для каждой группы визуальных состояний, которые требуется включить:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Обратите внимание, что `VisualStateGroup` тег имеет `x:Name` атрибут, указывающий, имя группы. `VisualStateGroup` Класс определяет `Name` свойство, которое можно использовать вместо этого:

```xaml
<VisualStateGroup Name="CommonStates">
```

Можно использовать любой `x:Name` или `Name` , но не одновременно в одном элементе.

`VisualStateGroup` Класс определяет свойство с именем [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), которое является коллекцией из [ `VisualState` ](xref:Xamarin.Forms.VisualState) объектов. `States` — _свойства содержимого_ из `VisualStateGroups` , можно включить `VisualState` теги непосредственно между `VisualStateGroup` тегов. (Содержимого, свойств будут рассмотрены в статье [Essential синтаксиса XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Следующим шагом является включение пару тегов для каждого визуального состояния в этой группе. Их также можно определить с помощью `x:Name` или `Name`:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` Определяет свойство с именем [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), которое является коллекцией из [ `Setter` ](xref:Xamarin.Forms.Setter) объектов. То же самое `Setter` объекты, используемые в [ `Style` ](xref:Xamarin.Forms.Style) объекта.

`Setters` — _не_ свойства content `VisualState`, поэтому необходимо включить теги элементов свойства для `Setters` свойства:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
    
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Теперь можно вставить один или несколько `Setter` объектов между каждой парой `Setters` тегов. Это `Setter` объектами, которые определяют визуальных состояний, описанных ранее:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Каждый `Setter` тег указывает значение конкретного свойства после текущего состояния. Любое свойство ссылается `Setter` объекта должны быть основаны на привязываемые свойства.

Аналогичным образом разметки является основой **VSM для представления** страницы в **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** образец программы. На этой странице доступны три `Entry` представления, но только второй имеет VSM разметки, подключенных к нему:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />

        <Label Text="Entry with VSM: " />

        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Обратите внимание, что второй `Entry` также имеет `DataTrigger` как часть его `Trigger` коллекции. В результате `Entry` отключена, пока что-то вводится в третий `Entry`. Вот страницы во время запуска, работающих на Android, iOS и универсальной платформы Windows (UWP).

[![VSM для представления: Отключено](vsm-images/VsmOnViewDisabled.png "VSM для представления - отключено")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Текущее визуальное состояние «Disabled» поэтому фона второго `Entry` используется розовый на iOS и Android экранов. Реализация UWP `Entry` не разрешает задание фонового цвета при `Entry` отключена. 

При вводе некоторый текст в третий `Entry`, второй `Entry` коммутаторы в состояние «Normal», а фон теперь является Салатовый:

[![VSM для представления: обычный](vsm-images/VsmOnViewNormal.png "VSM на представление — обычный")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Когда touch второй `Entry`, он получает фокус ввода. Он переключается в состояние «Фокуса» и расширяет высоту дважды:

[![VSM для представления: с фокусом ввода](vsm-images/VsmOnViewFocused.png "VSM на представление — с фокусом ввода")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Обратите внимание, что `Entry` не сохранять Салатовый фона, когда он получает фокус ввода. Как Диспетчер визуальных состояний переключается между визуальными состояниями, свойства, заданные в предыдущее состояние, не задано. Имейте в виду, что визуальные состояния являются взаимоисключающими. Состояние «Normal» означает, только что `Entry` включен. Это значит, что `Entry` включен и не имеет фокус ввода. 

Если вы хотите `Entry` чтобы фон Салатовый в состоянии «Фокуса», добавьте еще один `Setter` для этого визуального состояния:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

В порядке для этих `Setter` объектов для правильной работы `VisualStateGroup` должен содержать `VisualState` объектов для всех состояний в этой группе. При наличии визуального состояния, которое имеет `Setter` объектов, включите его все равно, как пустой тег:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Разметка Диспетчер визуальных состояний в стиле

Часто требуется совместно использовать одну и ту же разметку Диспетчер визуальных состояний между двумя или более представлений. В этом случае необходимо поместить в разметку `Style` определения.

Здесь подразумевается существующих `Style` для `Entry` элементов в **VSM на представление** страницы:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Добавить `Setter` теги для `VisualStateManager.VisualStateGroups` присоединенного привязываемые свойства:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Свойство содержимого для `Setter` — `Value`, поэтому значение `Value` свойство может быть задано непосредственно в эти теги. Свойство имеет тип `VisualStateGroupList`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style> 
```

В этих тегов можно включить один или несколько `VisualStateGroup` объектов:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style> 
```

В оставшейся части разметки VSM является прежней.

Вот **VSM в стиле** страницы, показывающая полную разметку VSM:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">

                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />
        
        <Label Text="Entry with VSM: " />

        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Теперь все `Entry` одинаково для их визуальных состояний отвечать представлений на этой странице. Обратите внимание, что состоянии «Фокуса» теперь содержит второй `Setter` , задающая каждого `Entry` Салатовый фоновых также, когда оно имеет фокус ввода:

[![VSM в стиле](vsm-images/VsmInStyle.png "VSM в стиле")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Определение собственных визуальных состояний

Каждый класс, производный от `VisualElement` поддерживает три общие состояния «Normal», «С фокусом ввода» и «Отключено». На внутреннем уровне [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) класс определяет, когда он становится включено или отключено, или фокус ввода и терять и вызывает статический [ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/) метод:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Это единственный код Диспетчер визуальных состояний, которые находятся в `VisualElement` класса. Поскольку `GoToState` вызывается для каждого объекта, на основе каждого класса, производного от `VisualElement`, можно использовать Диспетчер визуальных состояний с любым `VisualElement` объекта реагировать на эти изменения.

Интересно, что имя визуального состояния группы «CommonStates» не ссылается явно `VisualElement`. Имя группы не является частью API-Интерфейс для Диспетчер визуальных состояний. В одном из двух образец программы отображается на данный момент, можно изменить имя группы из «CommonStates», на что-либо еще, а также программы по-прежнему будет работать. Имя группы является просто общее описание состояниям в этой группе. Неявно понятно, что визуальных состояний в любой группе являются взаимоисключающими: одно состояние и только одно состояние текущего в любое время.

Если вы хотите реализовывать собственные визуальные состояния, необходимо вызвать `VisualStateManager.GoToState` из кода. Чаще всего будет вызывать этот метод в файле кода класса page.

**Проверки VSM** страницы в **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** образце показано, как использовать Диспетчер визуальных состояний в связи с проверки входных данных. В файле XAML состоит из двух `Label` элементов, `Entry`, и `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM разметки подключен ко второму `Label` (с именем `helpLabel`) и `Button` (с именем `submitButton`). Существуют два состояния взаимоисключающими, с именем «Допустимые» и «Недопустимый». Обратите внимание, что каждой из двух групп «ValidationState» `VisualState` теги «Действителен» и «Недопустимый», несмотря на то, что один из них является пустым в каждом случае. 

Если `Entry` не содержит допустимый номер телефона, то текущее состояние — «Недопустимо» и поэтому второй `Label` является видимым и `Button` отключена:

[![Проверка VSM: Недопустимое состояние](vsm-images/VsmValidationInvalid.png "проверка VSM - недопустимый")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Если введен допустимый номер телефона, текущее состояние становится «Допустимые». Второй `Entry` исчезает и `Button` включен:

[![Проверка VSM: Недопустимое состояние](vsm-images/VsmValidationValid.png "проверка VSM - допустимым")](vsm-images/VsmValidationValid-Large.png#lightbox)

Файл кода является reponsible для обработки `TextChanged` события из `Entry`. Обработчик использует регулярное выражение для определения допустимости входной строки или нет. Метод в файле кода, с именем `GoToState` вызывает статический `VisualStateManager.GoToState` метод для обоих `helpLabel` и `submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

Обратите внимание, что `GoToState` метод вызывается из конструктора для инициализации состояния. Всегда должно быть текущее состояние. Но нигде в коде есть любая ссылка на имя группы визуального состояния, несмотря на то, что она есть ссылки в XAML как «ValidationStates» в целях ясности. 

Обратите внимание, что файл кода программной необходимо выполнить учетной записи каждого объекта, на странице, зависит в этих визуальных состояний и вызова `VisualStateManager.GoToState` для каждого из этих объектов. В этом примере это только два объекта ( `Label` и `Button`), но может быть несколько больше.

Может возникнуть вопрос: Если файл кода программной должно ссылаться на странице, зависит от этих визуальных состояний каждого объекта, почему файл кода программной недоступны просто объекты непосредственно? Конечно же удалось. Преимущество использования VSM то, что можно выбрать, каким образом визуальные элементы реагировать на другое состояние полностью в XAML, который отслеживает все разработки пользовательского интерфейса в одном месте. Это позволяет избежать настройки внешнего вида, обратившись к визуальные элементы непосредственно из кода.

Может понадобиться рассмотрите возможность наследования от класса `Entry` и возможно определение свойства, можно задать для проверки внешних функций. Класс, производный от `Entry` затем может вызывать `VisualStateManager.GoToState` метод. Эта схема будет корректно работать, но только в том случае, если `Entry` были только объекта, связанного с различных визуальных состояний. В этом примере `Label` и `Button` будут работать некорректно. Нет способа VSM разметки, присоединенных к `Entry` для управления других объектов на странице и нет способа VSM разметки, присоединенных к ним других объектов для ссылки изменения визуального состояния другого объекта.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Используя Диспетчер визуальных состояний для адаптивной макета

Xamarin.Forms, который обычно приложение, работающее на телефоне можно просмотреть в книжной или Альбомная пропорции и Xamarin.Forms, выполняющихся на рабочем столе можно изменять принимать множество различных размеров и пропорций. Хорошо спроектированные приложения может отображать его содержимое по-разному для этих различных странице или в окне форм-факторов. 

Такой подход иногда называют _адаптивные размещение_. Поскольку макет адаптивной исключительно включает в себя визуальные элементы программы, это идеальное приложение Диспетчер визуальных состояний.

Простым примером является приложение, которое отображает небольшим набором кнопок, которые влияют на содержимое приложения. В режиме книжной ориентации эти кнопки может отображаться в верхней части страницы по горизонтали:

[![VSM адаптивной макета: Книжная](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM адаптивной макет - книжной ориентации")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

В альбомной ориентации массивом кнопок может перемещать с одной стороны и отображаются в столбце:

[![VSM адаптивной макета: Альбомная](vsm-images/VsmAdaptiveLayoutLandscape.png "адаптивной макет VSM - альбомная")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Сверху вниз программа выполняется в универсальную платформу Windows, iOS и Android.

**Адаптивные размещение VSM** страницы в [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) образец определяет группу с именем «OrientationStates» с двумя визуальные состояния с именем «» и «Книжной ориентации». (Более сложный подход может основываться на несколько разных ширина страницы или окна.) 

Разметка VSM происходит в четырех местах в файле XAML. `StackLayout` С именем `mainStack` содержит меню и содержимого, то есть `Image` элемента. Это `StackLayout` должны иметь вертикальную ориентацию в книжной ориентации и горизонтальной ориентации в альбомном режиме:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">

                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>

                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        
        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">

                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>

                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">

                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>

                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Внутренний `ScrollView` с именем `menuScroll` и `StackLayout` с именем `menuStack` реализации меню кнопок. Ориентация эти структуры является противоположностью из `mainStack`. Меню должно быть горизонтальный в книжной ориентации и по вертикали в альбомной ориентации.

В четвертой части VSM разметки находится в неявный стиль кнопок сами. Эта разметка задает `VerticalOptions`, `HorizontalOptions`, и `Margin` свойства, относящиеся к portait и альбомной ориентации.

Задает файл кода `BindingContext` свойство `menuStack` для реализации `Button` выполнение команд, а также присоединяет обработчик `SizeChanged` событий страницы:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged` Вызовов обработчика `VisualStateManager.GoToState` для двух `StackLayout` и `ScrollView` элементов, а затем обрабатывает в цикле дочерние элементы `menuStack` для вызова `VisualStateManager.GoToState` для `Button` элементов.

Может показаться, как если бы файл кода может обрабатывать изменения ориентации напрямую путем установки свойств элементов в файле XAML, но Диспетчер визуальных состояний определенно более структурированный подход. Все визуальные элементы сохраняются в файле XAML, где они становятся легче проверить, обслуживании и изменении.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Диспетчер визуальных состояний с Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Диспетчер визуальных состояний Xamarin.Forms 3.0, по [университета Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Связанные ссылки

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

