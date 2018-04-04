---
title: Soziale Framework
description: Das soziale Framework bietet eine einheitliche API, für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook sowie SinaWeibo für Benutzer in China.
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 334e05ad653d766b48f7f6028a1e98b0a0548c0c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="social-framework"></a>Soziale Framework

_Das soziale Framework bietet eine einheitliche API, für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook sowie SinaWeibo für Benutzer in China._


Mit sozialen Framework ermöglicht Anwendungen, die mit sozialen Netzwerken aus einer einzelnen API zu interagieren, ohne Authentifizierung zu verwalten. Sie enthält ein System modellansichtcontroller zum Verfassen von Beiträgen als auch eine Abstraktion, die ermöglicht, Nutzung von jedes sozialen Netzwerk-API über HTTP bereitgestellt.

> [!IMPORTANT]
> Eine plattformübergreifende API zur Verbindung mit verschiedenen sozialer Netzwerken, finden Sie unter der [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) -Komponente in den Speicher der Xamarin-Komponente.

## <a name="connecting-to-twitter"></a>Herstellen einer Verbindung mit Twitter

### <a name="twitter-account-settings"></a>Twitter-Konto

Zum Herstellen einer Verbindung mit sozialen Framework Twitter muss ein Konto in den geräteeinstellungen konfiguriert werden, wie unten dargestellt:

 [![](social-framework-images/twitter01.png "Twitter-Konto")](social-framework-images/twitter01.png#lightbox)

Sobald ein Konto eingegeben haben und mit Twitter überprüft wurde, wird jede Anwendung auf dem Gerät, das die soziale Framework-Klassen verwendet wird, um den Zugriff auf Twitter dieses Konto verwenden.

### <a name="sending-tweets"></a>Tweets senden

Das sozialen-Framework beinhaltet einen Controller aufgerufen `SLComposeViewController` stellt eine vom System bereitgestellte Ansicht zum Bearbeiten und einen Tweet gesendet. Der folgende Screenshot zeigt ein Beispiel für diese Ansicht:

 [![](social-framework-images/twitter02.png "Diese bildschirmabbildung zeigt ein Beispiel für die SLComposeViewController")](social-framework-images/twitter02.png#lightbox)

Verwenden einer `SLComposeViewController` mit Twitter, eine Instanz des Controllers erstellt werden muss, durch Aufrufen der `FromService` Methode mit `SLServiceType.Twitter` wie unten dargestellt:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Nach der `SLComposeViewController` -Instanz zurückgegeben, sondern die verwendet werden kann, um eine Benutzeroberfläche zum an Twitter anzuzeigen. Die erste Vorgehensweise ist jedoch in diesem Fall prüfen Sie die Verfügbarkeit des sozialen Netzwerks Twitter durch Aufrufen `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` sendet nie einen Tweet direkt ohne Eingreifen des Benutzers ein. Sie können jedoch mit den folgenden Methoden initialisiert werden:

-   `SetInitialText` – Fügt dieser Text in der Tweet angezeigt. 
-  `AddUrl` – Eine Url wird im Tweet hinzugefügt.
-  `AddImage` – Ein Bild wird im Tweet hinzugefügt.


Nach der Initialisierung aufrufen `PresentVIewController` zeigt die Ansicht erstellt, indem die `SLComposeViewController`. Der Benutzer kann dann optional bearbeiten und im Tweet gesendet, oder "Abbrechen" senden. In beiden Fällen der Controller verworfen werden sollte, der `CompletionHandler`, wobei das Ergebnis auch geprüft werden kann um festzustellen, ob im Tweet gesendet wurde oder abgebrochen wird, wie unten dargestellt:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Tweet-Beispiel

Der folgende Code veranschaulicht die Verwendung der `SLComposeViewController` zum Präsentieren von einer Sicht verwendet, um ein Tweet gesendet:

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

Die soziale Framework schließt auch Unterstützung für HTTP-Anforderungen mit sozialen Netzwerken herstellen. Sie kapselt die Anforderung in eine `SLRequest` Klasse, die bestimmten sozialen Netzwerk-API als Ziel verwendet wird.

Der folgende Code macht z. B. eine Anforderung an Twitter abzurufenden öffentliche Zeitachse (durch die Erweiterung des oben genannte Codes):

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

Sehen wir uns auf diesen Code im Detail. Zunächst erhält Zugriff auf die Store-Konto und den Typ der Twitter-Konto ab:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Als Nächstes fordert den Benutzer, wenn Ihre app Zugriff auf ihr Twitter-Konto haben kann, und wenn der Zugriff gewährt wird, wird das Konto in den Arbeitsspeicher und die Benutzeroberfläche aktualisiert geladen:

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

Wenn der Benutzer die Zeitachsendaten (durch Tippen auf eine Schaltfläche in der Benutzeroberfläche) anfordert, bildet die app zunächst eine Anforderung für den Datenzugriff von Twitter:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
In diesem Beispiel wird die zurückgegebenen Ergebnisse auf die letzten zehn Einträge einschränken, indem Sie z. B. `?count=10` in der URL. Schließlich fügt er die Anforderung an die Twitter-Konto (das oben geladen wurde) und führt der Aufruf von Twitter zum Abrufen der Daten:

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

Wenn die Daten erfolgreich geladen wurde, werden die JSON-Rohdaten (wie in der folgenden Beispielausgabe) angezeigt:

[![](social-framework-images/twitter03.png "Ein Beispiel für die unformatierte Darstellung der JSON-Daten")](social-framework-images/twitter03.png#lightbox)

In einer echten app konnte die JSON-Ergebnisse dann als Normal und die Ergebnisse, die dem Benutzer angezeigt analysiert werden. Finden Sie unter [Einführung in Web Services](~/cross-platform/data-cloud/web-services/index.md) Informationen zum Analysieren von JSON.

## <a name="connecting-to-facebook"></a>Herstellen einer Verbindung mit Facebook

### <a name="facebook-account-settings"></a>Einstellungen für die Facebook-Konto

Herstellen einer Verbindung mit Facebook mit sozialen Framework ist nahezu identisch mit den Verfahren für die oben gezeigte Twitter. Ein Facebook-Benutzerkonto muss in den geräteeinstellungen konfiguriert werden, wie unten dargestellt:

[![](social-framework-images/facebook01.png "Einstellungen für die Facebook-Konto")](social-framework-images/facebook01.png#lightbox)

Nach der Konfiguration wird jede Anwendung auf dem Gerät, das die soziale Framework verwendet, dieses Konto verwenden, für die Verbindung mit Facebook.

### <a name="posting-to-facebook"></a>Posten von Beiträgen in Facebook

Während sozialen Framework eine einheitliche API, die Zugriff auf mehrere soziale Netzwerke konzipiert ist, bleibt der Code nahezu identisch, unabhängig von sozialen Netzwerk verwendet wird.

Z. B. die `SLComposeViewController` können genau wie in der Twitter-Beispiel weiter oben, dargestellt wird, der nur anderen wechselt die Facebook-spezifische Einstellungen und Optionen verwendet werden. Zum Beispiel:

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

Bei Verwendung mit Facebook, die `SLComposeViewController` zeigt eine Ansicht, die nahezu identisch mit der Twitter-Beispiel zeigt aussieht **Facebook** als Titel in diesem Fall:

[![](social-framework-images/facebook02.png "Die Anzeige SLComposeViewController")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Aufrufen der Facebook Graph-API

Ähnlich wie Twitter, soziale Framework des Beispiels `SLRequest` Objekt mit Facebooks Graph-API verwendet werden kann. Der folgende Code gibt beispielsweise Informationen über die Graph-API über die Xamarin-Konto (durch die Erweiterung des oben genannte Codes) zurück:

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

Der einzige wirkliche Unterschied zwischen diesem Code und die oben genannten, Twitter-Version ist Facebooks-Anforderung eine bestimmte Developer-App-ID (die aus Facebooks-Entwicklerportal generiert werden kann) abrufen, die als eine Option festgelegt werden, wenn die Anforderung ausführen:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Fehler beim Festlegen dieser Option (oder mit einem ungültigen Schlüssel) führt einen Fehler oder keine Daten zurückgegeben wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie die soziale Framework verwenden, um die Interaktion mit Twitter und Facebook. Es wurde gezeigt, wo Sie Konten für jedes sozialen Netzwerk in den geräteeinstellungen konfigurieren. Er ebenfalls erläutert, wie die `SLComposeViewController` eine einheitliche Sicht für die Bereitstellung sozialen Netzwerken zu präsentieren. Außerdem überprüft er die `SLRequest` -Klasse, die verwendet wird, um jedes sozialen Netzwerk-API aufzurufen.


## <a name="related-links"></a>Verwandte Links

- [SocialFrameworkDemo (sample)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
