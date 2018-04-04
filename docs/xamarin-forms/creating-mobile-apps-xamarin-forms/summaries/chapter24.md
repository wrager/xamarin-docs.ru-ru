---
title: Сводка Глава 24. Переход по страницам
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d1a226a4532b745fddee28e943562fb51d34e65
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>Сводка Глава 24. Переход по страницам

Многие приложения состоят из нескольких страниц, между которыми переходе. Приложение всегда должно *основной* страницы или *домашней* страницы, и из него пользователь переходит на другие страницы, которые поддерживаются в стек для перехода назад. Дополнительные параметры, описанные в [ **25 главы. Страница диалекты**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Модальные страниц и страниц без режима

`VisualElement` Определяет [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) свойство типа [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), которая содержит следующие два метода для перехода на новую страницу:

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

Оба метода принимают `Page` экземпляр в качестве аргументов и возвращаемых `Task` объекта. Следующие два метода перехода на предыдущую страницу:

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

Если пользовательский интерфейс имеет свой собственный **обратно** кнопку (как и телефоны Android и Windows), то нет необходимости приложение может вызывать эти методы.

Несмотря на то, что эти методы доступны из любой `VisualElement`, обычно они вызываются из `Navigation` текущего элемента `Page` экземпляра.

Приложения обычно используют модальное страниц при пользователь должен предоставить некоторые сведения на этой странице перед возвратом к предыдущей странице. Страницы, которые не являются модальное иногда называются немодальное или *иерархические*. Ничего не на самой странице отличает его как модальные и немодальные; он управляет вместо него метод, используемый для перехода к нему. Для работы на всех платформах, модальное страницы необходимо предоставить свой собственный пользовательский интерфейс для перехода к предыдущей странице.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) пример позволяет исследовать разницу между страницами немодальное и модальное. Любое приложение, использующее навигацию по страницам необходимо передать его домашнюю страницу, чтобы [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) конструктор, обычно в этой программе `App` класса. Один премия — больше не требуется задать `Padding` страницы для операций ввода-вывода.

Можно узнать, что для немодального страницы, страницы [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) свойство отображается. IOS, Android и платформ планшетных и настольных компьютеров Windows укажите элемент пользовательского интерфейса для перехода на предыдущую страницу. Курс, Android и Windows phone устройства имеют стандартный **обратно** кнопку, чтобы вернуться назад.

Модальные страницы, страницы `Title` не отображается, и ни один элемент пользовательского интерфейса, предоставляемые для возврата на предыдущую страницу. Несмотря на то, что можно использовать стандартные Android и Windows phone **обратно** кнопку, чтобы вернуться на предыдущую страницу модальное страницы на других платформах необходимо предоставить собственный механизм, чтобы вернуться назад.

### <a name="animated-page-transitions"></a>Переходы анимацией

Альтернативные версии различные методы навигации, предоставляемых с второй логический аргумент, который задается для `true` Если требуется, чтобы переход страниц для включения анимации:

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

Тем не менее Стандартная навигацию по страницам перечислены методы, анимации по умолчанию, поэтому эти только для перехода к определенной странице при запуске (как описано в конце данной главы) или при предоставлении анимации входа (как описано в [ **Chapter22. Анимация**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Визуальные и функциональные изменения

`NavigationPage` содержит два свойства, которые можно задать при создании экземпляра класса в вашей `App` метод:

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` также содержит четыре вложенные привязываемые свойства, влияющие на определенную страницу, на котором они были заданы.

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) и [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) и [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) и [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) работа над только для iOS
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) и [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) работают в iOS и Android только

### <a name="exploring-the-mechanics"></a>Изучение механизм, позволяющий

Все асинхронные методы навигации страницы и должен использоваться с `await`. Завершение не указывает, что Навигация по страницам завершен, но только безопасность для просмотра стека навигацию по страницам.

Когда одна страница переходит на другой, первая страница обычно возвращает вызов его [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) метода, а вторая страница получает вызов, чтобы его [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) метод. Аналогичным образом, при возврате одной страницы в другую первая страница получает вызов его `OnDisappearing` метода, а вторая страница обычно получает вызов, чтобы его `OnAppearing` метод. Порядок этих вызовов (и завершения асинхронных методов, вызывающий навигации) не зависит от используемой платформы. Слово «обычно» в две предыдущие инструкции используется из-за Android modal-страницам, в котором эти вызовы метода не возникают.

Кроме того, вызовы `OnAppearing` и `OnDisappearing` методы не обязательно указывают навигацию по страницам.

`INavigation` Интерфейс включает в себя два свойства коллекции, которые позволяют проверить стек навигации:

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) Тип `IReadOnlyList<Page>` для немодального стека
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) Тип `IReadOnlyList<Page>` для модального стека

Доступ из этих стеков безопаснее `Navigation` свойство `NavigationPage` (должно быть `App` класса [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) свойство). Безопасно только для просмотра этих стеков после завершения методы асинхронной навигацию по страницам. [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) Свойство `NavigationPage` не указывает на текущей странице Если текущая страница является страницей модальное но указывает вместо последней страницы без режима.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) образец позволяет исследовать навигацию по страницам и стеки и переходах между страницами допустимые типы:

- Безрежимные страницу можно перейти к другой немодальное или модальное страницы
- Модальные страницу можно перейти только на другую страницу модального

### <a name="enforcing-modality"></a>Применение модальность

Приложение использует модальное страницы, при необходимости получить некоторые сведения от пользователя. Пользователь должен быть запрещен из возврат на предыдущую страницу, пока не предоставлены эти сведения. На iOS, очень легко предоставить **обратно** кнопку и включите ее только в том случае, когда пользователь закончил со страницей. Но для Android и Windows phone устройств, приложения должны переопределять [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) метода и возврата `true` Если программа обработала **обратно** кнопки, как показано в [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) образца.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) образце показано, как это работает в сценарии MVVM.

## <a name="navigation-variations"></a>Варианты навигации

Если модальные определенную страницу можно перейти несколько раз, он должен сохранить сведения, чтобы пользователь может изменять данные, а не вводить его на еще раз. Это позволяет обрабатывать, сохраняя определенный экземпляр модальное страницы, однако лучшим подходом (особенно на операций ввода-вывода) сохраняет данные в модель представления.

### <a name="making-a-navigation-menu"></a>Создание меню навигации

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) примере демонстрируется использование `TableView` для вывода списка элементов меню. Каждый элемент связан с `Type` объекта для отдельной страницы. При выборе этого пункта, программа создает страницы и переходит к нему.

[![Снимок экрана тройной типа представления коллекции](images/ch24fg21-small.png "пункты меню со списком TableView")](images/ch24fg21-large.png#lightbox "TableView вывод команды меню")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) образец будет немного отличаться в том, что меню содержит экземпляры каждой страницы, а не типы. Это позволяет сохранить данные из каждой страницы, но все страницы, которые должны создаваться при запуске программы.

### <a name="manipulating-the-navigation-stack"></a>Управление стеке навигации

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) показано несколько функций, определяемых `INavigation` , которые позволяют управлять стек навигации структурированных образом:

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) и [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) с необязательной анимации

### <a name="dynamic-page-generation"></a>Создание динамических страницы

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) образец демонстрирует создание страницы во время выполнения на основе ввода пользователя.

## <a name="patterns-of-data-transfer"></a>Шаблоны передачи данных

Часто бывает необходим для передачи данных между страницами &mdash; для передачи данных, навигацию страницы, а для страницы, чтобы вернуться на страницу, которая его вызвала данных. Существует несколько методов для выполнения этого.

### <a name="constructor-arguments"></a>Аргументы конструктора

При переходе на новую страницу, можно создать экземпляр класса страницы с аргументом конструктора, позволяющий страницы для инициализации самой себя. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) примере демонстрируется это. Можно также на странице навигацию, чтобы иметь его `BindingContext` задать по странице, который осуществляет переход к нему.

### <a name="properties-and-method-calls"></a>Вызовы методов и свойств

В остальных примерах передачи данных исследовать проблему передачи сведений между страницами, когда одна страница переходит на другую страницу и обратно. В этих обсуждениях *домашней* страница переходит к *сведения* страницы и необходимо передать инициализированный сведения для *сведения* страницы. *Сведения* страница получает Дополнительные сведения от пользователя и передает сведения для *домашней* страницы.

*Домашней* страницы можно легко получить открытые методы и свойства в *сведения* страницы, как только он создает этой страницы. *Сведения* страницы можно также получить доступ к открытые методы и свойства в *домашней* страницы, но выбрать подходящее время для этого может быть непростой задачей. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) пример делает это его `OnDisappearing` переопределения. Один недостаток заключается в, *сведения* необходимо знать тип страницу *домашней* страницы.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) класс предоставляет еще один способ две страницы для взаимодействия друг с другом. Сообщения идентифицируются текстовой строки и могут быть дополнены любого объекта.

Программы, который хочет получать сообщения от конкретного типа необходимо подписаться на них с помощью [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) и задайте функцию обратного вызова. Позже его можно отказаться от подписки, вызвав [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/). Функция обратного вызова получает все сообщения, отправленные из указанного типа с заданным именем, проходящие через [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) метод.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) показано, как передавать данные с помощью центра обмена сообщениями, но еще раз, это требует *сведения* страницы известен тип *домашней* страницы.

### <a name="events"></a>События

Событие организации подход для одного класса отправлять сведения в другой класс, не зная тип этого класса. В [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) пример *сведения* класс определяет событие, которое он применяется, когда информация готова. Однако есть не удобное место для *домашней* страницу, чтобы отделить обработчик событий.

### <a name="the-app-class-intermediary"></a>Класс посредника приложения

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) образце показано, как получить доступ к свойствам, заданным в `App` класс как *домашней* страницы и *сведения*страницы. Это является хорошим решением, но в следующем разделе описывается, что-нибудь лучше.

### <a name="switching-to-a-viewmodel"></a>Переключение на ViewModel

С помощью ViewModel сведения позволяют *домашней* страницы и *сведения* страницы для совместного использования экземпляра класса сведения. Это показано в [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) образца.

### <a name="saving-and-restoring-page-state"></a>Сохранение и восстановление состояния страницы

`App` Класс-посредник или подход ViewModel идеально подходит для приложения необходимо сохранить сведения, если программа переходит в спящий режим при *сведения* страница активна. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) примере демонстрируется это.

## <a name="saving-and-restoring-the-navigation-stack"></a>Сохранение и восстановление в стеке навигации

В общем случае многостраничных программу, которая переходит в спящий режим следует перейдите к той же странице при восстановлении. Это означает, что такой программы следует сохранить содержимое стека навигации. В этом разделе показано, как для автоматизации этого процесса в классе специально для этой цели. Этот класс также вызывает отдельных страниц, чтобы они могли сохранять и восстанавливать их состояние страницы.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека определяет интерфейс с именем [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) , классы могут реализовывать выполнить сохранение и восстановление элементов в `Properties`словарь.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Класса в **Xamarin.FormsBook.Toolkit** библиотека является производным от `Application`. Затем можно создать производные вашей `App` класса из `MultiPageRestorableApp` и выполнять некоторые вспомогательные.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) показано использование функции `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Что-то наподобие реальной жизни приложения

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) образец также используется `MultiPageRestorableApp` , что позволяет вводить и изменять заметок, хранящихся в `Properties` словаря.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 24 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Образцы Глава 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Модальные страницы](~/xamarin-forms/app-fundamentals/navigation/modal.md)
