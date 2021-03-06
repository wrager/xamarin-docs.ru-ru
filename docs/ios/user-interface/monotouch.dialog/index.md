---
title: Общие сведения о MonoTouch.Dialog для Xamarin.iOS
description: В этом документе описывается MonoTouch.Dialog (машинного перевода. D), платформа для быстрой, декларативной разработки пользовательского интерфейса с помощью Xamarin.iOS. В этом примере рассматривается использование MonoTouch.Dialog API-интерфейсы для создания интерфейса в коде или в JSON и использовать функции, например запросу на обновление, поиск, фоновую загрузку изображения и многое другое.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6511d8deed1800a8ae655f749feccd249bf4a8c0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790842"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Общие сведения о MonoTouch.Dialog для Xamarin.iOS

MonoTouch.Dialog, называют машинного перевода. D для краткости, является быстрой набор средств разработки пользовательского интерфейса, который позволяет разработчикам создавать экраны приложения и навигацию с помощью сведений, а не трудоемкость создания контроллеров представления, таблицы и т. д. Таким образом он обеспечивает значительное упрощение уменьшения разработки и код пользовательского интерфейса. Например рассмотрим следующий снимок экрана:

 [![](images/image1.png "Например рассмотрим следующий снимок экрана")](images/image1.png#lightbox)

Следующий код был использован для определения этого весь экран:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

При работе с таблицами в iOS, обычно имеется большой объем кода, то.
Например каждый раз при необходимости таблицы источника данных необходим для заполнения этой таблицы. В приложении, которое имеет два табличных экранов, подключенных через контроллер навигации каждый экран использует много того же кода.

МАШИННОГО ПЕРЕВОДА. D упрощает, инкапсулируя весь этот код в универсальном API для создания таблицы. Затем он предоставляет абстракции поверх этого API-интерфейса, позволяющим объект декларативный синтаксис, упрощающее даже привязки. Таким образом доступны два API в машинного перевода. D:

-   **Низкоуровневые элементы API** – *элементы API* основана на создании иерархическое дерево элементов, представляющих экраны и их компоненты. Элементы API предоставляет разработчикам наибольшую гибкость и управление в создании пользовательских интерфейсов. Кроме того элементы API с расширенным поддержка декларативного определения через JSON, позволяющий как объявление очень быстро, так и динамическое создание пользовательского интерфейса с сервера. 
-   **Высокоуровневый API отражения** — также называется *привязки**API* , в каких классах помечены атрибутом подсказки пользовательского интерфейса, а затем машинного перевода. D автоматически создает экраны в зависимости от объектов и обеспечивает привязку между что отображается (и при необходимости изменить) на экране, и у базового объекта резервного. В приведенном выше примере показано использование API-интерфейса отражения. Этот API не обеспечивают точное управление, выполняющий элементы API, однако он снижает сложность еще больше, автоматически построении иерархии элементов на основе атрибутов класс. 


МАШИННОГО ПЕРЕВОДА. D поставляется упакованный с большим набором встроенных элементов пользовательского интерфейса для создания экрана, но он также распознает потребность в настраиваемых элементов и макеты расширенный экран. Таким образом расширяемость является первого класса избранные готова еда к API. Разработчики могут расширить существующие элементы или создавать новые и полностью интегрироваться.

Кроме того машинного перевода. D имеет ряд общих функций UX операций ввода-вывода, встроенных в, такие как «запросу обновление» поддерживает асинхронной загрузки, изображения и поиск поддержки.

В этой статье будут рассмотрены комплексное по работе с машинного перевода. D, включая:

-   **МАШИННОГО ПЕРЕВОДА. Компоненты D** — это основное внимание уделяется Общие сведения о классах, которые составляют машинного перевода. D, чтобы включить получение быстро приступить к работе. 
-   **Справочник по элементам** — полный список встроенных элементов машинного перевода. Г. 
-   **Расширенное использование** — это рассматриваются дополнительные функции, например запросу на обновление, поиск, фоновую загрузку образа, с помощью LINQ, можно строить иерархии элементов и создание настраиваемых элементов ячейки, и контроллеров для использования с машинного перевода. Г. 

## <a name="understanding-the-pieces-of-mtd"></a>Основные сведения о части машинного перевода. D

Даже при использовании отражения API, машинного перевода. D создает иерархию элемент за кулисами, как если бы он были созданы напрямую через API элементов. Кроме того Поддержка JSON, упомянутых в предыдущем разделе создает элементы также. По этой причине важно иметь общее представление о составляющих элементов машинного перевода. Г.

МАШИННОГО ПЕРЕВОДА. D сборок экраны с использованием следующих четырех частей:

-  **DialogViewController**
-  **RootElement**
-  **Раздел**
-  **Элемент**


### <a name="dialogviewcontroller"></a>DialogViewController

Объект *DialogViewController*, или *DVC* сокращенно, наследует от `UITableViewController` и поэтому представляет экран с таблицей. DVCs можно отправить на контроллере навигации так же, как и обычный UITableViewController.

### <a name="rootelement"></a>RootElement

Объект *RootElement* является контейнером для элементов, входящих в DVC верхнего уровня. Он содержит подразделы, в которых может содержать элементы. RootElements не подготавливаются к просмотру; Вместо этого они просто контейнеров, что фактически возвращает отображены. RootElement назначается DVC, а затем DVC отображает свои дочерние элементы.

### <a name="section"></a>Раздел

Раздел — это группа ячеек в таблице. Как и с помощью раздела обычные таблицы, при необходимости верхний и нижний колонтитулы, могут быть текстом или даже пользовательские представления, как показано в следующем снимке экрана:

 [![](images/image2.png "Как и с помощью раздела обычные таблицы, при необходимости верхний и нижний колонтитулы, могут быть текстом или даже пользовательские представления, как показано на этом снимке экрана")](images/image2.png#lightbox)

### <a name="element"></a>Элемент

Элемент представляет фактический ячейку в таблице. МАШИННОГО ПЕРЕВОДА. D следует упакованный с самые разнообразные элементы, представляющие различные типы данных или разными вводными данными. Например следующих снимках экрана показано несколько доступных элементов:

 [![](images/image3.png "Например это снимки экрана показано несколько доступных элементов")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>В разделах и RootElements

Теперь рассмотрим RootElements и разделах более подробно.

### <a name="rootelements"></a>RootElements

Для запуска процесса MonoTouch.Dialog требуется по крайней мере один RootElement.

Если RootElement инициализируется со значением раздел или элемент, это значение используется для поиска дочерний элемент, который будет Сводная информация о конфигурации, которая отображается в правой части экрана. Например на следующем снимке экрана показана таблица слева с ячейку, содержащую название экран сведений в правой части «Десерты» вместе со значениями из выбранного desert.

 [![](images/image4.png "На этом снимке экрана показана таблица слева с ячейку, содержащую название экран сведений в правой части десерты, вместе со значениями из выбранного desert") ](images/image4.png#lightbox) [ ![ ] (images/image5.png "это Снимок экрана ниже показана таблица слева с ячейку, содержащую название экран сведений в правой части десерты, вместе со значениями из выбранного desert")](images/image5.png#lightbox)

Корневые элементы может также использоваться внутри разделов для запуска загрузки новой страницы вложенных конфигурации, как показано выше. При использовании в этом режиме указанный заголовок, используемых при подготовке к просмотру в раздел и также используется в качестве заголовка для вложенной страницы. Пример:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

В приведенном выше примере при нажатии кнопки на «Десерты» MonoTouch.Dialog будет создать новую страницу и перейти в эту папку с корневым элементом, «Десерты» и наличие группы переключателей с тремя значениями.

В данном конкретном примере группы переключателей выберите «Шоколадное Пирожное» в разделе «Десерты», поскольку мы передан RadioGroup значение «2». Это означает, что выбор 3-й элемент в списке (нулевой индекс).

Вызов метода Add или с помощью синтаксиса инициализатора 4 C# добавляет разделы.
Для вставки разделов с хотя бы одна анимация предоставляются методы вставки.

При создании RootElement с экземпляром группы (вместо RadioGroup) суммарного значения RootElement при отображении в раздел будет совокупного количества всех BooleanElements и CheckboxElements, имеют один и тот же ключ как значение Group.Key.

### <a name="sections"></a>Разделы

Разделы используются для группы элементов на экране, которые допускаются только прямые потомки RootElement. Разделы могут содержать любой из стандартных элементов, включая новый RootElements.

Для перехода к более глубоким используются RootElements, внедренных в разделе.

Разделы может иметь верхние и нижние колонтитулы в виде строк или UIViews.
Как правило, используются только строки, но для создания специальных интерфейсов можно использовать любой UIView как верхний или нижний колонтитул. Либо можно использовать строку для создания их следующим образом:

```csharp
var section = new Section ("Header", "Footer")
```

Для использования представлений, просто передайте конструктору представлений:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Получение уведомления

#### <a name="handling-nsaction"></a>Обработка NSAction

МАШИННОГО ПЕРЕВОДА. Предоставляет доступ к D `NSAction` качестве делегата для обработки обратных вызовов.
Например предположим, что необходимо обработать это событие сенсорного ввода для ячейки таблицы, созданные машинного перевода. Г. При создании элемента с помощью машинного перевода. D, просто введите функцию обратного вызова, как показано ниже:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Извлечение значения элемента

В сочетании с `Element.Value` свойство, обратный вызов можно получить значение, заданное в других элементах. Рассмотрим следующий пример кода:

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

Этот код создает пользовательский Интерфейс, как показано ниже. Полное Пошаговое руководство этого примера см. в разделе [пошагового руководства по API элементы](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) учебника.

 [![](images/image6.png "В сочетании со свойством Element.Value обратного вызова можно получить значение, заданное в других элементах")](images/image6.png#lightbox)

Когда пользователь нажимает нижнюю ячейку таблицы, выполняет код в анонимной функции, записи значения из `element` экземпляр **выходных данных приложения** pad в Visual Studio для Mac.

## <a name="built-in-elements"></a>Встроенные элементы

МАШИННОГО ПЕРЕВОДА. D поставляется с числом элементов ячейки встроенные таблицы, известные как элементы.
Эти элементы используются для отображения в ячейках таблицы, такие как строки, значений с плавающей запятой, дат и даже изображения, чтобы среди прочих разнообразных типов. Каждый элемент отвечает за отображение соответствующим образом тип данных. К примеру логический элемент будет отображаться переключатель, чтобы переключить его значение. Аналогично элемент float отобразит ползунок, чтобы изменить значение с плавающей запятой.

Существует еще более сложных элементов для поддержки более подробная типов данных, таких как изображения и html. Например HTML-элемент, что откроет UIWebView для загрузки веб-страницы при выборе отображается заголовок в ячейке таблицы.

### <a name="working-with-element-values"></a>Работа со значениями, элемент

Элементы, которые применяются для сбора входных данных пользователя предоставлять открытое `Value` свойство, которое содержит текущее значение элемента в любое время. Она автоматически обновляется, как пользователь использует приложение.

Это поведение для всех элементов, которые являются частью MonoTouch.Dialog, но не является обязательным для элементов, созданных пользователем.

### <a name="string-element"></a>Элемент строки

Объект `StringElement` показывает заголовок левой ячейке таблицы и строковое значение справа от ячейки.

 [![](images/image7.png "StringElement показывает заголовок левой ячейке таблицы и строковое значение справа от ячейки")](images/image7.png#lightbox)

Для использования `StringElement` как кнопка, предоставить делегат.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Чтобы использовать StringElement как кнопки, предоставить делегат")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Элемент стиля строки

Объект `StyledStringElement` позволяет строки должны быть представлены с помощью либо стили ячеек таблицы, встроенные или настраиваемые параметры форматирования.

 [![](images/image9.png "StyledStringElement позволяет строки должны быть представлены с помощью либо стили ячеек встроенных таблиц или пользовательский формат")](images/image9.png#lightbox)

`StyledStringElement` Класс является производным от `StringElement`, но позволяет разработчикам настроить небольшое число свойств, таких как шрифт, цвет текста, цвет фона ячеек, режим переноса строки, количество строк для отображения, и следует ли отображать стандартную программу.

### <a name="multiline-element"></a>Многострочный элемент

 [![](images/image10.png "Многострочный элемент")](images/image10.png#lightbox)

### <a name="entry-element"></a>Элемент

`EntryElement`, Как имя подразумевает, используется для получения ввода пользователя. Он поддерживает обычные строки или пароли, где скрытые символы.

 [![](images/image11.png "EntryElement используется для получения ввода пользователя")](images/image11.png#lightbox)

Он инициализируется с помощью трех значений:

-  Заголовок для записи, которая будет отображаться для пользователя.
-  Текст заполнителя (это серым out текст, который предоставляет подсказку для пользователя). 
-  Значение текста.


Заполнитель и значением может быть null. Однако заголовок является обязательным.

В любой момент обращение к свойству его значение можно получить значение `EntryElement`.

Кроме того `KeyboardType` может быть установлено в момент создания стиль типа клавиатуры, требуемого для ввода данных. Это можно использовать для настройки клавиатуры с использованием значения `UIKeyboardType` приведенные ниже:

-  Numeric
-  Номер телефона
-  URL-адрес
-  Адрес эл. почты


### <a name="boolean-element"></a>Логический элемент

 [![](images/image12.png "Логический элемент")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>CheckBox-элемент

 [![](images/image13.png "CheckBox-элемент")](images/image13.png#lightbox)

### <a name="radio-element"></a>Элемент переключатель

Объект `RadioElement` требует `RadioGroup` в `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "RadioElement требует RadioGroup должен быть задан в RootElement")](images/image14.png#lightbox)

 `RootElements` также используется для координации переключатель элементов. `RadioElement` Членов может охватывать несколько разделов (например для реализации примерно селектор тон кольцо и отдельные пользовательские мелодии из системы мелодии звонка). Представление "Сводка" будет показано переключатель элемента, выбранного в данный момент. Для этого создайте `RootElement` с помощью конструктора группы, следующим образом:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Имя группы в `RadioGroup` используется для отображения значения, выбранного в странице (если таковые имеются) и его значение, равное нулю, в этом случае, — это индекс первого выбранного элемента.

### <a name="badge-element"></a>Элемент эмблемы

 [![](images/image15.png "Элемент эмблемы")](images/image15.png#lightbox)

### <a name="float-element"></a>Float-элемент

 [![](images/image16.png "Float-элемент")](images/image16.png#lightbox)

### <a name="activity-element"></a>Элемент действия

 [![](images/image17.png "Элемент действия")](images/image17.png#lightbox)

### <a name="date-element"></a>Элемент даты

 ![](images/image18.png "Элемент даты")

При выборе ячейку, соответствующую DateElement элемент выбора даты представляется, как показано ниже:

 [![](images/image19.png "При выборе ячейку, соответствующую DateElement элемент выбора даты представляется, как показано")](images/image19.png#lightbox)

### <a name="time-element"></a>Элемент Time

 [![](images/image20.png "Элемент Time")](images/image20.png#lightbox)

При выборе ячейку, соответствующую TimeElement Выбор времени представляется, как показано ниже:

 [![](images/image21.png "При выборе ячейку, соответствующую TimeElement Выбор времени представляется, как показано")](images/image21.png#lightbox)

### <a name="datetime-element"></a>Элемент даты и времени

 [![](images/image22.png "Элемент даты и времени")](images/image22.png#lightbox)

При выборе ячейку, соответствующую DateTimeElement элемент выбора даты и времени представляется, как показано ниже:

 [![](images/image23.png "При выборе ячейку, соответствующую DateTimeElement представлен элемент выбора даты и времени, как показано")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML-элемент

 [![](images/image24.png "HTML-элемент")](images/image24.png#lightbox)

`HTMLElement` Отображает значение его `Caption` свойство ячейки таблицы. Флажок установлен, где `Url` элементу загружается в `UIWebView` управления, как показано ниже:

 [![](images/image25.png "URL-адрес, назначенный элементу загружается в элемент управления UIWebView, как показано ниже, где выбран")](images/image25.png#lightbox)

### <a name="message-element"></a>Элемент Message

 [![](images/image26.png "Элемент Message")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Загрузить дополнительные элемент

Этот элемент используется, чтобы разрешить пользователям загружать дополнительные элементы в списке. Вы можете настроить нормали и заголовки для загрузки, а также шрифт и цвет текста.
`UIActivity` Индикатор запускает анимации и подпись загрузки отображается в том случае, когда пользователь касается ячейки, а затем `NSAction` переданные в конструктор выполняется. Один раз в коде `NSAction` завершения работы, `UIActivity` индикатор останавливает анимации и снова отобразится обычного заголовка.

### <a name="uiview-element"></a>Элемент UIView

Кроме того, настраиваемых `UIView` могут отображаться с использованием `UIViewElement`.

### <a name="owner-drawn-element"></a>Определяемые владельцем элемента

Этот элемент должен быть подклассом, как он является абстрактным классом. Необходимо переопределить `Height(RectangleF bounds)` метода, в котором следует вернуть высоту элемента, а также `Draw(RectangleF bounds, CGContext context, UIView view)` в которой вам следует в настраиваемый документ в пределах заданного с помощью параметров контекста и представления. Этот элемент осуществляет трудную работу для создания подкласса `UIView`и поместите его в ячейке, должно быть возвращено останется только при необходимости реализовывать два простых overrides. Вы увидите оптимальной реализации образца в пример приложения `DemoOwnerDrawnElement.cs` файла.

Ниже приведен очень простой пример реализации этого класса:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
 }
```

### <a name="json-element"></a>Элемент JSON

`JsonElement` Является подклассом `RootElement` , расширяющий `RootElement` для загрузки содержимого вложенные дочерние локальный или удаленный URL-адресу.

`JsonElement` — `RootElement` , Может быть создан в двух формах. Создает одну версию `RootElement` , будет загружать содержимое по запросу. Эти отчеты создаются с помощью `JsonElement` конструкторы, принимающие дополнительный аргумент в конце URL-адрес для загрузки содержимого из:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Другая форма создает данные из локального файла или существующий `System.Json.JsonObject` , уже анализа:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Дополнительные сведения об использовании JSON с помощью машинного перевода. D, в разделе [пошаговом руководстве элемент JSON](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) учебника.

## <a name="other-features"></a>Другие функции

### <a name="pull-to-refresh-support"></a>Поддержка по запросу на обновление

 *По запросу-в-* *обновление* визуальный эффект изначально находится в *Tweetie2* приложение, которое становится популярным эффект для многих приложений.

Чтобы добавить поддержку автоматического запросу, чтобы обновить свои диалоги, необходимо только сделать две вещи: подключить обработчик событий, чтобы получать уведомления, когда пользователь запрашивает данные и уведомите службу `DialogViewController` после загрузки данных для возврата в состояние по умолчанию.

Подключение уведомление простой; Подключитесь к `RefreshRequested` события `DialogViewController`, следующим образом:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Затем в методе `OnUserRequestedRefresh`, будет очередь некоторые загрузка данных, некоторые данные запроса от net или запустить поток для вычисления данных. После загрузки данных необходимо уведомить `DialogViewController` , новые данные в, и чтобы восстановить представление в состояние по умолчанию, это делается путем вызова `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Поддержка поиска

Для поддержки поиска, задать `EnableSearch` свойство вашей `DialogViewController`. Можно также задать `SearchPlaceholder` свойство для использования в качестве текста водяного знака в строке поиска.

Поиск будет изменять содержимое представления как пользовательские типы. Он выполняет видимых полей и показывает их пользователю. `DialogViewController` Предоставляет три способа программно инициировать, окончание или активировать новую операцию фильтра на результаты. Эти методы перечислены ниже.

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Система является расширяемой, поэтому это поведение можно изменить, если требуется.

### <a name="background-image-loading"></a>Фон образа загрузки

Включает в себя MonoTouch.Dialog [TweetStation](https://github.com/migueldeicaza/TweetStation) загрузчик изображений приложения. Этот загрузчик изображений можно использовать для загрузки изображений в фоновом режиме, поддерживает кэширование и может уведомлять кода загружено изображение.

Он также будет ограничивать число исходящих сетевых соединений.

Загрузчик изображений реализована в `ImageLoader` класса, все, что нужно сделать — вызов `DefaultRequestImage` метод, необходимо будет предоставить Uri для изображения, которую требуется загрузить, а также экземпляр `IImageUpdated` интерфейс, который будет вызывается при изображения высокой доступности s был загружен.

Например следующий код загружает изображение из URL-адреса в `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

Класс ImageLoader предоставляет метод очистки, который можно вызвать, чтобы освободить все образы, которые в настоящее время кэшированных в памяти. Текущий код имеет кэш для 50 изображений. Если вы хотите использовать размер отдельного кэша (например, если вы предполагаете, изображения, слишком велико, что 50 изображения должно быть слишком много), можно просто создавать экземпляры ImageLoader и передать число образов, которые будут храниться в кэше.

## <a name="using-linq-to-create-element-hierarchy"></a>Использование LINQ для создания иерархии элементов

Через правильно использовать синтаксис инициализации LINQ и C# LINQ можно использовать для создания иерархии элемента. Например, следующий код создает экрана из некоторых массивов строк и дескрипторов ячейки выбора через анонимная функция, которая передается в каждую `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

Это легко может объединяться с хранения XML-данных или данные из базы данных, для создания сложных приложений почти полностью из данных.

## <a name="extending-mtd"></a>Расширение машинного перевода. D

### <a name="creating-custom-elements"></a>Создание настраиваемых элементов

Можно создать собственный элемент путем наследования из любого существующего элемента или путем наследования от класса корневого элемента.

Чтобы создать собственный элемент, необходимо переопределить следующие методы:

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

Если элемент может иметь переменный размер, необходимо реализовать `IElementSizing` интерфейс, который содержит один метод:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Если вы планируете для реализации вашей `GetCell` метод путем вызова `base.GetCell(tv)` и настройка возвращенную ячейку, необходимо также переопределить `CellKey` ключ, который является уникальной для элемента, возвращаемого свойства следующим образом:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Это работает для большинства элементов, но не для `StringElement` и `StyledStringElement` как их использовать собственные наборы ключей для различных сценариев подготовки к просмотру. Необходимо реплицировать код в этих классах.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Отражение и элементы API использовать тот же `DialogViewController`. Иногда требуется настроить внешний вид представления или использования некоторых возможностей может потребоваться `UITableViewController` выходят за рамки базовых Создание пользовательских интерфейсов.

`DialogViewController` Просто является подклассом `UITableViewController` и можно настроить его таким же образом, при настройке `UITableViewController`.

Например, если нужно изменить стиль списка должен быть `Grouped` или `Plain`, можно задать это значение, изменив свойство при создании контроллера, следующим образом:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Для создания расширенных настроек `DialogViewController`, например установку фоне, будет подкласс его и переопределить соответствующих методов, как показано в следующем примере:

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Другим аспектом настройки является следующие виртуальные методы `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Этот метод должен возвращать подкласс `DialogViewController.Source` для случаев, где к ячейкам одинакового размера или подкласс `DialogViewController.SizingSource` Если неравные к ячейкам.

Это переопределение позволяет выделить `UITableViewSource` методы. Например [TweetStation](https://github.com/migueldeicaza/TweetStation) использует его для отслеживания, когда пользователь прокручивает наверх и соответствующим образом обновите количество непрочитанных твиты.

## <a name="validation"></a>Проверка

Элементы не имеют проверки себя как модели, которые хорошо подходят для веб-страниц и приложений для настольных систем не сопоставляются непосредственно модель взаимодействия iPhone.

Если вы хотите выполнить проверку данных, это следует сделать при пользователь инициирует действие с данными, введенными. Например <span class="ui">сделать</span> или <span class="ui">Далее</span> кнопки в верхней части панели инструментов и некоторые `StringElement` использовать в качестве кнопки для перехода к следующему этапу.

Это следует выполнить основные проверки входных данных, куда может быть более сложным проверки, такие как проверка для действия пользователя и пароля с сервером.

Способ уведомления пользователя об ошибке, зависит от приложения. Удалось всплывающие `UIAlertView` или показывать подсказку.

## <a name="summary"></a>Сводка

В этой статье рассматривается множество сведений о MonoTouch.Dialog. Это обсуждалось принципы машинного перевода. D работает и описаны различные компоненты, составляющие машинного перевода. Г. Он также показали широкий набор элементов и настроек таблицы, поддерживаемых машинного перевода. D и рассматривается как машинного перевода. D могут быть расширены с помощью настраиваемых элементов. Кроме того, оно описано поддержке JSON в машинного перевода. D, который позволяет динамически создавать элементы из JSON.


## <a name="related-links"></a>Связанные ссылки

- [Демонстрация - Icaza de Мигель создает экран входа iOS с MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Демонстрация - позволяет быстро создавать пользовательские интерфейсы iOS с MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Пошаговое руководство. Создание приложения с помощью API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство. Создание приложения с помощью API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Пошаговое руководство. Использование элемента JSON для создания пользовательского интерфейса](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Разметка MonoTouch.Dialog JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Диалоговое окно MonoTouch на Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Ссылку на класс UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Ссылку на класс UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
