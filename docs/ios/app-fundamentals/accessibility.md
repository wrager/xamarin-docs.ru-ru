---
title: "Специальные возможности на iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: 28701daca18852be93724ddfe444e1e620788619
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="accessibility-on-ios"></a>Специальные возможности на iOS

На этой странице описаны способы использования iOS специальных возможностей API-интерфейсы для создания приложений в соответствии с [контрольный список специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md).
Ссылаться на [Android специальных возможностей](~/android/app-fundamentals/accessibility.md) и [специальных возможностей OS X](~/mac/app-fundamentals/accessibility.md) страницы для других API платформы.

## <a name="describing-ui-elements"></a>Описание элементов пользовательского интерфейса

предоставляет iOS `AccessibilityLabel` и `AccessibilityHint` свойства разработчикам добавлять описательный текст, который может использоваться VoiceOver экране чтения облегчающих элементов управления. Элементы управления можно также пометить один или несколько признаки, которые предоставляют дополнительный контекст в доступных режимов.

Некоторые элементы управления могут не должны быть доступны (для примера, метка на ввод текста или изображение, которое является исключительно декоративным) – `IsAccessibilityElement` предоставляется отключение специальных возможностей в таких случаях.

**Конструктор пользовательского интерфейса**

**Панель свойств** содержит раздел специальных возможностей, который позволяет изменять при выборе элемента управления в iOS пользовательского интерфейса конструктора эти параметры:

![](accessibility-images/ios-designer-sml.png "Параметры специальных возможностей")

**C#**

Эти свойства могут быть также заданы непосредственно в коде:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Что такое AccessibilityIdentifier?

`AccessibilityIdentifier` Используется для задания уникальный ключ, который может использоваться для ссылки на элементы пользовательского интерфейса через API-Интерфейс UIAutomation.

Значение `AccessibilityIdentifier` никогда не произносятся и не отображается для пользователя.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` Метод позволяет события для пользователей за пределами прямого взаимодействия (например, когда они взаимодействуют с конкретного элемента управления).

### <a name="announcement"></a>Замечание

Объявления могут отправляться из кода, чтобы информировать пользователей об изменении какое-либо состояние (например, выполняется фоновая операция завершена). Это может сопровождаться визуальное в пользовательском интерфейсе:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged представления

`LayoutChanged` Объявления используется при макета экрана:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Специальные возможности и локализация

Свойства специальных возможностей, такие как метки и подсказки можно локализовать просто как другой текст в пользовательском интерфейсе.

**MainStoryboard.strings**

Если пользовательский интерфейс располагается в раскадровку, можно предоставить перевод для свойства специальных возможностей в так же, как другие свойства. В следующем примере `UITextField` имеет **ИД локализации** из `Pqa-aa-ury` и два свойства специальных возможностей, устанавливаемое на испанском языке:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Этот файл будет помещен в **es.lproj** каталог для испанского языка содержимого.

**Localizable.strings**

Кроме того, можно добавить переводы **Localizable.strings** файла локализованного каталога содержимого (например) **ES.lproj** для испанского языка):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Эти преобразования могут использоваться в C# через `LocalizedString` метод:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Ссылаться на [руководство по локализации для операций ввода-вывода](~/ios/app-fundamentals/localization/index.md) Дополнительные сведения о локализации содержимого.

<a name="testing" />

## <a name="testing-accessibility"></a>Тестирование специальных возможностей

VoiceOver включен в **параметры** приложения, перейдя к **Общие > специальных > VoiceOver**:

![](accessibility-images/settings-sml.png "Параметр скорости озвучивания")

**Специальных возможностей** экрана также включает параметры для масштабирования, размер текста, цвет & контрастность параметры, параметры речи и других параметров конфигурации.

Выполните следующие [VoiceOver инструкции](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) для проверки специальных возможностей на устройствах iOS.


## <a name="simulator-testing"></a>Тестирование симулятора

При тестировании в симуляторе **специальных возможностей инспектора** доступен для проверки доступности свойства и события, настроены правильно. Включите инспектора в **параметры** приложения, перейдя к **Общие > специальных > инспектора специальных возможностей**:

![](accessibility-images/settings-inspector-sml.png "Включите инспектор специальных возможностей")

После включения окна инспектора наведет экрана операций ввода-вывода в любое время.
Ниже приведен пример выходных данных при выборе представления строки таблицы — Обратите внимание, **метка** содержит предложение, которое предоставляет содержимое строки, а также его «выполняется» (т. е. деления отображается):

![](accessibility-images/tableview-a11y-sml.png "С помощью инспектора специальных возможностей")

Пока инспектор видна, используйте значок «X» в верхней левой временно Показать и скрыть наложение и включить или отключить параметры специальных возможностей.



## <a name="related-links"></a>Связанные ссылки

- [Кросс платформенных специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS (Apple) специальные возможности](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
