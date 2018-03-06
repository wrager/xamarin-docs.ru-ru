---
title: "Отправка в Mac App Store"
description: "В этом руководстве описывается отправка пакета приложения Xamarin.Mac для публикации в Mac App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 533031016af68b1eb95a198e28ac40f445ff1579
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="upload-to-mac-app-store"></a>Отправка в Mac App Store

_В этом руководстве описано, как отправить приложение Xamarin.Mac для публикации в Mac App Store._

Приложения отправляются на утверждение команде Mac App Store через [iTunes Connect](http://itunesconnect.apple.com/).

1. Выберите **приложение macOS**, которое требуется создать: 

    [ ![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png)

2. Введите имя приложения и укажите другие сведения. Можно выбрать только значения существующего параметра **Идентификатор пакета**, который был создан ранее: 

    [ ![](uploading-images/image66.png "Выбор идентификатора пакета")](uploading-images/image66.png)

3. Выберите дату доступности и цену. Независимо от выбранной даты приложение станет доступно для продажи только после его утверждения. Если вам требуется больший контроль над фактической датой доступности, это значение можно задать далеко в будущем. 

    [![](uploading-images/image67.png "Задание даты доступности и цены")](uploading-images/image67.png)

4. Введите сведения о приложении, включая категорию Магазина приложений, в которую оно входит. 

    [ ![](uploading-images/image68.png "Ввод данных о приложении")](uploading-images/image68.png) 

    Выберите применимые оценки: 

    [ ![](uploading-images/image69.png "Установка оценок приложения")](uploading-images/image69.png) 

    Описание, ключевые слова и контактные URL-адреса: 

    [ ![](uploading-images/image70.png "Редактирование описания, ключевых слов и контактных URL-адресов")](uploading-images/image70.png) 

    Контактные данные и информация для рецензентов Магазина приложений: 

    [ ![](uploading-images/image71.png "Редактирование контактных данных и информации для рецензентов Магазина приложений")](uploading-images/image71.png) 

    И, наконец, снимки экранов: 

    [ ![] (uploading-images/image72.png "Добавление необходимых снимков экранов")](uploading-images/image72.png) 

    Снимки экрана должны иметь формат JPG, TIF или PNG и размер 1280 x 800, 1440 x 900, 2880 x 1800 или 2560 x 1600 пикселей. Нажмите клавишу **Сохранить** для завершения.

5. Сведения о приложении доступны для просмотра. Нажмите кнопку **Показать сведения**, чтобы изменить состояние: 

    [ ![](uploading-images/image73.png "Просмотр сведений о приложении")](uploading-images/image73.png)

6. В представлении сведений нажмите кнопку "Ready to Upload Binary" (Готово для отправки двоичного файла), чтобы отправить файл пакета приложения: 

    [ ![](uploading-images/image74.png "Кнопка "Ready to Upload Binary" (Готово для отправки двоичного файла)")](uploading-images/image74.png)

7. Ответьте на вопрос шифрования: 

    [ ![](uploading-images/image75.png "Ответ на вопрос шифрования")](uploading-images/image75.png)

8. Появится сообщение о том, когда сайт будет готов принять файл пакета приложения: 

    [ ![](uploading-images/image76.png "Уведомление о принятии")](uploading-images/image76.png)

9. Запустите загрузчик приложений и выполните вход с помощью идентификатора Apple ID.
Чтобы продолжить, нажмите кнопку **Deliver Your App** (Доставить приложение): 

    [ ![](uploading-images/image77.png "Интерфейс загрузчика приложений")](uploading-images/image77.png)

10. Выберите приложение в списке приложений со статусом **Ready to Upload Binary** (Готово для отправки двоичного файла) и нажмите кнопку **Далее**: 

    [ ![](uploading-images/image78.png "Выбор приложения для загрузки")](uploading-images/image78.png)

11. Проверьте метаданные приложения и нажмите кнопку **Выбрать**, чтобы найти файл пакета: 

    [ ![](uploading-images/image79.png "Проверка метаданных приложения")](uploading-images/image79.png)

12. Найдите файл пакета, который был создан в Visual Studio для Mac с помощью конфигурации сборки Магазина приложений: 

    [ ![](uploading-images/image80.png "Выбор файла для отправки")](uploading-images/image80.png)

13. Нажмите кнопку **Отправить**: 

    [ ![](uploading-images/image81.png "Отправка приложения")](uploading-images/image81.png)

14. Пакет будет проверен, после чего будут зафиксированы обнаруженные ошибки. Исправьте эти ошибки и повторно отправьте пакет. Отправленное приложение будет автоматически передано на проверку команде App Store: 

    [ ![](uploading-images/image82.png "Пример ошибки пи передаче")](uploading-images/image82.png)

После утверждения приложение будет доступно для скачивания или приобретения в Mac App Store.

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~//mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)