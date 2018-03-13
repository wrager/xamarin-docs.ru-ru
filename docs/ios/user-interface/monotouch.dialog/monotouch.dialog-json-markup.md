---
title: "Разметка MonoTouch.Dialog Json"
ms.topic: article
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 843e66a7979fc1aaa86371a3406c89af3f9ba967
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="monotouchdialog-json-markup"></a>Разметка MonoTouch.Dialog Json

На этой странице описаны разметки Json, принимаемое по MonoTouch.Dialog [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Сообщите нам Запустите пример. Ниже приведен полный Json-файл, который может быть передан в JsonElement.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

Выше разметки выводятся следующие пользовательского интерфейса:

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "Пользовательский Интерфейс, созданные данной разметки")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Каждый элемент в дерево может содержать свойство `"id"`. Это возможно, во время выполнения для ссылки на отдельные разделы или элементов с помощью индексатора JsonElement. Пример:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>Корневой элемент синтаксиса

Корневой элемент содержит следующие значения:

-  `title`
-  `sections` (необязательно)


Корневой элемент может использоваться в теле раздела как элемент для создания вложенных контроллера. В этом случае дополнительное свойство `"type"` должно быть равно `"root"`

 <a name="url" />


### <a name="url"></a>url

Если `"url"` свойство имеет значение, если пользователь нажимает на этом RootElement, код запроса файла с указанным URL-адрес и сделать содержимое отображаются новые данные. Это можно использовать для создания расширения пользовательского интерфейса из сервера на основании нажатии кнопки.

 <a name="group" />


### <a name="group"></a>группа

Если задано, это задает groupname для корневого элемента. Имена групп используются для выбора сводку, отображаемый в качестве значения корневого элемента один из вложенных элементов в элементе. Это значение checkbox или значение типа "переключатель".

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

Идентифицирует переключатель элемента, выбранного в вложенных элементов

 <a name="title" />


### <a name="title"></a>заголовок

Если присутствует, оно будет иметь заголовок, используемый для RootElement

 <a name="type" />


### <a name="type"></a>type

Должно быть присвоено `"root"` при появлении этого раздела (используется для вложения нескольких контроллеров).

 <a name="sections" />


### <a name="sections"></a>разделы

Это массив Json с помощью отдельных разделов

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Синтаксис раздела

Раздел содержит:

-  `header` (необязательно)
-  `footer` (необязательно)
-  Массив `elements`


 <a name="header" />


### <a name="header"></a>заголовок

Если он имеется, текст заголовка отображается как заголовок раздела.

 <a name="footer" />


### <a name="footer"></a>Нижний колонтитул

Если он имеется, нижний колонтитул отображается в нижней части раздела.

 <a name="elements" />


### <a name="elements"></a>элементы

Это массив элементов. Каждый элемент должен содержать по крайней мере один ключ `"type"` ключ, используемый для определения вида элемента, который требуется создать.
Некоторые элементы совместно используют некоторые общие свойства, такие как `"caption"` и `"value"`. Это список поддерживаемых элементов:

-  `string` элементы (как с и без задания стиля)
-  `entry` строки (обычной или пароль)
-  `boolean` значения (с помощью параметров или изображений)


Строка элементы могут использоваться как кнопки, предоставляя метод для вызова, когда пользователь нажимает на ячейки или аксессуары,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Подготовка к просмотру элементов

Отрисовке элементов основаны на C# StringElement и StyledStringElement и отображении сведения различными способами и имеется возможность отображать их различными способами. Простейший элементы могут создаваться следующим образом:

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

При этом будут отображены простую строку с все значения по умолчанию: шрифт, фон, цвет текста и оформление. Можно подключить действий для этих элементов и, чтобы они ведут себя как кнопки, задав `"ontap"` свойство или `"onaccessorytap"` свойства:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Выше будет вызывать метод «ShowPhotos» в классе «Acme.PhotoLibrary». `"onaccessorytap"` Выполняется аналогично, но он будет вызываться только если пользователь нажимает на стандартную программу, вместо касания в ячейке. Чтобы включить эту возможность, необходимо также задать метод:

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Подготовка к просмотру элементы могут отображаться две строки одновременно, он заголовок, а другая — значение. О том, как подготавливаются к просмотру эти строки зависит от стиля, это можно задать с помощью `"style"` свойство. Значение по умолчанию будет отображаться заголовок слева, а значение справа. Стиль для получения дополнительных сведений обратитесь к разделу. Цвета кодируются с помощью символа «#», и шестнадцатеричных цифр, представляющих значения для значений красного, зеленого, синего и может быть альфа-канала. Содержимое можно закодировать в краткая форма (3 или 4 шестнадцатеричных цифр), которое представляет значения RGBA или RGB. Или длинная форма (6 или 8 цифр), которые представляют значения RGB или RGBA. Сокращенная версия является сокращением для записи же шестнадцатеричную цифру дважды. Поэтому значение константы «#1bc» равно результате красным цветом = 0x11 зеленый = 0xbb и синий = 0xcc. Если отсутствует значение альфа, цвета непрозрачен. Ниже представлено несколько примеров.

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>Украшение

Определяет тип стандартную программу, отображаемых в ваш элемент отрисовки возможные значения:

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Если значение не существует, отображается не стандартную программу

 <a name="background" />


### <a name="background"></a>фон

Свойство фона задает цвет фона для ячейки. Значение является либо URL-адрес изображения (в этом случае будет вызываться загрузчик изображений async и фон будет обновлен после загрузки образа) или может быть указан с помощью синтаксиса цвет цвет.

 <a name="caption" />


### <a name="caption"></a>Заголовок

Основной строка будет отображаться в элементе подготовки к просмотру. Шрифт и цвет можно настроить, задав `"textcolor"` и `"font"` свойства. Определяется стиля отрисовки `"style"` свойство.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>цвет и detailcolor

Цвет, который будет использоваться для основного текста или подробный текст.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont и шрифта

Шрифт, используемый для заголовок или текст сведений. Формат спецификации шрифта — имя шрифта, при необходимости следуют дефис и размер шрифта.
Ниже приведены спецификации допустимый шрифт.

-  «Helvetica»
-  «Helvetica 14»


 <a name="linebreak" />


### <a name="linebreak"></a>LineBreak

Определяет, как разбить строки. Допустимые значения:

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


Оба `character-wrap` и `word-wrap` может использоваться вместе с `"lines"` установлено равным нулю, чтобы включить элемент отрисовки в элемент многострочного текста.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>OnTap и onaccessorytap

Эти свойства должен указывать имя статического метода в приложении, которое принимает объект в качестве параметра. При создании иерархии с помощью методов JsonDialog.FromFile или JsonDialog.FromJson можно передать необязательный объект значению. Значение этого объекта затем передается в методы. Это можно использовать для передачи какой-либо контекст статический метод. Пример:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>линии

Если задано равным нулю, это сделает размер автоматического элемента в зависимости от содержимого строк, содержащихся. Чтобы это работало, необходимо также задать `"linebreak"` свойства `"character-wrap"` или `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>стиль

Стиль определяет тип стиль ячейки, который будет использоваться для отображения содержимого и они соответствуют значениям перечисления UITableViewCellStyle.
Допустимые значения:

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : текст с подзаголовок.


 <a name="subtitle" />


### <a name="subtitle"></a>Подзаголовок

Значение, используемое для подзаголовка. Это представляет собой ярлык, чтобы задать стиль `"subtitle"` и задайте `"value"` строковое значение.
При этом произойдет и с одной записью.

 <a name="textcolor" />


### <a name="textcolor"></a>textcolor

Цвет, используемый для текста.

 <a name="value" />


### <a name="value"></a>value

Вторичный значения, которое будет отображаться в элементе подготовки к просмотру. Макет этого затрагивается `"style"` параметр. Шрифт и цвет можно настроить, задав `"detailfont"` и `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Логические элементы

Логических элементов следует выбрать тип `"bool"`, может содержать `"caption"` для отображения и `"value"` задано значение true или false. Если `"on"` и `"off"` свойств, они считаются образами. Изображения, разрешаются относительно текущего рабочего каталога приложения. Если вы хотите создать ссылки на файлы относительно пакета, можно использовать `"~"` как ярлык для представления каталога пакета приложения. Например `"~/favorite.png"` будет favorite.png, который содержится в файле пакета. Пример:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>type

Тип может быть задан либо `"boolean"` или `"checkbox"`. Если значение в логическое значение будет использоваться UISlider или изображений (если оба `"on"` и `"off"` заданы). Если значение checkbox, будет использоваться флажок. `"group"` Свойство можно использовать для маркировки на логический элемент как относящиеся к определенной группе. Это полезно, если содержащий корень также имеет `"group"` свойства, который является корнем будут использованы Помесячные результаты, относящиеся к той же группе с количеством все логические значения (или флажки).

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Запись элементов

Используйте элементы запись позволяет пользователю ввести данные. Тип для элементов записи — `"entry"` или `"password"`. `"caption"` Является свойство текст для отображения в правой части и `"value"` присвоено начальное значение для значение записи. `"placeholder"` Используется для отображения подсказку пользователю по пустым записям (она отображается неактивные). Далее приводятся некоторые примеры.

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>автозамены

Определяет стиль автоматического исправления, используемый для записи. Возможные значения: true или false (или строки `"yes"` и `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>написание прописными буквами

Регистр букв стиль, используемый для записи. Допустимые значения:

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>Заголовок

Текст, который используется для записи

 <a name="keyboard" />


### <a name="keyboard"></a>клавиатура

Тип клавиатуры для ввода данных. Допустимые значения:

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>Заполнитель

Пояснительный текст, который отображается, если операция имеет пустое значение.

 <a name="return-key" />


### <a name="return-key"></a>Ключ возвращаемого

Метка, используемая для возврата ключа. Допустимые значения:

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>value

Начальное значение для записи

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>Элементы переключателей

Переключатель элементы имеют тип `"radio"`. Выбран элемент выбран по `radioselected` свойства на его корневой элемент.
Кроме того Если значение задано для `"group"` , этот переключатель, к которому относится свойство этой группы.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Дата и время элементы

Типы элементов `"datetime"`, `"date"` и `"time"` используются для отображения дат, времени, даты или времени. Эти элементы принимают в качестве параметров с заголовком и значение. Значение может быть написан на любом формате, поддерживаемом функцией .NET DateTime.Parse. Пример

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>HTML и веб-элемент

Можно создать ячейки, при касании внедрит UIWebView, отображающий содержимое указанного URL-адреса локальных или удаленных с использованием `"html"` типа. Только два свойства для этого элемента, `"caption"` и `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
