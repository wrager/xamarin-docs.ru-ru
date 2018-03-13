---
title: "Объединение для магазина Mac App Store"
description: "В этом руководстве описывается создание пакета приложения Xamarin.Mac для публикации в Mac App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 77365d5ed62b2ef2e81407ab1fa5aef55c592d0b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="bundle-for-mac-app-store"></a>Создание пакета для Mac App Store

В этом разделе рассматриваются основы создания приложения для выпуска в Mac App Store с помощью Visual Studio для Mac. При наличии дополнительных функций (например, доступа к iCloud и push-уведомлений) могут потребоваться дальнейшее установки, которые выходят за рамки этой статьи.

> [!NOTE]
>  **Примечание**. Прежде чем выполнять действия в этом разделе, необходимо создать рабочий профиль подготовки, который будет использоваться для создания приложений для Mac App Store. Инструкции по созданию необходимых профилей подготовки см. ранее в этом документе.

## <a name="code-signing-options"></a>Параметры подписывания кода

Перед обновлением параметров подписывания кода и упаковки задайте параметру **Конфигурация** значение **Выпуск**. При подписывании приложения для выпуска в Магазин приложений необходимо использовать созданные ранее значения параметров **Удостоверение** и "Профиль подготовки".

 [![Изменение параметров подписывания кода](bundling-images/config02.png "Editing the code signing options")](bundling-images/config02-large.png#lightbox)

Убедитесь, что в разделе **Сборка Mac** выбран параметр создания пакета установщика:

[![Изменение параметров сборки](bundling-images/config03.png "Editing the build options")](bundling-images/config03-large.png#lightbox)

## <a name="build"></a>Построить

Перед выполнением сборки выберите конфигурацию **Выпуск**. При сборке приложения выводится предложение об использовании обоих сертификатов:

 ![Разрешение приложению использовать сертификат](bundling-images/image62.png "Allowing the app to use the certificate")

 ![Разрешение приложению использовать сертификат](bundling-images/image63.png "Allowing the app to use the certificate")

После сборки приложения щелкните проект правой кнопкой мыши и выберите команду **Открыть содержащую папку**, чтобы найти файл пакета (в приведенном ниже примере — в каталоге `bin/x86/AppStore`).  В этом файле содержится установщик для приложения, которое можно отправить в Apple для включения в Mac App Store.

 ![Выбор пакета сборки в Finder](bundling-images/image64.png "Selecting the build package in Finder")


## <a name="related-links"></a>Связанные ссылки

- [Установка](/visualstudio/mac/installation/)
- [Пример кода приложения "Привет, Mac"](~/mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)
