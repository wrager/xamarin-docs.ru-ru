---
title: CallKit в Xamarin.iOS
description: В этой статье рассматриваются новые API CallKit, Apple, выпущенные в iOS 10 и способах ее реализации в приложениях Xamarin.iOS VOIP.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: c674802eac9105d60471b6b130615e1b7efc1b28
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787205"
---
# <a name="callkit-in-xamarinios"></a>CallKit в Xamarin.iOS

_В этой статье рассматриваются новые API CallKit, Apple, выпущенные в iOS 10 и способах ее реализации в приложениях Xamarin.iOS VOIP._

Новый API CallKit в iOS 10 позволяет приложениям VOIP интегрировать iPhone пользовательского интерфейса и предоставляют знакомый интерфейс, а также реализовывать для конечного пользователя. Данный API пользователи могут просматривать и взаимодействовать с вызовами VOIP с экрана блокировки устройства iOS и управлять контактами, с помощью приложения Phone **Избранное** и **недавних объектов** представления.

## <a name="about-callkit"></a>О CallKit

В соответствии с Apple CallKit — это новая платформа, который будут повышены права сторонних приложений голоса по IP (VOIP) 1-го лицу взаимодействие для iOS 10. CallKit API позволяет приложениям VOIP интегрировать iPhone пользовательского интерфейса и предоставляют знакомый интерфейс, а также реализовывать для конечного пользователя. Так же, как и встроенные приложения телефона, пользователь может просматривать и взаимодействовать с вызовами VOIP с экрана блокировки устройства iOS и управление контактами, с помощью приложения Phone **Избранное** и **недавних объектов** представления.

Кроме того CallKit API предоставляет возможность создавать расширения приложения, которые можно связать с именем (идентификатор звонящего) номер телефона или сообщить, что в системе, если номер должен быть заблокирован (вызов блокировки).

### <a name="the-existing-voip-app-experience"></a>Процесс существующих приложений VOIP

Прежде чем обсуждать новый API CallKit и его возможности, взгляните на работу текущего пользователя со сходной стороны VOIP приложения в iOS 9 (и меньшей) с помощью вымышленной VOIP приложения вызывается MonkeyCall. MonkeyCall представляет собой простое приложение, которое дает пользователю возможность отправлять и получать звонки VOIP, с помощью существующего iOS API-интерфейсы.

В настоящее время, если пользователь принимает входящий вызов на MonkeyCall и их iPhone заблокирован, уведомления, полученного на экране блокировки невозможно отличить от других видов уведомления (как и из сообщения или почтовые приложения, например).

Если пользователю было ответить на звонок, они бы слайд MonkeyCall уведомление, чтобы открыть приложение и введите их секретный код (или пользователь Touch ID) можно разблокировать телефон, прежде чем они могут принимать вызовы и начать диалог.

Взаимодействие одинаково неудобно, если телефон разблокирован. Опять же входящий вызов MonkeyCall отображаются в виде стандартных баннер, слайды в верхней части экрана. Поскольку уведомление является временным, его можно легко пропущено пользователем заставив его открыть центр уведомлений и найти конкретных уведомления, чтобы ответить на затем вызовите или найти и запустить приложение MonkeyCall вручную.

### <a name="the-callkit-voip-app-experience"></a>Процесс CallKit VOIP приложений

Путем реализации новых API CallKit в приложении MonkeyCall, взаимодействие с пользователем с входящего вызова VOIP может сильно повысить в iOS 10. Рассмотрим в качестве примера пользователя, получение вызова VOIP при блокировке телефона выше. Благодаря использованию CallKit, вызов будет отображаться на экране блокировки iPhone, так же, как это происходит, если вызов поступил из встроенных телефона с весь экран, собственного пользовательского интерфейса и стандартных функциональных возможностей проведите ответов.

Опять же если iPhone разблокируется при получении вызова MonkeyCall VOIP, же весь экран, собственного пользовательского интерфейса и стандартных возможностей проведите ответов и tap отклонить встроенные Phone представлены приложения и MonkeyCall может воспроизводить настраиваемые мелодии звонка .

CallKit предоставляет функциональные возможности MonkeyCall, позволяя его VOIP вызывает для взаимодействия с другими типами вызовы отображаются в встроенной недавних объектов и список избранного, использовать встроенные функции не беспокоить и блока, запуск MonkeyCall вызовы из Siri и позволяет пользователям назначать вызовы MonkeyCall другим пользователям в приложении «контакты».

Следующих разделах будут рассмотрены архитектура CallKit, входящих и исходящих вызовов потоков и CallKit API подробно.


## <a name="the-callkit-architecture"></a>Архитектура CallKit

В iOS 10 Apple приняла CallKit во всех системных служб, таким образом, что вызовы, сделанные для CarPlay, например, известно, что пользовательский Интерфейс системы через CallKit. В пример, приведенный ниже, поскольку MonkeyCall принимает CallKit, он известен в системе в так же, как эти встроенные службы системы и получает все те же функции:

[![](callkit-images/callkit01.png "Служба CallKit стека")](callkit-images/callkit01.png#lightbox)

Внимательно ознакомьтесь приложение MonkeyCall из приведенной выше схеме. Приложение содержит весь код для взаимодействия с собственную сеть и содержит свои собственные пользовательские интерфейсы. Он связывает CallKit для взаимодействия с системой:

[![](callkit-images/callkit02.png "Архитектура приложения MonkeyCall")](callkit-images/callkit02.png#lightbox)

CallKit, который использует приложение состоит из двух основных интерфейсов.

- `CXProvider` -Это позволяет приложению MonkeyCall для информирования системы уведомления по каналу, которые могут возникнуть.
- `CXCallController` -Позволяет приложению MonkeyCall для информирования системы действия локального пользователя.

### <a name="the-cxprovider"></a>CXProvider

Как упоминалось выше, `CXProvider` позволяет приложению, для информирования системы уведомления по каналу, которые могут возникнуть. Это уведомление, которое не происходит из-за действий локального пользователя, но возникают из-за внешних событий, таких как входящие вызовы.

Приложение должно использовать `CXProvider` для следующего:

- Отчет получен входящий вызов в систему.
- О том, что исходящий вызов был подключен к системе.
- Отчет Завершение вызова в системе удаленного пользователя.

Когда приложение хочет обмениваться данными в системе, он использует `CXCallUpdate` класса и когда системы должен взаимодействовать с приложением, он использует `CXAction` класса:

[![](callkit-images/callkit03.png "Взаимодействие с помощью CXProvider системы")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` Позволяет приложению, для информирования системы таких пользователей, запускающих VOIP вызов действий локального пользователя. Путем реализации `CXCallController` приложение получает на взаимодействие с другими типами вызовов в системе. Например, если уже существует на активных телефонии вызов выполняется `CXCallController` можно разрешить приложению VOIP разместить вызова на удержании и запустить или ответ на вызов VOIP.

Приложение должно использовать `CXCallController` для следующего:

- Отчет, когда пользователь начинает исходящий вызов в систему.
- Отчет, когда пользователь отвечает на входящий звонок в системе.
- Отчет, когда пользователь завершает вызов в систему.

Когда приложение желает взаимодействовать действия локального пользователя в систему, он использует `CXTransaction` класса:

[![](callkit-images/callkit04.png "Отчеты в систему с помощью CXCallController")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Реализация CallKit

Приведенные ниже будет показано, как реализовать CallKit в приложении Xamarin.iOS VOIP. Для примера этот документ будет использовать код из вымышленной MonkeyCall VOIP приложения. Приведенный здесь код представляет несколько вспомогательных классов, CallKit, определенной части будут подробно описаны в следующих разделах.

### <a name="the-activecall-class"></a>Класс ActiveCall

`ActiveCall` Приложением MonkeyCall класс используется для хранения всех сведений о вызове VOIP, который сейчас активен следующим образом:

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` содержит несколько свойств, определяющих состояние вызова и два события, которые могут возникать при изменении состояния вызова. Поскольку это только для примера, существует три метода, используется для моделирования, ответ от вызова.

### <a name="the-startcallrequest-class"></a>Класс StartCallRequest

`StartCallRequest` Статический класс предоставляет несколько вспомогательных методов, которые будут использоваться при работе с исходящие вызовы:

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL` И `CallHandleFromActivity` классы используются в AppDelegate получить дескриптор контактного лица, вызываемой в исходящий вызов. Дополнительные сведения см. в разделе [обработку исходящих вызовов](#Handling-Outgoing-Calls) разделе ниже.

### <a name="the-activecallmanager-class"></a>Класс ActiveCallManager

`ActiveCallManager` Класс обрабатывает все открытые вызовы в приложении MonkeyCall.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID == uuid) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

Еще раз, так как это моделирование только `ActiveCallManager` только поддерживает коллекцию `ActiveCall` объектов и имеет подпрограммы для поиска в конкретном вызове его `UUID` свойство. Он также включает методы для запуска, завершения и изменить состояние на удержании исходящий вызов. Дополнительные сведения см. в разделе [обработку исходящих вызовов](#Handling-Outgoing-Calls) разделе ниже.

### <a name="the-providerdelegate-class"></a>Класс ProviderDelegate

Как было сказано выше, `CXProvider` предоставляет двусторонний обмен данными между приложением и системы для уведомления по каналу. Разработчику необходимо предоставить пользовательский `CXProviderDelegate` и присоединить его к `CXProvider` для приложения для обработки событий CallKit по каналу. MonkeyCall использует следующие `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

Когда создается экземпляр этого делегата, оно передается `ActiveCallManager` , он будет использоваться для обработки любого вызова действия. Затем он определяет типы дескрипторов (`CXHandleType`), `CXProvider` будет реагировать на:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

И он получает маску, которая будет применяться к значок приложения при завершении вызова.

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

Эти значения получить собраны в `CXProviderConfiguration` , будет использоваться для настройки `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Делегат затем создает новый `CXProvider` с этими конфигурациями и присоединена к нему:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

При использовании CallKit, приложение больше не будет создания и обработки аудио сеансов, вместо этого его необходимо настроить и использовать сеанс аудио, который система будет создать и обработать для него. 

Если бы это было настоящее приложение, `DidActivateAudioSession` метод будет использоваться для запуска с помощью предварительно настроенных вызова `AVAudioSession` , предоставленный системе:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Она также будет использовать `DidDeactivateAudioSession` аудио сеанса указанный метод finalize и освободит его подключение к системе:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

В следующих разделах подробно рассматриваются остальной части кода.

### <a name="the-appdelegate-class"></a>Класс AppDelegate

Для хранения экземпляров MonkeyCall используется AppDelegate `ActiveCallManager` и `CXProviderDelegate` , которые будут использоваться в пределах приложения:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` И `ContinueUserActivity` переопределение методов используются, когда приложение обрабатывает исходящий вызов. Дополнительные сведения см. в разделе [обработку исходящих вызовов](#Handling-Outgoing-Calls) разделе ниже.

## <a name="handling-incoming-calls"></a>Обработка входящих вызовов

Существует несколько состояний и процессы, которые входящего вызова VOIP может выполнить во время типичный рабочий процесс входящего вызова таких как:

- О том, пользователь (и системе), существует входящего вызова.
- Получать уведомление, когда пользователь хочет ответить на звонок и инициализация вызова с другим пользователем.
- Информирующие системной и сетевой связи пользователю для завершения текущего вызова.

Следующие разделы займет подробное рассмотрение как приложение может использовать CallKit для обработки входящего вызова рабочего процесса, снова, используя в качестве примера приложения MonkeyCall VOIP.

### <a name="informing-user-of-incoming-call"></a>Информирования пользователя вызовов

Удаленный пользователь началась VOIP диалога с локального пользователя происходит следующее:

[![](callkit-images/callkit05.png "Удаленный пользователь начинает диалог VOIP")](callkit-images/callkit05.png#lightbox)

1. Приложение получает уведомление от его сеть обмена данными это входящего вызова VOIP.
2. Приложение использует `CXProvider` для отправки `CXCallUpdate` в систему, сообщая ей вызова.
3. Система публикует вызов пользовательского интерфейса системы, системных служб и других приложениях VOIP, с помощью CallKit.

Например, в `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Этот код создает новый `CXCallUpdate` экземпляра и прикрепляет дескриптор к нему, обозначающие вызывающего объекта. Затем он использует `ReportNewIncomingCall` метод `CXProvider` класса для информирования системы вызова. При успешном ее вызов добавляется коллекцию активных вызовов приложения, в противном случае ошибка должен отображаться пользователю.

### <a name="user-answering-incoming-call"></a>Отвечая на вызов пользователя

Если пользователю необходимо ответить на входящий звонок VOIP, происходит следующее.

[![](callkit-images/callkit06.png "Пользователь отвечает на входящий звонок VOIP")](callkit-images/callkit06.png#lightbox)

1. Пользовательский Интерфейс системы сообщает системе, что пользователю необходимо ответить на звонок VOIP.
2. Система отправляет `CXAnswerCallAction` в приложение `CXProvider` о том, его назначение ответов.
3. Приложение сообщает сетевой связи, что пользователь является ответ на звонок, а вызов VOIP продолжает работу как обычно.

Например, в `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Этот код сначала выполняет поиск данного вызова в своем списке вызовов active. Если не удается найти вызов, система получает уведомление и выход из метода. Если он найден, `AnswerCall` метод `ActiveCall` класса вызывается для запуска вызова и система сведения при его успешном или неуспешном.

### <a name="user-ending-incoming-call"></a>Завершение входящего вызова пользователя

Если пользователь хочет Завершение вызова пользовательского интерфейса приложения, происходит следующее.

[![](callkit-images/callkit07.png "Пользователь завершает вызов из в рамках пользовательского интерфейса приложения")](callkit-images/callkit07.png#lightbox)

1. Приложение создает `CXEndCallAction` , возвращает собраны в `CXTransaction` отправляется в систему для информирования, вызов завершается.
2. Система проверяет цель вызова End и отправляет `CXEndCallAction` вернитесь к приложению через `CXProvider`.
3. Приложение сообщает сетевой связи, вызов завершается.

Например, в `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Этот код сначала выполняет поиск данного вызова в своем списке вызовов active. Если не удается найти вызов, система получает уведомление и выход из метода. Если он найден, `EndCall` метод `ActiveCall` класса вызывается для завершения вызова, и система находится информация, при его успешном или неуспешном. В случае успешного вызова удаляется из коллекции активных вызовов.

## <a name="managing-multiple-calls"></a>Управление несколькими вызовами

Большинство приложений VOIP может обрабатывать несколько вызовов за один раз. Например, если в настоящее время имеется активный вызов VOIP и получает уведомление приложения, что устанавливается новый входящий вызов пользователя можно приостановить или сигнала отбоя при первом вызове для ответа на второй.

В ситуации предоставить выше, система будет отправлять `CXTransaction` приложение, которое будет содержать список из нескольких действий (таких как `CXEndCallAction` и `CXAnswerCallAction`). Все эти действия будут должны быть выполнены по отдельности, чтобы система соответствующим образом обновить пользовательский Интерфейс.

## <a name="handling-outgoing-calls"></a>Обработка исходящих вызовов

Если пользователь касается запись из списка недавних объектов (в приложении для телефона), например, это вызов, относящиеся к приложению, они будут отправляться _запустить цель вызова_ системой:

[![](callkit-images/callkit08.png "Получает цель вызова начала")](callkit-images/callkit08.png#lightbox)

1. Приложение создаст _действие при запуске вызова_ на основе запуск вызова пожеланий, полученное из системы. 
2. Приложение будет использовать `CXCallController` для запроса действие при запуске вызова из системы.
3. Если система принимает действие, он будет возвращаться в приложение через `XCProvider` делегата.
4. Исходящий вызов запуска приложения с помощью его сетевого взаимодействия.

Дополнительные сведения о цели см. в разделе нашей [целей и целей расширения пользовательского интерфейса](~/ios/platform/sirikit/understanding-sirikit.md) документации. 

### <a name="the-outgoing-call-lifecycle"></a>Жизненный цикл исходящих вызовов

При работе с CallKit и исходящий вызов, приложению потребуется для информирования системы следующие события жизненного цикла:

1. **Запуск** -информирования системы, исходящий вызов является запуск.
2. **Запущен** -сообщить системе о запуске исходящий вызов.
3. **Подключение** -информирования системы, подключение исходящего вызова.
4. **Подключение** -информировать исходящий вызов был подключен, но обе стороны могут взаимодействовать теперь.

Например следующий код запускает исходящий вызов:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Он создает `CXHandle` и использует ее для настройки `CXStartCallAction` получаемого в `CXTransaction` , отправляются в систему с помощью `RequestTransaction` метод `CXCallController` класса. Путем вызова `RequestTransaction` метод системы можно разместить любой существующий вызовы на удержании, независимо от источника (для телефона, FaceTime, VOIP, т. д.), перед запуском нового вызова.

Запрос на запуск исходящий вызов VOIP могут поступать из нескольких источников, например Siri, запись в карточку контакта (в приложении «контакты») или из списка недавних объектов (в приложении для телефона). В таких случаях приложения будут отправляться запуск вызова намерение внутри `NSUserActivity` и AppDelegate должен обработать его:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

Здесь `CallHandleFromActivity` метод вспомогательный класс `StartCallRequest` используется для получения дескриптора пользователю, вызываемой (см. [класс StartCallRequest](#The-StartCallRequest-Class) выше). 

`PerformStartCallAction` Метод [ProviderDelegate класс](#The-ProviderDelegate-Class) используется для наконец запуска фактическое исходящего вызова и сообщить системе его жизненного цикла:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Он создает экземпляр `ActiveCall` класса (для хранения информации о вызове выполняется), а также заполняет вызываемой лица. `StartingConnectionChanged` И `ConnectedChanged` события используются для мониторинга и жизненном цикле исходящего вызова. Вызов запускается, и система проинформировать, что было выполнено действие.

### <a name="ending-an-outgoing-call"></a>Завершение исходящий вызов

По завершении пользователя с исходящий вызов и хочет завершить его можно использовать следующий код:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Если создает `CXEndCallAction` с UUID завершить вызов, связывает его в `CXTransaction` , отправляются в систему с помощью `RequestTransaction` метод `CXCallController` класса. 

## <a name="additional-callkit-details"></a>Сведения о дополнительных CallKit

В этом разделе мы рассмотрим некоторые дополнительные сведения, которые разработчик будет необходимо принять во внимание при работе с CallKit, такие как:

- Конфигурация поставщика
- Действие ошибки
- Системные ограничения
- VOIP аудио

### <a name="provider-configuration"></a>Конфигурация поставщика

Конфигурация поставщика обеспечивает VOIP приложения iOS 10, чтобы настроить взаимодействие с пользователем (внутри собственного пользовательского интерфейса во время вызова) при работе с CallKit.

Приложение может сделать следующие типы настроек:

- Отображение локализованного имени.
- Включите поддержку видео вызова.
- Настройте кнопки в пользовательском Интерфейсе в вызов, используя свой собственный значок скрытого изображения. Взаимодействие с пользователем с пользовательскими кнопками отправляется этому приложению для обработки. 

### <a name="action-errors"></a>Действие ошибки

приложения для iOS 10 VOIP CallKit должны обработки действий корректное поведение при ошибках и пользователю следить состояния действия в любое время. 

Следующий пример принимает во внимание:

1. Приложение получило действие при запуске вызова и начался процесс инициализации вызов VOIP с его сетевого взаимодействия.
2. Из-за ограниченной или сетевые функции связи это соединение завершается ошибкой.
3. Приложение *должен* отправки **ошибкой** сообщение обратно в действие при запуске вызова (`Action.Fail()`) для информирования системы сбоя.
4. Это позволяет системе информировать пользователей о состоянии вызова. Например, для отображения пользовательского интерфейса сбой вызова.

Кроме того, приложение iOS 10 VOIP должны отвечать на _ошибки времени ожидания_ , которые могут возникнуть при ожидаемое действие не может обрабатываться в течение заданного времени. Каждый тип действия, предоставляемые CallKit имеет максимальное значение времени ожидания, связанные с ним. Эти значения времени ожидания убедитесь, любое действие CallKit запрошенной пользователем обработки образом отвечать на запросы таким образом хранение ОС жидкости и отвечать на запросы также.

Существует несколько способов на делегат поставщика (`CXProviderDelegate`), следует переопределить для правильной обработки также ситуациях это время ожидания.

### <a name="system-restrictions"></a>Системные ограничения

На основе текущего состояния устройства iOS, запуск приложения iOS 10 VOIP, определенные системные ограничения может применяться.

Например входящего вызова VOIP может быть ограничен в системе, если:

1. Пользователь, вызвавший включен список заблокированных вызывающего пользователя.
2. Устройства iOS находится в режиме Not беспокоить.

Если вызов VOIP ограничено по любой из этих ситуаций, используйте следующий код, чтобы обработать его:

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP аудио

CallKit предоставляет несколько преимуществ для обработки звуковые ресурсы, требующие приложения iOS 10 VOIP во время вызова динамической VOIP. Одним из важнейших преимуществ является приложения аудио сеанса будет повышенными приоритеты при работе в iOS 10. Это тот же уровень приоритета, как встроенные в Phone и FaceTime приложениям и данным уровнем приоритета улучшенные помешает другие работающие приложения прерывания сеанса аудио приложения VOIP.

Кроме того CallKit имеет доступ к другим аудио маршрутизации подсказки, которые можно улучшить производительность и интеллектуально перенаправления звука VOIP отдельных устройств вывода во время вызова динамической на основе персональных настроек пользователя и состояний устройств. Например на основе подключенные устройства, например наушники bluetooth, активное соединение CarPlay или параметры специальных возможностей.

В течение жизненного цикла типичные VOIP вызовы с использованием CallKit, потребуется настроить поток аудио, который предоставит CallKit приложения. Рассмотрим следующий пример:

[![](callkit-images/callkit09.png "Начальная последовательность вызова действия")](callkit-images/callkit09.png#lightbox)

1. Действие при запуске вызова получает приложение для ответа на входящий вызов.
2. Прежде чем это действие выполняется в приложении, предоставляет конфигурацию, потребуются для его `AVAudioSession`.
3. Приложение сообщает системе, что действие было выполнено.
4. Перед подключением вызова, CallKit обеспечивает более высокий приоритет `AVAudioSession` соответствия конфигурации, который запросил приложение. Приложение будет уведомлен по `DidActivateAudioSession` метод его `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Работа с расширениями Directory вызова

При работе с CallKit, _вызывать расширения каталогов_ обеспечивают способ добавить номера заблокированных вызовов и определения числа, характерные для приложения VOIP контактам в контакт приложения на устройстве iOS.

### <a name="implementing-a-call-directory-extension"></a>Реализация модуля каталога вызова

Чтобы реализовать расширение Directory вызывать в приложении Xamarin.iOS, выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Откройте решение для приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **обозревателе решений** и выберите **добавить** > **Добавление нового проекта**.
3. Выберите **iOS** > **расширения** > **вызывать расширения каталогов** и нажмите кнопку **Далее** кнопки: 

    [![](callkit-images/calldir01.png "Создание нового расширения вызовите каталога")](callkit-images/calldir01.png#lightbox)
4. Введите **имя** расширение и нажмите кнопку **Далее** кнопки: 

    [![](callkit-images/calldir02.png "Введите имя для модуля")](callkit-images/calldir02.png#lightbox)
5. Настройка **имя проекта** и/или **имя решения** , если требуется и нажмите кнопку **создать** кнопки: 

    [![](callkit-images/calldir03.png "Создание проекта")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Откройте решение для приложения в Visual Studio.
2. Щелкните правой кнопкой мыши имя решения в **обозревателе решений** и выберите **добавить** > **Добавление нового проекта**.
3. Выберите **iOS** > **расширения** > **вызывать расширения каталогов** и нажмите кнопку **Далее** кнопки: 

    [![](callkit-images/calldir01w.png "Создание нового расширения вызовите каталога")](callkit-images/calldir01.png#lightbox)
4. Введите **имя** расширение и нажмите кнопку **ОК** кнопки

-----


При этом будет добавлено `CallDirectoryHandler.cs` класс в проект, который выглядит следующим образом:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest` Метод в обработчике вызовите каталог будет необходимо изменить, чтобы обеспечить требуемую функциональность. В случае с примером выше он пытается установить список чисел заблокированных и доступны в базе данных приложения VOIP контактов. При сбое либо запросов по любой причине, создать `NSError` опишите проблему и передать его `CancelRequest` метод `CXCallDirectoryExtensionContext` класса.

Настройка использования заблокированных номера `AddBlockingEntry` метод `CXCallDirectoryExtensionContext` класса. Значения, приведенные в метод _должен_ в числовом порядке по возрастанию. Для оптимальной работы и использование памяти при наличии многие телефонные номера, рекомендуется только Загрузка подмножества чисел в определенный момент времени и использовать пулы autorelease освободить объекты, выделенные во время выполнения каждого пакета чисел, в который загружаются.

Информирование приложения контактов, контактных чисел, известные этому приложению VOIP, используйте `AddIdentificationEntry` метод `CXCallDirectoryExtensionContext` класса и укажите номер и метку. Еще раз, чисел, переданной методу _должен_ в числовом порядке по возрастанию. Для оптимальной работы и использование памяти при наличии многие телефонные номера, рекомендуется только Загрузка подмножества чисел в определенный момент времени и использовать пулы autorelease освободить объекты, выделенные во время выполнения каждого пакета чисел, в который загружаются.


## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован новый API CallKit, Apple, выпущенные в iOS 10 и способах ее реализации в приложениях Xamarin.iOS VOIP. Он узнали, как CallKit позволяет приложению интегрировать в системе iOS, как предоставляет функционального соответствия с помощью встроенных приложений (например, телефон) и как повышает уровень видимости приложения на протяжении операций ввода-вывода в местах, например блокировки и Главная экраны через Siri взаимодействия и via приложения контактов.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
