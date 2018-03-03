---
title: "Значки настраиваемый документ"
description: "Этом статье описаны включая и управлении ресурса изображения в приложении Xamarin.iOS для использования в качестве значок пользовательский тип документа."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: 582fcbacbf1959e05773babb1219817ba319a937
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="custom-document-icons"></a>Значки настраиваемый документ

_Этом статье описаны включая и управлении ресурса изображения в приложении Xamarin.iOS для использования в качестве значок пользовательский тип документа._

Если приложения Xamarin.iOS поддерживает загрузку определенного типа документов, разработчик может предоставить значков, которые система будет использовать при обнаружении типа документа, например если пользователь удерживает нажатую вложения в *почтовое приложение* как показан ниже:

 [ ![](custom-document-types-images/17.png "Примером значок типа документа")](custom-document-types-images/17.png)

Разработчик может добавить сведения о типе документа для формате файла приложения является возможность открытия, включая словарей для `CFBundleTypeName` строки и `LSItemContentTypes` массива в приложении `Info.plist`. Значки для типа документа перейдите `CFBundleTypeIconFiles` массива. Если значок документов не указан, iOS, производной от значка приложения.
Значки можно задать несколько размеров, оптимизированный для различных разрешений экрана устройства. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Чтобы назначить эти значения в Visual Studio для Mac, используйте **типов документов** раздела **Дополнительно** вкладки `Info.plist` редактора для добавления типа документа и назначьте ему изображения значков. Например ниже приведен снимок регистрации для поддержки PDF:

 [ ![](custom-document-types-images/18.png "В разделе типы документов на вкладке Advanced в редакторе «Info.plist»")](custom-document-types-images/18.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Чтобы назначить эти значения в Visual Studio, используйте **типов документов** раздела **Дополнительно** вкладке на `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Откройте раздел типов документов, вкладка "Дополнительно"")

Нажмите кнопку **добавить тип документа** кнопку и заполните обязательные поля:

![](custom-document-types-images/doc02w.png "Форма добавления типа документа")

-----


Дополнительные сведения о типах документов см. в разделе Apple [идентификаторы ссылка на универсальный тип](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) и [разделы документа взаимодействия программирования для iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Связанные ссылки

- [Работа с изображениями (пример)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Привет, iPhone](~/ios/get-started/hello-ios/index.md)
- [Пользовательский значок и правила создания образа](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
