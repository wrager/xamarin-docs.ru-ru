---
title: Вопросы и ответы
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: c0c8a6f4736bdcbb028425296f2e05dd500294d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>Вопросы и ответы


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Можно ли обновить шаблон по умолчанию Xamarin.Forms до более поздней версии пакета NuGet?](update-forms-template.md)
В этом руководстве в качестве примера используется шаблон Xamarin.Forms PCL, но один и тот же общий метод будет работать для Xamarin.Forms общий проект шаблона. 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Почему конструктор XAML в Visual Studio работает с XAML-файлами Xamarin.Forms?](forms-xaml-designer.md)
Xamarin.Forms не в настоящее время поддерживает визуальные конструкторы для XAML-файлов.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Ошибка сборки Android: непредвиденный сбой задачи LinkAssemblies](android-linkassemblies-error.md)
Может появиться сообщение об ошибке `The "LinkAssemblies" task failed unexpectedly` при построении проекта Xamarin.Android, использует формы. Это происходит, когда компоновщик активна (обычно при *выпуска* сборки, чтобы уменьшить размер пакета приложения); и происходит потому, что целевых объектах Android не последние framework. 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>[«Почему Мой проект Xamarin.Forms.Maps Android не с COMPILETODALVIK: НЕПРЕДВИДЕННАЯ ошибка верхнего уровня?»](maps-compiletodalvik-error.md)
Эта ошибка может появиться в панели ошибок Visual Studio для Mac или в окне вывода построения Visual Studio; в Android проектов, с помощью Xamarin.Forms.Maps. Чаще всего она будет устранена, увеличив размер кучи Java для проекта Xamarin.Android.

