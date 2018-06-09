---
title: Сводка Глава 18. MVVM
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка Глава 18. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 4a9e8221828ddd49c10dc4f14dc996acc167ae2f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240435"
---
# <a name="summary-of-chapter-18-mvvm"></a>Сводка Глава 18. MVVM

Один из лучших способов архитектуры приложения является разделив пользовательский интерфейс базовый код, который иногда называют *бизнес-логики*. Существует несколько методик, однако, адаптированные для среды на основе XAML называется Model-View-ViewModel или MVVM.

## <a name="mvvm-interrelationships"></a>Взаимосвязи MVVM

Приложение MVVM имеется три уровня:

- Эта модель обеспечивает базовые данные иногда через файлы или обращается к web
- Представление является пользовательского интерфейса или презентацию слоя, обычно реализуется в XAML
- ViewModel подключается модели и представления

Модель является относительно этих ViewModel и ViewModel игнорирующих представления. Эти три уровня обычно подключаться друг к другу с помощью указанных ниже:

![Представление, ViewModel и представление](images/ch18fg03.png "MVVM")

Многие небольшие программы (и даже большие) часто модели отсутствует или его функциональность интегрирована в ViewModel.

## <a name="viewmodels-and-data-binding"></a>ViewModels и привязка данных

Выполнять привязки данных, ViewModel должен поддерживать уведомления представления при изменении свойства ViewModel. ViewModel делает это путем реализации [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) интерфейс `System.ComponentModel` пространства имен. Это является частью .NET, а не Xamarin.Forms. (Обычно ViewModels попытка поддержания независимости от платформы.)

`INotifyPropertyChanged` Интерфейс объявляет одно событие с именем [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) указывает измененного свойства.

### <a name="a-viewmodel-clock"></a>Часы ViewModel

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) В [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) библиотека определяет свойство типа `DateTime` , изменения на основе таймера. Этот класс реализует `INotifyPropertyChanged` и запускает `PropertyChanged` событие каждый раз, когда `DateTime` изменения свойств.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) образец создает этот ViewModel и использует ViewModel привязки данных для отображения обновленных датой и временем.

### <a name="interactive-properties-in-a-viewmodel"></a>Интерактивный свойства ViewModel

Свойства в ViewModel могут быть более интерактивны, как показано в предыдущем [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) класс, который входит в состав из [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) образца. Привязки данных укажите множимое и множитель значения из двух `Slider` элементы и отображения продукт с `Label`. Тем не менее то сможете значительные изменения этот пользовательский интерфейс в XAML без последующая изменений в ViewModel или в файле кода.

### <a name="a-color-viewmodel"></a>ViewModel цвета

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) В [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) библиотеки интегрируется в моделях цвета RGB и HSL. Демонстрируется в [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) образца:

[![Снимок экрана тройной TK](images/ch18fg08-small.png "HSL цветовой модели")](images/ch18fg08-large.png#lightbox "HSL цветовой модели")

### <a name="streamlining-the-viewmodel"></a>Оптимизировать ViewModel

Можно упростить код в ViewModels, определив `OnPropertyChanged` с помощью метода [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) атрибут, который автоматически получает имя свойства вызывающего. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) библиотеки делает именно это и предоставляет базовый класс для ViewModels.

## <a name="the-command-interface"></a>Интерфейс командной

MVVM работает с привязок данных и привязки данных работать со свойствами, поэтому MVVM кажется объемов когда дело доходит до обработки `Clicked` событие `Button` или `Tapped` событие `TapGestureRecognizer`. Чтобы разрешить ViewModels для обработки таких событий, поддерживает Xamarin.Forms *интерфейс командной*.

Интерфейс командной проявляется в `Button` с два открытых свойства:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) Тип [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (определенные в `System.Windows.Input` пространства имен)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) типа `Object`

Чтобы поддерживать интерфейс команды, ViewModel необходимо определить свойство типа `ICommand` , затем данными, привязанными к `Command` свойство `Button`. `ICommand` Интерфейс объявляет два метода и одно событие:

- [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) Метода с аргументом типа `object`
- Объект [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) метода с аргументом типа `object` , возвращающий `bool`
- Объект [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) событий

На внутреннем уровне ViewModel задает каждое свойство типа `ICommand` к экземпляру класса, который реализует `ICommand` интерфейса. Через привязку данных, `Button` первоначально вызываемые `CanExecute` метода и прекращает работу, если метод возвращает `false`. Этот параметр также устанавливает обработчик `CanExecuteChanged` событий и вызывает метод `CanExecute` при каждом запуске события. Если `Button` — включен, он вызывает `Execute` метод всякий раз, когда `Button` нажата.

Возможно, некоторые ViewModels, предшествуют Xamarin.Forms и они могут уже поддерживают интерфейс командной. Для новых ViewModels предназначен для использования только с помощью Xamarin.Forms, предоставляющий Xamarin.Forms [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) класса и [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) класса, реализующих интерфейс `ICommand` интерфейс. Универсальный тип является типом аргумента `Execute` и `CanExecute` методы.

### <a name="simple-method-executions"></a>Простой способ выполнения

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) образце показано, как использовать интерфейс командной в ViewModel. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Класс определяет два свойства типа `ICommand` и также определяет два закрытого свойства, которые она передает простой [ `Command` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/). Программа содержит привязки данных из этого ViewModel для `Command` свойства двух `Button` элементов.

`Button` Элементы можно легко заменить `TapGestureRecognizer` объектов в XAML без изменения кода.

### <a name="a-calculator-almost"></a>Калькулятор почти

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) пример делает использование обеих `Execute` и `CanExecute` методы `ICommand`. Она использует [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) библиотеки. ViewModel содержит шесть свойств типа `ICommand`. Они инициализируются из [ `Command` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) и [ `Command` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) из `Command` и [ `Command<T>` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) из `Command<T>`. Числовые ключи, добавление машины привязываются к свойству, которое инициализируется с `Command<T>`и `string` аргумент `Execute` и `CanExecute` идентифицирует конкретный ключ.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels и жизненного цикла приложения

`AdderViewModel` Используется в **AddingMachine** образец также определяет два метода с именем `SaveState` и `RestoreState`. Эти методы вызываются из приложения при переходе в спящий режим и при его запуске.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 18 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Образцы Глава 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
