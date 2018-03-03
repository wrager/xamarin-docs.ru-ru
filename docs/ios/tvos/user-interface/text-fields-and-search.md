---
title: "Работа с текстом и поля поиска"
description: "В этой статье рассматриваются разработки и работы с текстом и поля внутри приложения Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: cb6917f9cd0dc22cc32a2d32c203328f1d6d963b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-text-and-search-fields"></a>Работа с текстом и поля поиска

_В этой статье рассматриваются разработки и работы с текстом и поля внутри приложения Xamarin.tvOS._



При необходимости, Xamarin.tvOS приложения могут запрашивать небольшие части текста от пользователя (например, идентификаторы пользователей и пароли) с помощью текстового поля и экранной клавиатуры:

[ ![](text-fields-and-search-images/intro01.png "Пример поиска")](text-fields-and-search-images/intro01.png)

При необходимости можно предоставить возможность поиска ключевое слово содержимое приложения, с помощью поля поиска:

[ ![](text-fields-and-search-images/intro02.png "Пример результатов поиска")](text-fields-and-search-images/intro02.png)

В этом документе рассматриваются сведения о работе с текстом и полей в приложении Xamarin.tvOS.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>О текста и полей

Как упоминалось выше, при необходимости, ваш Xamarin.tvOS может представлять один или несколько текстовых полей для сбора небольших объемов текста с помощью пользователя на экране (или необязательным bluetooth клавиатура в зависимости от версии пользователь установил tvOS). 

Кроме того приложения пользователю больших объемов содержимого (например, музыки, фильмов или коллекции изображений), можно включить поле поиска, который позволяет пользователю ввести небольшой объем текста для фильтрации списка доступных элементов.

<a name="Text-Fields" />

## <a name="text-fields"></a>Текстовые поля

В tvOS, текстовое поле будет представлено в виде появится поле фиксированной высоты, округленное угол запись экранную клавиатуру, когда пользователь щелкает:

[ ![](text-fields-and-search-images/text01.png "Текстового поля в tvOS")](text-fields-and-search-images/text01.png)

Когда пользователь перемещает [фокус](~/ios/tvos/app-fundamentals/navigation-focus.md) данного текстовому полю, его дальнейшее расширение и отобразить глубокой тень. Необходимо учитывать это при разработке пользовательского интерфейса, как текстовые поля могут пересекаться другие элементы пользовательского интерфейса, когда фокус.

Apple имеет следующие рекомендации по работе с текстовых полей.

- **Текст записи только в случае необходимости используйте** — из-за особенностей экранную клавиатуру, введя много разделов текста или заполнении несколько текстовых полей трудоемкой для пользователя. Лучше ограничить объем ввода текста с помощью списков выбора или [кнопки](~/ios/tvos/user-interface/buttons.md).
- **Использование подсказок для взаимодействия цели** -текстовое поле может отображать заполнитель «подсказки», когда становятся пустыми. Там, где это применимо, указания можно используйте для описания цели текстовое поле, а не отдельные метки.
- **Выберите соответствующий тип по умолчанию сочетания** -tvOS предоставляет несколько различных, типов клавиатуры цели построения, которые можно указать для текстового поля. Например сочетание адреса электронной почты может облегчить запись, позволяя пользователю выбрать из списка недавно введенные адреса.
- **При необходимости используйте Secure поля** -Secure текстовое поле представляет символов, при вводе точки (а не реальных буквы). Всегда используйте текстовое поле Secure при сборе конфиденциальные сведения, например пароли.

<a name="Keyboards" />

## <a name="keyboards"></a>Клавиатура

Всякий раз, когда пользователь щелкает текстовое поле в пользовательском интерфейсе, линейный на экране отображается клавиатуры. Пользователь использует область Touch [Siri удаленного](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) для выбора отдельных букв с клавиатуры и введите требуемые сведения:

[ ![](text-fields-and-search-images/keyboard01.png "Удаленное Siri клавиатуры")](text-fields-and-search-images/keyboard01.png)

Если имеется более одного текстового поля в текущем представлении **Далее** автоматически будет отображаться кнопка перенаправить пользователя в поле следующий текст. Объект **сделать** для последнего текстового поля, будет завершить ввод текста и вернуться к предыдущему экрану пользователя будет отображаться кнопка. 

В любое время, пользователь может также нажать **меню** кнопки на удаленном Siri до завершения ввода текста и снова вернуться к предыдущему экрану.

Apple имеет следующие рекомендации по работе с экранной клавиатуры.

- **Выберите соответствующий тип по умолчанию сочетания** -tvOS предоставляет несколько различных, типов клавиатуры цели построения, которые можно указать для текстового поля. Например сочетание адреса электронной почты может облегчить запись, позволяя пользователю выбрать из списка недавно введенные адреса.
- **При необходимости использовать представления аксессуар клавиатуры** — в дополнение к стандартной сведения, которые всегда отображается, необязательно представления аксессуар (например, изображения или метки), которые могут добавляться в Экранная клавиатура объяснить ввод текста или значение помочь пользователю ввести необходимые сведения.

Дополнительные сведения о работе с экранной клавиатуры, см. в разделе Apple [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [управление с клавиатуры](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [настраиваемые представления для входных данных](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) и [ Текст руководство по программированию для операций ввода-вывода](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) документации.

<a name="Search" />

## <a name="search"></a>Поиск

Поле поиска представления специализированные экрана, предоставляя текстовое поле и экранную клавиатуру, позволяет пользователю фильтровать коллекцию элементов, расположенных под клавиатуры:

[ ![](text-fields-and-search-images/search01.png "Пример результатов поиска")](text-fields-and-search-images/search01.png)

При нажатии пользователем буквы в поле поиска, ниже результаты будут автоматически отражают результаты поиска. В любое время пользователь может переместить фокус на результатах и выберите один из элементов, представленных.

Apple имеет следующие рекомендации по работе с поля поиска.

- **Укажите последних поисков** — так как ввод текста с помощью удаленного Siri может оказаться трудоемкой и пользователей, как правило, повторите запросы поиска, рассмотрите возможность добавления раздела последних результатов поиска перед текущие результаты в области клавиатуры.
- **Если это возможно, ограничить число результатов** — так как большой список элементов может быть трудно пользователю синтаксический анализ и перехода, рекомендуется ограничить количество возвращаемых результатов.
- **При необходимости укажите результат поиска фильтров** — Если содержимое приложения, предоставляемые решается, рассмотрите возможность добавления области панели, чтобы разрешить пользователю для дальнейшей фильтрации возвращаемых результатов поиска.

Дополнительные сведения см. в разделе Apple [ссылку на класс UISearchController](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Работа с полями текст

Для работы с текстовых полей в приложении Xamarin.tvOS проще всего добавить их в пользовательский интерфейс конструктора, с помощью iOS конструктора.

Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **Pad решения**, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования.
1. Перетащите один или несколько **текстовые поля** int рабочей области конструктора в представлении: 

    [ ![](text-fields-and-search-images/text02.png "Текстовое поле")](text-fields-and-search-images/text02.png)
1. Выберите **текстовые поля** и дать каждому уникальный **имя** в **мини-приложение** вкладке **Pad свойства**: 

    [ ![](text-fields-and-search-images/text03.png "На вкладке панели свойств мини-приложения")](text-fields-and-search-images/text03.png)
1. В **текстовое поле** можно определить элементы, такие как **заполнитель** по умолчанию и указание **значение**: 

    [ ![](text-fields-and-search-images/text04.png "В разделе текстовое поле")](text-fields-and-search-images/text04.png)
1. Прокрутите вниз, чтобы определить свойства, такие как **проверки правописания**, **капитализации** и значение по умолчанию **тип клавиатуры**: 

    [ ![](text-fields-and-search-images/text05.png "Проверка орфографии, регистр символов и значение по умолчанию тип клавиатуры")](text-fields-and-search-images/text05.png) 
1. Сохраните изменения раскадровки.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. В **обозревателе решений** дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования.
1. Перетащите один или несколько **текстовые поля** int рабочей области конструктора в представлении: 

    [ ![](text-fields-and-search-images/text02-vs.png "Текстовое поле")](text-fields-and-search-images/text02-vs.png)
1. Выберите **текстовые поля** и дать каждому уникальный **имя** в **мини-приложение** вкладке **свойства обозревателя**: 

    [ ![](text-fields-and-search-images/text03-vs.png "Вкладка мини-приложения")](text-fields-and-search-images/text03-vs.png)
1. В **текстовое поле** можно определить элементы, такие как **заполнитель** по умолчанию и указание **значение**: 

    [ ![](text-fields-and-search-images/text04-vs.png "В разделе текстовое поле")](text-fields-and-search-images/text04-vs.png)
1. Прокрутите вниз, чтобы определить свойства, такие как **проверки правописания**, **капитализации** и значение по умолчанию **тип клавиатуры**: 

    [ ![](text-fields-and-search-images/text05-vs.png "Проверка орфографии, регистр символов и значение по умолчанию тип клавиатуры")](text-fields-and-search-images/text05-vs.png) 
1. Сохраните изменения раскадровки.
    
-----

В коде, можно получить или задать значение в текстовое поле с помощью его `Text` свойство:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

При необходимости можно использовать `Started` и `Ended` текстовое поле события реагировать на ввод текста начала и окончания.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Работа с полями поиска

Для работы с полями поиска в приложении Xamarin.tvOS проще всего добавить их в пользовательский интерфейс конструктора, с помощью интерфейса конструктора.

Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
    
1. В **Pad решения**, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования.
1. Перетащите новый контроллер представление коллекции раскадровки для представления результатов поиска пользователя: 

    [ ![](text-fields-and-search-images/search02.png "Контроллер представление коллекции")](text-fields-and-search-images/search02.png)
1. В **мини-приложение** вкладке **панель свойств**, используйте `SearchResultsViewController` для **класса** и `SearchResults` для **идентификатор раскадровки**: 

    [ ![](text-fields-and-search-images/search03.png "Вкладка мини-приложения")](text-fields-and-search-images/search03.png)
1. Выберите **прототип ячейки** в рабочей области конструирования.
1. В **мини-приложение** вкладке **свойства обозревателя**, используйте `SearchResultCell` для **класса** и `ImageCell` для **идентификатор**: 

    [ ![](text-fields-and-search-images/search04.png "Вкладка мини-приложения")](text-fields-and-search-images/search04.png)
1. Макет конструктора из **прототип ячейки** и предоставления каждого элемента с уникальным **имя** в **мини-приложение** вкладке **свойства обозревателя**: 

    [ ![](text-fields-and-search-images/search05.png "Макет конструктора прототипа ячейки")](text-fields-and-search-images/search05.png)
1. Сохраните изменения раскадровки.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. В **обозревателе решений** дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования.
1. Перетащите новый контроллер представление коллекции раскадровки для представления результатов поиска пользователя: 

    [ ![](text-fields-and-search-images/seach02-vs.png "Контроллер представление коллекции")](text-fields-and-search-images/seach02-vs.png)
1. В **мини-приложение** вкладке **свойства обозревателя**, используйте `SearchResultsViewController` для **класса** и `SearchResults` для **идентификатор раскадровки**: 

    [ ![](text-fields-and-search-images/search03-vs.png "Вкладка мини-приложения")](text-fields-and-search-images/search03-vs.png)
1. Выберите **прототип ячейки** в рабочей области конструирования.
1. В **мини-приложение** вкладке **свойства обозревателя**, используйте `SearchResultCell` для **класса** и `ImageCell` для **идентификатор**: 

    [ ![](text-fields-and-search-images/search04-vs.png "Вкладка мини-приложения")](text-fields-and-search-images/search04-vs.png)
1. Макет конструктора из **прототип ячейки** и предоставления каждого элемента с уникальным **имя** в **мини-приложение** вкладке **свойства обозревателя**: 

    [ ![](text-fields-and-search-images/search05-vs.png "Макет конструктора прототипа ячейки")](text-fields-and-search-images/search05-vs.png)
1. Сохраните изменения раскадровки.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Модель данных

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Далее необходимо предоставить класс в качестве модели данных для результатов, который пользователь будет выполняться поиск. В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **добавить** > **новый файл...**   >  **Общие** > **пустым классом** и предоставить **имя**: 

[ ![](text-fields-and-search-images/search06.png "Выберите пустой класс и укажите имя")](text-fields-and-search-images/search06.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Далее необходимо предоставить класс в качестве модели данных для результатов, который пользователь будет выполняться поиск. В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **добавить** > **новый элемент...**   >  **Apple** > **Прочее** > **класса** и предоставить **имя**: 

[ ![](text-fields-and-search-images/search06-vs.png "Выберите класс и укажите имя")](text-fields-and-search-images/search06-vs.png)

-----

Например приложение, которое дает пользователю возможность поиска в коллекции изображений, заголовков и ключевое слово может выглядеть следующим образом:

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>Ячейку представления коллекции

С моделью данных в месте, изменить **ячейки прототип** (`SearchResultViewCell.cs`) и сделать его внешний вид находятся следующие:

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

`UpdateUI` Метод будет использоваться для отображения отдельных полей из **PictureInformation** элементы ( `PictureInfo` свойство) в именованные элементы пользовательского интерфейса при каждом обновлении свойства. Например изображение и заголовок, связанный с изображением.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Контроллер представление коллекции

Далее следует изменить контроллер представление коллекции результаты поиска (`SearchResultsViewController.cs`) и сделать его выглядеть следующим образом:

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

Во-первых, `IUISearchResultsUpdating` интерфейс добавляется класс для обработки обновляются пользователем фильтра поиска контроллера:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Константы также определены для указания Идентификаторов **ячейки прототип** (, совпадает с Идентификатором, определенных в интерфейсе конструктора выше), будет использоваться позже при контроллера коллекции запрашивает новую ячейку:

```csharp
public const string CellID = "ImageCell";
```

Полный список элементов, выполняется поиск условие фильтра поиска создается хранилище и список элементов, удовлетворяющих этот термин:

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

Когда `SearchFilter` будет изменен, обновляется список соответствующих элементов и перезагрузки содержимое представления коллекции. `FindPictures` Процедура отвечает за поиск элементов, соответствующих нового условия поиска:

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

Значение `SearchFilter` будет обновляться (которого будет обновлять результаты представление коллекции) при изменении фильтра в контроллере поиска:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures` Метод изначально заполняет коллекцию доступных элементов:

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

Для данного примера все образцы данных создается в памяти при загрузке контроллера представления коллекции. В реальном приложении возможно, эти данные будут считаны из базы данных или веб-службы и только при необходимости, чтобы предотвратить переполнение большом Apple недостаточно памяти.

`NumberOfSections` И `GetItemsCount` методы предоставляют число сопоставленных элементов:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

`GetCell` Метод возвращает новый **ячейки прототип** (на основе `CellID` определенный выше в раскадровке) для каждого элемента в представлении коллекции:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell` Метод вызывается перед ячейку, будет отображаться, поэтому его можно настроить:

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

`DidUpdateFocus` Метод предоставляет визуальную обратную связь для пользователя, как они выделять элементы в представлении коллекции результатов:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Наконец `ItemSelected` метод обрабатывает пользователя, при выборе элемента (щелкнув область Touch удаленное Siri) в представлении коллекции результатов:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Если поле поиска было отображается как модальное диалоговое окно представления (поверх вызов его представления), используйте `DismissViewController` метод, чтобы закрыть представление поиска, когда пользователь выбирает элемент. В этом примере поле поиска будет представлено в виде содержимого вкладки представление в виде вкладок, поэтому оно не было отклонено здесь.

Дополнительные сведения о представлениях коллекций см. в разделе нашей [работа с представлениями коллекции](~/ios/tvos/user-interface/collection-views.md) документации.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Презентация поля поиска

Существует два основных способа, поля поиска (и связанный с ним экранную клавиатуру и результаты поиска) могут быть представлены пользователю в tvOS: 

- **Модальное диалоговое окно представления** -поле поиска могут быть представлены через текущего представления и представления учетной записью во весь экран модальное диалоговое окно. Обычно это делается в ответ на щелчок кнопки или другой элемент пользовательского интерфейса. Диалоговое окно закрывается, когда пользователь выбирает элемент в результатах поиска.
- **Просмотр содержимого** -поле поиска является прямой частью данного представления. Например как содержимое вкладки поиска в контроллер вкладку представления.

Пример список изображений, приведенном выше поле поиска будет представлено в виде Просмотр содержимого на вкладке «Поиск» и представление контроллер вкладку поиска выглядит следующим образом:

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Во-первых, определяется константой, соответствующий **раскадровки идентификатор** , назначенный для контроллера представления коллекции результаты поиска в конструкторе интерфейса:

```csharp
public const string SearchResultsID = "SearchResults";
```

Далее, `ShowSearchController` метод создает новый контроллер поиска представления коллекции и отображает его требовалось:

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

В приведенном выше методе один раз `SearchResultsViewController` был создан экземпляр из раскадровки, новый `UISearchController` создан для представления поля поиска и экранную клавиатуру для пользователя. Сбор результатов поиска (в соответствии с определением `SearchResultsViewController`) будет отображаться под эту клавиатуру.

Далее, `SearchBar` настраивается с сведения, такие как **заполнитель** подсказку. Предоставляет сведения о тип поиска в том случае.

Затем в поле поиска предоставляется пользователя одним из двух способов:

- **Модальное диалоговое окно представления** - `PresentViewController` метод вызывается для представления поиска на существующее представление полноэкранном режиме.
- **Просмотр содержимого** - `UISearchContainerViewController` для записей контроллера поиска. Объект `UINavigationController` создается в контейнере поиска, то контроллер навигации добавляется View-Controller `AddChildViewController (navController)`и представление представлены `View.Add (navController.View)`.

Наконец, а затем на основе типа представления, либо `ViewDidLoad` или `ViewDidAppear` вызывает метод `ShowSearchController` метод, чтобы предоставить пользователю поиска:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

При запуске приложения а вкладке поиска, выбранных пользователем, нефильтрованные полный список элементов, отображаются для пользователя:

[ ![](text-fields-and-search-images/intro02.png "Результаты поиска по умолчанию")](text-fields-and-search-images/intro02.png)

Как пользователь начинает вводить условие поиска, список результатов отобранные этот термин и обновляется автоматически.

[ ![](text-fields-and-search-images/intro03.png "Отфильтрованные результаты.")](text-fields-and-search-images/intro03.png)

В любое время пользователь может перенести фокус на элемент в результатах поиска и щелкните область Touch удаленного Siri, чтобы выбрать его.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован разработки и работы с текстом и поля внутри приложения Xamarin.tvOS. Оно было рассмотрено создание содержимого текста и поиска коллекции в конструкторе интерфейса и ней отображались в двумя различными способами, которые удалось представлены пользователю в tvOS поле поиска.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
