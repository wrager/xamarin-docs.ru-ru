---
title: "Привет, Контрольные значения"
description: "Начало работы с Xamarin и watchOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 732fd9808ded4bf97e99e7ab0e6ab63b989452d1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="hello-watch"></a>Привет, Контрольные значения

После создания решения, приведенные в [установки и настройки](~/ios/watchos/get-started/installation.md), будет иметь 3 проекта:

- Родительского приложения iOS, которая используется для установки или других административных задач на устройстве. (И другие типы операций ввода-вывода расширения, это часто называется «Контейнер» приложения.) С приложениями Контрольные значения могут быть использованы пользователям начать выполнение приложения наблюдения за без **когда-либо** под управлением родительского приложения;
- Контрольное значение расширения, которое содержит программный код для приложения Контрольные значения; и
- Приложение Контрольные значения, которое содержит раскадровки и графические ресурсы, которые подготавливаются к просмотру на часах.

Убедитесь, что ваш [правильности ссылок на](~/ios/watchos/get-started/project-references.md): что родительского приложения содержит ссылку на модуль, а расширение имеет ссылку на приложение контрольных значений.

Убедитесь, что ваш пакет идентификаторы выполните \*.watchkitextension \*.watchkitapp соглашение, а файл Info.plist вашего расширения имеет он имеет **WKApp идентификатор пакета** значение идентификатор пакета Приложение контрольных значений.

Можно запустить приложение Контрольные значения теперь, но так как файл раскадровки в приложении Контрольное значение пусто, невозможно определить.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "В обозревателе решений")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "В обозревателе решений")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
    
Дважды щелкните Interface.storyboard в приложении контрольных значений для запуска Xamarin iOS конструктор (при наличии на компьютере Mac вы можете щелкнуть правой кнопкой и **открыть с помощью > Xcode интерфейс построителя**)


1.  Убедитесь, **элементов** и **свойства** дополняет видимы,
1.  Щелкните, чтобы выбрать адаптер,
1.  Задание идентификатора и заголовка интерфейс контроллера для **interfaceController** и **Контрольные значения Hi**,
1.  Проверьте **класса** равно **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Значение interfaceController» и «Контрольное Hi идентификатор и название контроллер интерфейса")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Дважды щелкните Interface.storyboard в приложении контрольных значений для изменения с Xamarin iOS конструктора в Visual Studio.

1.  Откройте панель свойств;
1.  Измените класс, чтобы **InterfaceController**;
1.  Выберите контроллер интерфейса; и
1.  Задание идентификатора и заголовка интерфейс контроллера для **interfaceController** и **Контрольные значения Hi**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Значение interfaceController» и «Контрольное Hi идентификатор и название контроллер интерфейса")

-----


Создание пользовательского интерфейса:

1. Из **элементов** pad,
1. Перетаскивание **кнопку** и **метка** на сцены, и
1. Задайте текст и атрибуты элементов управления, как показано.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "Задайте текст и атрибуты элементов управления, как показано")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "Задайте текст и атрибуты элементов управления, как показано")

-----

1. Задать **имя** в **свойства** pad для каждого элемента управления. В этом примере мы используем `myButton` и `myLabel`.

1. Выберите кнопку на раскадровку и перейдите к **свойства** панель **события** список, затем

1. Создайте новый **действия** , введя `OnButtonPress` и нажав клавишу **ввод**.
  Действие будет отображаться в списке и разделяемый метод будут автоматически созданы на языке C#.

![](hello-watch-images/buttonaction.png "Действие OnButtonPress, добавить кнопки")

После сохранения раскадровки, **InterfaceController.designer.cs** добавляются имена элементов управления и действия... Если открыть этот файл, после обновления, можно увидеть как `RegisterAttribute` соответствует контроллеру и элементов управления пользовательского интерфейса соответствие переменные экземпляра C#, отмеченные `OutletAttribute` и способ сопоставления действий в разделяемые методы, помеченных `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Теперь откройте **InterfaceController.cs** (*не* InterfaceController.designer.cs) и добавьте следующий код:

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

Этот код должен быть достаточно прозрачно: переменная экземпляра `clickCount` — увеличивается каждый раз при функция `OnButtonPress` вызывается. Текст `myLabel` изменяется в соответствии с этой count. `myLabel`, конечно, является имя одного из выходов, созданный в XCode. `partial` Функция — это реализация функции, связанный с именем указанного действия.

Если он еще не запускаемого проекта

1. Щелкните правой кнопкой мыши проект расширения контрольных значений и выберите **Назначить запускаемым проектом**,

1. Цель развертывания можно задать в образ симулятор совместимое комплект средств для наблюдения за (таких как iPhone 6 iOS 8.2),

1. Нажмите клавишу **отладки** кнопки для вызова сборки и симулятора запуска.

    [![](hello-watch-images/readytodebug-sml.png "Элементы интерфейса Visual Studio")](hello-watch-images/readytodebug.png#lightbox)

При запуске симулятора, нажмите кнопку, чтобы увеличить метку.
Итак, вы получите в свое приложение Watch.

![](hello-watch-images/running.png "Приложение, работающее в симуляторе")


## <a name="related-links"></a>Связанные ссылки

- [Приступая к работе (пример)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Настройка и установка](~/ios/watchos/get-started/installation.md)
- [Первый видео приложение Watch](http://blog.xamarin.com/your-first-watch-kit-app/)
