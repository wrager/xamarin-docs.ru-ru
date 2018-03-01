---
title: "Работа с размерами экранов"
ms.topic: article
ms.prod: xamarin
ms.assetid: 156D6D1C-83CA-4088-BA08-40B22312269C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c64e5821c1067b64caf3afc85c0e290a354e914d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-screen-sizes"></a>Работа с размерами экранов

Apple Watch доступен в двух размеров:

- **38 мм**
  - логических пикселях 136 x 170 (272 x 340 физических пикселей)

- **42 мм**
  - 156 x 195 логических пикселях (312 x 390 физических пикселях).

Вам следует учитывать размер экрана при разработке и тестирование приложений.

## <a name="watchos-interface-designer"></a>watchOS конструктора интерфейса

По умолчанию Visual Studio для Mac конструктора появится смотреть интерфейс контроллеров в **Any Apple Watch**.

![](screen-sizes-images/screen-any-sml.png "Контрольное значение интерфейса конструктора отображает контроллеров в любой Apple Watch")

Используйте меню «Формат» для предварительного просмотра раскадровки ни в одном из доступных размеров: **38 мм** или **42 мм**:

![](screen-sizes-images/screen-menu-sml.png "При выборе размера 38 мм или 42 мм")

Размер экрана иногда будет отображаться содержимое, которое может быть усечен или скрытого на экране меньшего размера.
Убедитесь, что тестирование на обоих размеров.


### <a name="interface-design"></a>Разработка интерфейса

Приложения должны отображения на экране, независимо от размера, то же содержимое и следует развернуть или свернуть элементов соответствующим образом. В Visual Studio для Mac конструктора, в инспекторе атрибута следует использовать **относительно контейнера** или **размер в соответствии с содержимым** предпочтительнее, чем фиксированный размер.

![](screen-sizes-images/sizeattributepanel-sml.png "Использование относительно контейнера или размер в соответствии с содержимым предпочтительнее, чем фиксированных размеров")

Поскольку внешняя панель черного цвета, заключается в окне контрольных значений, предоставляя отступ вокруг интерфейс не рекомендуется. Разрешить элементы rest по краю экрана и позволить панели, которые образуют естественным границы вокруг приложения.


## <a name="watchos-simulator"></a>watchOS симулятора

При тестировании в имитаторе можно легко переключаться между размеры два экрана, с помощью **оборудования > устройства** меню.

![](screen-sizes-images/simulator.png "Имитатор можно переключаться между размеры два экрана, с помощью меню «устройства»")


## <a name="image-resources"></a>Графические ресурсы

Несколько графических ресурсов следует использовать, если одного актива не хорошо выглядят на разных размеров. Образ ОС каталоги разрешить для точечных должен быть задан для каждого размера.

![](screen-sizes-images/images-xcassets.png "Редактор каталога образа ОС")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Кроме того можно используйте код для определения размера экрана и полностью загрузки различные изображения:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Дополнительные сведения о с помощью [управления](~/ios/watchos/user-interface/image.md).



## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
