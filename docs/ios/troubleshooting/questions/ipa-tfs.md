---
title: "Как скопировать выходные файлы IPA транзитный каталог TFS?"
ms.topic: article
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c89d81434cac43505c4f0341a10aaf4fc99407fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Как скопировать выходные файлы IPA транзитный каталог TFS?

Откройте `.csproj` в файле проекта приложения iOS в текстовом редакторе, а затем добавьте следующие строки в конец (непосредственно перед закрывающим тегом `</Project>` тега):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Примечания

-   Это же общего метода, описанного в [можно изменить путь выходного файла IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Два важных аспекта, чтобы задать `$(TF_BUILD_BINARIESDIRECTORY)` качестве конечной папки и добавлять дополнительное условие так `CopyIpa` будет выполняться только для сборок TFS.

-   Описание `TF_BUILD_BINARIESDIRECTORY` разделе [https://msdn.microsoft.com/en-us/library/hh850448.aspx](https://msdn.microsoft.com/en-us/library/hh850448.aspx).

## <a name="additional-references"></a>Дополнительные ссылки

- [Документация по установке TFS для использования с Xamarin](https://docs.microsoft.com/vsts/tfvc/overview)
- [Задача сборки TFS: Xamarin.Android](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-android)
- [Задача сборки TFS: Xamarin.iOS](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Следующие шаги
В этом документе рассматриваются текущее поведение начиная с Xamarin 3.11.666 для Visual Studio и создавайте Xamarin.iOS 8.10.3 на MAC-адрес узла. Для получения дополнительной помощи, свяжитесь с нами, или если эта проблема остается даже после использования указанные выше сведения см. в разделе [какие варианты поддержки доступны для Xamarin?](~/cross-platform/troubleshooting/support-options.md) сведения о параметрах контактов, предложения, а также как файл новую ошибку, при необходимости. 



