---
title: Работа с tvOS навигации и фокус находится на Xamarin
description: В этой статье рассматриваются концепции фокус и как она используется для представления и обрабатывать навигацию внутри приложения Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2af3306b605ee802b72b159d5f2759d71d292a64
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788736"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Работа с tvOS навигации и фокус находится на Xamarin

_В этой статье рассматриваются концепции фокус и как она используется для представления и обрабатывать навигацию внутри приложения Xamarin.tvOS._


В этой статье рассматриваются концепции [фокус](#Focus-and-Selection) и как она используется для обработки [навигации](#Navigation) в приложении Xamarin.tvOS пользовательский интерфейс. Мы рассмотрим способа использования tvOS встроенные элементы управления навигацией фокуса, выделения и выделения для обеспечения навигации Xamarin.tvOS приложения.

[![](navigation-focus-images/intro01.png "tvOS приложения навигации")](navigation-focus-images/intro01.png#lightbox)

Далее мы будем взгляните на использование фокус с [фокусировки](#Focus-and-Parallax) и *многоуровневая изображения* для предоставления визуальные подсказки для текущего состояния навигации для конечного пользователя.

И, наконец, мы рассмотрим работа с [фокус](#Working-with-Focus), [обновления фокус](#Working-with-Focus-Updates), [направляющие фокус](#Working-with-Focus-Guides), [фокус в коллекциях](#Working-with-Focus-in-Collections) и [ Включение фокусировки](#Enabling-Parallax) представлений изображений в приложениях Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Навигация

Пользователи приложения Xamarin.tvOS не взаимодействия с его интерфейс непосредственно как с iOS они tap изображения на экране устройства, но косвенно из в комнате с помощью [Siri удаленного](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Необходимо учитывать это при разработке пользовательского интерфейса приложения, чтобы он проходит естественным образом, но сохранение занимается Apple TV взаимодействие пользователя.

Приложение успешно tvOS реализует навигации в результате которого плавно поддерживает назначение приложения и структуры данных, которые он представляет без вызова внимания к самой навигации. Разработать переходы, таким образом, чтобы его мнению естественным и знакомые без тратиться пользовательского интерфейса или рисование фокус от содержимого и пользовательском интерфейсе приложения.

[![](navigation-focus-images/nav01.png "Параметры приложения tvOS")](navigation-focus-images/nav01.png#lightbox)

Хотя обычно с помощью Apple TV, пользователь переходит к стеках экраны, каждого представления данного набора содержимого. В свою очередь, каждый новый экран может привести к один или несколько вложенных экранов содержимого с помощью стандартных элементов управления пользовательского интерфейса, такие как [кнопки](~/ios/tvos/user-interface/buttons.md), [полосы вкладки](~/ios/tvos/user-interface/tab-bars.md), таблиц, [представления коллекций](~/ios/tvos/user-interface/collection-views.md) или [ Представления с разделением](~/ios/tvos/user-interface/split-views.md).

Каждый новый экран данных пользователь перемещается глубже в стек экранов. С помощью **меню** кнопки на удаленном Siri, можно перейти вперед по стеку для возврата к предыдущему экрану или главного меню.

Apple предлагает, учитывая следующие при разработке навигации для tvOS приложения:

- **Макет навигации таким образом, чтобы сделать поиск содержимого файлов и простота** -пользователей для доступа к содержимому в приложении наименьшее число касания, щелкает и предъявляет максимально. Упростить переходы и организовать содержимое на минимальное количество экранов.
- **Создание жидкости интерфейса с помощью Touch** -убедитесь, что пользователь может перемещаться между _может иметь фокус элементы_ с минимальными повышение эффективности с помощью наименьшее число возможных жесты.
- **Разработки с фокусом помните** -так, как пользователь взаимодействует с содержимым в комнате, их нужно переместить фокус на элемент пользовательского интерфейса, перед взаимодействием с его с помощью удаленного Siri. Пользователи будут получать разочарованы вместе с приложением, если для него требуется слишком много жесты для них для достижения поставленных целей.
- **Укажите обратная Навигация с помощью кнопки меню** - для создания простой и знакомой среды позволяют пользователям переходить вперед с помощью удаленного Siri **меню** кнопки. Нажав клавишу **меню** кнопка всегда следует вернуться к предыдущему экрану или вернуться к главному меню приложения. В приложения верхнего уровня, нажав клавишу **меню** кнопка должна возвращать Apple TV начального экрана.
- **Отображение кнопки обратно обычно следует избегать** — так как нажатие **меню** кнопки на удаленном Siri переходит вперед через стек экрана, отключать дополнительного элемента управления, которое дублирует это поведение. Исключением из этого правила является за приобретение экранов и экраны с разрушительных действий (например, удаление содержимого) где **отменить** также должна появиться кнопка.
- **Показать больших коллекций на одном экране, а не на всех** -удаленного Siri был разработан для перемещения по коллекции больших содержимого быстрого и просто с помощью жестов. Если приложение работает с большим набором элементов может иметь фокус, рекомендуется использовать их на одном экране вместо остановом много экраны, которые требуют больше перемещение со стороны пользователя.
- **Используйте стандартные элементы управления для навигации** -опять же, чтобы создать интерфейс пользователя простым и понятным там, где возможно, использовать встроенные `UIKit` элементов управления, например элементов управления на странице, полосы вкладки, сегментированных элементов управления, таблицы, представления коллекции и разбиение Представления для навигации приложения. Так как пользователь уже знакомы с этими элементами, они интуитивно будут возможности перехода приложения.
- **Предпочитать горизонтальной навигацию по содержимому** -открытостью Apple TV считывания слева направо на удаленном Siri более естественным, чем вверх и вниз. Этот вариант рекомендуется при разработке содержимого макеты для приложения.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>Фокуса и выделения

Для Apple TV, изображения, кнопки или другой элемент пользовательского интерфейса будет считаться _в фокусе_ при целевого объекта текущей навигации.

[![](navigation-focus-images/focus01.png "Пример фокуса и выделения")](navigation-focus-images/focus01.png#lightbox)

В отличие от, где пользователь непосредственно взаимодействует с элементами на сенсорном экране устройства, устройства iOS пользователи взаимодействуют с tvOS элементы из в комнате с помощью удаленного Siri. Для представления и управления взаимодействием с пользователем, использует Apple TV _фокус_ модель на основе.

С помощью жестов и кнопку нажимает на [Siri удаленного](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), пользователь перемещает фокус от одного элемента пользовательского интерфейса в другую. Когда фокус переходит к нужному элементу, пользователь нажимает кнопку, выберите содержимое или активировать действие, представленное этого элемента.

При изменении фокуса слабая анимации и эффектов (например, эффект фокусировки на образах) используются четко определить элемент пользовательского интерфейса, который в данный момент имеет фокус.

Apple имеет следующие рекомендации по работе с фокуса и выделения.

- **Использование встроенных элементов управления пользовательского интерфейса для эффектов движения** — с помощью `UIKit` и API фокус в пользовательском интерфейсе, фокус модели автоматически будет применяться по умолчанию движения и визуальные эффекты и элементы пользовательского интерфейса. Это делает чувствовать себя приложения, собственный и знакомые пользователям Apple TV платформы и обеспечивает гибкость и интуитивно понятный перемещения между элементами может иметь фокус.
- **Перемещение фокуса в направлениях ожидается** -на Apple TV практически любой элемент использует косвенных манипуляции. Например пользователь использует удаленный Siri для перемещения фокуса, и система автоматически прокручивается интерфейс закрепить элемент в данный момент находится фокус. Если ваше приложение реализует этот тип взаимодействия, убедитесь, что фокус перемещается в направлении жест пользователя. Поэтому если пользователь проведите правой кнопкой фокус Siri удаленного следует переместить вправо (что может привести экрана для прокрутки влево). Единственным исключением из этого правила — во весь экран элементы, использующие прямого управления (где экрану вверх перемещает элемент вверх).
- **Убедитесь, что элемент с фокусом ввода Obvious** -так, как взаимодействие пользователей с элементы пользовательского интерфейса из удаленного местоположения, очень важно, что в данный момент элемента важные моменты. Обычно это будет автоматически обрабатываться встроенной `UIKit` элементов. Для пользовательских элементов управления используйте функции, такие как размер элемента или тени для отображения фокус.
- **Используйте фокусировки, чтобы сделать с фокусом ввода элементов отвечать на запросы** — небольшой, циклические жестов на удаленном Siri привести легкими, в режиме реального времени перемещения элемента. Это [эффект фокусировки](#Focus-and-Parallax) встроено в `UIKit` _многоуровневая изображения_ для предоставления пользователю смысле подключения для элемента.
- **Создание элементов может иметь фокус подходящего размера** -крупные объекты с интервалом, можно в полной мере упрощает выбор и перемещение элементов меньшего размера.
- **Создать элемент пользовательского интерфейса для поиска хорошо либо с фокусом ввода или Unfocused** -обычно Apple TV представляет элемент в фокусе, увеличив его размер. Убедитесь, что элементы пользовательского интерфейса вашего приложения удобна в любом масштабе презентации, а при необходимости предоставить активов для более крупных размеров элементов.
- **Изменения фокуса гибкого представляют** -используйте анимацию плавно затеняются между элементами **фокуса** и **Unfocused** состояние для сохранения резать глаз переходит из срока.
- **Не отображать курсор** -пользователи ожидают, что для перемещения фокуса с помощью пользовательского интерфейса приложения, а не при перемещении курсора по экрану. Пользовательский интерфейс следует всегда использовать модель фокуса для представления согласованного пользовательского интерфейса.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Работа с фокусом

Возможны ситуации, которые вы хотите создать настраиваемый элемент управления, который может стать элемент может иметь фокус. Если таким образом переопределить `CanBecomeFocused` свойства и возвращаемые `true`, в противном случае возвращаемый `false`. Пример:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

В любое время можно использовать `Focused` свойства `UIKit` элемента управления, является ли текущий элемент. Если `true` элемент пользовательского интерфейса в настоящее время находится в фокусе, в противном случае это не так. Пример:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Хотя нельзя перемещать непосредственно фокуса на другой элемент пользовательского интерфейса через код, можно указать, какой элемент пользовательского интерфейса сначала получает фокус, когда экран загружается, установив его `PreferredFocusedView` свойства `true`. Пример:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Работа с обновлениями фокус 

Если пользователь вызывает перемещение от одного элемента пользовательского интерфейса фокуса на другой (например, в ответ на жест на удаленном Siri) _события фокуса Update_ будут отправляться на элемент, потере фокуса и элемента, получившего фокус.

Для настраиваемых элементов, которые наследуют от `UIView` или `UIViewController`, можно переопределить несколько методов для работы с события фокуса обновления:

- **DidUpdateFocus** -этот метод будет вызываться каждый раз, представление Получает или теряет фокус.
- **ShouldUpdateFocus** -используйте этот метод, чтобы определить, где разрешено перемещать фокус.

Для запроса, что подсистема фокус перемещается обратно фокус `PreferredFocusedView` элемент пользовательского интерфейса, вызов `SetNeedsUpdateFocus` метод контроллера представления.

> [!IMPORTANT]
> Вызов `SetNeedsUpdateFocus` оказывает воздействие только если он вызывается для контроллер представление содержит представление, которое в данный момент имеет фокус.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Работа с направляющими фокус

Подсистема фокус, встроенные в tvOS отлично подходит для обработки перемещения между элементами, которые делятся на горизонтальной и вертикальной сетки. Как правило необходимо ничего не больше, чем добавить элементы пользовательского интерфейса для интерфейса конструкции и подсистема фокус автоматически будет обрабатывать перемещение между этими элементами, не привлекая разработчиков.

Однако возможны ситуации, из-за потребности вашего разработки пользовательского интерфейса, когда элементы пользовательского интерфейса, не попадающие в горизонтальной и вертикальной сетки и могут быть недоступны, поскольку они являются диагональным друг с другом. Это происходит, так как подсистема фокус была разработана для обработки простых вверх, вниз, влево и вправо, перемещение между только элементы пользовательского интерфейса.

Выполните следующий пример макета пользовательского интерфейса.

 [![](navigation-focus-images/guide01.png "Работа с примером фокус направляющие")](navigation-focus-images/guide01.png#lightbox)
 
Поскольку **детали** кнопка не входят в сетке с горизонтальной и вертикальной **купить** кнопка, он станет недоступным для пользователя. Тем не менее, это можно легко исправить с помощью _фокус руководство_ для обеспечения указания перемещения в подсистему фокус. 

Руководство фокус (`UIFocusGuide`) предоставляет невидимые область представления в виде Focusable механизму фокус, позволяя тем самым фокус перенаправляться в другое представление.

Таким образом, чтобы решить, приведенном выше примере, следующий код удалось добавить представление-контроллер для создания структуры фокус между **детали** и **купить** кнопки:

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

Во-первых, новый `UIFocusGuide` создается и добавляется в коллекцию направляющей разметки представления с помощью `AddLayoutGuide` метод.

Далее в руководстве по фокус сверху, слева, ширина и высота привязки корректируются относительно **детали** и **купить** расположите его между ними. Пример

[![](navigation-focus-images/guide02.png "Пример фокус руководства")](navigation-focus-images/guide02.png#lightbox)

Также важно отметить, что при их создании, задав в процессе активации новых ограничений их `Active` свойства `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

С новой структуры фокус устанавливается и добавляются в представление, представление-контроллер `DidUpdateFocus` метод может быть переопределен и добавлен следующий код для перемещения между **детали** и **купить** кнопки:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

Во-первых, этот код get `NextFocusedView` из `UIFocusUpdateContext` , переданный в (`context`). Если это представление будет `null`, обработка не требуется и метод завершил работу.

Далее, `nextFocusableItem` вычисляется. Если он соответствует либо **детали** или **купить** кнопки, фокус отправляется противоположной кнопка, с помощью руководства фокус `PreferredFocusedView` свойство. Пример:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

В случае, если ни одна из кнопок является источником смены фокуса `PreferredFocusedView` свойство очищается:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Работа с фокусом в коллекциях

При определении того, можно ли отдельный элемент может иметь фокус в `UICollectionView` или `UITableView`, будет переопределять методы `UICollectionViewDelegate` или `UITableViewDelegate` соответственно. Пример:

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

`CanFocusItem` Возвращает `true` Если текущий элемент в фокусе, в противном случае он возвращает `false`.

Если вы хотите `UICollectionView` или `UITableView` запомнить и восстановить фокус до последнего элемента при потере и снова получит фокус, задайте `RemembersLastFocusedIndexPath` свойства `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Фокус и фокусировки

Как уже говорилось выше, элемент пользовательского интерфейса будет считаться _в фокусе_ Если текущий элемент имеет во время события навигации. Обычно включается элемент в фокусе его размер увеличивается немного и визуально повышаются, с помощью тень. 

Если пользователь делает медленно, циклические жестов на удаленном Siri, элемент с фокусом ввода будет sway в режиме реального времени в ответ на такое перемещение. По мере sway оформленный sheen применяется до создания рабочей области отображаются для освещения образа. После заданного периода бездействия любое содержимое элемента в фокусе затемняет и фокуса элемента будет дальнейшее расширение. 

Эти эффекты работают совместно для обеспечения покажется несколько соединения между содержимое на экране ТВ и взаимодействия с Apple TV, хранящиеся на диске пользователя.

На Apple TV этот эффект фокусировки используется для передачи сведений о смысле глубина и dynamics элементы основное внимание во всей системе. tvOS предлагает ряд прозрачным, [многоуровневая изображения](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) которого перемещает и масштабирует динамически, чтобы создать этот эффект.

Слоев изображения являются обязательными для значка tvOS приложения и поддерживается динамическое содержимое, Полка Top. Хотя это не обязательно Apple настоятельно рекомендует реализации многоуровневая изображений в других может иметь фокус элементов в пользовательском Интерфейсе приложения.

### <a name="enabling-parallax"></a>Включение фокусировки

`UIImageView` Управления (или любой элемент управления, который наследует от `UIImageView`) автоматически поддерживает эффект фокусировки. По умолчанию эта поддержка отключена, чтобы включить его, используйте следующий код:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Это свойство равным `true`, автоматически получают представление изображения эффект фокусировки при выборе пользователем и находится в фокусе.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован концепции фокус и как она используется для обработки навигации в приложении Xamarin.tvOS пользовательский интерфейс. Он проверить, как использовать tvOS встроенные элементы управления навигацией фокуса, выделения и выделения для обеспечения навигации. Затем он рассмотрели использование фокус в с фокусировки и многоуровневая изображения для предоставления визуальные подсказки для текущего состояния навигации для конечного пользователя. Наконец он проверяет работу с фокусом, обновления фокус фокуса в коллекций и включение фокусировки.




## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
