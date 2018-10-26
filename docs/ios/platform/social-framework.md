---
title: Social-Framework in Xamarin.iOS
description: Das soziale Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook als auch SinaWeibo für Benutzer in China.
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 08ccd5b5ac78e82bf745764d70e59d2db9ec6776
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115704"
---
# <a name="social-framework-in-xamarinios"></a>Social-Framework in Xamarin.iOS

_Das soziale Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook als auch SinaWeibo für Benutzer in China._

Mit dem sozialen Framework kann Anwendungen für soziale Netzwerke über eine einzige API interagieren, ohne Authentifizierung verwalten zu müssen. Sie enthält ein System ansichtscontroller für das Verfassen von Beiträgen sowie eine Abstraktion, die ermöglicht die Nutzung jedes soziale Netzwerk-API über HTTP bereitgestellt.

> [!IMPORTANT]
> Eine plattformübergreifende API, um verschiedene soziale Netzwerke herzustellen, finden Sie unter den [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) -Komponente in der Xamarin Component Store.

## <a name="connecting-to-twitter"></a>Herstellen einer Verbindung mit Twitter

### <a name="twitter-account-settings"></a>Twitter Kontoeinstellungen

Zur Verbindung mit Twitter, die mit dem sozialen Framework muss ein Konto in den geräteeinstellungen konfiguriert werden, wie unten dargestellt:

 [![](social-framework-images/twitter01.png "Twitter Kontoeinstellungen")](social-framework-images/twitter01.png#lightbox)

Sobald ein Konto eingegeben haben und mit Twitter überprüft wurde, wird jede Anwendung auf dem Gerät, das die sozialen Framework-Klassen verwendet wird, um den Zugriff auf Twitter dieses Konto verwenden.

### <a name="sending-tweets"></a>Senden von Tweets

Das Social-Framework beinhaltet einen Controller namens `SLComposeViewController` , die eine vom System bereitgestellte Ansicht zum Bearbeiten und Senden eines TWEETS darstellt. Der folgende Screenshot zeigt ein Beispiel für diese Ansicht:

 [![](social-framework-images/twitter02.png "Dieser Screenshot zeigt ein Beispiel für die SLComposeViewController")](social-framework-images/twitter02.png#lightbox)

Verwenden eine `SLComposeViewController` mit Twitter, eine Instanz des Controllers erstellt werden muss, durch Aufrufen der `FromService` -Methode mit `SLServiceType.Twitter` wie unten dargestellt:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Nach der `SLComposeViewController` Instanz zurückgegeben, sondern die verwendet werden kann, um eine Benutzeroberfläche in Twitter Posten anzuzeigen. Der erste Schritt ist jedoch, überprüfen Sie die Verfügbarkeit des sozialen Netzwerks Twitter in diesem Fall durch Aufrufen von `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` nie Senden eines TWEETS direkt ohne Eingreifen des Benutzers durch. Sie können jedoch mit den folgenden Methoden initialisiert werden:

-   `SetInitialText` – Fügt den ursprünglichen Text in TWEETS angezeigt. 
-  `AddUrl` – Eine Url wird der Tweet hinzugefügt.
-  `AddImage` – Fügt ein Bild auf den Tweet.


Nach der Initialisierung aufrufen `PresentVIewController` zeigt die Ansicht erstellt haben, durch die `SLComposeViewController`. Der Benutzer kann dann optional bearbeiten und Tweet senden oder Abbrechen, senden. In beiden Fällen der Controller verworfen werden sollte, der `CompletionHandler`, in denen das Ergebnis auch überprüft werden kann um festzustellen, ob es sich bei der Tweet gesendet wurde, oder abgebrochen, wie unten dargestellt:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>TWEETS-Beispiel

Der folgende Code veranschaulicht die Verwendung der `SLComposeViewController` zur Darstellung einer Ansicht verwendet, um einen Tweet zu senden:

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

### <a name="calling-twitter-api"></a>Twitter-API aufrufen

Das Social-Framework unterstützt auch zum Senden von HTTP-Anforderungen in sozialen Netzwerken. Kapselt die Anforderung in eine `SLRequest` Klasse, die bestimmten sozialen Netzwerk-API als Ziel verwendet wird.

Zum Beispiel: der folgende Code sendet eine Anforderung mit Twitter, um die öffentliche Zeitachse (durch die Erweiterung des oben angegebenen Codes) zu erhalten:

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

Sehen wir uns diesen Code im Detail an. Zunächst erhält Zugriff auf die Store-Konto und ruft den Typ ein Twitter-Konto ab:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Als Nächstes fordert den Benutzer, wenn Ihre app Zugriff auf ihr Twitter-Konto verfügen kann, wenn Zugriff gewährt wird, wird das Konto in der Benutzeroberfläche aktualisiert den Speicher geladen:

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

Wenn der Benutzer die Zeitachsendaten anfordert (durch Tippen auf eine Schaltfläche in der Benutzeroberfläche), bildet die app zunächst eine Anforderung zum Zugriff auf die Daten von Twitter:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
In diesem Beispiel beschränkt die zurückgegebenen Ergebnisse auf die letzten zehn Einträge dazu `?count=10` in der URL. Abschließend fügt er die Anforderung an den Twitter-Konto (das oben geladen wurde) und führt der Aufruf von Twitter, um die Daten abzurufen:

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

Wenn die Daten erfolgreich geladen wurde, werden die unformatierten JSON-Daten (wie in der folgenden Beispielausgabe) angezeigt:

[![](social-framework-images/twitter03.png "Ein Beispiel für die unformatierte JSON-Daten anzeigen")](social-framework-images/twitter03.png#lightbox)

In einer echten app konnte der JSON-Ergebnisse klicken Sie dann als Normal, und die Ergebnisse, die dem Benutzer angezeigten analysiert werden. Finden Sie unter [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md) für Informationen zum Analysieren von JSON.

## <a name="connecting-to-facebook"></a>Herstellen einer Verbindung mit Facebook

### <a name="facebook-account-settings"></a>Facebook-Konto-Einstellungen

Herstellen einer Verbindung mit Facebook mit sozialen Framework ist nahezu identisch mit der Prozess für die oben gezeigte Twitter verwendet. Ein Facebook-Benutzerkonto muss in den geräteeinstellungen konfiguriert werden, wie unten dargestellt:

[![](social-framework-images/facebook01.png "Facebook-Konto-Einstellungen")](social-framework-images/facebook01.png#lightbox)

Nach der Konfiguration wird jede Anwendung auf dem Gerät, das Social-Framework verwendet, dieses Konto verwenden, für die Verbindung mit Facebook.

### <a name="posting-to-facebook"></a>Auf Facebook Posten

Wie das Social-Framework eine einheitliche API, die Zugriff auf mehrere soziale Netzwerke ausgelegt ist, bleibt der Code fast identisch, unabhängig davon, im sozialen Netzwerk, das verwendet wird.

Z. B. die `SLComposeViewController` kann verwendet werden, genau wie in der Twitter vorherigen Beispiel lautet der einzige Unterschied ist, wechseln zu den Facebook-spezifische Einstellungen und Optionen. Zum Beispiel:

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

Bei Verwendung mit Facebook, die `SLComposeViewController` zeigt eine Ansicht, die nahezu identisch mit der Twitter-Beispiel mit aussieht **Facebook** als Titel in diesem Fall:

[![](social-framework-images/facebook02.png "Die Anzeige SLComposeViewController")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Aufrufen der Facebook Graph-API

Ähnlich wie im Twitter Beispiel, das Social-Framework die `SLRequest` Objekt kann mit Facebooks Graph-API verwendet werden. Der folgende Code gibt z. B. Informationen aus der Graph-API über das Xamarin-Konto (durch die Erweiterung des oben angegebenen Codes) zurück:

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

Der einzige wirkliche Unterschied zwischen diesem Code und die Twitter-Version, die oben genannten, ist die Facebook Anforderung zum Abrufen von einer bestimmten Entwickler-App-ID (die Sie aus Facebooks-Entwicklerportal generieren können), das als Option festgelegt werden muss, wenn es sich bei der Anforderung:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Fehler beim Festlegen dieser Option (oder einen ungültigen Schlüssel) führt ein Fehler oder keine Daten zurückgegeben werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie die sozialen Framework verwenden, um die Interaktion mit Twitter und Facebook. Es wurde gezeigt, wo Sie Konten für jedes soziale Netzwerk in den geräteeinstellungen konfigurieren. Es ebenfalls erläutert, wie Sie mit der `SLComposeViewController` eine einheitliche Ansicht für Beiträge in sozialen Netzwerken präsentieren. Darüber hinaus wurde die `SLRequest` -Klasse, die verwendet wird, um jedes soziale Netzwerk-API aufrufen.


## <a name="related-links"></a>Verwandte Links

- [SocialFrameworkDemo (Beispiel)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
