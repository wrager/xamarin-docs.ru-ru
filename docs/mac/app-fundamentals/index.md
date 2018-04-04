---
title: Принципы работы приложения
description: Документ содержит ссылки на руководства, описывающие различные основных понятий, необходимых для понимания при разработке Xamarin.Mac приложений.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 807cf0e16cafc00483cecfc578367bc110657672
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>Принципы работы приложения

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Общие методы и стили](~/mac/app-fundamentals/patterns.md)

На протяжении Apple интерфейсы API, предоставляемые через C# определенные стили и шаблоны возникающие снова и снова. Если у вас есть опыт работы с программирование с использованием Xamarin.iOS, они могут вам знакомы. Документации будет часто ссылаться на эти шаблоны и стили несколько раз, поэтому наличие хорошо понять их поможет вам наиболее понятном виде документации, которые можно найти.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[API понимания Mac](~/mac/app-fundamentals/mac-apis.md)

Для большой объем времени разработки с использованием Xamarin.Mac вы думаете, чтение и запись в C# не столь важно, базовые интерфейсы API Objective-C. Тем не менее иногда последующего необходимо прочитать документацию по API из Apple, преобразовать ответа от переполнения стека в решение для проблемы либо сравнения существующего образца.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Работа с файлами .xib](~/mac/app-fundamentals/xib.md)

В этой статье рассматривается работа с файлами .xib, созданные в построителе Xcode интерфейс для создания и поддержания пользовательские интерфейсы для приложений Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.xib меньше пользовательского интерфейса](~/mac/app-fundamentals/xibless-ui.md)

В этой статье представлены сведения по созданию пользовательского интерфейса приложения Xamarin.Mac непосредственно из кода C# без использования построителя интерфейса в Xcode с .storyboard или .xib файлов.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Работа с образами](~/mac/app-fundamentals/image.md)

В этой статье рассматривается работа с изображениями и значков в приложении Xamarin.Mac. Здесь рассматривается создание и обслуживание изображений, необходимые для создания значок приложения и использование изображений в код C# и в Xcode интерфейс построителя.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Привязка данных и кодирование ключ значение](~/mac/app-fundamentals/databinding.md)

В этой статье рассказывается, используя ключ значение кодирования и наблюдения для привязки данных к элементам пользовательского интерфейса в Xcode интерфейс построителя ключ значение. При использовании этого метода можно значительно снизить объем кода C#, который необходимо записать Xamarin.Mac приложения. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Работа с базами данных](~/mac/app-fundamentals/databases.md)

В этой статье рассказывается, используя ключ значение кодирования и наблюдения для привязки данных с прямой доступ к базам данных SQLite для элементов пользовательского интерфейса в Xcode интерфейс построителя ключ значение. Она также охватывает использование SQLite.NET ORM для предоставления доступа к данным SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Работа с копирования и вставки](~/mac/app-fundamentals/copy-paste.md)

В этой статье рассматривается работа с монтажного стола для предоставления копирования и вставки в приложении Xamarin.Mac. Показано, как работать со стандартными типами данных, могут совместно использоваться несколькими приложениями и как поддержать пользовательских данных в приложении выдать.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[«Песочница» для приложения Xamarin.Mac](~/mac/app-fundamentals/sandboxing.md)

В этой статье рассматриваются песочницы приложении Xamarin.Mac для выпуска в магазине приложений. Она охватывает все элементы, входящие в "песочницы": каталоги контейнера, прав, определенного пользователем разрешения, разделение прав доступа и принудительного применения ядра.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Воспроизведение звука с AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

В этой статье показано, как использовать вспомогательный класс для управления воспроизведение звука с помощью AVAudioPlayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Регистрации ошибок](~/mac/app-fundamentals/troubleshooting.md)

Иногда мы все возникли затруднения при работе с проектом на невозможность получения API, чтобы работать так, мы хотим или при попытке устранить ошибки. Наша цель в Xamarin предназначен для успешного создания мобильных и настольных приложений и предлагаются некоторые ресурсы, помогающие.
