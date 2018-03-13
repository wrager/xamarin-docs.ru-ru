---
title: "Среда"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7489c2fe38e8433811c5f298296baebacf1c0727
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="environment"></a>Среда

*Среда выполнения* — это набор переменных среды, которые влияют на выполнение программ. Установить переменные среды можно временно в свойствах проекта или окончательно, указав дополнительные аргументы для средства упаковки mtouch.

## <a name="temporary-environment-variables"></a>Временные переменные среды

Временные переменные задаются в окне проекта **Свойства**/**Параметры** окна в разделе **Запуск > Общие**. Эти переменные среды действуют только при запуске приложения с помощью Visual Studio для Mac. Если приложение запускается вручную, переменные среды не будут заданы.

## <a name="permanent-environment-variables"></a>Постоянные переменные среды

Переменные среды с постоянной задаются при указании дополнительных аргументов для средства упаковки mtouch. Эти переменные среды скомпилированы в исполняемый файл. Они будут заданы, даже если приложение не запускается из Xamarin Studio.

## <a name="example"></a>Пример

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

