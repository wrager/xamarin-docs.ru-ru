---
title: "Контакты и ContactsUI"
description: "В этой статье описывается работа с новых контактов и контактов пользовательского интерфейса платформы в приложения Xamarin.iOS. Эти платформы заменить существующий адресной книги и пользовательского интерфейса книги адрес, используемый в предыдущих версиях iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 996723db83a1f972cce26090d1253f97b6c818d3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="contacts-and-contactsui"></a>Контакты и ContactsUI

_В этой статье описывается работа с новых контактов и контактов пользовательского интерфейса платформы в приложения Xamarin.iOS. Эти платформы заменить существующий адресной книги и пользовательского интерфейса книги адрес, используемый в предыдущих версиях iOS._

С появлением iOS 9, Apple выпустила две новые платформы, `Contacts` и `ContactsUI`, что замена существующего адресная книга и инфраструктур пользовательского интерфейса адресной книги, используемые iOS 8 и более ранних версий.

Две новые платформы содержат следующие функциональные возможности:

- [**Контакты** ](#contacts) -предоставляет доступ к данным списка контактов пользователя.
    Так как большинство приложений требуется только доступ только для чтения, эта платформа была оптимизирована для доступ к потокам безопасный, только для чтения.

- [**ContactsUI** ](#contactsui) -предоставляет Xamarin.iOS элементы пользовательского интерфейса для отображения, редактирования, выбрать и создать контакты на устройствах iOS.

[![](contacts-images/add01.png "Пример листа контакта на устройстве iOS")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> **Примечание:** существующий `AddressBook` и `AddressBookUI` платформы используется iOS 8 (или более ранних) являются устаревшими в iOS 9 и должна быть заменена новой `Contacts` и `ContactsUI` платформы как можно быстрее для любой существующей Xamarin.iOS приложение. Новые приложения должны работать с новым платформам.




В следующих разделах мы будем рассмотрим эти новые платформы и способы их реализации в приложении Xamarin.iOS.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Платформа контактов

Контакты Framework предоставляет доступ Xamarin.iOS контактные сведения пользователя. Так как большинство приложений требуется только доступ только для чтения, эта платформа была оптимизирована для доступ к потокам безопасный, только для чтения.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Объекты-контакты

`CNContact` Класс предоставляет доступ безопасный, только для чтения поток для свойства контакта, такие как имена, адреса и номера телефонов. `CNContact` функции, например `NSDictionary` и содержит несколько коллекций только для чтения свойства (например, адреса и номера телефонов):

[![](contacts-images/contactobjects.png "Общие сведения о связи объектов")](contacts-images/contactobjects.png#lightbox)

Для любого свойства, которое может иметь несколько значений (например, электронной почты адресу или номерам телефонов), они будут представлены в виде массива `NSLabeledValue` объектов. `NSLabeledValue` представляет собой безопасный кортеж потока, состоящим из ряда меток только для чтения и значения, где метка Определяет значение для пользователя (например домашней или работа электронной почты). Контакты framework предоставляет набор предопределенных метки (через `CNLabelKey` и `CNLabelPhoneNumberKey` статические классы), можно использовать в приложении, или у вас есть возможность определить пользовательские метки для потребностей.

Для любого приложения Xamarin.iOS, необходимо настроить параметры существующего контакта (или создать новые шаблоны), используйте `NSMutableContact` версии класса и его вложенные классы (такие как `CNMutablePostalAddress`).

Например следующий код создания нового контакта и добавить его в коллекцию пользователей, контактов:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Если этот код выполняется на устройстве с iOS 9, новый контакт будет добавлен в коллекцию пользователей. Пример:

[![](contacts-images/add01.png "Новый контакт, добавляемый в коллекцию пользователя")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Обратитесь в службу форматирование и локализация

Платформа контактов содержит несколько объектов и методы, которые помогут вам форматирования и локализации содержимое для отображения пользователю. Например следующий код будет правильно отформатировать название контакты организации и почтовый адрес для отображения:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Для свойства метки, которые будут отображаться в пользовательском Интерфейсе приложения framework контакт имеет методы для локализации эти строки также. Опять же основывается на текущий языковой стандарт приложение выполняется на устройстве iOS. Пример:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOption.Nickname));
Console.WriteLine(CNLabeledValue.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Извлечение существующих контактов

С помощью экземпляра `CNContactStore` класса, можно получить контактные данные из базы данных контактов пользователя. `CNContactStore` Содержит все методы, необходимые для выборки или обновления, контакты и группы из базы данных. Так как эти методы являются синхронными, рекомендуется выполнять в фоновом потоке, чтобы избежать блокировки пользовательского интерфейса.

С помощью предикатов (построенная `CNContact` класса), можно отфильтровать результаты, возвращаемые при их получении контакты из базы данных. Для получения только контакты, которые содержат строку `Appleseed`, используйте следующий код:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> **Примечание:** универсального и составные предикаты не поддерживаются платформой контактов.

Например, чтобы ограничить выборки только **GivenName** и **FamilyName** свойства контакта, используйте следующий код:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Наконец чтобы найти в базе данных и возвращают результаты, используйте следующий код:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Если этот код был выполнен после образца, созданную в **объекта контактов** предыдущем разделе, возвратят только что созданной контакта «Джон Appleseed».

### <a name="contact-access-privacy"></a>Обратитесь в службу доступа к конфиденциальности

Поскольку конечным пользователям можно разрешить или запретить доступ к его контактные данные на основе каждого приложения, первый раз можно вызвать `CNContactStore`, появится диалоговое окно, запросом на разрешение доступа для приложения.

Запрос на разрешение будут представлены только один раз, в первый раз, приложение будет запущена и последующие выполняется или вызовы `CNContactStore` будет использовать разрешения, которые пользователь выбрал в это время.

Следует разрабатывать приложения, чтобы он правильно обрабатывает запрет на доступ к их контактные базы данных пользователя.

#### <a name="fetching-partial-contacts"></a>Выборка частичного контактов

A _частичного обратитесь к_ для связи, часть доступных свойств, выбранных из хранилища для контактов. При попытке доступа к свойству, не извлечь ранее он приведет к исключению.

Можно легко проверить на предмет наличия нужного свойства с помощью данного контакта `IsKeyAvailable` или `AreKeysAvailable` методы `CNContact` экземпляра. Пример:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> **Примечание:** `GetUnifiedContact` и `GetUnifiedContacts` методы `CNContactStore` класса _только_ возвращать частичные контакт ограничено свойства, запрашиваемые из выборки ключах, предоставленных.

### <a name="unified-contacts"></a>Унифицированный контактов

Пользователь может иметь различные источники контактные данные одного человека в их базе данных, обратитесь в службу (например iCloud, Facebook или Google почты). В iOS и OS X приложений, эти данные будут автоматически связаны друг с другом и будут отображаться для пользователя как отдельный _единой обратитесь к_:

[![](contacts-images/unified01.png "Общие сведения о единой контактов")](contacts-images/unified01.png#lightbox)

Единой обратитесь — временный в памяти представление связи контактной информации, получает свой собственный уникальный идентификатор (который должен использоваться для повторно извлечь контакта, если это необходимо). По умолчанию платформа контакты вернет единой контакта по возможности.

### <a name="creating-and-updating-contacts"></a>Создание и обновление контактов

Как мы видели в [обратитесь в службу объекты](#Contact_Objects) выше можно выбрать `CNContactStore` и экземпляром `CNMutableContact` для создания новых контактов, которые затем записываются в пользователя обратитесь к базе данных с помощью `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

Объект `CNSaveRequest` также можно кэшировать несколько контактов и группировать изменения в одной операции и пакета изменений `CNContactStore`.

Чтобы обновить недоступного контакта, полученные из операции выборки, сначала нужно запросить изменяемую копию, которая затем изменить и сохранить в хранилище, обратитесь в службу. Пример:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Уведомления об изменении контакта

При каждом изменении контакта, обратитесь в службу хранилища учитывает `CNContactStoreDidChangeNotification` центр уведомлений по умолчанию. Если кэшированные или отображаются все контакты, необходимо обновить эти объекты в магазине контакт (`CNContactStore`).

### <a name="containers-and-groups"></a>Контейнеры и группы

Контактов пользователя может существовать либо локально на устройстве пользователя или как контакты, синхронизированные на устройстве из одного или нескольких учетных записей сервера (например, Google или Facebook). Каждый пул контактов имеет свой собственный _контейнера_ и данного контакта может существовать только в одном контейнере.

[![](contacts-images/containers01.png "Обзор контейнеров и групп")](contacts-images/containers01.png#lightbox)

Разрешить некоторые контейнеры для контактов, упорядоченных в одну или несколько _группы_ или _вложенным группам_. Это поведение определяется резервного хранилища для заданного контейнера. Например iCloud имеет только один контейнер, но может иметь несколько групп (но не вложенные группы). С другой стороны, Microsoft Exchange не поддерживает группы, но может содержать несколько контейнеров (по одному для каждой папки Exchange).

[![](contacts-images/containers02.png "Перекрываться в пределах контейнеров и групп")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Платформа ContactsUI

В ситуациях, когда приложение не требуется пользовательский интерфейс можно использовать для представления элементов пользовательского интерфейса для отображения, изменения, выберите и создайте контактов в приложении Xamarin.iOS ContactsUI framework.

С помощью встроенных элементов управления Apple не только уменьшить объем кода, необходимо создать для контактные данные службы поддержки в вашем приложении Xamarin.iOS, но согласованный интерфейс предлагать пользователям приложения.

### <a name="the-contact-picker-view-controller"></a>Выбор контактов View Controller

View Controller выбора контакт (`CNContactPickerViewController`) управляет стандартное представление выбора контакта, позволяющий пользователю выбрать свойства контакта или контакта из базы данных контактов пользователя. Пользователь может выбрать один или несколько контактов (с учетом использования), и обратитесь в службу выбора View Controller не запрашивать разрешения перед отображением окна выбора.

Перед вызовом метода `CNContactPickerViewController` класса, определяют свойства, которые пользователь может выбрать и Определение предикатов для управления отображением и Выбор свойств контакта.

Использовать экземпляр класса, который наследует от `CNContactPickerDelegate` реагировать на взаимодействие пользователя с помощью средства выбора. Пример:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Чтобы разрешить пользователю выбирать адрес электронной почты от контактов в их базе данных, можно использовать следующий код:

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Обратитесь в службу представление-контроллер

View Controller контакт (`CNContactViewController`) класс предоставляет контроллера для представления стандартного представления контакта для конечного пользователя. В представлении контакт можно отображать новые контакты, создать, неизвестно или существующие и перед вызовом правильный статический конструктор будет отображено представление необходимо указать тип (`FromNewContact`, `FromUnknownContact`, `FromContact`). Пример.

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Сводка

Подробное описание работы с платформами контактных данных и обратитесь в службу пользовательского интерфейса, в приложении Xamarin.iOS вступила в этой статье. Во-первых он рассмотрены различные типы объектов, которые предоставляет платформа обратитесь в службу и как их использовать для создания новых или существующих контактов доступ к. Он также проверяет framework обратитесь в службу пользовательского интерфейса для выбора существующих контактов и отображения контактной информации.


## <a name="related-links"></a>Связанные ссылки

- [Образец QuickContacts](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Новые возможности iOS 9](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-DontLinkElementID_14))
- [Справочник по Framework контактов](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)
- [Справочник по ContactsUI Framework](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)
