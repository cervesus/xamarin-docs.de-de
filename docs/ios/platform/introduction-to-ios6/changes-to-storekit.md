---
title: Änderungen an StoreKit in iOS 6
description: 'mit IOS 6 werden zwei Änderungen an der Store-Kit-API eingeführt: die Möglichkeit, iTunes-Produkte (und App Store/ibookstore) in Ihrer APP anzuzeigen und eine neue in-App-Kaufoption, bei der Apple ihre herunterladbaren Dateien hostet. In diesem Dokument wird erläutert, wie diese Funktionen mit xamarin. IOS implementiert werden.'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 7cf18934c70acf59213a697ab57b6c5e308e7b2a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725221"
---
# <a name="changes-to-storekit-in-ios-6"></a>Änderungen an StoreKit in iOS 6

_IOS 6 hat zwei Änderungen an der Store-Kit-API eingeführt: die Möglichkeit, iTunes-Produkte (und App Store/ibookstore) in Ihrer APP anzuzeigen und eine neue in-App-Kaufoption, bei der Apple ihre herunterladbaren Dateien hostet. In diesem Dokument wird erläutert, wie diese Funktionen mit xamarin. IOS implementiert werden._

Die wichtigsten Änderungen am Store Kit in iOS6 sind die folgenden beiden neuen Features:

- **Anzeige von in-App-Inhalten & Einkauf** – Benutzer können apps, Musik, Bücher und andere iTunes-Inhalte kaufen und herunterladen, ohne Ihre APP zu überlassen. Sie können auch eine Verknüpfung mit ihren eigenen apps herstellen, um den Einkauf zu fördern, oder Sie können nur Überprüfungen und Bewertungen
- **In-App-Käufe gehostete Inhalte** – Apple speichert und liefert die Inhalte, die ihren in-App-Kauf Produkten zugeordnet sind. Dadurch entfällt die Notwendigkeit eines separaten Servers zum Hosten Ihrer Dateien, das automatische Herunterladen von Inhalten wird automatisch unterstützt, und Sie können weniger Code schreiben.

Ausführliche Informationen zu den storekit-APIs finden Sie in den [in-App-Kauf](~/ios/platform/in-app-purchasing/index.md) Handbüchern.

## <a name="requirements"></a>Requirements (Anforderungen)

Die in diesem Dokument erläuterten Store-Kit-Features erfordern IOS 6 und Xcode 4,5 zusammen mit xamarin. IOS 6,0.

## <a name="in-app-content-display--purchasing"></a>Anzeige von in-App-Inhalten & Einkauf

Das neue in-App-Einkaufs Feature in ios ermöglicht Benutzern das Anzeigen von Produktinformationen und das kaufen oder Herunterladen des Produkts in Ihrer APP.
Frühere Anwendungen müssten iTunes, den App Store oder den ibookstore (ibookstore) auslöst, was dazu führen würde, dass der Benutzer die ursprüngliche Anwendung verlässt. Mit dieser neuen Funktion wird der Benutzer automatisch an die APP zurückgegeben, wenn Sie abgeschlossen sind.

[![](changes-to-storekit-images/image1.png "Automatically returning to an app after purchase")](changes-to-storekit-images/image1.png#lightbox)

Beispiele für die Verwendung dieses Beispiels:

- **Benutzer werden dazu ermutigt, Ihre APP zu bewerten** – Sie können die Seite "App Store" öffnen, sodass der Benutzer Ihre APP bewerten und überprüfen kann, ohne Sie zu belassen.
- **Cross-propagieren von apps** – Hiermit können Benutzer andere apps anzeigen, die Sie veröffentlichen, und Sie können sofort kaufen/herunterladen.
- Unter **Stützung von Benutzern beim Suchen und Herunterladen von Inhalten** – unterstützen von Benutzern beim Kauf von Inhalten, die Ihre APP findet, verwaltet oder aggregiert (z.b. eine musikbezogene app könnte eine Wiedergabeliste von Liedern bereitstellen und jedem Song das kaufen innerhalb der APP ermöglichen.

Nachdem der `SKStoreProductViewController` angezeigt wurde, kann der Benutzer mit den Produktinformationen interagieren, als ob er sich in iTunes, im App Store oder in ibookstore befand. Der Benutzer kann folgende Aktionen ausführen:

- Screenshots anzeigen (für apps)
- Beispiel Videos oder Videos (für Musik, Fernsehsendungen und Filme),
- Lese-und Schreib Überprüfungen
- Kaufen Sie & Download, der vollständig im Ansichts Controller und im Store-Kit erfolgt.

Einige Optionen in der `SKStoreProductViewController` erzwingen weiterhin, dass der Benutzer Ihre APP verlässt und die relevante Store-APP öffnet, z. b. das Klicken auf **Verwandte Produkte** oder den **Support** Link einer App.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

Die API zum Anzeigen eines Produkts in einer beliebigen APP ist einfach: Es ist nur erforderlich, dass Sie eine `SKStoreProductViewController`erstellen und anzeigen. Führen Sie die folgenden Schritte aus, um ein Produkt zu erstellen und anzuzeigen:

1. Erstellen Sie ein `StoreProductParameters` Objekt, um Parameter an den Ansichts Controller zu übergeben, einschließlich des `productId` im Konstruktor.
1. Instanziieren Sie die `SKProductViewController`. Weisen Sie diese einem Feld auf Klassenebene zu.
1. Weisen Sie dem `Finished`-Ereignis des Ansichts Controllers einen Handler zu, der den Ansichts Controller verwerfen sollte. Dieses Ereignis wird aufgerufen, wenn der Benutzer Abbrechen drückt. Andernfalls schließt eine Transaktion im Ansichts Controller ab.
1. Ruft die `LoadProduct`-Methode auf, die die `StoreProductParameters` und einen Vervollständigungs Handler übergibt. Der Abschluss Handler sollte überprüfen, ob die Produktanforderung erfolgreich war. wenn dies der Fall ist, wird der `SKProductViewController` modale dargestellt. Die entsprechende Fehlerbehandlung sollte hinzugefügt werden, falls das Produkt nicht abgerufen werden kann.

### <a name="example"></a>Beispiel

Das *ProductView* -Projekt im *storekit* -Beispielcode für diesen Artikel implementiert eine `Buy` Methode, die die Apple-ID eines beliebigen Produkts annimmt und die `SKStoreProductViewController`anzeigt. Der folgende Code zeigt die Produktinformationen für eine beliebige Apple-ID an:

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

Die APP sieht wie der folgende Screenshot aus, wenn die Ausführung von – heruntergeladen oder der Einkauf vollständig innerhalb der `SKStoreProductViewController`erfolgt:

[![](changes-to-storekit-images/image2.png "The app looks like this when running")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>Unterstützung älterer Betriebssysteme

Die Beispielanwendung enthält Code, der zeigt, wie der App Store, iTunes oder der ibookstore in früheren Versionen von IOS geöffnet wird. Verwenden Sie die `OpenUrl`-Methode, um eine ordnungsgemäß erstellte **iTunes.com** -URL zu öffnen.

Sie können eine Versions Überprüfung implementieren, um zu bestimmen, welcher Code ausgeführt werden soll, wie hier gezeigt:

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

### <a name="errors"></a>Errors

Der folgende Fehler tritt auf, wenn die von Ihnen verwendete Apple-ID ungültig ist. Dies kann verwirrend sein, da es sich um ein Netzwerk-oder Authentifizierungs Problem handelt.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Lesen der Ziel-C-Dokumentation

Entwickler, die das Store-Kit im Entwickler Portal von Apple lesen, sehen ein Protokoll – [skstoreproductviewcontrollerdelegat](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) –, das im Zusammenhang mit diesem neuen Feature erläutert wird. Das delegatprotokoll verfügt nur über eine Methode – productviewcontrollerdidfinish –, die im `SKStoreProductViewController` in xamarin. IOS als `Finished` Ereignis verfügbar gemacht wurde.

## <a name="determining-apple-ids"></a>Bestimmen von Apple-IDs

Die für den `SKStoreProductViewController` erforderliche Apple-ID ist eine *Zahl* (nicht zu verwechseln mit Bündel-IDs wie "com. xamarin. mwc2012"). Es gibt verschiedene Möglichkeiten, wie Sie die Apple-ID für Produkte ermitteln können, die Sie anzeigen möchten, die unten aufgeführt sind:

### <a name="itunesconnect"></a>iTunesConnect

Für Anwendungen, die Sie veröffentlichen, ist es einfach, die **Apple-ID** in iTunes Connect zu finden:

[![](changes-to-storekit-images/image3.png "Finding the Apple ID in iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />

### <a name="search-api"></a>Search-API

Apple stellt eine dynamische Such-API bereit, um alle Produkte im App Store, iTunes und ibookstore abzufragen. Informationen zum Zugriff auf die Such-API finden Sie in den Partner Ressourcen von Apple, obwohl die API für beliebige Personen (nicht nur registrierte Unternehmen) verfügbar gemacht wird. Der resultierende JSON-Code kann analysiert werden, um die `trackId` zu ermitteln, die die Apple-ID ist, die mit `SKStoreProductViewController`verwendet werden soll.

Die Ergebnisse enthalten auch andere Metadaten, einschließlich Anzeigeinformationen und Grafik-URLs, mit denen das Produkt in Ihrer APP dargestellt werden kann.

Im Folgenden finden Sie einige Beispiele:

- **iBooks-App** – [https://itunes.apple.com/search?term=ibooks&amp; Entity = Software&amp;Land = US](https://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us)
- **Punkt und das Känguru iBook** – [https://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; Entity = eBook&amp;Country = US](https://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us)

### <a name="enterprise-partner-feed"></a>Enterprise-Partner Feed

Apple bietet genehmigte Partnern ein vollständiges Daten Abbild aller Produkte in Form von herunterladbaren Flatfiles, die für die Datenbank bereit sind. Wenn Sie sich für den Zugriff auf den Enterprise-Partner Feed qualifizieren, finden Sie die Apple-ID für ein beliebiges Produkt in diesem DataSet.

Viele Benutzer des Enterprise-Partner Feeds sind Mitglieder des Partner [Programms](https://www.apple.com/itunes/affiliates) , das das verdienen von Aufträgen für den Produktverkauf ermöglicht. `SKStoreProductViewController` unterstützt keine Partner-IDs (zum Zeitpunkt des Schreibens).

### <a name="direct-product-links"></a>Direkte Produkt Verknüpfungen

Die Apple-ID für ein Produkt kann über den Link iTunes Preview URL abgeleitet werden.
In allen iTunes-Produkt Links (für apps, Musik oder Bücher) finden Sie den Teil der URL, beginnend mit `id`, und verwenden Sie die folgende Nummer.

Der direkte Link zu iBooks lautet z. b.

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

und die Apple-ID ist **364709193**. Ebenso gilt für die MWC2012-App der direkte Link.

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

und die Apple-ID ist **496963922**.

## <a name="in-app-purchase-hosted-content"></a>In-App-Käufe gehostete Inhalte

Wenn Ihre in-App-Käufe aus herunter ladbarem Inhalt bestehen (z. b. Bücher oder andere Medien, Art und Konfiguration auf Spielebene oder andere große Dateien), dann werden diese Dateien auf dem Webserver gehostet, und apps mussten Code enthalten, um Sie sicher herunterzuladen. zulegen. Ab IOS 6 hostet Apple Ihre Dateien auf Ihren Servern, wodurch kein separater Server mehr benötigt wird. Die-Funktion ist nur für nicht nutzbare Produkte verfügbar (nicht für die Verwendbarkeit oder Abonnements). Zu den Vorteilen der Verwendung des Apple-Hostingdiensts gehören:

- Sparen Sie die Kosten für Hosting & Bandbreite.
- Wahrscheinlich skalierbarer als der Server Host, den Sie zurzeit verwenden.
- Weniger Code zum Schreiben, da Sie keine serverseitige Verarbeitung erstellen müssen.
- Das Herunterladen im Hintergrund ist für Sie implementiert.

Hinweis: das Testen von gehosteten in-App-Kauf Inhalten im IOS-Simulator wird nicht unterstützt, daher müssen Sie mit einem echten Gerät testen.

### <a name="hosted-content-basics"></a>Grundlagen zu gehosteten

Vor IOS 6 gab es zwei Möglichkeiten, ein Produkt bereitzustellen (in der Dokumentation zu [xamarin in-App-Käufe](~/ios/platform/in-app-purchasing/index.md) ausführlicher beschrieben):

- **Integrierte Produkte** – Features, die durch den Einkauf "entsperrt" werden, aber in die Anwendung integriert sind (entweder als Code oder als eingebettete Ressourcen). Beispiele für integrierte Produkte sind entsperrtes Fotofilter oder in-Game-Power-ups.
- Vom **Server übermittelte Produkte** – nach dem Kauf muss die Anwendung Inhalt von einem Server herunterladen, auf dem Sie arbeiten. Dieser Inhalt wird während des Kaufs heruntergeladen, auf dem Gerät gespeichert und dann als Teil der Bereitstellung des Produkts gerendert. Beispiele hierfür sind Bücher, Magazine-Probleme oder Spielebenen, die aus Hintergrundgrafiken und Konfigurationsdateien bestehen.

In ios 6 bietet Apple eine Variation der von einem Server gelieferten Produkte an: Sie hosten Ihre Inhalts Dateien auf Ihren Servern. Dies vereinfacht die Erstellung von Server Produkten, da Sie keinen separaten Server betreiben müssen, und das Store-Kit bietet Funktionen zum Herunterladen im Hintergrund, die Sie zuvor selbst schreiben mussten. Um das Hosting von Apple zu nutzen, aktivieren Sie das Hosting von Inhalten für neue in-App-Produkte, und ändern Sie den Store-Kit-Code, um ihn zu nutzen. Produkt Inhalts Dateien werden dann mithilfe von Xcode erstellt und zur Überprüfung und Freigabe auf die Server von Apple hochgeladen.

[![](changes-to-storekit-images/image4.png "The build and deliver process")](changes-to-storekit-images/image4.png#lightbox)

Wenn Sie den App Store für den in-App-Einkauf *mit gehosteten Inhalten* verwenden, sind folgende Setup-und Konfigurationsschritte erforderlich:

- **iTunes Connect** – Sie *müssen* Ihre Bank-und Steuerinformationen an Apple weitergegeben haben, damit Sie in Ihrem Namen erfasste Gelder durchsetzen können. Anschließend können Sie die zu verkaufenden Produkte konfigurieren und Sandbox-Benutzerkonten einrichten, um den Einkauf zu testen.  _Außerdem müssen Sie gehostete Inhalte für die nicht nutzbaren Produkte konfigurieren, die Sie mit Apple hosten möchten_.
- **IOS-Bereitstellungs Portal** – Erstellen eines Bündel Bezeichners und Aktivieren des App Store-Zugriffs für Ihre APP, wie für jede Anwendung, die den in-App-Einkauf unterstützt.
- **Store-Kit** – Hinzufügen von Code zu Ihrer APP zum Anzeigen von Produkten, kaufen von Produkten und Wiederherstellen von Transaktionen.  _In ios 6 Store Kit verwaltet außerdem das Herunterladen Ihrer Produktinhalte im Hintergrund mit Fortschritts Aktualisierungen._
- **Benutzerdefinierter Code** – um Käufe von Kunden zu verfolgen und die Produkte oder Dienste bereitzustellen, die Sie gekauft haben. Verwenden Sie neue IOS 6 Store Kit-Klassen wie `SKDownload`, um die von Apple gehosteten Inhalte abzurufen.

In den folgenden Abschnitten wird erläutert, wie gehostete Inhalte, von der Erstellung und dem Hochladen des Pakets zum Verwalten des Kauf-und Downloadvorgangs, mithilfe des Beispielcodes für diesen Artikel implementiert werden.

### <a name="sample-code"></a>Beispielcode

Im Beispiel Projekt *hostednonverbrauchs* (in StoreKitiOS6. zip) werden gehostete Inhalte verwendet. Die APP bietet zwei "Buchkapitel" für den Verkauf. der Inhalt, für den auf den Servern von Apple gehostet wird. Der Inhalt besteht aus einer Textdatei und einem Bild, obwohl viel komplexere Inhalte in einer echten Anwendung verwendet werden können.

Die APP sieht wie folgt aus, während und nach einem Kauf:

 [![](changes-to-storekit-images/image5.png "The app looks like this before, during and after a purchase")](changes-to-storekit-images/image5.png#lightbox)

Die Textdatei und das Bild werden heruntergeladen und in das Verzeichnis "Dokumente" der Anwendung kopiert. Weitere Informationen zu den verschiedenen Verzeichnissen, die für den Anwendungs Speicher verfügbar sind, finden Sie in der [Dateisystem Dokumentation](~/ios/app-fundamentals/file-system.md).

## <a name="itunes-connect"></a>iTunes Connect

Wenn Sie neue Produkte erstellen, die das Hosting von Inhalten von Apple verwenden, stellen Sie sicher, dass Sie den **nicht** verwendbaren Produkttyp auswählen. Bei anderen Produkttypen wird das Hosting von Inhalten nicht unterstützt. Außerdem sollten Sie das Hosting von Inhalten für *vorhandene* Produkte, die Sie verkaufen, nicht aktivieren. Aktivieren Sie nur das Hosting von Inhalten für neue Produkte.

 [![](changes-to-storekit-images/image6.png "Select the Non-Consumable product type")](changes-to-storekit-images/image6.png#lightbox)

Geben Sie eine **Produkt-ID**ein. Diese ID wird später bei der Erstellung des Inhalts für dieses Produkt benötigt.

 [![](changes-to-storekit-images/image7.png "Enter a Product ID")](changes-to-storekit-images/image7.png#lightbox)

Das Hosting von Inhalten wird im Detail Abschnitt festgelegt. Bevor der in-App-Einkauf aktiv ist, deaktivieren Sie das Kontrollkästchen **Host Inhalt mit Apple** , wenn Sie den Vorgang abbrechen möchten (auch wenn Sie einige Test Inhalte hochgeladen haben). Das Hosting von Inhalten kann jedoch nicht entfernt werden, nachdem der in-App-Einkauf online geschaltet wurde.

 [![](changes-to-storekit-images/image8.png "Hosting content with Apple")](changes-to-storekit-images/image8.png#lightbox)

Nachdem Sie das Hosting von Inhalten aktiviert haben, wird das Produkt auf den **Uploadstatus warten** und diese Meldung anzeigen:

 [![](changes-to-storekit-images/image9.png "The product will enter Waiting for Upload status and show this message")](changes-to-storekit-images/image9.png#lightbox)

Das Inhalts Paket sollte mit Xcode erstellt und mit dem Archiv Tool hochgeladen werden. Anweisungen zum Erstellen von Inhalts Paketen finden Sie im nächsten Abschnitt **Erstellen von. Pkg-Dateien**.

## <a name="creating-pkg-files"></a>Schufen. Pkg-Dateien

Die Inhalts Dateien, die Sie in Apple hochladen, müssen die folgenden Einschränkungen erfüllen:

- Die Größe von 2 GB darf nicht überschritten werden.
- Darf keinen ausführbaren Code enthalten (oder Symlinks, die außerhalb des Inhalts liegen).
- Muss korrekt formatiert sein (einschließlich einer **plist** -Datei) und über die Dateierweiterung " **. pkg** " verfügen. Dies erfolgt automatisch, wenn Sie diese Anweisungen mithilfe von Xcode befolgen.

Sie können viele verschiedene Dateien und Dateitypen hinzufügen, sofern diese Einschränkungen erfüllt sind. Der Inhalt wird vor der Übermittlung an Ihre Anwendung gezippt und vom Store Kit entzippt, bevor der Code darauf zugreift.

Nach dem Hochladen eines Inhalts Pakets kann es durch neueren Inhalt ersetzt werden. Neuer Inhalt muss hochgeladen und zur Überprüfung/Genehmigung über den normalen Prozess übermittelt werden. Erhöhen Sie das Feld `ContentVersion` in aktualisierten Inhalts Paketen, um anzugeben, dass es neuer ist.

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode-in-App-Kauf Inhalts Projekte

Das Erstellen von Inhalts Paketen für in-App-Produkte erfordert derzeit Xcode. Es ist keine Ziel-C-Codierung erforderlich. Xcode verfügt über einen neuen Projekttyp für diese Pakete, der nur Ihre Dateien und eine plist enthält.

Unsere Beispielanwendung enthält Buchkapitel für den Verkauf – jedes Kapitel Inhalts Paket enthält Folgendes:

- eine Textdatei, und
- ein Bild, das das Kapitel darstellt.

Wählen Sie zunächst im Menü **Datei > Neues Projekt** aus, und wählen Sie **in-App-Kauf Inhalt**aus:

 [![](changes-to-storekit-images/image10.png "Choose In-App Purchase Content")](changes-to-storekit-images/image10.png#lightbox)

Geben Sie den **Produktnamen** und die **Unternehmens Kennung** so ein, dass die **Bündel** -ID mit der in iTunes Connect eingegebenen **Produkt-ID** für dieses Produkt übereinstimmt.

[![](changes-to-storekit-images/image11.png "Enter the  Name and Identifier")](changes-to-storekit-images/image11.png#lightbox)

Nun verfügen Sie über ein leeres **in-App-Einkaufs Inhalts** Projekt. Sie können mit der rechten Maustaste klicken und **Dateien hinzufügen...** oder ziehen Sie Sie in den **Projekt Navigator**. Stellen Sie sicher, dass **ContentVersion** korrekt ist (es sollte bei 1,0 gestartet werden, aber wenn Sie sich später entscheiden, den Inhalt zu aktualisieren, denken Sie daran, ihn zu erhöhen).

Dieser Screenshot zeigt Xcode mit den Inhalts Dateien, die im Projekt enthalten sind, und die plist-Einträge, die im Hauptfenster sichtbar sind:

[![](changes-to-storekit-images/image12.png "This screenshot shows Xcode with the content files included in the project and the plist entries visible in the main window")](changes-to-storekit-images/image12.png#lightbox)

Nachdem Sie alle Inhalts Dateien hinzugefügt haben, können Sie dieses Projekt speichern und später erneut bearbeiten oder den Uploadvorgang starten.

## <a name="uploading-pkg-files"></a>Hochladen. Pkg-Dateien

Die einfachste Möglichkeit zum Hochladen von Inhalts Paketen ist das **Xcode Archive-Tool**. Wählen Sie im Menü den Befehl **Product > Archive** aus, um zu beginnen:

![](changes-to-storekit-images/image13.png "Choose Archiven")

Das Inhalts Paket wird dann wie unten dargestellt im Archiv angezeigt.
Der archivistyp und das Symbol zeigen diese Zeile ist ein **in-App-Kauf Inhalts Archiv**. Klicken Sie auf überprüfen **...** zum Überprüfen des Inhalts Pakets auf Fehler, ohne den Upload tatsächlich auszuführen.

[![](changes-to-storekit-images/image14.png "Validate the package")](changes-to-storekit-images/image14.png#lightbox)

Melden Sie sich mit Ihren iTunes Connect-Anmelde Informationen an:

[![](changes-to-storekit-images/image15.png "Login with your iTunes Connect credentials")](changes-to-storekit-images/image15.png#lightbox)

Wählen Sie die richtige Anwendung und den in-App-Einkauf aus, um diesen Inhalt zuzuordnen:

[![](changes-to-storekit-images/image16.png "Choose the correct application and in-app purchase to associate this content with")](changes-to-storekit-images/image16.png#lightbox)

Es sollte eine Meldung wie der folgende Screenshot angezeigt werden:

![Ein Beispiel für eine Meldung ohne Probleme](changes-to-storekit-images/image17.png "Ein Beispiel für eine Meldung ohne Probleme")

Durchlaufen Sie nun einen ähnlichen Prozess, klicken Sie aber auf **verteilen...** der Inhalt wird tatsächlich hochgeladen.

[![Verteilen der APP](changes-to-storekit-images/image18.png "Verteilen der APP")](changes-to-storekit-images/image18.png#lightbox)

Wählen Sie die erste Option aus, um den Inhalt hochzuladen:

![Inhalt hochladen](changes-to-storekit-images/image19.png "Inhalt hochladen")

Erneut anmelden:

[![](changes-to-storekit-images/image15.png "Login in")](changes-to-storekit-images/image15.png#lightbox)

Wählen Sie den richtigen Anwendungs-und in-App-Kauf Daten Satz aus, um den Inhalt hochzuladen:

[![](changes-to-storekit-images/image20.png "Choose the application and in-app purchase record")](changes-to-storekit-images/image20.png#lightbox)

Warten Sie, während Ihre Dateien hochgeladen werden:

[![](changes-to-storekit-images/image21.png "The content upload dialog")](changes-to-storekit-images/image21.png#lightbox)

Wenn der Upload abgeschlossen ist, wird eine Meldung angezeigt, die Sie darüber informiert, dass der Inhalt an den App Store übermittelt wurde.

[![](changes-to-storekit-images/image22.png "An example successful upload message")](changes-to-storekit-images/image22.png#lightbox)

Wenn Sie die Produktseite auf iTunes Connect zurückkehren, werden die Paket Details angezeigt, und Sie können den Status über **Mitteln** . Wenn sich das Produkt in diesem Status befindet, können Sie mit dem Testen in der Sandbox Umgebung beginnen. Sie müssen das Produkt nicht für Tests in der Sandbox übermitteln.

[![](changes-to-storekit-images/image23.png "iTunes Connect it will show the package details and be in Ready to Submit status")](changes-to-storekit-images/image23.png#lightbox)

Dies kann einige Zeit in Anspruch nehmen (z. b. einige Minuten) zwischen dem Hochladen des Archivs und dem iTunes Connect-Status, der aktualisiert wird. Sie können das Produkt zur Überprüfung separat übermitteln oder in Verbindung mit einer Anwendungs Binärdatei übermitteln. Erst nachdem Apple die Inhalte offiziell genehmigt hat, ist es im Produktions-App-Store verfügbar und kann in Ihrer APP erworben werden.

### <a name="pkg-file-format"></a>Pkg-Datei Format

Wenn Sie Xcode und das Archiv-Tool verwenden, um ein gehostetes Inhalts Paket zu erstellen und hochzuladen, bedeutet dies, dass Sie den Inhalt des Pakets niemals sehen. Die Dateien und Verzeichnisse in den für die Beispiel-App erstellten Paketen sehen wie der folgende Screenshot aus, wobei sich die **plist** -Datei im Stammverzeichnis und die Produktdateien im Unterverzeichnis " **Content** " befinden:

[![](changes-to-storekit-images/image24.png "The plist file in the root and the product files in a Contents subdirectory")](changes-to-storekit-images/image24.png#lightbox)

Beachten Sie die Verzeichnisstruktur des Pakets (insbesondere den Speicherort der Dateien im `Contents` Unterverzeichnis), da Sie diese Informationen kennen müssen, um die Dateien aus dem Paket auf dem Gerät zu extrahieren.

### <a name="updating-package-content"></a>Paket Inhalt wird aktualisiert.

Die Vorgehensweise zum Aktualisieren von Inhalten, nachdem Sie genehmigt wurde:

- Bearbeiten Sie das Projekt in-App Purchase Content in Xcode.
- Bump-Versionsnummer aufwärts.
- Erneutes hochladen in iTunes Connect. Nachfolgende Käufer erhalten automatisch die neueste Version, aber Benutzer, die bereits über die alte Version verfügen, erhalten keine Benachrichtigung.
- Ihre APP ist dafür verantwortlich, Benutzer zu benachrichtigen und Sie dazu zu ermutigen, eine neuere Version des Inhalts abzurufen. Die APP muss außerdem eine Funktion erstellen, die die neue Version mithilfe der Wiederherstellungs Funktion von Store Kit herunterlädt.
- Um zu ermitteln, ob eine neuere Version vorhanden ist, können Sie ein Feature in Ihrer APP erstellen, um skproducts abzurufen (z. b. der gleiche Prozess, der zum Abrufen von Produktpreisen verwendet wird, und Vergleichen der ContentVersion-Eigenschaft.

## <a name="purchasing-overview"></a>Übersicht über den Einkauf

Lesen Sie vor dem Lesen dieses Abschnitts die vorhandene [Dokumentation zum in-App-Einkauf](~/ios/platform/in-app-purchasing/index.md).

Die Abfolge der Ereignisse, die auftreten, wenn ein Produkt mit gehosteten Inhalten gekauft und heruntergeladen wird, wird in diesem Diagramm veranschaulicht:

[![](changes-to-storekit-images/image25.png "The sequence of events that occurs when a product with hosted content is purchased and download")](changes-to-storekit-images/image25.png#lightbox)

1. Neue Produkte können in iTunes Connect mit aktiviertem gehostetem Inhalt erstellt werden. Der tatsächliche Inhalt wird separat in Xcode erstellt (so einfach wie das Ziehen von Dateien in einen Ordner) und anschließend archiviert und in iTunes hochgeladen (es ist keine Codierung erforderlich). Alle Produkte werden dann zur Genehmigung übermittelt, und anschließend werden Sie für den Kauf verfügbar. Im Beispielcode sind diese Produkt-IDs hart codiert, aber das Hosting von Inhalten mit Apple ist flexibler, wenn Sie die Liste der verfügbaren Produkte auf einem Remote Server speichern, sodass Sie aktualisiert werden kann, wenn Sie neue Produkte und Inhalte an iTunes Connect übermitteln.
1. Wenn der Benutzer ein Produkt erwirbt, wird eine Transaktion zur Verarbeitung in der Zahlungs Warteschlange abgelegt.
1. Store Kit leitet die Kauf Anforderung zur Verarbeitung an iTunes-Server weiter.
1. Die Transaktion wurde auf den iTunes-Servern abgeschlossen (z. b. der Kunde wird in Rechnung gestellt), und es wird eine Bestätigung an die APP zurückgegeben, wobei Produktinformationen angefügt werden, z.b. ob Sie heruntergeladen werden können (und wenn ja, die Dateigröße und andere Metadaten).
1. Ihr Code sollte überprüfen, ob das Produkt heruntergeladen werden kann. wenn dies der Fall ist, sollten Sie eine Anforderung zum Herunterladen von Inhalten überprüfen Store Kit sendet diese Anforderung an die iTunes-Server.
1. Der Server gibt die Inhalts Datei an das Store-Kit zurück, die einen Rückruf bereitstellt, um den Download Fortschritt und die verbleibenden verbleibenden Schätzwerte an
1. Sobald der Vorgang beendet ist, erhalten Sie eine Benachrichtigung und einen Datei Speicherort im Cache Ordner.
1. Der Code sollte die Dateien kopieren und überprüfen. speichern Sie jeden Zustand, den Sie in Erinnerung behalten müssen, um sich zu merken, dass das Produkt gekauft wurde. Nehmen Sie diese Gelegenheit, das Sicherungsflag für die neuen Dateien ordnungsgemäß festzulegen (Hinweis: Wenn Sie von einem Server stammen und nie vom Benutzer bearbeitet werden, sollten Sie die Sicherung möglicherweise überspringen, da der Benutzer Sie in Zukunft immer von den Servern von Apple abrufen kann).
1. Aufrufen von finishtransaction. Dieser Schritt ist wichtig, da er die Transaktion aus der Zahlungs Warteschlange entfernt. Es ist auch wichtig, dass Sie finishtransaction erst aufrufen, nachdem Sie den Inhalt aus dem Cache Verzeichnis kopiert haben. Nachdem Sie finishtransaction aufgerufen haben, werden die zwischengespeicherten Dateien wahrscheinlich schnell bereinigt.

## <a name="implementing-hosted-content-purchase"></a>Implementieren von Kauf von gehosteten Inhalten

Die folgenden Informationen sollten in Verbindung mit der Dokumentation zum kompletten [in-App-Einkauf](~/ios/platform/in-app-purchasing/index.md)gelesen werden. Die Informationen in diesem Dokument konzentrieren sich auf die Unterschiede zwischen gehosteten Inhalten und der vorherigen Implementierung.

### <a name="classes"></a>Klassen

Die folgenden Klassen wurden zur Unterstützung von gehosteten Inhalten in ios 6 hinzugefügt oder geändert:

- **Skdownload** – neue Klasse, die einen laufenden Download darstellt. Die API ermöglicht mehr als ein Produkt pro Produkt, aber anfänglich wurde nur eine implementiert.
- **Skproduct** – neue Eigenschaften hinzugefügt: `Downloadable`, `ContentVersion``ContentLengths` Array.
- **Skpaymenttransaction** – neue Eigenschaft hinzugefügt: `Downloads`, die eine Auflistung von `SKDownload` Objekten enthält, wenn dieses Produkt gehostete Inhalte zum Herunterladen verfügbar ist.
- **Skpaymentqueue** – neue Methode hinzugefügt: `StartDownloads`. Rufen Sie diese Methode mit `SKDownload`-Objekten auf, um ihren gehosteten Inhalt abzurufen. Das herunterladen kann im Hintergrund erfolgen.
- **Skpaymenttransaktionobserver** – neue Methode: `UpdateDownloads`. Store Kit ruft diese Methode mit Fortschrittsinformationen zu aktuellen Download Vorgängen auf.

Details zur neuen `SKDownload`-Klasse:

- Status **– ein** Wert zwischen 0-1, den Sie verwenden können, um einen Prozentwert Indikator für den Benutzer anzuzeigen. Verwenden Sie "Progress = = 1" nicht, um zu ermitteln, ob der Download abgeschlossen ist, überprüfen Sie den Status = = abgeschlossen.
- **Timeremaineing** – Schätzung der verbleibenden Downloadzeit (in Sekunden). -1 bedeutet, dass die Schätzung noch berechnet wird.
- **Status** – aktiv, gewartet, abgeschlossen, Fehler, angehalten, abgebrochen.
- **Contenturl** – Datei Speicherort, an dem der Inhalt im `Cache` Verzeichnis auf dem Datenträger abgelegt wurde. Wird nur aufgefüllt, sobald der Download abgeschlossen ist.
- **Fehler** – überprüfen Sie diese Eigenschaft, wenn der Status "failed" lautet.

Die Interaktionen zwischen den Klassen im Beispielcode sind in diesem Diagramm dargestellt (der Code, der für den Erwerb von gehosteten Inhalten spezifisch ist, wird grün angezeigt):

[![](changes-to-storekit-images/image26.png "Hosted content purchases is shown in green in this diagram")](changes-to-storekit-images/image26.png#lightbox)

Der Beispielcode, in dem diese Klassen verwendet wurden, wird im restlichen Teil dieses Abschnitts angezeigt:

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>Custompaymentobserver (skpaymenttransaktionobserver)

Ändern Sie die vorhandene `UpdatedTransactions` außer Kraft setzung, um nach herunter ladbarem Inhalt zu suchen und `StartDownloads` bei Bedarf aufzurufen:

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

Die neue überschriebene Methode `UpdatedDownloads` wird unten dargestellt. Das Store-Kit ruft diese Methode auf, nachdem `StartDownloads` in `UpdatedTransactions`ausgelöst wurde. Diese Methode wird mehrmals in unbestimmten *Intervallen aufgerufen,* um den Fortschritt des Downloads bereitzustellen, und dann erneut, wenn der Download abgeschlossen ist. Beachten Sie, dass die-Methode ein Array von `SKDownload` Objekten akzeptiert, sodass jeder Methoden aufrufzug den Status mehrerer Downloads in der Warteschlange bereitstellen kann. Wie in der folgenden-Implementierung gezeigt, werden die Download Status jedes Mal geprüft, und die entsprechende Aktion wird durchgeführt.

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

Diese Klasse enthält eine neue Methode `SaveDownload`, die aufgerufen wird, nachdem jeder Download erfolgreich abgeschlossen wurde.

Der gehostete Inhalt wurde erfolgreich heruntergeladen und in das `Cache` Verzeichnis entzippt. Die Struktur von. Die pkg-Datei erfordert, dass alle Dateien in einem `Contents` Unterverzeichnis gespeichert werden, sodass der nachfolgende Code Dateien aus dem Unterverzeichnis `Contents` extrahiert.

Der Code durchläuft alle Dateien im Inhalts Paket und kopiert sie in das `Documents` Verzeichnis, in einem Unterordner, der für die `ProductIdentifier`benannt ist. Schließlich wird `CompleteTransaction`aufgerufen, der `FinishTransaction` aufruft, um die Transaktion aus der Zahlungs Warteschlange zu entfernen.

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

Wenn `FinishTransaction` aufgerufen wird, werden die heruntergeladenen Dateien nicht mehr im `Cache` Verzeichnis angezeigt. Vor dem Aufrufen von `FinishTransaction`sollten alle Dateien kopiert werden.

## <a name="other-considerations"></a>Weitere Überlegungen

Der obige Beispielcode veranschaulicht eine relativ einfache Implementierung des Erwerbs von gehosteten Inhalten. Sie müssen einige zusätzliche Punkte beachten:

### <a name="detecting-updated-content"></a>Erkennen von aktualisierten Inhalten

Obwohl es möglich ist, ihre gehosteten Inhalts Pakete zu aktualisieren, bietet Store Kit keinen Mechanismus, um diese Updates an Benutzer zu übertragen, die das Produkt bereits heruntergeladen und gekauft haben. Um diese Funktionalität zu implementieren, kann Ihr Code die neue `SKProduct.ContentVersion`-Eigenschaft (wenn die `SKProduct` `Downloadable`) regelmäßig überprüfen und erkennen, ob der Wert inkrementiert wird. Alternativ dazu können Sie auch ein pushbenachrichtigungssystem erstellen.

### <a name="installing-updated-content-versions"></a>Installieren aktualisierter Inhalts Versionen

Der obige Beispielcode überspringt das Kopieren von Dateien, wenn die Datei bereits vorhanden ist. Dies ist keine gute Idee, wenn Sie neuere Versionen der Inhalte unterstützen möchten, die heruntergeladen werden.

Eine Alternative besteht darin, den Inhalt in einen Ordner mit dem Namen für die Version zu kopieren und nachzuverfolgen, welche die aktuelle Version ist (z. b. in `NSUserDefaults` oder wo Sie abgeschlossene Kauf Datensätze speichern).

### <a name="restoring-transactions"></a>Wiederherstellen von Transaktionen

Wenn `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` aufgerufen wird, gibt das Store-Kit alle vorherigen Transaktionen für den Benutzer zurück. Wenn Sie eine große Anzahl von Elementen gekauft haben oder bei jedem Kauf umfangreiche Inhalts Pakete vorhanden sind, kann die Wiederherstellung viel Netzwerk Datenverkehr zur Folge haben, wenn alles auf einmal heruntergeladen wird.

Denken Sie daran, ob ein Produkt separat vom eigentlichen Download des zugehörigen Inhalts Pakets gekauft wurde.

### <a name="pausing-restarting-and-canceling-downloads"></a>Anhalten, Neustarten und Abbrechen von Downloads

Obwohl der Beispielcode diese Funktion nicht veranschaulicht, ist es möglich, gehostete Downloads von Inhalten anzuhalten und neu zu starten. Der `SKPaymentQueue.DefaultQueue` verfügt über Methoden für `PauseDownloads`, `ResumeDownloads` und `CancelDownloads`.

Wenn der Code `FinishTransaction` in der Zahlungs Warteschlange aufruft, bevor der Download `Finished` wird, wird dieser Download automatisch abgebrochen.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>Festlegen des Flags zum Überspringen der Sicherung für den heruntergeladenen Inhalt

Die icloud-Sicherungs Richtlinien von Apple deuten darauf hin, dass Nichtbenutzer Inhalte, die auf einfache Weise von einem Server wieder hergestellt werden sollen, *nicht* gesichert werden sollten (weil der icloud-Speicher unnötig genutzt werden würde). Weitere Informationen zum Festlegen des Backup-Attributs finden Sie in der [Dateisystem](~/ios/app-fundamentals/file-system.md) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden zwei neue Features von Store Kit in iOS6 eingeführt: der Erwerb von iTunes und anderen Inhalten innerhalb Ihrer APP und die Verwendung des Apple-Servers zum Hosten Ihrer eigenen in-App-Käufe. Diese Einführung sollte in Verbindung mit der vorhandenen [in-App-Kauf Dokumentation](~/ios/platform/in-app-purchasing/index.md) gelesen werden, um eine umfassende Abdeckung der Implementierung der Store-Kit-Funktionalität zu erhalten.

## <a name="related-links"></a>Verwandte Links

- [Storekit (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit)
- [In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)
- [Referenz zum storekit-Framework](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [Skstoreproductviewcontroller-Klassenreferenz](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [Skdownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [Skpaymentqueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [Skproduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC-Video: Verkauf von Produkten mit dem Store-Kit](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
