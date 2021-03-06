---
title: Общие сведения о разработке игр с помощью Xamarin
description: В этом документе общий обзор разработки игр с помощью Xamarin, описывающие как выполняются игры и выборки из технологий, доступных для использования с Xamarin.iOS и Xamarin.Android.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: b1bd6d011cdc10352ce3b9258da7e16c2899d2d8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783920"
---
# <a name="introduction-to-game-development-with-xamarin"></a>Общие сведения о разработке игр с помощью Xamarin

Разработка игр может быть очень интересно, особенно учитывая как просто можно публикация приложения на мобильных платформах. В этой статье рассматриваются концепции и технологии, относящиеся к разработке игр, которые помогут создавать игры, ли вашей целью является создание AAA высокого качества, игры или просто для программы для развлечения.

В этой статье рассматриваются следующие темы:

- **Игры и принципы программирования не игры** — мы рассмотрим некоторые основные понятия, которые могут быть уникальными для разработки игр или совместно с другими типами разработки, но заслуживают внимания из-за их важности.
- **Команда разработчиков игры** — в этом разделе рассматриваются различные роли в команде разработчиков игр.
- **Создание игр идея** — в этом разделе могут помочь в создании нова игры — первый шаг при создании новой игры.
- **Технология разработки игры** — здесь будут перечислены некоторые из доступных кросс платформенных технологий, может повысить эффективность работы разработчик игры.


# <a name="game-vs-non-game-programming-concepts"></a>Игры vs. Основные понятия программирования не игры

Программисты, переместив в разработки игр часто приходится сталкиваться с новой концепции и шаблоны разработки. В этом разделе описываются некоторые из этих понятий высокоуровневое представление.


## <a name="the-game-loop"></a>Игры цикла

Типичные игру требуется постоянное перемещения или изменения происходит в ответ на взаимодействие с пользователем и автоматической логику игры на экране. Это можно сделать то, что обычно называется *игры цикла*. Игры цикла является своего рода циклов инструкции (например, время цикла), которая выполняется на очень высокой частоты, например, 30 или 60 *кадров в секунду*.

Ниже приведен простой игры цикл:

![](images/image1.png "Это схема простой цикл игры")

Технологии, которые рассматриваются ниже сделают абстрактными фактическое цикл while, но несмотря на это абстракция будет присутствовать концепцию обновлений каждые кадра.

Производительность кода можно имеют более высокий приоритет в даже самая простая игр. Например: функции, которая занимает 10 миллисекунд для выполнения может иметь значительное влияние на производительность игры — особенно в том случае, если она вызывается несколько раз в кадре. Если выполняется игру, равной 30 кадров в второй затем это означает, что каждый кадр должен выполняться в миллисекундах в разделе 33. Напротив такой функции может не быть заметной, если она выполняется только в ответ на нажатие кнопки в приложении не игры.

Общие типы логику, которая может быть выполнено каждые кадра:

- **Чтение входных** — может потребоваться проверить, если пользователь взаимодействовать с ними игры путем проверки входных оборудованием, таким как сенсорный экран, клавиатура, мышь или игровое игры.
- **Перемещение** — каждый кадр для создания иллюзии плавность движения сумма какие перемещение из одного места в другое обычно переместит очень маленьких объектов.
- **Конфликт** — многие игры требуют частого тестирования ли перекрывающиеся или пересекающиеся различных объектов. Мы обсудим конфликтов более подробно в следующих разделах этой статьи. Перемещение и конфликтов может быть обработано системой выделенный физический моделирования.
- **Проверка условия играми** — состояния игры может управляться определенных условий, таких как ли игрок получил достаточное количество точек или ли истекло отведенное время.
- **Поведение AI** — каждые кадра логику, которая может использоваться для управления поведением объектов, которые не управляются проигрыватель, например patrolling enemy или перемещение сопернику драйверы вокруг Овальный.
- **Подготовка к просмотру** — обновит большинство игр, отображаемых на экране каждого кадра. Это может быть выполнено в ответ на изменения, которые оказывают воздействие на игры (таких как символ, перемещаясь по уровня) или просто для предоставления visual Польский (например, уменьшение Снегопад или анимированных значки).

Имейте в виду, что многие действия, перечисленные выше может изменить состояние приложения, в то время как во многих приложениях не игры, как правило, для изменения состояния в ответ на события.


## <a name="content-loading-and-unloading"></a>Содержимого загрузки и выгрузки

Содержимое вручную Загрузка и выгрузка (или уничтожения) может потребоваться в зависимости от того, какую технологию вы используете в разработке. Вручную Загрузка и выгрузка активов могут потребоваться по ряду причин.

 - Активы может занять много времени для загрузки относительно длину один кадр. Некоторые средства может потребоваться даже секунд для загрузки, которые значительно нарушило бы взаимодействие загруженного mid игрового процесса. Если время загрузки особенно длительное время (например, более чем одной-двух секунд), можно отобразить анимированный индикатор выполнения и экран загрузки.
 - Ресурсы могут потреблять много оперативной памяти, требующее активного управления из компонентов, загружаемых в соответствии с размерами предоставляемыми игры в целевых платформ.
 - Игры может потребоваться отобразить несколько ресурсов, чем может поместиться в оперативной памяти. «При открытии World» игры часто содержат крупных средах — легко перемещаться по какой проигрывателей с экранами без загрузки. В этом случае может потребоваться создание настраиваемой системы для потоковой передачи содержимого в и управление памятью.

Настраиваемые форматы файлов может потребоваться обработка во время загрузки, требуя пользовательской загрузки кода.


## <a name="math"></a>Математический

Многие игры требуют более сложных математических операций, чем приложения не игры. Конечно уровень математические зависит от сложности игры. В целом трехмерных игр требуются дополнительные математические, чем 2D. К счастью, всегда можно начать работу с простой игры и курсы. Разработка игр может быть хорошим способом узнать математические!

Если вы знакомы с декартовой —, использующего координаты X и Y для размещения объектов —, то вам достаточно информации, чтобы начать разработку игр на. Ниже приведен декартовой положительное Y указывает вверх.

![](images/image2.png "В следующем примере показано декартовой положительное Y указывает вверх")

> [!IMPORTANT]
> Некоторые модули или интерфейсы API используется система координат, где увеличение значения Y объекта будет перемещать его вниз, а в других систем системе координат, где Y положительное. Это помнить при перемещении между системами.
Тригонометрические функции (например синус и косинус) часто используются в двухмерной игры, реализующие поворота в любой форме.



Если вы планируете для внесения трехмерных игр затем вы скорее всего, потребуется можно ознакомиться с основными понятиями линейной алгебры (для смены и перемещения в трехмерном пространстве), а также некоторые математического анализа (для реализации ускорение).


## <a name="content-pipelines"></a>Конвейеры содержимого

Термин *конвейера содержимого* относится к процессу, принимающий файл для получения из формата при среде (например, изображения PNG-файл) в окончательный формат при использовании в игру. Конечный формат зависит от того, на котором содержимого используется тип и который технология используется для представления содержимого.

Некоторые конвейеры содержимого может выполняться очень быстро и требуют никаких действий вручную. Например большинство обработчиков игры и API-интерфейсы можно загрузить в формате .png в необработанном формате. С другой стороны более сложный форматов (например, трехмерной модели) может должны быть обработаны в другой формат до загрузки и этот процесс может занять некоторое время в зависимости от размера и сложности актива.


# <a name="game-development-teams"></a>Команды разработки игр

Разработка игр представлены новые роли и заголовки для частных лиц, участвующих в процессе. Большинство разработчиков игр не смогут удовлетворить широкий набор навыков, необходимых для выпуска полный игры, поэтому существует ряд дисциплин. Имейте в виду, что это не полный список областей разработки — лишь некоторые из наиболее распространенных окон.

- **Программист** — большинство пользователей, читающих в эту категорию попадают в этой статье. Роль программиста в разработки игр аналогична программиста роли в приложении не игры. В обязанности входит, написав логику для управления потоком игры, разработке системы для общих задач в контексте данного проекта, добавление и отображение содержимого — разумеется — исправления ошибок.
- **2D исполнителя** — 2D исполнители отвечают за создание *2D активы*. К ним относятся файлы изображений для графического пользовательского интерфейса игры, примитивов, сред и символов. Если вы разрабатываете игру объемные эффекты, затем 2D исполнители может оказаться ответственность за сред и символы. Можно найти свободного рисунка для игры на [ http://opengameart.org/ ](http://opengameart.org/) .
- **3D исполнители** — 3D исполнители отвечают за создание *3D активы*. К ним относятся трехмерных моделей для сред, символы и props (мебель, производство и другие inanimate объекты). Некоторые команды различать 3D исполнителей и 3D мультипликаторы в зависимости от размера рабочей группы. Можно найти бесплатные трехмерной графикой для игры на [ http://opengameart.org/ ](http://opengameart.org/) .
- **Игры конструктор** — игры конструкторы несут ответственность за определение как игры. Это могут быть решений высокого уровня как параметр игры, игры и как проигрыватель переходит через игры общей цели. Игры конструкторы также может быть занимающиеся детальную принятия решений, например, сопоставление входных данных для действия, определение коэффициенты для перемещения или уровне ИБП и проектирование уровня макета. Имейте в виду, что термин *конструктор* могут ссылаться на игры конструктор или визуальный конструктор, в зависимости от контекста.
- **Звуковой конструктор** — звуковой конструкторы отвечают за игры в аудио активы. Некоторые команды могут различать сотрудников, ответственных за создание звуковыми эффектами и композиторы, в то время небольших групп возможно одного лица, ответственного за все аудио.


# <a name="creating-a-game-idea"></a>Создание игр идею

Разработка игр может появиться быть легко сделать — в конце концов единственное требование «сделать что-нибудь интересна.» К сожалению многие разработчики оказываются в убыток когда придет время, чтобы создать представление запуска разработки.

Дисциплину игры разработки не указывает легко и требуется рекомендаций для повышения так же, как картинки или программирования, но в этом разделе помогут вам начать работу путь.

Начинающим разработчикам следует приступить к работе. Может усложниться от стремления повторное создание больших, современный игру, но меньше игры может быть лучше среды обучения и делает быстрее выполняется для более эффективной работы.

Многие игры, оба с целью учебные, а также коммерческие игры, начать как улучшения или изменения существующих игры. Для просмотра других игр вдохновения — один из способов создания идеи. Например можно игры, который лично и попробуйте для определения того, какие характеристики об игре обеспечивают развлечений. Может быть исследования мастерства в работе механизм игры или переходе статьи. Не забудьте при поиске новых идей, рассмотрите возможность также «ретро» игры.

Другим способом создания новых идей является рассмотрение определенного жанра, например головоломки игры, стратегия игры или platformers. Жанра, знакомые разработчикам может обеспечить хорошей отправной точкой.

Изменяют существующие игры также является возможности для образования, несмотря на то, что это может ограничивать коммерческих устойчивости готового продукта. Процесс создания игры, хотя бы один которого является точным клоном предоставляет полезные возможности для образования.


# <a name="game-development-technology"></a>Технология разработки игр

Разработчики, использующие Xamarin.Android и Xamarin.iOS имеют широкий спектр технологий, доступных для помощи в разработке игр. В этом разделе рассматриваются некоторые из наиболее популярных решений кросс платформенных.


## <a name="cocossharp"></a>CocosSharp

CocosSharp является версией открытым исходным кодом, кросс платформенных игр 2D подсистема Cocos. Обработчик предоставляет доступ к Android, iOS, Mac OS X, Windows Desktop, Windows RT и Windows Phone.

CocosSharp основное внимание уделяется простой программист API для разработки двухмерных игр. Рост в игры на мобильных устройствах позволило reignite популярности 2D разработки игр, делая CocosSharp реальную технология хобби и коммерческих проекты одинаково. Предоставляется в виде исходного кода или DLL-файлов (которые можно получить с помощью NuGet), но не предлагает визуальный редактор; Таким образом любое взаимодействие с подсистемой CocosSharp требует знания программирования.

Чтобы начать работу с CocosSharp, извлечь нашей [CocosSharp направляющие](~/graphics-games/cocossharp/index.md).

Игра Сердитая ниндзя создается с CocosSharp, и она может быть хорошей отправной точкой, если вы ищете игра уже выполняется для нескольких платформ:

![](images/image3.png "Игра Сердитая ниндзя был создан с CocosSharp")

Можно загрузить и получить дополнительные сведения в [странице AngryNinjas Github](https://github.com/xamarin/AngryNinjas).


## <a name="monogame"></a>MonoGame

MonoGame имеет открытый исходный код, кросс-версия платформы Microsoft XNA API-интерфейса. MonoGame можно использовать для создания игр для iOS, Android, Mac OS X, Linux, Windows, Windows RT и Windows Phone.

В отличие от CocosSharp MonoGame является технически не игры подсистему, но вместо разработки игр API. Это означает, что работа с MonoGame непосредственно управление объектов игры, вручную графических объектов и реализация общих объектов, таких как камеры и *сцены графы* (родительской дочерней иерархии между объектами игры). Чтобы понимать их различие, рассмотрим, что CocosSharp является надстройкой MonoGame. MonoGame обобщает некоторые платформой технологии, графики, визуализации и аудио, а CocosSharp предоставляет код для организации и реализация логики игры.

MonoGame не поддерживает стандартной visual среде разработки, поэтому для работы с MonoGame требуются знания программирования.

Важные игр с помощью MonoGame примеры.

FEZ:

![](images/image7.png "FEZ")

Бастиона:

![](images/image8.jpg "Бастиона")

Чтобы приступить к работе с MonoGame, перейдите к нашей [MonoGame направляющие](~/graphics-games/monogame/index.md).


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp представляет собой кросс платформенных высокоуровневые 3D и 2D модуль, можно использовать для создания анимированных 3D и 2D автоматически для приложений с помощью геометрических объектов, материалов, индикаторы и камеры.

![](images/urhosharp.gif "UrhoSharp представляет собой кросс платформенных высокого уровня 3D и 2D модуль, можно использовать для создания анимированных 3D и 2D сцены")

Извлечение [UrhoSharp направляющие](~/graphics-games/urhosharp/index.md) Чтобы приступить к работе.

## <a name="additional-technology"></a>Дополнительные технологии

Технологии, выделяются выше является примером доступных технологий. Другие важные включает следующие технологии:

- **Комплект Sprite** — Xamarin обеспечивает поддержку Apple Sprite Kit игры платформу, которая предоставляет доступ ко всем функциям собственного API. Поскольку Sprite пакета — это технология, созданная компанией Apple, он предоставляет тесная интеграция с остальной частью экосистемы операций ввода-вывода. Конечно, Sprite пакет не кросс платформенных, поэтому его нельзя использовать в Android. Дополнительные сведения об использовании пакета Sprite см. в этой записи блога:  [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Комплект средств для сцены** — Xamarin также обеспечивает поддержку Apple Kit сцены платформу, которая упрощает внедрение трехмерной графики в приложения iOS. Комплект сцены также является технология компании Apple, поэтому имеет интеграцию и выше для комплекта Sprite вопросы специфический для платформы. Дополнительные сведения о Kit сцены см. в этой записи блога: [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK —** OpenTK (что означает открыть набор средств) предоставляет низкоуровневые OpenGL доступ к Mac, iOS и Apple оборудования. Дополнительные сведения о OpenTK см. на главной странице:  [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>Сводка

В этой статье рассматриваются основные понятия разработки игр и предоставляет сведения о том, как начать создание первой игры. После завершения этой статьи следующие действия, чтобы выбрать технологии и приступить к работе посредством серии связаны с соответствующими разделами руководства.

## <a name="related-links"></a>Связанные ссылки

- [Направляющие CocosSharp](~/graphics-games/cocossharp/index.md)
- [Направляющие MonoGame](~/graphics-games/monogame/index.md)
- [Направляющие UrhoSharp](~/graphics-games/urhosharp/index.md)
