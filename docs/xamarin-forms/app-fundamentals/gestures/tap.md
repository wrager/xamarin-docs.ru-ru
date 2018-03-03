---
title: "Добавление распознавания жестов касания"
description: "Используется для обнаружения tap жеста касания и реализуется с помощью класса TapGestureRecognizer."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: d767b50d98b88e6b97a07caffcc103c70cfda428
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Добавление распознавания жестов касания

_Используется для обнаружения tap жеста касания и реализуется с помощью класса TapGestureRecognizer._

## <a name="overview"></a>Обзор

Чтобы сделать элемент пользовательского интерфейса активную с жеста касания, создать [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) экземпляра, обрабатывать [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) событий и добавить новый распознаватель жестов для [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) коллекции элемента пользовательского интерфейса. В следующем примере кода показан `TapGestureRecognizer` присоединяется к [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

По умолчанию изображение будет реагировать на одном касания. Задать [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) свойство ожидать дважды щелкните (или более касания при необходимости).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Когда [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) имеет значение выше, обработчик событий будет выполняться только, если касания происходят в установленный период времени (этот период не подлежат настройке). Если второй (или последующей) касания не происходят в течение этого периода они фактически обрабатываются и перезапускает «подсчета tap».

<a name="Using_Xaml" />

## <a name="using-xaml"></a>С помощью Xaml

Распознаватель жестов можно добавить к элементу управления в Xaml, с помощью вложенных свойств. Синтаксис для добавления [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) изображения, ниже приведен (в данном случае определение *двойного касания* событий):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Код обработчика событий (в этом образце) увеличивает на единицу счетчик и изменяет изображение из цвета черный &amp; белый.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>С помощью ICommand

Приложения, использующие шаблон Mvvm обычно используют `ICommand` вместо подключения напрямую обработчиков событий. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Можно легко поддерживать `ICommand` либо задав привязку в коде:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

или с помощью Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Полный код для этого представления модели можно найти в образце. Соответствующий `Command` ниже приведены сведения о реализации:

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## <a name="summary"></a>Сводка

Используется для обнаружения tap жеста касания и реализуется с помощью [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) класса. — Числом ответвлений можно указать для распознавания дважды щелкните (или троекратного нажатия или более касается) поведение.


## <a name="related-links"></a>Связанные ссылки

- [TapGesture (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
