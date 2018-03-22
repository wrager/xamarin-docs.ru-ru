---
title: "MDocArchiveToMsxDocConverter.exe не найден на сервере. BaseCommand.OnRequest"
ms.topic: article
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1e49c270d5836379d60f50ec72960ddc83bfbba4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe не найден на сервере. BaseCommand.OnRequest

> [!IMPORTANT]
> Эта проблема устранена в последних версиях Xamarin. Тем не менее, если эта проблема возникает на последнюю версию программного обеспечения, проверьте файл [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с помощью полное управление версиями сведения и полный создать выходные данные журнала.


## <a name="error-message"></a>Сообщение об ошибке

Эта ошибка может возникать в *журнал сервера Mac* в Visual Studio:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Существует 2 отдельных проблем в этом сообщении.

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Эта ошибка не опасна, но он также является неправильным. Он [будут удалены](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) в будущем выпуске.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Эта ошибка является реальной проблемы. К сожалению, из-за для [ограничение](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) это трассировка стека исключений *неполные*. Если трассировка стека неполные следующим образом, в журнале сервера Mac, можно проверить `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` файл на узле сборки Mac, чтобы найти полный стек трассировки.
