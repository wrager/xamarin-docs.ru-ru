---
title: Специальные возможности на macOS
description: Это руководство описывает средства для построения Xamarin.Mac-приложении со специальными возможностями.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: ad04e0276c046f133a6f71abb38912343d2d86b6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility-on-macos"></a>Специальные возможности на macOS

На этой странице описаны способы использования macOS специальных возможностей API-интерфейсы для создания приложений в соответствии с [контрольный список специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md).
Ссылаться на [Android специальных возможностей](~/android/app-fundamentals/accessibility.md) и [специальных возможностей iOS](~/ios/app-fundamentals/accessibility.md) страницы для других API платформы.

Основные сведения о доступности API-интерфейсы работе в macOS (ранее называвшиеся OS X), первая проверка [модель специальных возможностей для OS X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Описание элементов пользовательского интерфейса

Использует AppKit `NSAccessibility` протокол предоставляют интерфейсы API, чтобы сделать доступной пользовательского интерфейса. Это включает в себя поведение по умолчанию, которое пытается установить значимые значения для свойств специальных возможностей, например установку кнопки `AccessibilityLabel`. Метка является обычно одно слово или фраза, описывающий элемент управления или представлении.

### <a name="storyboard-files"></a>Файлы раскадровки

Xamarin.Mac используется построитель интерфейс Xcode для редактирования файлов раскадровки.
Можно изменить сведения о специальных возможностях в **удостоверение инспектора** при выборе элемента управления в области конструктора (как показано на снимке экрана ниже):

[![Добавление специальных возможностей в построителе интерфейса в Xcode](accessibility-images/xcode.png "Добавление специальных возможностей в построителе интерфейса в Xcode")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Код

Xamarin.Mac не предоставляет в данный момент как `AccessibilityLabel` setter.  Добавьте следующий вспомогательный метод, чтобы задать метку специальных возможностей:

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Этот метод может затем использоваться в коде, как показано:

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` Свойство является описание элемента управления или представлении назначение, а должны быть добавлены только если метка может не предоставлять достаточно информации. Текст справки по-прежнему следует максимально сократить, для примере документа «удаляется».

Некоторые элементы пользовательского интерфейса не существенны для доступного доступа (например, метки рядом с входным, имеет свои собственные специальные меток и подсказок).
В этих случаях установить `AccessibilityElement = false` , чтобы эти элементы управления или представления будет пропускать чтения с экрана или другие специальные возможности.

Компания Apple предоставляет [специальных возможностей](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) объясняются оптимальные методы специальных возможностей меток и текста справки.

## <a name="custom-controls"></a>Пользовательские элементы управления

Ссылаться на Apple [рекомендации для доступных пользовательских элементов управления](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) сведения о дополнительные действия, необходимые.

## <a name="testing-accessibility"></a>Тестирование специальных возможностей

предоставляет macOS **специальных возможностей инспектора** , помогает теста специальные возможности. Инспектор входит в состав Xcode.

При первом запуске, **специальных возможностей инспектора** потребует разрешения для управления компьютером через специальных возможностей:

![Специальные возможности инспектора, запрашивающего разрешение на запуск](accessibility-images/accessibility-inspector-1.png "инспектора специальных возможностей, запрашивающего разрешение на выполнение")

Разблокировать экран параметров (при необходимости на нижний левый) и деления **специальных возможностей инспектора**:

![Параметры экрана, чтобы включить инспектор специальных](accessibility-images/accessibility-inspector-2.png "параметры экрана, чтобы включить инспектор специальных возможностей")

После включения инспектора отображается как плавающее окно, которое могут перемещаться по экрану. На следующем снимке экрана показано инспектор под управлением рядом с Mac пример приложения. Как курсор перемещается над окном, Инспектор отображает все свойства, доступного для каждого элемента управления:

[![Пример использования специальных возможностей инспектора](accessibility-images/accessibility-example.png "выполняется пример инспектора специальных возможностей")](accessibility-images/accessibility-example-large.png#lightbox)

Дополнительные сведения см. в статье [тестирование специальных возможностей для OS X руководства](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Связанные ссылки

- [Кросс платформенных специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md)
- [Доступность Mac](https://www.apple.com/accessibility/mac/)
