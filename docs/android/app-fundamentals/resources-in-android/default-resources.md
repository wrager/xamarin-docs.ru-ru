---
title: "Ресурсы по умолчанию"
ms.topic: article
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/20/2017
ms.openlocfilehash: f632162fc0611f08c6a52dc36b12d7c80d5b456f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="default-resources"></a>Ресурсы по умолчанию

Ресурсы по умолчанию являются элементами, которые не являются специфичными для любого конкретного устройства или форм-факторов и, следовательно, по умолчанию с ОС Android Если не более конкретные сведения можно найти. Таким образом они являются наиболее распространенным типом ресурса. Они они организованы в подкаталоги из **ресурсов** каталог в соответствии с типом ресурса:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Файл ресурсов по умолчанию](default-resources-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![Файл ресурсов по умолчанию](default-resources-images/01-resource-files-xs.png)
 
-----

В приведенном выше рисунке проект имеет значения по умолчанию для drawable ресурсов, макеты и значений (XML-файлов, содержащих простые значения).

Далее приведен полный список типов ресурсов.

-  **animator** &ndash; XML-файлы, описывающие свойства анимации.
   Свойства анимации были введены в API уровня 11 (Android 3.0) и предоставляет для анимации свойства объекта. Анимация свойств являются более гибкими и мощными описать анимации на любой тип объекта.

-  **anim** &ndash; XML-файлы, которые описывают *анимации* анимации. Анимации представляют собой ряд инструкций анимации для осуществления преобразований содержимое представления объекта или примере поворот изображения или увеличение размера шрифта. Чтобы просматривать только те объекты ограничены анимации.

-  **цвет** &ndash; XML-файлы, описывающие состояние список цветов. Цвет состояния списки устроена графического элемента пользовательского интерфейса, таких как кнопка.
   Может быть имеют разные состояния таких как активный или отключена, и кнопки может изменять цвет при каждом изменении состояния. Список выражается в списке состояние.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-  **drawable** &ndash; Drawable ресурсы, общие понятия для графики, которая может быть скомпилирован в приложение и затем осуществляется из вызовов API или ссылаться на другие XML-ресурсы.
   Файлы растровых изображений (.png, .gif, .jpg) приводятся примеры drawables, специальные с раскрывающимися списками растровые изображения, известные как [девять исправлений](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), перечислены состояния универсального фигур, определенные в XML и т. д.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

-  **Шрифт** &ndash; начиная с API уровня 26, существует возможность встраивать шрифты в качестве ресурса в приложение. 26 библиотеки поддержки будут backport шрифтов для уровень API 14. Внедрение шрифтов позволяет приложениям 

-  **MIP-карт** &ndash; Drawable ресурсы, общие понятия для графики, которая может быть скомпилирован в приложение и затем осуществляется из вызовов API или ссылаться на другие XML-ресурсы.
   Файлы растровых изображений (.png, .gif, .jpg) приводятся примеры drawables, специальные с раскрывающимися списками растровые изображения, известные как [девять исправлений](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), перечислены состояния универсального фигур, определенные в XML и т. д.

-----

-  **макет** &ndash; XML-файлы, описывающие макет пользовательского интерфейса, например, действия или строку в список.

-  **меню** &ndash; XML-файлы, описывающие меню приложения, такие как *параметры меню*, *контекстные меню*, и *подменю*. Пример меню см. в разделе [Демонстрация всплывающее меню](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) или [стандартных элементов управления](https://developer.xamarin.com/samples/mobile/StandardControls/) образца.

-  **Необработанный** &ndash; произвольные файлы, сохраненные в необработанные двоичные форме. Эти файлы компилируются в приложения в двоичном формате.

-  **значения** &ndash; XML-файлы, содержащие простых значений. XML-файла в каталоге значения не определяет один ресурс, но вместо этого можно определить несколько ресурсов. Например один XML-файл может содержаться список строковых значений, пока другой XML-файл может содержать список значений цветов.

-  **XML** &ndash; XML-файлы, аналогичные функции, в файлах конфигурации .NET. Это произвольный XML, который может быть прочитан во время выполнения приложения