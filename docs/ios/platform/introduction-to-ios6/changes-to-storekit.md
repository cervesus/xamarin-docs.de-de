---
title: Änderungen an StoreKit unter iOS 6
description: 'iOS 6 verwendet wird, werden zwei Änderungen an der Store Kit-API eingeführt: die Möglichkeit zum Anzeigen von iTunes (und App-Store/iBookstore) Produkte in Ihrer app und eine neue in-app den Kauf von Apple, in dem Ihre herunterladbaren Dateien hostet. In diesem Dokument wird erläutert, wie diese Features mit Xamarin.iOS zu implementieren.'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 5d1bb5ab636cd7527a560332a9890e9907fac454
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118317"
---
# <a name="changes-to-storekit-in-ios-6"></a>Änderungen an StoreKit unter iOS 6

_iOS 6, zwei Änderungen an den Store Kit-API eingeführt: die Möglichkeit zum Anzeigen von iTunes (und App-Store/iBookstore) Produkte in Ihrer app und eine neue in-app den Kauf von Apple, in dem Ihre herunterladbaren Dateien hostet. In diesem Dokument wird erläutert, wie diese Features mit Xamarin.iOS zu implementieren._

Die wichtigsten Änderungen in Store Kit in iOS6 sind diese zwei neue Features:

- **In-App-Inhalte anzeigen & Purchasing** – Benutzer kaufen und Download von apps, Musik, Bücher und andere iTunes Inhalte, ohne Ihre app verlassen zu müssen. Sie können auch für Ihre eigenen apps zu bewerben, erwerben, oder nur empfehlen, Rezensionen und Bewertungen verknüpfen.
- **In-App-Kauf gehosteten Inhalt** – Apple gespeichert wird, und übermitteln der Inhalte Ihrer in app-Käufe Produkten beseitigt die Notwendigkeit von einem anderen Server zum Hosten Ihrer Dateien, die automatisch unterstützt das Herunterladen von Hintergrund und können Sie geschrieben Sie weniger Code werden.

Finden Sie in der [In App-Käufe](~/ios/platform/in-app-purchasing/index.md) Anleitungen für die detaillierte Abdeckung der StoreKit-APIs.

## <a name="requirements"></a>Anforderungen

Der Store Kit-Funktionen, die in diesem Dokument beschriebenen erfordern Ios6 und Xcode 4.5 zusammen mit Xamarin.iOS 6.0.

## <a name="in-app-content-display--purchasing"></a>In-App-Inhalte anzeigen und erwerben

Das neue kaufmodell in-app-Feature in iOS kann Benutzer hier finden Sie Produktinformationen, erwerben und Herunterladen des Produkts aus Ihrer app.
Anwendungen müssen zuvor auslösen, iTunes, den App Store oder den iBookstore, was den verlässt die ursprüngliche Anwendung Benutzer führen würde. Dieses neue Feature gibt den Benutzer automatisch zu Ihrer app zurück, wenn sie fertig sind.

[![](changes-to-storekit-images/image1.png "Automatisch zurückgeben an eine app nach dem Kauf")](changes-to-storekit-images/image1.png#lightbox)

Beispiele wie diese verwendet werden kann:

- **Fördern die Benutzer Ihre app zu bewerten** –, damit der Benutzer zu bewerten und Ihre app überprüfen, ohne ihn zu verlassen kann, öffnen Sie die App-Store-Seite kann.
- **Heraufstufen Cross apps** – kann der Benutzer andere apps, die Sie veröffentlichen, können Sie mit der Möglichkeit, kaufen und sofort herunterladen, finden Sie unter.
- **Unterstützen von Benutzern suchen und Herunterladen von Inhalten** : unterstützen von Benutzern, die Inhalte zu kaufen, die Ihre app findet, verwaltet oder aggregiert (z. b. eine Musik-bezogene app konnte bieten eine Wiedergabeliste von Titeln und ermöglichen jedes "Song" aus, die innerhalb der app erworben werden).

Sobald die `SKStoreProductViewController` angezeigt wurde der Benutzer kann mit den Produktinformationen interagieren, als wären sie in iTunes, den App Store oder den iBookstore. Der Benutzer kann:

- Anzeigen von Screenshots (für apps)
- Beispiel-Titel oder ein Video (für Musik, Fernsehsendungen und Filme)
- Reviews lesen (und Schreiben)
- Erwerben Sie und herunterladen Sie, die vollständig innerhalb der ansichtscontroller und der Store Kit erfolgt.

Einige Optionen in der `SKStoreProductViewController` wird-Force-weiterhin den Benutzer Ihre app verlassen, und öffnen Sie die relevanten Store-app, wie z. B. durch Klicken auf **verwandte Produkte** oder einer app **Unterstützung** Link.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

Die API zum Anzeigen eines Produkts in einer app ist einfach: Es ist nur erforderlich, Sie erstellen und Anzeigen einer `SKStoreProductViewController`. Zum Erstellen und Anzeigen eines Produkts, gehen Sie wie folgt vor:

1. Erstellen Sie eine `StoreProductParameters` Objekt, das Übergeben von Parametern an den View-Controller, einschließlich der `productId` im Konstruktor.
1. Instanziieren der `SKProductViewController`. Weisen sie Sie einem Servicelevel Klassenfeld.
1. Weisen Sie einen Handler auf des ansichtscontrollers `Finished` -Ereignis, das den ansichtscontroller verzichten sollten. Dieses Ereignis wird immer dann aufgerufen, wenn der Benutzer drückt Abbrechen; oder andernfalls schließt eine Transaktion innerhalb des View Controller ab.
1. Rufen Sie die `LoadProduct` -Methode übergeben die `StoreProductParameters` und einen Abschlusshandler. Der Abschlusshandler überprüfen Sie, dass die Product-Anforderung erfolgreich war, und wenn dies der Fall ist, zeigen Sie die `SKProductViewController` modal. Für den Fall, dass das Produkt nicht abgerufen werden kann, sollten entsprechende Fehlerbehandlung hinzugefügt werden.

### <a name="example"></a>Beispiel

Die *ProductView* -Projekt in der *StoreKit* Beispielcode für diesen Artikel implementiert eine `Buy` -Methode, ein Produkt akzeptiert, die Apple-ID und zeigt die `SKStoreProductViewController`. Der folgende Code zeigt die Produktinformationen für alle angegebenen Apple-ID:

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

Die app sieht wie der folgende Screenshot aus, bei der Ausführung, herunterladen oder kaufen, tritt ein, komplett in – die `SKStoreProductViewController`:

[![](changes-to-storekit-images/image2.png "Die app sieht wie folgt aus, bei der Ausführung")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>Unterstützung von älteren Betriebssystemen

Die beispielanwendung enthält Code, der zeigt, wie in früheren Versionen von iOS die App-Store, iTunes oder die iBookstore geöffnet. Verwenden der `OpenUrl` Methode, um eine ordnungsgemäße gestaltete öffnen **itunes.com** URL.

Sie können eine versionsüberprüfung, um welche Code ausgeführt wird, zu bestimmen, wie folgt implementieren:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>Fehler

Der folgende Fehler wird auftreten, wenn die Apple-ID, die Sie verwenden nicht gültig ist, ein Netzwerk- oder Authentifizierungsfehler Problem irgendeiner Art impliziert die da sie verwirrend sein kann.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Lesen von Objective-C-Dokumentation

Entwickler, die Informationen zu Store Kit im Apple Developer Portal sehen ein Protokoll – [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – in Bezug auf dieses neue Feature erläutert. Das Protokoll der Delegat hat nur eine Methode – ProductViewControllerDidFinish – die als verfügbar gemacht wurde die `Finished` Ereignis auf der `SKStoreProductViewController` in Xamarin.iOS.

## <a name="determining-apple-ids"></a>Bestimmen die Apple-IDs

Die Apple-ID, die erforderlich sind, durch die `SKStoreProductViewController` ist eine *Anzahl* (nicht zu verwechseln mit der Bündel-IDs wie "com.xamarin.mwc2012"). Es gibt mehrere Möglichkeiten, die Sie die Apple-ID für die Produkte, die Sie anzeigen möchten ermitteln können, unten:

### <a name="itunesconnect"></a>iTunesConnect

Für Anwendungen, die Sie veröffentlichen, es ist einfach, Sie finden die **Apple-ID** in iTunes Connect:

[![](changes-to-storekit-images/image3.png "Suchen die Apple-ID in iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />

### <a name="search-api"></a>Search-API

Apple bietet eine dynamische Suche-API, um alle Produkte in den App Store, iTunes und die iBookstore Abfragen. Informationen zum Zugreifen auf die Such-API finden Sie im [Apple Partneranwendungen Ressourcen](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html), obwohl die API für alle Benutzer (nicht nur registrierte Tochtergesellschaften) verfügbar gemacht wird. Der resultierende JSON-Code analysiert werden kann, um zu ermitteln die `trackId` , die Apple-ID für die Verwendung mit `SKStoreProductViewController`.

Die Ergebnisse umfasst auch andere Metadaten, einschließlich Informationen angezeigt und Grafik-URLs, die verwendet werden können, um das Produkt in Ihrer app zu rendern.

Hier einige Beispiele:

- **iBooks-app** – [ http://itunes.apple.com/search?term=ibooks&amp; Entity = Software&amp;Country = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us)
- **Punkt und dem iBook Kangaroo** – [ http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; Entity = e-Book&amp;Country = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us)

### <a name="enterprise-partner-feed"></a>Enterprise-Partner-Feed

Apple bietet genehmigte Partner mit einer vollständigen datenssicherung ihrer Produkte, in Form von Flatfiles der herunterladbaren Datenbank bereit. Wenn Sie für den Zugriff auf Qualifizieren der [Enterprise-Partner Feed](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html), und klicken Sie dann die Apple-ID für jedes Produkt im Dataset gefunden werden kann.

Viele Benutzer, der den Feed zu Enterprise-Partner sind Mitglied der [-Partnerprogramm](http://www.apple.com/itunes/affiliates) , mit der Provisionen, um auf Product Sales forumsanwendungen verliehen werden. `SKStoreProductViewController` Partner-IDs unterstützt (zum Zeitpunkt der Verfassung) nicht.

### <a name="direct-product-links"></a>Direkte Produktlinks

Die Apple-ID für ein Produkt kann von der iTunes-Vorschau-URL-Link abgeleitet werden.
In jedem iTunes Produktlinks (für apps, Musik oder Bücher) finden Sie den Teil der URL beginnend mit `id` und verwenden Sie die Zahl, folgt.

Beispielsweise ist der direkte Link zum iBooks

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

die Apple-ID ist **364709193**. Auf ähnliche Weise ist für die MWC2012-app, die direkte Verknüpfung

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

die Apple-ID ist **496963922**.

## <a name="in-app-purchase-hosted-content"></a>In-App-Käufe gehosteten Inhalt

Wenn es sich bei Ihrer in-app-Käufe besteht aus herunterladbaren Inhalt (z. B. Bücher oder andere Medien, Spiele zweidimensionalen Grafik und Konfiguration oder andere großen Dateien), und klicken Sie dann diese Dateien verwendet, auf dem Webserver gehostet werden, und apps Code sicher nach dem Download einbinden mussten erwerben. Ab iOS 6, hostet Apple Ihre Dateien auf ihren Servern, die Notwendigkeit eines separaten Servers entfernen. Das Feature ist nur für nicht-verbrauchsartikeln (nicht verwendet werden können oder Abonnements) verfügbar. Fehler bei Vorteile der Verwendung von Apple Hostingdienst:

- Hosting & bandbreitenverwendung Kosten zu sparen.
- Wahrscheinlich besser skalierbar als den Serverhost, die Sie derzeit verwenden. 
- Weniger Code geschrieben werden, da Sie keine serverseitige Verarbeitung zu erstellen. 
- Herunterladen der Hintergrund ist für Sie implementiert.

Hinweis: Tests in app-Käufe-Inhalt in iOS-Simulator nicht unterstützt wird, die gehostet, damit Sie mit einem echten Gerät testen müssen.

### <a name="hosted-content-basics"></a>Grundlagen der gehostete Inhalt

Bevor Sie iOS 6, gab es zwei Möglichkeiten, ein Produkt liefern (ausführlicher beschrieben [Xamarin In-App-Käufe](~/ios/platform/in-app-purchasing/index.md) Dokumentation):

- **Integrierte Produkte** – Funktionen, sind "entsperrt" durch den Erwerb, jedoch, die bei der Anwendung (entweder als Code oder eingebettete Ressourcen) integriert sind. Beispiele für integrierte Produkte sind entsperrt Foto-Filter oder im Spiel leistungsschübe.
- **Server bereitgestellte Produkte** – nach dem Kauf wird die Anwendung muss Herunterladen von Inhalten von einem Server, die Sie ausgeführt werden. Dieser Inhalt ist beim Erwerb heruntergeladen, auf dem Gerät gespeichert und dann als Teil der Bereitstellung des Produkts gerendert. Beispiele hierfür sind Bücher, magazine-Ausgaben oder Level in Spielen, die Art und die Konfigurationsdateien Hintergrund bestehen.

In iOS 6 Apple eine Variation des bereitgestellte Server-Produkte bietet: sie werden die Inhaltsdateien auf ihren Servern hosten. Dadurch viel einfacher, bereitgestellte Server-Produkten erstellt werden, da Sie nicht auf einen separaten Server ausgeführt werden müssen und Store Kit bietet Hintergrund-herunterladen-Funktionen, die bisher mussten Sie selbst schreiben. Zur Nutzung von Apple hosting aktivieren Sie Hosten von Inhalten für neue in-app-Käufe Produkte zu, und ändern Sie den Store Kit-Code, um die Vorteile zu nutzen. Produkt-Inhaltsdateien werden dann mithilfe von Xcode erstellt und auf Apple Server zur Überprüfung und Freigabe hochgeladen.

[![](changes-to-storekit-images/image4.png "Der Prozess zum Erstellen und bereitstellen")](changes-to-storekit-images/image4.png#lightbox)

Verwenden die App-Store zum Bereitstellen von in-app-Käufe *mit gehosteten Inhalt* erfordert die folgenden Setup und Konfiguration:

- **iTunes Connect** – Sie *müssen* Ihre Bank- und Steuer-Informationen müssen an Apple bereitgestellt werden, sodass sie Geldmittel gesammelt, die in Ihrem Namen führen können. Anschließend können Sie konfigurieren, dass Produkte verkaufen und Einrichten von Sandbox-Benutzerkonten zum Kauf zu testen.  _Außerdem müssen Sie konfigurieren gehosteten Inhalt für diese nicht-verbrauchsartikeln, die Sie bei Apple hosten möchten_.
- **iOS-Bereitstellungsportal** – erstellen Sie eine Bündel-ID und den App Store-Zugriff für Ihre app aktivieren, wie für jede Anwendung, die in-app-Käufe unterstützt.
- **Store Kit** – Hinzufügen von Code zu Ihrer app für Produkte anzeigen, Erwerb von Produkten und Wiederherstellen von Transaktionen.  _In iOS 6 wird Store Kit auch verwalten, das Herunterladen des Inhalts Produkt, im Hintergrund mit Statusupdates._
- **Benutzerdefinierter Code** um Einkäufe von Kunden nachverfolgen, und geben Sie die Produkte oder Dienste, die sie erworben haben. Nutzen Sie neue Ios6 Store Kit-Klassen wie `SKDownload` zum Abrufen des Inhalts von Apple gehostet wird.

In den folgenden Abschnitten wird erläutert, wie gehosteten Inhalt, zu erstellen und Hochladen des Pakets für die Verwaltung des Kaufs zu implementieren und Downloadvorgang, mit dem Beispielcode für diesen Artikel.

### <a name="sample-code"></a>Beispielcode

Das Beispielprojekt *HostedNonConsumables* (in StoreKitiOS6.zip) verwendet, die Inhalte gehostet. Die app bietet zwei "Buchkapitel" für den Verkauf, der Inhalt für die auf der Apple Server gehostet wird. Der Inhalt besteht aus einer Textdatei und ein Bild, jedoch wesentlich komplexer Inhalt in einer echten Anwendung verwendet werden kann.

Die app lautet wie folgt vor, während und nach einem Kauf:

 [![](changes-to-storekit-images/image5.png "Die app sieht wie folgt vor, während und nach einem Kauf")](changes-to-storekit-images/image5.png#lightbox)

Die Textdatei und Image werden heruntergeladen und in Dokumente im Verzeichnis der Anwendung kopiert. Weitere Informationen zu den verschiedenen Verzeichnissen, die für den Anwendungsspeicher, finden Sie unter den [Datei Systemdokumentation](~/ios/app-fundamentals/file-system.md).

## <a name="itunes-connect"></a>iTunes Connect

Beim Erstellen neuer Produkte, die Apple verwendet den Inhalt hosten, werden Sie sicher, dass die **nicht verwendbar** Produkttyp. Andere Product-Typen unterstützt Hosten von Inhalten nicht. Darüber hinaus aktivieren Sie nicht, Hosten von Inhalten für *vorhandenen* Produkte, die Sie verkaufen; nur auf das Hosten von Inhalten für neue Produkte aktivieren.

 [![](changes-to-storekit-images/image6.png "Wählen Sie den Product-Typ nicht verwendbar")](changes-to-storekit-images/image6.png#lightbox)

Geben Sie einen **Produkt-ID**. Diese ID wird später erforderlich sein, wenn Sie den Inhalt für dieses Produkt zu erstellen.

 [![](changes-to-storekit-images/image7.png "Geben Sie eine Produkt-ID")](changes-to-storekit-images/image7.png#lightbox)

Hosten von Inhalten ist im Detailabschnitt festgelegt. Vor der liveschaltung in app-Käufe, deaktivieren Sie die **Host-Inhalten mit Apple** Kontrollkästchen, wenn Sie möchten zum abzubrechen (selbst wenn Sie einige testinhalte hochgeladen haben). Jedoch kann nicht Hosten von Inhalten entfernt, nachdem die in-app-Käufe live geschaltet wurde.

 [![](changes-to-storekit-images/image8.png "Hosten von Inhalten mit Apple")](changes-to-storekit-images/image8.png#lightbox)

Sobald Sie aktiviert haben, auf dem Inhalt gehostet wird, geben das Produkt Sie **des Uploads gewartet** Status und diese Meldung angezeigt:

 [![](changes-to-storekit-images/image9.png "Das Produkt wird Wartezeit für Uploadstatus eingeben und diese Meldung anzeigen")](changes-to-storekit-images/image9.png#lightbox)

Content-Paket mit Xcode erstellt werden soll, und das Archiv-Tool mithilfe hochgeladen. Anweisungen zum Erstellen von Content-Pakete werden im nächsten Abschnitt angegeben **erstellen. PKG-Dateien**.

## <a name="creating-pkg-files"></a>Wird erstellt. PKG-Dateien

Die Inhaltsdateien, die Sie in Apple hochladen, müssen die folgenden Einschränkungen einhalten:

- 2 GB Größe darf nicht überschreiten.
- Darf keine enthalten ausführbaren Code (oder symbolische Verknüpfungen, die außerhalb des Inhalts zu verweisen).
- Muss ordnungsgemäß formatiert sein (einschließlich einer **plist** Datei) und eine **.pkg** Dateierweiterung. Dies wird automatisch ausgeführt werden, wenn Sie diese mithilfe von Xcode Anweisungen befolgen.

Sie können viele verschiedene Dateien und Arten von Dateien, hinzufügen, solange sie diese Einschränkungen erfüllen. Der Inhalt wird komprimiert, die vor der Übermittlung Ihrer Anwendung und vom Store Kit entpackt werden, bevor Ihr Code zugegriffen.

Nach dem Hochladen eines Inhaltspakets können sie durch neuere Inhalte ersetzt werden. Neuer Inhalte muss hochgeladen und für die Überprüfung/Genehmigung, über den normalen Prozess gesendet werden. Erhöhen der `ContentVersion` Feld aktualisierte Content-Pakete, um anzugeben, dass es neuer ist.

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode-App-Kauf Content-Projekte

Erstellen Pakete von Inhalten für in-app-Käufe Produkte aktuell ist Xcode erforderlich. Es gibt keine OBJECTIVE-C-Codierung erforderlich. Xcode wurde einen neuer Projekttyp für diese Pakete, der nur die Dateien und einer plist-Datei enthält.

Unsere beispielanwendung enthält Kapitel des Buches für den Verkauf – jedes Kapitel Content-Paket enthält:

-  eine Textdatei, und
-  ein Bild, das Kapitel darstellen.


Wählen Sie zunächst **Datei > Neues Projekt** im Menü auswählen und **In-App-Kauf Content**:

 [![](changes-to-storekit-images/image10.png "Wählen Sie In App-Käufe Inhalt")](changes-to-storekit-images/image10.png#lightbox)

Geben Sie die **Produktname** und **Unternehmensbezeichner** so, dass die **Bündel-ID** entspricht der **Produkt-ID** Sie eingegeben haben, klicken Sie in iTunes Verbinden Sie für dieses Produkt.

[![](changes-to-storekit-images/image11.png "Geben Sie den Namen und Bezeichner")](changes-to-storekit-images/image11.png#lightbox)

Nun Sie ein leeres müssen **In-App-Kauf Content** Projekt. Sie können mit der rechten Maustaste und **Dateien hinzufügen...** oder ziehen Sie sie in der **Projektnavigator**. Sicherstellen, dass die **ContentVersion** (Dies sollte bei 1.0 starten, aber wenn Sie später beim Aktualisieren Ihrer Inhalte, denken Sie daran, es erhöht) korrekt ist.

Dieser Screenshot zeigt Xcode, mit den Content-Dateien im Projekt und die im Hauptfenster sichtbar Plist-Einträge enthalten:

[![](changes-to-storekit-images/image12.png "Dieser Screenshot zeigt Xcode mit den Dateien im Projekt und die Plist-Einträge sichtbar im Hauptfenster von Inhalt")](changes-to-storekit-images/image12.png#lightbox)

Nachdem Sie alle Inhaltsdateien hinzugefügt haben können Sie dieses Projekt speichern und bearbeiten sie es später noch Mal, oder während des Uploads beginnt.

## <a name="uploading-pkg-files"></a>Hochladen. PKG-Dateien

Die einfachste Möglichkeit zum Hochladen von Content-Paketen ist mit der **Xcode-Archiv-Tools**. Wählen Sie **Product > Archiv** aus, um zu beginnen:

![](changes-to-storekit-images/image13.png "Auswählen von Archiven")

Das Inhaltspaket wird im Archiv wie unten dargestellt angezeigt. Der Archivtyp und das Symbol zeigen diese Zeile ist ein **In-App-Kauf Content Archiv**. Klicken Sie auf **überprüfen...** um unser Inhaltspaket auf Fehler überprüfen, ohne Ausführen des Uploads.

[![](changes-to-storekit-images/image14.png "Überprüfen Sie das Paket")](changes-to-storekit-images/image14.png#lightbox)

Melden Sie sich mit Ihrem iTunes Connect-Anmeldeinformationen:

[![](changes-to-storekit-images/image15.png "Melden Sie sich mit Ihrem iTunes Connect-Anmeldeinformationen")](changes-to-storekit-images/image15.png#lightbox)

Wählen Sie die richtige Anwendung und die in app-Käufe auf diesen Inhalt zuzuordnen:

[![](changes-to-storekit-images/image16.png "Wählen Sie die richtige Anwendung und in-app-Käufe, um diesen Inhalt zuzuordnen")](changes-to-storekit-images/image16.png#lightbox)

Daraufhin sollte eine Fehlermeldung wie etwa diesen Screenshot:

![Ein Beispiel für keine Meldung Probleme](changes-to-storekit-images/image17.png "ein Beispiel für die Nachricht keine Probleme")

Jetzt ein ähnlicher Prozess durchlaufen, sondern durch Klicken auf **verteilen...** den Inhalt wird tatsächlich hochgeladen werden.

[![Verteilen der app](changes-to-storekit-images/image18.png "verteilen die app")](changes-to-storekit-images/image18.png#lightbox)

Wählen Sie die erste Option, um den Inhalt hochladen:

![Den Inhalt hochladen](changes-to-storekit-images/image19.png "den Inhalt hochladen")

Melden Sie sich erneut an:

[![](changes-to-storekit-images/image15.png "Anmeldung")](changes-to-storekit-images/image15.png#lightbox)

Wählen Sie die richtige Anwendung und die in-app-Käufe Datensatz, den Inhalt hochladen:

[![](changes-to-storekit-images/image20.png "Wählen Sie den Eintrag für die Anwendung und in-app-Kauf")](changes-to-storekit-images/image20.png#lightbox)

Warten Sie, während die Dateien hochgeladen werden:

[![](changes-to-storekit-images/image21.png "Das Dialogfeld \"Hochladen\"")](changes-to-storekit-images/image21.png#lightbox)

Wenn der Upload abgeschlossen ist, wird eine Meldung angezeigt, informiert Sie, dass der Inhalt an den App Store übermittelt wurde.

[![](changes-to-storekit-images/image22.png "Eine beispielmeldung für den erfolgreichen upload")](changes-to-storekit-images/image22.png#lightbox)

Nach, die durchgeführt wurde, wenn Sie zurückkehren, auf der Seite "Product" in iTunes Connect wird die Paketdetails anzeigen und werden **senden möchten** Status. Wenn das Produkt dieser Status ist, können Sie die Tests in der sandboxumgebung beginnen. Sie müssen nicht "das Produkt für das Testen in der Sandbox-submit".

[![](changes-to-storekit-images/image23.png "iTunes Connect wird die Paketdetails anzeigen und bereit sein, zu Submit-status")](changes-to-storekit-images/image23.png#lightbox)

Es kann (z. b. einige Zeit dauern ein paar Minuten) zwischen hochladen, die Archivierungs- und der Status in iTunes Connect aktualisiert wird. Sie können das Produkt zur Überprüfung separat senden oder in Verbindung mit einer Anwendung Binär zu übermitteln. Nur verwendet werden, nachdem der Inhalt offiziell von Apple genehmigt wurde werden im Produktions-App-Store erworben, die in Ihrer app verfügbar.

### <a name="pkg-file-format"></a>PKG-Dateiformat

Mithilfe von Xcode und dem Archiv-Tool zum Erstellen und Hochladen eines gehosteten Inhaltspakets bedeutet, dass Sie nie den Inhalt des Pakets selbst. Die Dateien und Verzeichnisse in den Paketen, die erstellt werden, für die Darstellung des Beispiel-app wie im folgenden Screenshot mit der **plist-Datei** Datei in den Stammseiten und die Produktdateien in einer **Inhalt** Unterverzeichnis:

[![](changes-to-storekit-images/image24.png "Die Plist-Datei in den Stammseiten und die Produktdateien in einem Unterverzeichnis, das Inhalt")](changes-to-storekit-images/image24.png#lightbox)

Beachten Sie die Verzeichnisstruktur des Pakets (insbesondere den Speicherort der Dateien in die `Contents` Unterverzeichnis), da Sie benötigen diese Informationen zum Extrahieren der Dateien aus dem Paket auf dem Gerät zu verstehen.

### <a name="updating-package-content"></a>Aktualisieren von Paketinhalt

Das Verfahren zum Aktualisieren von Inhalt nach dem sie genehmigt wurde:

- Bearbeiten Sie das In-App-Kauf Content-Projekt in Xcode.
- Erhöhen der Versionsnummer.
- Laden in iTunes Connect erneut hoch. Benutzer, die bereits über die alte Version verfügen keine Benachrichtigung erhalten, aber nachfolgende zurück erhalten automatisch die neueste Version.
- Ihre app ist verantwortlich für die Benachrichtigung der Benutzer, und diese zum Abrufen einer neueren Version des Inhalts dazu zu ermutigen. Die app muss auch eine Funktion erstellen, die die neue Version, mit dem Feature Wiederherstellen des Store Kit heruntergeladen.
- Um festzustellen, ob eine neuere Version vorhanden ist, können Sie eine Funktion in Ihrer app zum Abrufen von SKProducts (z.B.) erstellen. derselbe Prozess, der zum Abrufen von Produktpreisen verwendet wird), und vergleichen Sie die Eigenschaft "ContentVersion".

## <a name="purchasing-overview"></a>Übersicht über die erwerben

Lesen Sie vor dem Lesen dieses Abschnitts, die die vorhandene [In App-Käufe Dokumentation](~/ios/platform/in-app-purchasing/index.md).

Die Abfolge der Ereignisse, die bei einem Produkt mit gehosteten Inhalt auftritt, wird gekauft und Download ist in diesem Diagramm dargestellt:

[![](changes-to-storekit-images/image25.png "Die Abfolge der Ereignisse, die bei einem Produkt mit gehosteten Inhalt auftritt, wird gekauft und herunterladen")](changes-to-storekit-images/image25.png#lightbox)

1. Neue Produkte können in iTunes Connect mit gehosteten Inhalt aktiviert erstellt werden. Der eigentliche Inhalt wird separat in Xcode (als einfach als ziehen-Dateien in einen Ordner) erstellt und dann archiviert und in iTunes (keine Codierung erforderlich ist) hochgeladen werden. Jedes Produkt wird dann zur Genehmigung übermittelt nach dem es erworben wird. Im Beispielcode diesen Produkt-IDs sind hartcodiert, aber das Hosten von Inhalten mit Apple ist flexibler, wenn Sie die Liste der verfügbaren Produkte auf einem Remoteserver speichern, damit er aktualisiert werden kann, wenn Sie neue Produkte und Inhalt in iTunes Connect übermitteln.
1. Wenn der Benutzer ein Produkt erwirbt, wird eine Transaktion in der Warteschlange Zahlung für die Verarbeitung platziert.
1. Store Kit leitet die Bestellanforderung an iTunes-Server zur Verarbeitung weiter.
1. Transaktion wird auf dem iTunes-Servern (z. b. abgeschlossen. Kunden wird berechnet) und eine Bestätigung an die app mit Produktinformationen, die angefügt werden, einschließlich gibt an, ob sie herunterladbare wird zurückgegeben (und gegebenenfalls die Größe und andere Metadaten).
1. Der Code sollte überprüfen, ob das Produkt zum Herunterladen, und wenn dies der Fall, stellen Sie eine Download des Inhalts-Anforderung, die auch für die Zahlung-Warteschlange platziert wird. Store Kit sendet diese Anforderung an die iTunes-Server.
1. Server gibt die Inhaltsdatei an Store Kit bietet einen Rückruf zum Fortschritt des Downloads und Schätzungen an Ihrem Code verbleibende Zeit zurück.
1. Nach Abschluss des Vorgangs erhalten Sie benachrichtigt, und übergeben einen Speicherort im Ordner "Cache".
1. Der Code sollte die Dateien kopieren und überprüfen, speichern Sie alle Status, die Sie benötigen, beachten Sie, dass das Produkt erworben wurde. Nutzen Sie die Gelegenheit, richtig festzulegen, das backup-Flag auf die neuen Dateien (Hinweis: Wenn sie von einem Server stammen und nicht vom Benutzer bearbeitet werden, sollten Sie wahrscheinlich überspringen gestützt, da der Benutzer immer sie von Apple Servern in Zukunft abrufen kann).
1. Rufen Sie FinishTransaction. Dieser Schritt ist wichtig, da es sich bei die Transaktion aus der Warteschlange für die Zahlung wird entfernt. Es ist auch wichtig, dass Sie FinishTransaction bis nicht aufrufen, nachdem Sie den Inhalt aus dem Cache-Verzeichnis kopiert haben. Wenn Sie FinishTransaction aufrufen, werden die zwischengespeicherten Dateien wahrscheinlich schnell gelöscht werden.

## <a name="implementing-hosted-content-purchase"></a>Implementieren von gehosteten Inhalt erwerben

Lesen Sie die folgenden Informationen zusammen mit den vollständigen [In-App-Käufe Dokumentation](~/ios/platform/in-app-purchasing/index.md). Die Informationen in diesem Dokument konzentriert sich auf die Unterschiede zwischen gehosteten Inhalt und die vorherige Implementierung.

### <a name="classes"></a>Klassen

Die folgenden Klassen wurden hinzugefügt oder geändert werden, damit die Unterstützung für gehosteten Inhalt in iOS 6:

- **SKDownload** – neue Klasse, die eine heruntergeladen darstellt. Die API ermöglicht mehr als ein pro-Produkt, anfänglich jedoch nur eine implementiert wurde.
- **SKProduct** – neue Eigenschaften hinzugefügt: `Downloadable`, `ContentVersion`, `ContentLengths` Array.
- **SKPaymentTransaction** – neue Eigenschaft hinzugefügt: `Downloads`, enthält eine Auflistung von `SKDownload` Objekte an, wenn Sie dieses Produkt zum Download verfügbaren Inhalte gehostet hat.
- **SKPaymentQueue** – neue Methode hinzugefügt: `StartDownloads`. Beim Aufrufen dieser Methode `SKDownload` Objekte ihre gehosteten Inhalte abgerufen werden sollen. Herunterladen, kann im Hintergrund auftreten.
- **SKPaymentTransactionObserver** – neue Methode: `UpdateDownloads`. Store Kit ruft diese Methode mit Statusinformationen zur aktuellen Downloadvorgänge.

Details des neuen `SKDownload` Klasse:

- **Fortschritt** – ein Wert zwischen 0 und 1, die Sie verwenden können, um ein Statusanzeige-Indikator für den Benutzer anzuzeigen. Verwenden Sie keine Bearbeitung == 1 erkennen, ob der Download abgeschlossen ist, überprüfen Sie für die Zustände == fertig gestellt.
- **%Timeremaining** : Schätzung der Download verbleibende Zeit, in Sekunden. -1 bedeutet, dass es immer noch Schätzung berechnet wird.
- **Status** – aktiv ist, warten, abgeschlossen, fehlgeschlagen, angehalten, abgebrochen.
- **ContentURL** – Speicherort, in dem der Inhalt zu auf dem Datenträger in platzieren wurde, die `Cache` Verzeichnis. NUR aufgefüllt, wenn der Download abgeschlossen wurde.
- **Fehler** – diese Eigenschaft zu überprüfen, wenn der Status fehlgeschlagen ist.

In diesem Diagramm werden die Interaktionen zwischen den Klassen im Beispielcode angezeigt (der Code, der spezifisch für den gehosteten Inhalt Käufe werden in grün angezeigt):

[![](changes-to-storekit-images/image26.png "Gehosteten Inhalt Einkäufe ist Grün in diesem Diagramm dargestellte")](changes-to-storekit-images/image26.png#lightbox)

Der Beispielcode, in denen diese Klassen verwendet wurden, wird in der übrige Teil dieses Abschnitts dargestellt:

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

Ändern Sie die vorhandene `UpdatedTransactions` überschreiben, um die Überprüfung auf herunterladbare Inhalte, und rufen `StartDownloads` bei Bedarf:

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

Neue außer Kraft gesetzte Methode `UpdatedDownloads` ist unten dargestellt. Store Kit ruft diese Methode nach `StartDownloads` wird ausgelöst, `UpdatedTransactions`. Diese Methode wird aufgerufen, *mehrmals* in Intervallen unbestimmten bieten Ihnen Fortschritt beim Herunterladen und dann erneut nachdem der Download abgeschlossen ist. Beachten Sie, dass die Methode akzeptiert ein Array von `SKDownload` Objekte, sodass Sie jeden Aufruf einer Methode mit dem Status des mehrere Downloads in der Warteschlange bereitstellen kann. Siehe überprüft die nachfolgende Implementierung die Download-Status werden jeder Zeit und die zugehörigen Aktionen ausgeführt.

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Canceled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

Diese Klasse enthält eine neue Methode `SaveDownload` , die aufgerufen wird, nachdem jeder Download erfolgreich abgeschlossen wurde.

Der gehostete Inhalt wurde erfolgreich heruntergeladen und entzippt in wurde die `Cache` Verzeichnis. Die Struktur der. PKG-Datei muss alle Dateien gespeichert werden soll eine `Contents` Unterverzeichnis, damit Sie der folgenden Code aus werden die Dateien extrahiert die `Contents` Unterverzeichnis.

Der Code durchläuft alle Dateien in Content-Paket, und kopiert sie in der `Documents` Verzeichnis, in einem Unterordner mit dem Namen für die `ProductIdentifier`. Ruft schließlich `CompleteTransaction`, welche Aufrufe `FinishTransaction` So entfernen Sie die Transaktion aus der Warteschlange für die Zahlung.

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

Wenn `FinishTransaction` aufgerufen wird, die heruntergeladenen Dateien werden nicht mehr unbedingt in der `Cache` Verzeichnis. Alle Dateien kopiert werden sollen, vor dem Aufruf `FinishTransaction`.


## <a name="other-considerations"></a>Andere Überlegungen

Die oben stehenden Beispielcode veranschaulicht eine relativ einfache Implementierung der gehostete Inhalt erwerben. Es gibt weitere Aspekte, die Sie berücksichtigen müssen:

### <a name="detecting-updated-content"></a>Erkennen von aktualisierten Inhalten

Es ist, zwar möglich, Ihre gehosteten Inhalt Pakete zu aktualisieren bietet Store Kit keiner Mechanismen, um diese Updates für Benutzer zu verteilen, die bereits heruntergeladen und das Produkt gekauft haben. Zur Implementierung dieser Funktionalität kann Ihren Code die neue überprüfen `SKProduct.ContentVersion` Eigenschaft (wenn die `SKProduct` ist `Downloadable`) regelmäßig, und ermittelt, wenn der Wert erhöht wird. Alternativ könnten Sie eine Pushbenachrichtigung System erstellen.

### <a name="installing-updated-content-versions"></a>Installieren die aktualisierte Inhaltsversionen

Der obige Beispielcode überspringt Kopieren von Dateien, wenn die Datei bereits vorhanden ist. Dies ist nicht empfiehlt sich, wenn Sie neuere Versionen von Inhalten, die heruntergeladen unterstützen möchten.

Eine Alternative wäre, kopieren Sie den Inhalt in einen Ordner mit dem Namen für die Version, und Verfolgen der ist, dass die aktuelle Version (z. b. in `NSUserDefaults` oder ganz egal, wo Sie abgeschlossene Kaufdatensätze speichern).

### <a name="restoring-transactions"></a>Wiederherstellen von Transaktionen

Wenn `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` wird aufgerufen, im Store Kit gibt alle vorherige Transaktionen für den Benutzer zurück. Wenn sie eine große Anzahl von Elementen erworben haben oder die erworbenen große Content-Paketen hat, klicken Sie dann verursachen die Wiederherstellung viel Netzwerkdatenverkehr wie alles, was zum Download auf einmal in die Warteschlange ruft.

Erwägen Sie nachzuverfolgen, ob ein Produkt aus dem tatsächlichen Download des zugehörigen Content-Paket separat erworben wurde.

### <a name="pausing-restarting-and-canceling-downloads"></a>Anhalten, Neustarten und Abbrechen von Downloads

Obwohl der Code dieser Funktion nicht veranschaulicht, ist es möglich, Anhalten und Neustarten der gehostete Herunterladen von Inhalte. Die `SKPaymentQueue.DefaultQueue` verfügt über Methoden zum `PauseDownloads`, `ResumeDownloads` und `CancelDownloads`.

Wenn der Code ruft `FinishTransaction` für die Zahlung Warteschlange vor dem Download wird `Finished` und diesen Download automatisch abgebrochen wird.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>Das SKIP-Backup-Flag festlegen für den heruntergeladenen Inhalt

Apple iCloud-Backup-Richtlinien wird empfohlen, dass Nichtbenutzercode-Inhalte, die leicht von einem Server wiederhergestellt werden sollte *nicht* werden gesichert wurden (weil es unnötig iCloud-Speicher verwenden würde). Weitere Informationen zum Festlegen der backup-Attribut finden Sie unter den [Dateisystem](~/ios/app-fundamentals/file-system.md) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden zwei neue Features des Store Kit in iOS6: kaufen, iTunes und andere Inhalte aus Ihrer App und die Verwendung von Apple-Server zum Hosten von eigene in-app-Käufe. Diese Einführung in Verbindung mit dem vorhandenen lesen [In App-Käufe Dokumentation](~/ios/platform/in-app-purchasing/index.md) für die vollständige Abdeckung für die Implementierung Store Kit-Funktionalität.

## <a name="related-links"></a>Verwandte Links

- [StoreKit (Beispiel)](https://developer.xamarin.com/samples/StoreKit/)
- [In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)
- [Frameworkverweis StoreKit](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController-Klassenreferenz](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes-Suche-API-Referenz](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC-Video: Verkauf von Produkten mit dem Store Kit](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
