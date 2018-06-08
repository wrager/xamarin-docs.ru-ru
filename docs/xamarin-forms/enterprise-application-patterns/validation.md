---
title: Проверка
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 484f3b3d45e41d0dd0406681250ac90943a1cdde
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847593"
---
# <a name="validation"></a>Проверка

Любое приложение, которое принимает ввод от пользователей следует убедиться, что ввод был допустимым. Приложения например, проверьте наличие ввода, который содержит только символы в конкретном диапазоне имеет определенную длину и соответствует определенному формату. Без проверки пользователи могут вводить данные, приведет к сбою приложения. Проверка применяет бизнес-правил и не позволяет злоумышленнику вводится вредоносные данные.

В контексте объекта модели ViewModel (MVVM) шаблон модели представления или модели часто должны будут выполнять проверку данных и указывают ошибок проверки в представление, чтобы пользователь мог устранить их. Мобильное приложение eShopOnContainers выполняет синхронный клиентской проверки свойства модели представления и уведомляет пользователя ошибки проверки, выделив элемент управления, который содержит недопустимые данные, а также путем отображения сообщения об ошибках, информирующие пользователя из почему данные являются недопустимыми. Рис. 6-1 показывает классы, участвующие в проверке в мобильном приложении eShopOnContainers.

[![](validation-images/validation.png "Классы проверки в мобильном приложении eShopOnContainers")](validation-images/validation-large.png#lightbox "классы проверки в мобильном приложении eShopOnContainers")

**Рис. 6-1**: классы проверки в мобильном приложении eShopOnContainers

Просмотр свойств модели, требующих проверки, относятся к типу `ValidatableObject<T>`и каждый `ValidatableObject<T>` экземпляр имеет правила проверки, добавленные его `Validations` свойство. Проверка вызывается из модели представления путем вызова `Validate` метод `ValidatableObject<T>` экземпляр, который извлекает проверки правил и выполняет их от `ValidatableObject<T>` `Value` свойство. Ошибки проверки, помещаются в `Errors` свойство `ValidatableObject<T>` экземпляр и `IsValid` свойство `ValidatableObject<T>` экземпляр добавлена информация о том, успешно ли выполнено проверки.

Уведомление об изменении свойства обеспечивается `ExtendedBindableObject` класса и поэтому [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления можно привязать к `IsValid` свойство `ValidatableObject<T>` экземпляр в классе представления модели, чтобы получать уведомления о том, следует ли введенные данные являются допустимыми.

## <a name="specifying-validation-rules"></a>Указание правил проверки

Правила проверки задаются путем создания класса, производного от `IValidationRule<T>` интерфейс, как показано в следующем примере кода:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Этот интерфейс указывает, должен предоставить класса правила проверки `boolean` `Check` метод, который используется для выполнения проверки, и `ValidationMessage` , значение которого является сообщение об ошибке проверки, которое будет отображаться, если свойство Проверка не пройдена.

В следующем примере кода показан `IsNotNullOrEmptyRule<T>` правило проверки, который используется для проверки имени пользователя и пароля, введенные пользователем `LoginView` при использовании макета служб в мобильном приложении eShopOnContainers:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check` Возвращает `boolean` , определяющее, является ли аргумент значение `null`, пустой или состоит только из пробельных символов.

Несмотря на то, что не используется eShopOnContainers мобильного приложения, в следующем примере кода показано правило проверки для проверки адреса электронной почты:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check` Возвращает `boolean` , указывающее, является ли аргумент значение допустимый адрес электронной почты. Это достигается путем поиска значение аргумента для первого вхождения шаблон регулярного выражения, указанного в `Regex` конструктора. Шаблон регулярного выражения найдены во входной строке можно определить с помощью проверки значения `Match` объекта `Success` свойство.

> [!NOTE]
> Проверка свойства иногда может включать зависимые свойства. Пример зависимые свойства: набор допустимых значений для свойств типа зависит определенное значение, которое было задано в свойстве б. Для проверки того, что значение свойства типа является одним из допустимых значений будет включать в себя получение значения свойства б. Кроме того при изменении значения свойства B, свойство A может потребоваться повторно проверить.

## <a name="adding-validation-rules-to-a-property"></a>Добавление правил проверки к свойству

В мобильном приложении eShopOnContainers просмотреть свойства модели, требующих проверки объявлены с типом `ValidatableObject<T>`, где `T` — тип данных для проверки. В следующем примере кода показан пример двух свойств.

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Чтобы выполнялась проверка, необходимо добавить правила проверки `Validations` коллекции каждого `ValidatableObject<T>` экземпляра, как показано в следующем примере кода:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Этот метод добавляет `IsNotNullOrEmptyRule<T>` правило проверки `Validations` коллекции каждого `ValidatableObject<T>` экземпляра, указав значения для правила проверки `ValidationMessage` свойство, которое указывает сообщение об ошибке проверки, которое будет отображаться, если Проверка не пройдена.

## <a name="triggering-validation"></a>Запуск проверки

Подход проверки, используемый в мобильном приложении eShopOnContainers могут вручную включать и выключать проверку свойства автоматически триггер проверку и при изменении свойства.

### <a name="triggering-validation-manually"></a>Запуск проверки вручную

Для представления свойства модели проверки можно запустить вручную. Например, это происходит в мобильном приложении eShopOnContainers, когда пользователь касается **входа** кнопку `LoginView`, при использовании макета служб. Команда вызывает делегат `MockSignInAsync` метод в `LoginViewModel`, который вызывает проверку при выполнении `Validate` метода, как показано в следующем примере кода:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate` Метод выполняет проверку имя пользователя и пароль, введенный пользователем `LoginView`, путем вызова метода Validate каждого `ValidatableObject<T>` экземпляр. В следующем примере кода показан метод Validate из `ValidatableObject<T>` класса:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Этот метод очищает `Errors` коллекции, а затем извлекает проверку правил, которые были добавлены в объект `Validations` коллекции. `Check` Метод для каждого полученного условие выполняется и `ValidationMessage` добавляется значение свойства для любого правила проверки, который не удалось проверить данные `Errors` коллекцию `ValidatableObject<T>` экземпляра. Наконец `IsValid` является свойство и его значение возвращается в вызывающий метод, указывающее, успешно ли выполнено проверки.

### <a name="triggering-validation-when-properties-change"></a>Активируя проверку при изменении свойств

Проверка также может запускаться при каждом изменении связанного свойства. Например, когда двустороннюю привязку в `LoginView` задает `UserName` или `Password` запускается свойства проверки. В следующем примере кода показано, как это происходит:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Управления привязывается к `UserName.Value` свойство `ValidatableObject<T>` экземпляра и элемент управления `Behaviors` коллекция `EventToCommandBehavior` экземпляра, добавить к нему. Это поведение выполняет `ValidateUserNameCommand` в ответ на [`TextChanged`] запуск события на `Entry`, которое вызывается, когда текст в `Entry` изменения. В свою очередь `ValidateUserNameCommand` выполняет делегат `ValidateUserName` метод, который выполняет `Validate` метод `ValidatableObject<T>` экземпляра. Таким образом, каждый раз пользователь вводит символ `Entry` выполняется управления для имени пользователя, проверки введенных данных.

Дополнительные сведения о поведении см. в разделе [реализации поведения](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Отображение ошибок проверки

EShopOnContainers мобильное приложение уведомляет пользователя ошибки проверки, выделив элемент управления, который содержит недопустимые данные красной линией, и отображается сообщение об ошибке, информирующего пользователя о том, почему данные являются недопустимыми ниже управления, содержащего недопустимые данные. Если исправить недопустимые данные, строке изменяет черный и сообщение об ошибке будет удалено. Рис. 6-2 показывает LoginView в мобильном приложении eShopOnContainers при наличии ошибок проверки.

![](validation-images/validation-login.png "Отображение ошибок проверки во время входа")

**Рис. 6-2:** отображение ошибок проверки во время входа

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Выделите элемент управления, который содержит недопустимые данные.

`LineColorBehavior` Вложенное поведение, используемым для выделения [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) элементов управления, где произошли ошибки проверки. В следующем примере кода показан способ `LineColorBehavior` вложенное поведение подключен к `Entry` управления:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Управления использует явный стиль, как показано в следующем примере кода:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Задает стиль `ApplyLineColor` и `LineColor` вложенные свойства `LineColorBehavior` присоединенного поведение в [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления. Дополнительные сведения о стилях см. в разделе [стили](~/xamarin-forms/user-interface/styles/index.md).

Если значение `ApplyLineColor` вложенное свойство — это набор или изменения, `LineColorBehavior` выполняет вложенное поведение `OnApplyLineColorChanged` метода, как показано в следующем примере кода:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Параметры для этого метода укажите экземпляр элемента управления, применяется поведение и старое и новое значения `ApplyLineColor` вложенное свойство. `EntryLineColorEffect` Класс добавляется для элемента управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции Если `ApplyLineColor` вложенное свойство `true`, в противном случае он удаляется из элемента управления `Effects` коллекции. Дополнительные сведения о поведении см. в разделе [реализации поведения](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Подклассов [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) класса и показано в следующем примере кода:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Класс представляет эффекта независимая от платформы, которая служит оболочкой для внутреннего эффект, зависит от платформы. Это упрощает процесс удаления эффекта, так как нет доступа во время компиляции для сведений о типе для создания эффекта от платформы. `EntryLineColorEffect` Вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения и уникальный идентификатор, указанный для каждого класса эффект от платформы.

В следующем примере кода показан `eShopOnContainers.EntryLineColorEffect` реализацию для iOS:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached` Метод извлекает собственный элемент управления для Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления и обновляет цвет линии, вызвав `UpdateLineColor` метод. `OnElementPropertyChanged` Переопределение реагирует на изменения привязываемые свойства на `Entry` управления, обновив цвет линии, если вложенный `LineColor` изменения свойств или [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) свойство `Entry`изменения. Дополнительные сведения об эффектах см. в разделе [эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

При вводе в допустимые данные [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления, будут применяться черную линию в нижней части элемента управления для указания, что нет ошибок проверки. Рис. 6-3 показан пример этого.

![](validation-images/validation-blackline.png "Черная линия, указывающих на ошибку без проверки")

**Рис. 6-3**: черная линия, указывающих на ошибку без проверки

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Управления также имеет [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) добавлены его [ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) коллекции. В следующем примере кода показан `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Это [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) мониторы `UserName.IsValid` свойства, а если значение становится `false`, он выполняет [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/), какие изменения `LineColor` присоединенного Свойство `LineColorBehavior` присоединенного поведение на красный. Рис. 6-4 показан пример этого.

![](validation-images/validation-redline.png "Красная линия, указывающих на ошибку проверки")

**Рис. 6-4**: красной линией, сообщая об ошибке проверки

В строке [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления останется красным, пока введенные данные недопустимы, в противном случае это приведет к изменению в черный, и для указания правильность введенных данных.

Дополнительные сведения о триггерах см. в разделе [триггеры](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Отображение сообщений об ошибках

Пользовательский Интерфейс отображает сообщения об ошибках проверки в элементы управления Label ниже каждого элемента управления, в которых данные не прошли проверку. В следующем примере кода показан [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , отображается сообщение об ошибке проверки, если пользователь не вошел на допустимое имя пользователя:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Каждый [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) привязывает `Errors` свойство объекта модели представления, подлежащий проверке. `Errors` Предоставляется свойство `ValidatableObject<T>` класса и имеет тип `List<string>`. Поскольку `Errors` свойство может содержать несколько ошибок проверки, `FirstValidationErrorConverter` экземпляра используется для получения из коллекции для отображения первой ошибки.

## <a name="summary"></a>Сводка

Мобильное приложение eShopOnContainers выполняет синхронный клиентской проверки свойства модели представления и уведомляет пользователя ошибки проверки, выделив элемент управления, который содержит недопустимые данные, а также путем отображения сообщения об ошибках, информирующие пользователя Почему данные являются недопустимыми.

Просмотр свойств модели, требующих проверки, относятся к типу `ValidatableObject<T>`и каждый `ValidatableObject<T>` экземпляр имеет правила проверки, добавленные его `Validations` свойство. Проверка вызывается из модели представления путем вызова `Validate` метод `ValidatableObject<T>` экземпляр, который извлекает проверки правил и выполняет их от `ValidatableObject<T>` `Value` свойство. Ошибки проверки, помещаются в `Errors` свойство `ValidatableObject<T>`экземпляр и `IsValid` свойство `ValidatableObject<T>` экземпляр добавлена информация о том, успешно ли выполнено проверки.


## <a name="related-links"></a>Связанные ссылки

- [Загрузить электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
