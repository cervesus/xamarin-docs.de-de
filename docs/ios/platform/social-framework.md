---
title: Social Framework in xamarin. IOS
description: Das Social Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook, sowie sinaweibo für Benutzer in China.
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: a77b5cd33710a7a8755441efc8b7134d82855c2a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937968"
---
# <a name="social-framework-in-xamarinios"></a>Social Framework in xamarin. IOS

_Das Social Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook, sowie sinaweibo für Benutzer in China._

Mit dem Social Framework können Anwendungen mit sozialen Netzwerken über eine einzige API interagieren, ohne die Authentifizierung verwalten zu müssen. Es enthält einen vom System bereitgestellten Ansichts Controller zum Verfassen von Beiträgen sowie eine Abstraktion, die die Verwendung der API eines sozialen Netzwerks über HTTP ermöglicht.

## <a name="connecting-to-twitter"></a>Herstellen einer Verbindung mit Twitter

### <a name="twitter-account-settings"></a>Twitter-Kontoeinstellungen

Zum Herstellen einer Verbindung mit Twitter über das Social Framework muss ein Konto in den Geräteeinstellungen konfiguriert werden, wie unten dargestellt:

 [![Twitter-Kontoeinstellungen](social-framework-images/twitter01.png)](social-framework-images/twitter01.png#lightbox)

Nachdem ein Konto eingegeben und mit Twitter überprüft wurde, verwendet jede Anwendung auf dem Gerät, die die Social Framework-Klassen für den Zugriff auf Twitter verwendet, dieses Konto.

### <a name="sending-tweets"></a>Senden von Tweets

Das Social Framework enthält einen Controller mit dem Namen `SLComposeViewController` , der eine vom System bereitgestellte Ansicht zum Bearbeiten und Senden eines tweets darstellt. Der folgende Screenshot zeigt ein Beispiel für diese Ansicht:

 [![Dieser Screenshot zeigt ein Beispiel für den slcompoabviewcontroller.](social-framework-images/twitter02.png)](social-framework-images/twitter02.png#lightbox)

Wenn Sie ein `SLComposeViewController` mit Twitter verwenden möchten, müssen Sie eine Instanz des Controllers erstellen, indem Sie die- `FromService` Methode mit aufrufen, `SLServiceType.Twitter` wie unten dargestellt:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Nachdem die `SLComposeViewController` Instanz zurückgegeben wurde, kann Sie verwendet werden, um eine Benutzeroberfläche zum Posten an Twitter bereitzustellen. Der erste Schritt besteht jedoch darin, die Verfügbarkeit des sozialen Netzwerks (in diesem Fall Twitter) zu überprüfen, indem Sie Folgendes aufrufen `IsAvailable` :

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController`sendet einen Tweet niemals direkt ohne Benutzerinteraktion. Sie kann jedoch mit den folgenden Methoden initialisiert werden:

- `SetInitialText`– Fügt den Anfangstext hinzu, der im Tweet angezeigt werden soll.
- `AddUrl`– Fügt dem Tweet eine URL hinzu.
- `AddImage`– Fügt dem Tweet ein Bild hinzu.

Nach der Initialisierung `PresentVIewController` zeigt der Aufruf der von erstellten Ansicht an `SLComposeViewController` . Der Benutzer kann dann optional den Tweet bearbeiten und senden oder den Sendevorgang abbrechen. In beiden Fällen sollte der Controller in der verworfen werden `CompletionHandler` , wo das Ergebnis auch geprüft werden kann, um festzustellen, ob der Tweet gesendet oder abgebrochen wurde, wie unten dargestellt:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Tweet-Beispiel

Der folgende Code veranschaulicht die Verwendung `SLComposeViewController` von zum Darstellen einer Ansicht, die zum Senden eines tweets verwendet wird:

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {

        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Aufrufen der Twitter-API

Das Social Framework bietet auch Unterstützung für die Durchführung von HTTP-Anforderungen an soziale Netzwerke. Er kapselt die Anforderung in einer `SLRequest` -Klasse, die verwendet wird, um die API eines bestimmten sozialen Netzwerks als Ziel festzustellen.

Der folgende Code sendet z. b. eine Anforderung an Twitter, um die öffentliche Zeitachse zu erhalten (durch Erweiterung des oben genannten Codes):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Sehen wir uns diesen Code ausführlich an. Zuerst erhält er Zugriff auf den Konto Speicher und ruft den Typ eines Twitter-Kontos ab:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Anschließend wird der Benutzer gefragt, ob Ihre APP Zugriff auf das Twitter-Konto haben kann. wenn der Zugriff gewährt wird, wird das Konto in den Arbeitsspeicher geladen, und die Benutzeroberfläche wird aktualisiert:

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

Wenn der Benutzer die Zeitachsen Daten anfordert (indem er auf eine Schaltfläche in der Benutzeroberfläche tippt), bildet die APP zuerst eine Anforderung für den Zugriff auf die Daten von Twitter:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```

In diesem Beispiel werden die zurückgegebenen Ergebnisse auf die letzten zehn Einträge beschränkt, indem `?count=10` in die URL eingeschlossen wird. Schließlich fügt Sie die Anforderung an das Twitter-Konto an (das oben geladen wurde) und führt den Twitter-Rückruf aus, um die Daten abzurufen:

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Wenn die Daten erfolgreich geladen wurden, werden die unformatierten JSON-Daten angezeigt (wie in der folgenden Beispielausgabe gezeigt):

[![Ein Beispiel für die unformatierte JSON-Datenanzeige](social-framework-images/twitter03.png)](social-framework-images/twitter03.png#lightbox)

In einer echten App können die JSON-Ergebnisse dann wie üblich analysiert werden, und die Ergebnisse werden dem Benutzer angezeigt. Weitere Informationen zum Analysieren von JSON finden Sie unter [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md) .

## <a name="connecting-to-facebook"></a>Herstellen einer Verbindung mit Facebook

### <a name="facebook-account-settings"></a>Facebook-Kontoeinstellungen

Das Herstellen einer Verbindung mit Facebook mit dem Social Framework ist fast identisch mit dem Prozess, der für Twitter oben verwendet wird. Ein Facebook-Benutzerkonto muss wie unten gezeigt in den Geräteeinstellungen konfiguriert werden:

[![Facebook-Kontoeinstellungen](social-framework-images/facebook01.png)](social-framework-images/facebook01.png#lightbox)

Nach der Konfiguration wird von jeder Anwendung auf dem Gerät, die das Social Framework verwendet, dieses Konto verwendet, um eine Verbindung mit Facebook herzustellen.

### <a name="posting-to-facebook"></a>Veröffentlichen an Facebook

Da das Social Framework eine einheitliche API ist, die für den Zugriff auf mehrere soziale Netzwerke konzipiert ist, bleibt der Code unabhängig vom verwendeten sozialen Netzwerk nahezu identisch.

Beispielsweise `SLComposeViewController` kann genau wie im zuvor gezeigten Twitter-Beispiel verwendet werden. der einzige Unterschied besteht darin, zu den Facebook-spezifischen Einstellungen und Optionen zu wechseln. Beispiel:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {

        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

Bei der Verwendung mit Facebook `SLComposeViewController` zeigt das eine Ansicht an, die fast identisch mit dem Twitter-Beispiel ist, das in diesem Fall **Facebook** als Titel anzeigt:

[![Die slcompoabviewcontroller-Anzeige](social-framework-images/facebook02.png)](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Aufrufen von Facebook Graph-API

Ähnlich wie beim Twitter-Beispiel kann das-Objekt des sozialen Frameworks `SLRequest` mit der Graph-API von Facebook verwendet werden. Der folgende Code gibt z. b. Informationen aus der Graph-API über das xamarin-Konto zurück (durch Erweiterung des oben genannten Codes):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Der einzige wirkliche Unterschied zwischen diesem Code und der oben dargestellten Twitter-Version besteht darin, dass die Facebook-Anforderung eine spezifische ID für Entwickler/apps (die Sie aus dem Facebook-Entwickler Portal generieren können) erhält, die als Option für die Anforderung festgelegt werden muss:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Wenn Sie diese Option nicht festlegen (oder einen ungültigen Schlüssel verwenden), führt dies zu einem Fehler, oder es werden keine Daten zurückgegeben.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie das Social Framework für die Interaktion mit Twitter und Facebook verwendet werden kann. Es wurde gezeigt, wo Konten für jedes soziale Netzwerk in den Geräteeinstellungen konfiguriert werden müssen. Außerdem wurde erläutert, wie verwendet wird `SLComposeViewController` , um eine einheitliche Ansicht für die Bereitstellung in sozialen Netzwerken darzustellen. Außerdem wurde die-Klasse untersucht, die `SLRequest` zum Abrufen der API eines sozialen Netzwerks verwendet wird.

## <a name="related-links"></a>Verwandte Links

- [Socialframeworkdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/socialframeworkdemo)
- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
