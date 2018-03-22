---
title: "Работа с контроллерами навигации"
description: "В этой статье описывается проектирование и работа с панели переходов внутри приложения Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 0f59e8f5e732a45f7e6148a08de80fffc56dbb26
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-navigation-controllers"></a>Работа с контроллерами навигации

_В этой статье описывается проектирование и работа с панели переходов внутри приложения Xamarin.tvOS._

Можно добавить панели навигации в верхней части представления для отображения заголовка и необязательный кнопок панели навигации. Обычно они используются, когда пользователь переходит из главной странице в табличном представлении, коллекции или меню, чтобы вложенное представление, подробными сведениями выбранного элемента.

[![](navigation-bars-images/navbar01.png "Пример панели навигации")](navigation-bars-images/navbar01.png#lightbox)

В дополнение к заголовку (которое отображается в центре) панелей навигации может содержать один или несколько кнопок панели навигации (`UIBarButtonItem`) в левой и правой стороны панели.

> [!IMPORTANT]
> Панели навигации полностью прозрачны по умолчанию. Следует соблюдать осторожность, чтобы содержимое панели навигации оставались для чтения над содержимым под ним. Например, при в табличном представлении или коллекции прокрутка содержимого под ним.




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Панели навигации и раскадровки

Для работы с панели навигации в приложении Xamarin.tvOS проще всего добавить их с помощью iOS конструктор пользовательского интерфейса приложения.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)


1. В **Pad решения**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **панель навигации** из **элементов** и поместите его в представлении в верхней части экрана: 

    [![](navigation-bars-images/navbar02.png "Панель навигации")](navigation-bars-images/navbar02.png#lightbox)
1. Дважды щелкните **панель навигации** для **элемент навигации**. В **мини-приложение** вкладке **панель свойств**, можно задать **заголовок**: 

    [![](navigation-bars-images/navbar03.png "Задание заголовка")](navigation-bars-images/navbar03.png#lightbox)
1. Затем можно добавить один или несколько **элементы кнопки панели** либо конца строке: 

    [![](navigation-bars-images/navbar04.png "На линейчатой элемент кнопки")](navigation-bars-images/navbar04.png#lightbox)
1. Наконец, доступ к сети **элементы кнопки панели** на действия в **событий** вкладке **свойства обозревателя**: 

    [![](navigation-bars-images/navbar05.png "На линейчатой действие элемента кнопки")](navigation-bars-images/navbar05.png#lightbox)
1. Сохраните изменения.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **панель навигации** из **элементов** и поместите его в представлении в верхней части экрана: 

    [![](navigation-bars-images/navbar02-vs.png "Панель навигации")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Дважды щелкните **панель навигации** для **элемент навигации**. В **мини-приложение** вкладке **свойства обозревателя**, можно задать **заголовок**: 

    [![](navigation-bars-images/navbar03-vs.png "Задание заголовка")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Затем можно добавить один или несколько **элементы кнопки панели** либо конца строке: 

    [![](navigation-bars-images/navbar04-vs.png "На линейчатой элементов кнопки")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Наконец, доступ к сети **элементы кнопки панели** на действия в **событий** вкладке **свойства обозревателя**: 

    [![](navigation-bars-images/navbar05-vs.png "На линейчатой действия кнопок элемента")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Сохраните изменения.


-----

> [!IMPORTANT]
> Хотя и существует возможность назначить события, например `TouchUpInside` элемент пользовательского интерфейса (например, UIButton) в конструкторе iOS, он не будет вызван так как для Apple TV нет сенсорном экране или поддерживает событий сенсорного экрана. Следует всегда использовать `Primary Action` событий при создании обработчиков событий для tvOS элементов пользовательского интерфейса.




Ниже приведен пример обработчиков событий на трех разных BarButtonItems: `ShowFirstHotel`, `ShowSecondHotel`, и `ShowThirdHotel`. При выборе каждого элемента фоновое изображение `HotelImage` изменяется. Это редактировании в View-Controller (пример `ViewController.cs`) файл:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Пока кнопки `Enabled` свойство `true` и не соответствует другим элементом управления или представление, его можно сделать элемент в фокусе, с помощью удаленного Siri.

Дополнительные сведения о работе с помощью раскадровки см. в разделе нашей [Hello, tvOS краткое руководство по](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован проектирование и работа с панели переходов внутри приложения Xamarin.tvOS.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
