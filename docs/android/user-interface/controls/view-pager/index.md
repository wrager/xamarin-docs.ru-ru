---
title: ViewPager
description: "ViewPager представляет собой диспетчер макет, который позволяет реализовать жестовую навигации. Жестовую навигации позволяет пользователю проведите влево и вправо для пошагового просмотра страниц данных. В этом руководстве объясняется, как реализовать жестовую навигации с ViewPager, с и без фрагмента. Также описывается добавление с помощью PagerTitleStrip и PagerTabStrip индикаторы страницы."
ms.topic: article
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 80ba525b87d2008f290e32fde56265630bac729a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="viewpager"></a>ViewPager

_ViewPager представляет собой диспетчер макет, который позволяет реализовать жестовую навигации. Жестовую навигации позволяет пользователю проведите влево и вправо для пошагового просмотра страниц данных. В этом руководстве объясняется, как реализовать жестовую навигации с ViewPager, с и без фрагмента. Также описывается добавление с помощью PagerTitleStrip и PagerTabStrip индикаторы страницы._

<a name="overview" />
 
## <a name="overview"></a>Обзор

Распространенный сценарий в разработке приложений является необходимость предоставить пользователям жестовую перехода между представлениями одного уровня. В этом случае пользователь предъявляет влево или вправо для доступа к страницам содержимого (например, в мастере установки или слайд-шоу). Эти представления проведите можно создать с помощью `ViewPager` мини-приложения, доступные в [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). `ViewPager` Является макета мини-приложение состоит из нескольких дочерних представлений, где каждый дочернее представление составляет страницы в макете: 

[![Снимки экрана TreePager приложение с горизонтальной проведите пример](images/01-intro-sml.png)](images/01-intro.png)

Как правило `ViewPager` используется в сочетании с [фрагментов](https://developer.xamarin.com/guides/android/platform_features/fragments/), однако существуют ситуации, где вы можете использовать `ViewPager` без дополнительной сложностью `Fragment`s.

`ViewPager` шаблон адаптера снабжать представлений для отображения. Адаптер, используемый здесь аналогичен, используемая [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; предоставить реализацию `PagerAdapter` для создания страниц, `ViewPager` отображает для пользователя. Страниц, отображаемых по `ViewPager` может быть `View`s или `Fragment`s. Когда `View`отображаются, адаптер подклассы Android в `PagerAdapter` базового класса. Если `Fragment`отображаются, адаптер подклассы Android в `FragmentPagerAdapter`. Также включает библиотеку поддержки Android `FragmentPagerAdapter` (подкласс `PagerAdapter`) для подробные сведения о подключении `Fragment`s к данным. 

В этом руководстве показаны оба подхода: 

-   В [Viewpager с представлениями](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) приложение разработано для использования `ViewPager` для отображения представления дерева каталога (коллекции образов листопадное и вечнозеленое деревьев). 
    `PagerTabStrip`  и `PagerTitleStrip` используются для заголовков, упрощающие навигацию по страницам.

-   В [Viewpager с фрагментами](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), немного более сложная [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) приложение разработано для использования `ViewPager` с `Fragment`s, чтобы построить приложение, которое предоставляет математические задачи как флэш-карты и отвечает на ввод данных пользователем. 

<a name="requirements" />

## <a name="requirements"></a>Требования

Для использования `ViewPager` в проекте приложения необходимо установить [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) пакета. Дополнительные сведения об установке пакетов NuGet см. в разделе [Пошаговое руководство: включая NuGet в проекте](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

<a name="architecture" />
 
## <a name="architecture"></a>Архитектура

Три компоненты используются для реализации жестовую навигации с `ViewPager`:

-   ViewPager
-   Адаптер
-   Индикатор страничного навигатора

Ниже приведена общая каждого из этих компонентов.


<a name="viewpager" />

### <a name="viewpager"></a>ViewPager

`ViewPager` представляет диспетчер макет, который отображает коллекцию `View`s, одну за другой. Для обнаружения проведите жест пользователя и перехода к следующему или предыдущему представлению соответствующим образом — свою работу. Например, на следующем снимке экрана показано `ViewPager` перехода от одного изображения к другому в ответ на жест пользователя: 

[![Крупный план TreePager приложения отображение перехода между представлениями](images/02-transition-sml.png)](images/02-transition.png)


<a name="adapter" />

### <a name="adapter"></a>Адаптер

`ViewPager` Извлекает данные из *адаптер*. Задание адаптера является создание `View`s, отображаемого элементом `ViewPager`, предоставляя им при необходимости. На схеме ниже показан эту концепцию &ndash; адаптер создает и заполняет `View`s и предоставляет их `ViewPager`. Как `ViewPager` обнаруживает жесты проведите пользователя, он запрашивает у адаптер для предоставления соответствующего `View` для отображения: 

[![Диаграмма, иллюстрирующая как адаптер подключается изображения и имена к ViewPager](images/03-adapter-sml.png)](images/03-adapter.png)

В данном конкретном примере каждый `View` создается на основе изображение дерева и имя дерева, прежде чем оно передается `ViewPager`. 


<a name="indicator" />

### <a name="pager-indicator"></a>Индикатор страничного навигатора

`ViewPager` может использоваться для отображения большого набора данных (например, коллекции образов может содержать сотни изображения). Чтобы помочь пользователю перемещение по большим наборам данных `ViewPager` часто сопровождается *индикатор страничного навигатора* , отображается строка. Строка может быть название, заголовок или просто текущее представление позиции в наборе данных. 

Доступно два представления, которые может создать эти сведения навигации: `PagerTabStrip` и `PagerTitleStrip.` каждая строка в верхней части `ViewPager`, и каждая его данные извлекаются из `ViewPager`элемента адаптера, так что он всегда сохраняется в соответствии с в настоящее время отображается `View`. Различие между ними состоит в том `PagerTabStrip` содержит визуальный индикатор, предназначенный для строки «текущая» при `PagerTitleStrip` не (так, поскольку эти снимки экрана показано): 

[![Снимки экрана приложения с PagerTitleStrip и PagerTabStrip TreePager](images/04-comparison-sml.png)](images/04-comparison.png)

В этом руководстве показано, как immplement `ViewPager`, адаптер и индикатор компонентов приложения и интегрировать их для поддержки жестовую навигации. 



## <a name="related-links"></a>Связанные ссылки

- [TreePager (пример)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (пример)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)