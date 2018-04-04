---
title: Работа с элементом управления страницы
description: В этой статье рассматриваются разработки и работы со страницы управления внутри приложения Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1cb07c050aeb2ee72e34048baab991df2d5775d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-page-control"></a>Работа с элементом управления страницы

_В этой статье рассматриваются разработки и работы со страницы управления внутри приложения Xamarin.tvOS._

Иногда может потребоваться отображение ряда страниц или изображений в приложении Xamarin.tvOS. Элемент управления страницы была разработана для четкого отображения страницы пользователь выходит максимальное число страниц. Элемент управления страницы отображает ряд точек для темной Овал были сформированы в фоновом режиме. Текущая страница отобразит Закрашенная точка, все остальные страницы Показать виде пустых точек. Страница управления изменяет внешнего большинство точек Если слишком много для его фона области.

[![](page-controls-images/page01.png "Пример элемента управления страницы")](page-controls-images/page01.png#lightbox)

Элемент управления страницы в неинтерактивной элемент позволяет предоставлять только пользователю. Необходимо добавить другие элементы управления, чтобы изменить номер текущей страницы (например, жестов или кнопки).

При использовании элемента управления страницы, Apple имеет следующие рекомендации:

- **Используется только в полной коллекции** -элементов управления на странице наилучшим образом среде весь экран для отображения нескольких страниц, которые существуют в одной коллекции.
- **Ограничить число страниц** -элементов управления на странице лучше подходят для десяти (10) или меньше страниц и не более двадцати (20) страниц. Для более двадцати страниц, рассмотрите возможность использования [представление коллекции](~/ios/tvos/user-interface/collection-views.md) и отображения страницы в виде сетки.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Элементы управления страницы и раскадровки

Для работы с элементами управления страницы в приложении Xamarin.tvOS проще всего добавить их с помощью iOS конструктор пользовательского интерфейса приложения.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

    
1. В **Pad решения**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **управления страницы** из **элементов** и поместите его в представлении: 

    [![](page-controls-images/page02.png "Элемент управления страницы")](page-controls-images/page02.png#lightbox)
1. В **вкладку графического** из **панель свойств**, можно настроить несколько свойств элемента управления страницы, такие как его **текущей страницы** и **# страниц**: 

    [![](page-controls-images/page03.png "Вкладка мини-приложения")](page-controls-images/page03.png#lightbox)
1. Добавьте элементы управления или жесты для перехода вперед или назад по коллекции страниц в представлении.
1. Наконец, назначьте **имена** к элементам управления, чтобы могли отвечать на них в коде C#. Пример: 

    [![](page-controls-images/page04.png "Имя элемента управления")](page-controls-images/page04.png#lightbox)
1. Сохраните изменения.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **управления страницы** из **элементов** и поместите его в представлении: 

    [![](page-controls-images/page02-vs.png "Элемент управления страницы")](page-controls-images/page02-vs.png#lightbox)
1. В **вкладку графического** из **свойства обозревателя**, можно настроить несколько свойств элемента управления страницы, такие как его **текущей страницы** и **#страниц**: 

    [![](page-controls-images/page03-vs.png "Вкладка мини-приложения")](page-controls-images/page03-vs.png#lightbox)
1. Добавьте элементы управления или жесты для перехода вперед или назад по коллекции страниц в представлении.
1. Наконец, назначьте **имена** к элементам управления, чтобы могли отвечать на них в коде C#. Пример: 

    [![](page-controls-images/page04-vs.png "Имя элемента управления")](page-controls-images/page04-vs.png#lightbox)
1. Сохраните изменения.
    

-----

> [!IMPORTANT]
> Хотя и существует возможность назначить события, например `TouchUpInside` элемент пользовательского интерфейса (например, UIButton) в конструкторе iOS, он не будет вызван так как для Apple TV нет сенсорном экране или поддерживает событий сенсорного экрана. Следует всегда использовать `Primary Action` событий при создании обработчиков событий для tvOS элементов пользовательского интерфейса.




Изменение контроллер представление (пример `ViewController.cs`) и добавьте код для обработки страниц изменяемого файла. Пример:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Давайте подробнее рассмотрим два свойства элемента управления страницы. Во-первых чтобы указать максимальное число страниц, используйте следующую команду:

```csharp
PageView.Pages = 6;
```

Чтобы изменить номер текущей страницы, используйте следующий код:

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` Свойства начинается с нуля (0), поэтому первая страница будет равно нулю, и последний будет иметь одно минус максимальное число страниц.

Дополнительные сведения о работе с помощью раскадровки см. в разделе нашей [Hello, tvOS краткое руководство по](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован разработки и работы со страницы управления внутри приложения Xamarin.tvOS.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
