---
title: Сводка Глава 6. Нажатие кнопки
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a45d1f1663b141912744e83763e7d618866159d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>Сводка Глава 6. Нажатие кнопки

[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) — Представление, которое пользователь может запустить команду. Объект `Button` идентифицируется текста (и при необходимости образ, как показано в [главе 13, точечные рисунки](chapter13.md)). Следовательно `Button` определяет многие теми же свойствами, что `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` также определяет три свойства, определяют внешний вид его границы, но эти свойства и их взаимной независимость поддерживается конкретной платформе:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) типа `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) типа `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) типа `Double`

`Button` также наследует все свойства `VisualElement` и `View`, в том числе `BackgroundColor`, `HorizontalOptions`, и `VerticalOptions`.

## <a name="processing-the-click"></a>Обработка нажмите кнопку

`Button` Класс определяет [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) событие, возникающее, когда пользователь касается `Button`. `Click` Обработчик имеет тип `EventHandler`. Первым аргументом является `Button` объект, вызывающий событие; второй аргумент — `EventArgs` объект, предоставляющий дополнительные сведения отсутствуют.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) образце демонстрируется простой `Clicked` обработки.

## <a name="sharing-button-clicks"></a>Нажимает кнопку для управления доступом

Несколько `Button` представления не могут иметь одинаковый `Clicked` обработчик, но обработчик обычно требуется определить, какие `Button` отвечает за определенное событие. Один способ — хранить различные `Button` объекты в виде полей и проверьте, какой из них он генерирует события в обработчик.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) этот метод продемонстрирован в примере. Программа также демонстрируется задание [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) свойство `Button` для `false` при нажатии клавиши `Button` больше не действительна. Объект отключен `Button` не создает `Clicked` событий.

## <a name="anonymous-event-handlers"></a>Обработчики событий анонимный

Существует возможность определить `Clicked` обработчиками в качестве анонимного лямбда-функции, как [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) демонстрирует образец. Тем не менее анонимные обработчиков не может передаваться без некоторый код упорядочить отражения.

## <a name="distinguishing-views-with-ids"></a>Отличительный признак представления с идентификаторами

Несколько `Button` объектов также различаются, задав [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) свойство или [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) свойства `string`. Это свойство определяется `Element` , но не используется в Xamarin.Forms. Он предназначен для использования исключительно с приложениями.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) образец использует один и тот же обработчик событий для всех 10 число ключей на цифровой клавиатуре и различать их с `StyleId` свойство:

[![Тройной экрана простой клавиатуре](images/ch06fg04-small.png "калькулятора")](images/ch06fg04-large.png#lightbox "калькулятора")

## <a name="saving-transient-data"></a>Сохранение временных данных.

Во многих приложениях требуется, чтобы сохранить данные при прерывании программы перезагрузить эти данные, когда программа запускается снова. [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Класс определяет несколько членов, которые позволить программе сохранять и восстанавливать временных данных:

- [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) Свойство — это словарь с `string` ключей и `object` элементы. Содержимое словаря, автоматически сохраняются в локальном хранилище приложения перед завершением работы программы и загружаются в память при запуске программы.
- `Application` Класс определяет три защищенных виртуальных методов, программа данного Стандартная `App` класса переопределения: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), и [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). Они ссылаются на *жизненного цикла приложения* события.
- [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) Метод сохраняет содержимое словаря.

Нет необходимости вызывать `SavePropertiesAsync`. Содержимое словаря, автоматически сохраняются перед завершением работы программы и получить до запуска программы. Это полезно при тестировании программы для сохранения данных в том случае, если программа завершает работу.

Также полезен:

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), статическое свойство, которое возвращает текущий `Application` объект, который затем можно использовать для получения `Properties` словаря.

Первым шагом является определение всех переменных на странице, должны сохраняться при завершении работы программы. Если вы знаете всех местах, где изменить эти переменные, можно просто добавьте их в `Properties` словаря на данный момент. В конструкторе страницы можно задать переменные из `Properties` словарь, если ключ существует.

Более крупной программы, возможно, потребуется обрабатывать события жизненного цикла приложения. Наиболее важным является `OnSleep` метод. Вызов этого метода указывает, что программа покинул переднего плана. Возможно, пользователь нажал **Главная** кнопку на устройстве, или отображаются все приложения или завершает работу по телефону. Вызов `OnSleep` только уведомление о том, что программа получит до его отключения. Программа должна использовать эту возможность убедитесь, что `Properties` словаря находится в актуальном состоянии.

Вызов `OnResume` указывает, что программа не завершился после последнего вызова `OnSleep` , но теперь работает на переднем плане еще раз. Программа может использовать эту возможность для обновления подключения к Интернету (например).

Вызов `OnStart` происходит во время запуска программы. Нет необходимости дождитесь вызывать этот метод для доступа к `Properties` словарь поскольку содержимое уже были восстановлены при `App` вызов конструктора.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) пример очень похож на **SimplestKeypad** за исключением того, что программа использует `OnSleep` переопределения для сохранения текущей записи клавиатуре и Конструктор страниц для восстановления данных.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 6 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Образцы Глава 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Образцы Глава 6 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
