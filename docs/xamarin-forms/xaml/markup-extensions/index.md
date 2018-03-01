---
title: "Расширения разметки XAML"
description: "Необходимо расширить диапазон источников, из которых XAML атрибуты имеют значения"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 11889115b65608c750690c33052a9c86f7081e25
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-markup-extensions"></a>Расширения разметки XAML

Расширения разметки XAML помочь расширить возможности и гибкость XAML, позволяя атрибуты элемента для задания из источника, отличного от литеральные текстовые строки.

Например, обычно устанавливается `Color` свойства `BoxView` следующим образом:

```xaml
<BoxView Color="Blue" />
```

Или можно задать значение шестнадцатеричное значение цвета RGB:

```xaml
<BoxView Color="#FF0080" />
```

В любом случае текстовой строки, значение `Color` атрибут преобразуется в `Color` значение [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) класса.

Вместо этого можно присвоить `Color` атрибут из значения, хранимого в словаре ресурсов, или значение статического свойства класса, который вы создали или свойство с типом `Color` другого элемента на странице или сконструированный из Разделяйте значения цветового тона, насыщенности и яркости.

Все эти параметры являются возможно с помощью расширения разметки XAML. Пусть фразу «расширения разметки» должна вас пугать: расширения разметки XAML — это *не* расширения XML. Даже при расширения разметки XAML XAML всегда является допустимым XML. 

Расширение разметки — это просто другой способ express атрибут элемента. Расширения разметки XAML обычно опознаваемые параметр атрибута, заключенный в фигурные скобки:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Атрибут параметров в фигурных скобках — *всегда* расширение разметки XAML. Тем не менее как вы увидите расширения разметки XAML можно ссылаться без использования фигурных скобок.

Статья состоит из двух частей:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Использование расширения разметки XAML](consuming.md)  

Использование расширения разметки XAML, определенных в Xamarin.Forms.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Создание расширений разметки XAML](creating.md) 

Написать собственные пользовательские расширения разметки XAML.



## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Глава расширений разметки XAML из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
