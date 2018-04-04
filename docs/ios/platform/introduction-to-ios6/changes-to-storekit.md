---
title: Änderungen an StoreKit
description: 'iOS 6 führt zwei Änderungen auf die Store-Kit-API: die Möglichkeit zum Anzeigen von iTunes (und Store-App/iBookstore) innerhalb der app und eine neue in-app Produkte kaufen Option Apple, in dem Ihre Dateien zum Herunterladen hostet. Dieses Dokument erläutert, wie diese Funktionen mit Xamarin.iOS implementiert wird.'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8a7a70c3f84518141cf44d630fb4137051d0c866
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="changes-to-storekit"></a>Änderungen an StoreKit

_iOS 6 führt zwei Änderungen auf die Store-Kit-API: die Möglichkeit zum Anzeigen von iTunes (und Store-App/iBookstore) innerhalb der app und eine neue in-app Produkte kaufen Option Apple, in dem Ihre Dateien zum Herunterladen hostet. Dieses Dokument erläutert, wie diese Funktionen mit Xamarin.iOS implementiert wird._

Die wichtigen Änderungen Store Kit in iOS6 werden diese zwei neuen Funktionen:

-   **In-App-Inhalte anzeigen & Purchasing** – Benutzer kaufen und Inhalts-apps herunterladen "," Musik "," Books "und" andere iTunes ohne die app verlassen zu müssen. Sie können auch auf Ihre eigenen apps höher stufen, Erwerb oder nur empfehlen Bewertungen und Bewertungen verknüpfen. 
-   **Gehostet in App-Inhalte erwerben** – Apple gespeichert wird und die zugeordneten Inhalte mit Ihren Produkten in app-Käufe beseitigt die Notwendigkeit für einen separaten Server zum Hosten der Dateien, automatisch im Hintergrund heruntergeladen unterstützt und können Sie übermitteln geschrieben Sie weniger Code werden. 


Es wird empfohlen, dieses Dokument gelesen werden, zusammen mit den vorhandenen Xamarin.iOS [In App-Käufe](~/ios/platform/in-app-purchasing/index.md) Dokumentation.

## <a name="requirements"></a>Anforderungen

Die Store-Kit-Funktionen, die in diesem Dokument beschriebenen erforderlich ist, iOS 6 und Xcode 4.5 zusammen mit Xamarin.iOS 6.0.


## <a name="in-app-content-display--purchasing"></a>Inhalt in-App-Anzeige & erwerben

Das neue Kaufverhalten in app-Feature in iOS kann Benutzer Produktinformationen anzuzeigen, und erwerben, oder das Produkt in Ihrer app herunterladen.
Clientanwendungen müssten zuvor auslösen iTunes, dem App Store oder die iBookstore, was der Benutzer, die die ursprüngliche Anwendung führen würde. Diese neue Funktion gibt den Benutzer automatisch zu Ihrer app zurück, wenn sie fertig sind.

 [![](changes-to-storekit-images/image1.png "Automatisch zurückgeben nach dem Kauf an eine Anwendung")](changes-to-storekit-images/image1.png#lightbox)

Es gibt eine Reihe von Szenarien, in denen dies hilfreich sein kann, einschließlich (aber nicht beschränkt auf):

-   **Ermutigen Benutzer zu Ihrer app bewerten** –, damit der Benutzer kann bewerten und Ihre app überprüfen, ohne ihn verlassen zu müssen, öffnen Sie die App Store-Seite können. 
-   **Cross-Förderung apps** – ermöglicht dem Benutzer andere apps, die Sie veröffentlichen, können Sie mit der Fähigkeit zum Kauf/sofort herunterladen, finden Sie unter. 
-   **Unterstützung von Benutzern suchen und Herunterladen von Inhalten** : unterstützen von Benutzern, die Inhalte zu kaufen, die Ihre app sucht, verwaltet oder aggregiert (z. b. eine app Musik-bezogene konnte Geben Sie eine Wiedergabeliste von Titeln und jeder Titel aus, die innerhalb der app erworben werden können). 


Sobald die `SKStoreProductViewController` angezeigt wurde der Benutzer kann mit der Produktinformationen interagieren, als wären sie im iTunes App Store oder die iBookstore. Dies umfasst Folgendes:

-  Anzeigen von Screenshots (für apps)
-  Sampling-Titel oder Video (für Musik, TV-Serien und Filme)
-  Prüfen der lesen (und Schreiben)
-  Erwerben und herunterladen, erfolgt die vollständig in die View-Controller und die Store-Kit. Die Anwendung muss nicht bieten keinen zusätzlichen Code, damit dies funktioniert. 


Beachten Sie, dass einige Optionen in der `SKStoreProductViewController` werden trotzdem Force lassen Ihre app, und öffnen Sie die relevanten Store-app, z. B. durch Klicken auf den Benutzer **verwandte Produkte** oder eine app **Unterstützung** Link.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

Die API anzuzeigende ein Produkt in einer app ist sehr einfach: Es erfordert nur, dass Sie beim Erstellen und Anzeigen einer `SKStoreProductViewController`. Führen Sie diese Schritte zum Erstellen und Anzeigen eines Produkts:

1.  Erstellen einer `StoreProductParameters` Objekt zum Übergeben von Parametern zum View-Controller, einschließlich der `productId` im Konstruktor. 
1.  Instanziieren der `SKProductViewController` . Weisen sie einem Servicelevel Klassenfeld. 
1.  Weisen Sie einen Handler mit des View-Controllers `Finished` -Ereignis, das die View-Controller verworfen werden sollen. Dieses Ereignis wird aufgerufen, wenn der Benutzer drückt "Abbrechen". oder andernfalls schließt eine Transaktion in der Ansicht Controller ab. 
1.  Rufen Sie die `LoadProduct` -Methode übergeben der `StoreProductParameters` und eine Abschlusshandler. Der Abschlusshandler überprüfen Sie, dass die Product-Anforderung erfolgreich war, und wenn dies der Fall ist, stellen die `SKProductViewController` modal. Für den Fall, dass das Produkt nicht abgerufen werden kann, sollten entsprechende Fehlerbehandlung hinzugefügt werden. 

### <a name="example"></a>Beispiel

Die *ProductView* -Projekt in der *StoreKit* Beispielcode für diesen Artikel implementiert eine `Buy` -Methode, die ein Produkt akzeptiert Apple-ID des und zeigt die `SKStoreProductViewController`. Der folgende Code zeigt die Produktinformationen für alle angegebenen Apple-ID:

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

Die app sieht wie folgt beim Ausführen der Download oder der Erwerb erfolgt ausschließlich innerhalb – die `SKStoreProductViewController`:

 [![](changes-to-storekit-images/image2.png "Die app sieht wie folgt, Ausführung")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>Unterstützung von älteren Betriebssystemen

Die beispielanwendung enthält Code, der zeigt, wie in früheren Versionen von iOS App Store, iTunes oder die iBookstore geöffnet. Verwenden der `OpenUrl` Methode, um eine ordnungsgemäß gestaltete öffnen **itunes.com** URL.

Sie können eine versionsüberprüfung, um zu bestimmen, welcher Code ausgeführt werden, wie folgt implementieren:

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

Der folgende Fehler wird angezeigt, wenn die Apple-ID, die Sie verwenden nicht gültig ist, die seit dem verwirrend sein kann ein Netzwerk- oder Authentifizierungsfehler Problem irgendeine impliziert.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Objective-C-Dokumentation lesen

Entwickler, die Informationen zu Store-Kit für Apple Entwicklerportal sehen ein Protokoll – [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – in Bezug auf dieses neue Feature erläutert. Der Delegat Protokoll hat nur eine Methode – ProductViewControllerDidFinish – die als verfügbar gemacht wurde die `Finished` Ereignis auf der `SKStoreProductViewController` in Xamarin.iOS.


## <a name="determining-apple-ids"></a>Bestimmen die Apple-IDs

Die Apple-ID, die erforderlich sind, indem Sie die `SKStoreProductViewController` ist ein *Anzahl* (nicht zu verwechseln mit Paket-IDs wie "com.xamarin.mwc2012"). Es gibt mehrere Möglichkeiten, die Sie die Apple-ID für die Produkte, die angezeigt werden sollen ermitteln können, im folgenden aufgeführt:

### <a name="itunesconnect"></a>iTunesConnect

Für Anwendungen, die Sie veröffentlichen, leicht zu finden ist die **Apple ID** in iTunes Connect:

 [![](changes-to-storekit-images/image3.png "Suchen die Apple-ID in iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />


### <a name="search-api"></a>Search-API

Apple bietet es sich um eine dynamische-Suchdienst-API zum Abfragen aller Produkte in den App Store, iTunes- und die iBookstore. Informationen zum Such-API zugreifen werden, finden Sie unter [Apple Partneradministratoren Ressourcen](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html), obwohl die API für Personen, die (nicht nur registrierte Tochtergesellschaften) verfügbar gemacht wird. Die resultierende JSON analysiert werden kann, um zu ermitteln der `trackId` also die Apple-ID für die Verwendung mit `SKStoreProductViewController`.

Die Ergebnisse enthalten auch andere Metadaten, einschließlich Anzeigeinformationen und Bildmaterial-URLs, die verwendet werden können, um das Produkt in Ihrer app zu rendern.

Hier einige Beispiele:

-   **iBooks app*- [http://itunes.apple.com/search?term=ibooks&amp;Entität = Software&amp;Country = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **Punkt und dem Ibooks Kangaroo*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;Entität = e-Book&amp;Country = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>Enterprise-Partner-Feed

Apple bietet genehmigte Partnern einen vollständigen Daten Dump ihre Produkte in Form von Flatfiles der herunterladbare Datenbank bereit. Wenn Sie für den Zugriff auf Qualifizieren der [Enterprise Partner Feed](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html) dann die Apple-ID für jedes Produkt im Dataset gefunden werden können.

Beachten Sie, dass viele Benutzer von den Enterprise-Partner-Feed gehören die [Partnerprogramm](http://www.apple.com/itunes/affiliates) , mit der Provisionen, die für Produktverkäufe erworben werden. `SKStoreProductViewController` Partneranwendungen IDs nicht unterstützt (zum Zeitpunkt der Verfassung; Dies kann hinzugefügt werden von Apple in Zukunft).

### <a name="direct-product-links"></a>Direkte Verknüpfungen Produkt

Die Apple-ID für ein Produkt können die iTunes-Vorschau-URL-Link abgeleitet werden.
In jeder iTunes suchen produktlink (für apps, Musik oder Bücher) den Teil der URL beginnend mit `id` und verwenden Sie die Nummer, die folgt.

Beispielsweise ist der direkte Link zum iBooks

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

und die Apple-ID ist **364709193**. In ähnlicher Weise ist für die app MWC2012 der direkte link

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

und die Apple-ID ist **496963922**.

## <a name="in-app-purchase-hosted-content"></a>In App-Käufe gehosteten Inhalt

Wenn Ihre app-Käufe bestehen Sie aus herunterladbare Inhalt (z. B. Bücher oder andere Medien, Spiel Ebene Grafiken und Konfiguration oder anderen großen Dateien), und klicken Sie dann diese Dateien verwendet, auf dem Webserver gehostet werden, und apps mussten Integrieren von Code zum sicheren nach dem Herunterladen erwerben. In iOS wurde 6 Apple eingeführt, eine Option aus, in dem sie Ihre Dateien auf ihren Servern gehostet wird, die Erfordernis für einen separaten Server. Das Feature ist nur für nicht verwendbar Produkte (nicht umgewandelt oder Abonnements) zur Verfügung. Mithilfe des Apple-Hostingdienst bietet folgende Vorteile:

-  Hosting & Bandbreite Kosten zu speichern.
-  Wahrscheinlich besser skalierbar als den Server-Host, die Sie derzeit verwenden. 
-  Weniger Code geschrieben werden, da Sie keine serverseitige Verarbeitung erstellen, werden soll. 
-  Herunterladen der Hintergrund wird für Sie implementiert.


Hinweis: Testen in app-Käufe Inhalt in iOS-Simulator wird nicht unterstützt, die gehostet, sodass Sie mit einem echten Gerät testen müssen.

### <a name="hosted-content-basics"></a>Grundlagen der gehosteten Inhalt

Bevor Sie iOS 6, gab es zwei Möglichkeiten, ein Produkt liefern (ausführlich in [Xamarin In-App-Käufe](~/ios/platform/in-app-purchasing/index.md) Dokumentation):

-   **Integrierte Produkte** – Funktionen, sind "entsperrt" durch den Kauf jedoch die in der Anwendung (entweder als Code oder eingebettete Ressourcen) integriert sind. Beispiele für integrierte Produkte sind entsperrt Foto-Filter oder im Spiel Power Sicherungen. 
-   **Server bereitgestellte Produkte** – nach dem Kauf, muss die Anwendung Inhalt herunterladen, von einem Server, die Sie ausgeführt werden. Dieser Inhalt ist während der Bestellung heruntergeladen, auf dem Gerät gespeichert und dann im Rahmen der Bereitstellung des Produkts gerendert. Beispiele sind Bücher, Magazin Probleme oder Spiel Ebenen, die Art und die Konfigurationsdateien Hintergrund bestehen. 


IOS 6 Apple bietet eine Variation des Produkte Server übermittelt: sie werden die Inhaltsdateien auf ihren Servern hosten. Dadurch wesentlich einfacher Server bereitgestellte Produkte zu erstellen, da Sie nicht erforderlich sind, einen separaten Server ausgeführt werden, und Store Kit Hintergrund herunterladen Funktionen bereitstellt, die Sie zuvor bereits profitierten selbst schreiben. Apple hosting nutzen möchten, aktivieren Sie Content hosting für neue in-app-Käufe Produkte, und ändern Sie den Store-Kit-Code, um nutzen. Inhaltsdateien Produkt werden dann mithilfe von Xcode erstellt und in der Apple Server für die Überprüfung und Version hochgeladen.

 [![](changes-to-storekit-images/image4.png "Erstellungs-und Bereitstellung")](changes-to-storekit-images/image4.png#lightbox)

Verwenden Sie den App Store von in-app zu erwerben *mit gehosteten Inhalt* erfordert die folgenden Setup und Konfiguration:

-   **iTunes Connect** – Sie *müssen* Ihre Bankgeschäfte und Steuer-Informationen wurden an Apple angegeben werden, damit der Betrag in Ihrem Auftrag erfasst überwiesen werden können. Anschließend können Sie konfigurieren, dass Produkte verkaufen und Sandbox-Benutzerkonten zum Erwerb von testen einrichten.  *Sie müssen auch gehosteten Inhalt konfigurieren**für diese nicht verwendbar-Produkte, die Sie mit Apple hosten möchten* *.* 
-   **iOS-Bereitstellungsportal** – erstellen Sie ein Paket-ID und App-Store-Zugriffs für Ihre app aktivieren, wie Sie für jede Anwendung, die in der app zu erwerben unterstützt. 
-   **Speichern von Kit** – Hinzufügen von Code für Ihre Anwendung auf das Anzeigen von Produkten, Produkte kaufen und Wiederherstellung von Transaktionen.  *In iOS 6 verwalten das Herunterladen des Inhalts Produkt, im Hintergrund mit Statusupdates auch Store Kit.* 
-   **Benutzerdefinierter Code** – zum Nachverfolgen der Einkäufe von Kunden, und geben Sie die Produkte oder Dienste, die sie gekauft haben. Nutzen Sie die neue Ios6 Store-Kit-Klassen wie `SKDownload` zum Abrufen des Inhalts von Apple gehostet wird. 


In den folgenden Abschnitten wird erläutert, wie gehosteten Inhalt, erstellen und Hochladen des Pakets für die Verwaltung des Kauf implementieren und Downloadvorgang, verwenden den Beispielcode für diesen Artikel.


### <a name="sample-code"></a>Beispielcode

Das Beispielprojekt *HostedNonConsumables* (in StoreKitiOS6.zip) veranschaulicht, wie eine app zu erstellen, dass der Inhalt gehostet wird. Die app bietet zwei "Buchkapitel" für den Verkauf, der Inhalt für die auf der Apple Server gehostet wird. Der Inhalt besteht aus einer Textdatei und ein Bild, jedoch wesentlich komplexer Inhalt in einer realen Anwendung verwendet werden kann.

Die app sieht wie folgt vor, während und nach einem Kauf:

 [![](changes-to-storekit-images/image5.png "Die app sieht wie folgt vor, während und nach einem Kauf")](changes-to-storekit-images/image5.png#lightbox)

Die Textdatei und Image werden heruntergeladen und in Verzeichnis "Dokumente" der Anwendung kopiert. Finden Sie unter der [arbeiten mit der Dateisystem-Dokumentation](~/ios/app-fundamentals/file-system.md) für Weitere Informationen zu den verschiedenen Verzeichnissen, die zum Speichern der Anwendung verfügbar.

## <a name="itunes-connect"></a>iTunes Connect

Erstellen neuer Produkte, die von Apple verwenden, werden der Inhalt des hosting Achten Sie beim Auswählen der **nicht verwendbar** Product-Typs. Andere Typen unterstützen keine Inhalt hosten. Darüber hinaus aktivieren Sie nicht für die Inhalte gehostet *vorhandenen* Produkte, das Sie verkaufen; aktivieren Sie nur für neue Produkte Inhalt hosten.

 [![](changes-to-storekit-images/image6.png "Wählen Sie die Product-Typs nicht verwendbar")](changes-to-storekit-images/image6.png#lightbox)

Geben Sie einen **Produkt-ID**. Dies wird später erforderlich sein, wenn Sie den Inhalt für dieses Produkt erstellt.

 [![](changes-to-storekit-images/image7.png "Geben Sie eine Produkt-ID")](changes-to-storekit-images/image7.png#lightbox)

Inhalt wird im Detailbereich festgelegt. Deaktivieren Sie vor der in-app-Käufe, die Livedaten einfach das Kontrollkästchen "Host Inhalt mit Apple" gegebenenfalls auf "Abbrechen" (auch wenn Sie einige testinhalte hochgeladen haben). Jedoch kann nicht Inhalt entfernt, nachdem der in-app-Käufe veröffentlicht wurde.

 [![](changes-to-storekit-images/image8.png "Hosten von Inhalt bei Apple")](changes-to-storekit-images/image8.png#lightbox)

Nachdem Sie aktiviert haben, auf dem Inhalt gehostet wird, geben das Produkt Sie **hochladen warten** Status und diese Nachricht anzeigen:

 [![](changes-to-storekit-images/image9.png "Das Produkt wird Geben Sie wartende für Uploadstatus und diese Nachricht anzeigen")](changes-to-storekit-images/image9.png#lightbox)

Der Inhalt muss jetzt mit Xcode erstellt werden und mit dem Tool Archiv hochgeladen. Anweisungen zum Erstellen von Inhaltspaketen erhält im nächsten Abschnitt **erstellen. PKG-Dateien**.

## <a name="creating-pkg-files"></a>Erstellen. PKG-Dateien

Die Inhaltsdateien, die Sie in Apple hochladen müssen die folgenden Einschränkungen einhalten:

-  Darf 2 GB Größe nicht überschreiten.
-  Sind keine ausführbaren Code (oder symbolische Verknüpfungen, die außerhalb des Inhalts an). 
-  Muss ordnungsgemäß formatiert sein (einschließlich plist-Datei) und eine Erweiterung ".pkg-Datei" haben. Dies wird automatisch konfiguriert werden, wenn Sie die folgenden Anweisungen, die mithilfe von Xcode ausführen. 


Sie können viele verschiedene Dateien und Arten von Dateien, hinzufügen, solange sie diese Einschränkungen erfüllen. Der Inhalt vor der Übermittlung für Ihre Anwendung ZIP und von Store-Kit entpackt haben, bevor er auf Code zugreift.

Nach dem Hochladen eines Inhaltspakets, kann es durch neuere Inhalt ersetzt werden. Neue Inhalte muss hochgeladen und zum Überprüfen/Genehmigung über den normalen Prozess gesendet werden. Erhöhen Sie die `ContentVersion` Feld aktualisierte Content-Paketen.

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode In-App Kauf Content-Projekte

Erstellen Pakete den Inhalt für in-app-Käufe Produkte derzeit ist Xcode erforderlich. Es ist keine OBJECTIVE-C-Codierung erforderlich. Xcode verfügt über einen neuen Projekttyp für diese Pakete, der nur die Dateien und eine Plist enthält.

Unsere beispielanwendung hat Buchkapitel für den Verkauf – jedes Kapitel Content-Paket enthält:

-  eine Textdatei, und
-  ein Bild, das im Kapitel über das darstellen.


Dazu starten **Datei > Neues Projekt** wählen Sie im Menü auswählen und **In-App Kauf Content**:

 [![](changes-to-storekit-images/image10.png "Wählen Sie In der App-Käufe Inhalt")](changes-to-storekit-images/image10.png#lightbox)

Geben Sie die **Produktname** und **Unternehmensbezeichner** so, dass die **Paket-ID** entspricht der **Produkt-ID** Sie eingegeben haben, im iTunes Verbinden Sie sich für dieses Produkt.

 [![](changes-to-storekit-images/image11.png "Geben Sie den Namen und Bezeichner")](changes-to-storekit-images/image11.png#lightbox)

Nun Sie eine leere müssen **In-App Kauf Content** Projekt. Sie können mit der rechten Maustaste und **Dateien hinzufügen...** oder ziehen Sie sie in der **Projektnavigators**. Sicherstellen, dass die **ContentVersion** (er sollte an 1.0 beginnen, denken Sie aber bei Auswahl später, um Ihren Inhalt aktualisieren increment) richtig ist.

Diese bildschirmabbildung zeigt Xcode mit der Inhaltsdateien im Projekt und die im Hauptfenster sichtbar Plist-Einträge enthalten:

 [![](changes-to-storekit-images/image12.png "Diese bildschirmabbildung zeigt Xcode mit der Inhaltsdateien im Projekt und die Plist-Einträge sichtbar im Hauptfenster enthalten")](changes-to-storekit-images/image12.png#lightbox)

Nachdem Sie alle Inhaltsdateien hinzugefügt haben können Sie dieses Projekt speichern und bearbeiten es später erneut, oder den Uploadprozess zu beginnen.

## <a name="uploading-pkg-files"></a>Hochladen. PKG-Dateien

Die einfachste Möglichkeit zum Hochladen der Content-Pakete wird mit der **Xcode Archiv Tool**. Wählen Sie **Product > Archiv** aus, um zu beginnen:

 ![](changes-to-storekit-images/image13.png "Wählen Sie Archiven")

Content-Paket wird dann in das Archiv angezeigt, wie unten dargestellt. Beachten Sie, dass die Archivtyp und Symbol an Dies ist ein **In-App Kauf Content Archiv**. Klicken Sie auf **überprüfen...** um unser Inhaltspaket auf Fehler überprüfen, ohne den Upload tatsächlich preforming.

 [![](changes-to-storekit-images/image14.png "Überprüfen Sie das Paket")](changes-to-storekit-images/image14.png#lightbox)

Melden Sie sich mit Ihrem iTunes Connect-Anmeldeinformationen:

 [![](changes-to-storekit-images/image15.png "Melden Sie sich mit Ihrem iTunes Connect-Anmeldeinformationen")](changes-to-storekit-images/image15.png#lightbox)

Wählen Sie die richtige Anwendung und die in app-Käufe zu diesem Inhalt zuzuordnen:

 [![](changes-to-storekit-images/image16.png "Wählen Sie die richtige Anwendung und die in-app-Käufe dieser Inhalt mit zuordnen")](changes-to-storekit-images/image16.png#lightbox)

Daraufhin sollte eine Meldung wie folgt:

 ![](changes-to-storekit-images/image17.png "Ein Beispiel für die Nachricht keine Probleme")

Nun einem ähnlichen Prozess durchlaufen, jedoch durch Klicken auf **verteilen...** den Inhalt wird tatsächlich hochgeladen werden.

 [![](changes-to-storekit-images/image18.png "Verteilen der app")](changes-to-storekit-images/image18.png#lightbox)

Wählen Sie die erste Option, um den Inhalt hochladen:

 ![](changes-to-storekit-images/image19.png "Den Inhalt hochladen")

Melden Sie sich erneut:

 [![](changes-to-storekit-images/image15.png "Anmeldung bei")](changes-to-storekit-images/image15.png#lightbox)

Wählen Sie die richtige Anwendung und die in app-Käufe Datensatz, den Inhalt hochladen:

 [![](changes-to-storekit-images/image20.png "Wählen Sie die Anwendung und in-app-Purchase-Datensatz")](changes-to-storekit-images/image20.png#lightbox)

Warten Sie, während die Dateien hochgeladen werden:

 [![](changes-to-storekit-images/image21.png "Das Dialogfeld "inhaltsupload"")](changes-to-storekit-images/image21.png#lightbox)

Wenn das Hochladen abgeschlossen ist, wird eine Meldung angezeigt, um Sie darüber zu informieren, dass der Inhalt auf den App Store übermittelt wurde.

 [![](changes-to-storekit-images/image22.png "Eine beispielmeldung für den erfolgreichen upload")](changes-to-storekit-images/image22.png#lightbox)

Nach, die ausgeführt wurde, wenn Sie zurück zur Seite "Product" in iTunes Connect wird die Paketdetails anzeigen und werden **bereit zum Senden** Status. Wenn das Produkt dieser Status ist, können Sie beginnen, in der Sandbox-Umgebung testen. Sie müssen nicht 'des Produkts zu Testzwecken in der Sandbox-submit'.

 [![](changes-to-storekit-images/image23.png "iTunes Connect wird Sendestatus bereit sein und die Paketdetails anzeigen")](changes-to-storekit-images/image23.png#lightbox)

Es kann (z. b. einige Zeit dauern ein paar Minuten) zwischen Hochladen des Archivs und iTunes Connect Status aktualisiert wird. Sie können das Produkt zur Überprüfung separat übermitteln oder in Verbindung mit einer Anwendungsdatei zu übermitteln. Nur verwendet werden, nachdem der Inhalt von Apple offiziell genehmigt wurde werden in der Produktion App Store erworben werden in der app verfügbar.

### <a name="pkg-file-format"></a>PKG-Dateiformat

Mithilfe von Xcode und Archiv-Tools zum Erstellen und Hochladen eines gehosteten Inhaltspakets bedeutet, dass der Inhalt des Pakets selbst nie angezeigt. Die Dateien und Verzeichnisse in den Paketen, die für die Beispiel-app erstellt aussehen, mit der `plist` -Datei im Stammverzeichnis und die Produktdateien in einem `Contents` Unterverzeichnis:

 [![](changes-to-storekit-images/image24.png "Die Plist-Datei in das Stammverzeichnis und die Produktdateien in einem Unterverzeichnis Inhalt")](changes-to-storekit-images/image24.png#lightbox)

Beachten Sie die Verzeichnisstruktur des Pakets (insbesondere den Speicherort der Dateien in den `Contents` Unterverzeichnis), da Sie benötigen diese Informationen zum Extrahieren der Dateien aus dem Paket auf dem Gerät zu verstehen.

### <a name="updating-package-content"></a>Aktualisieren von Paketinhalt

Das Verfahren für Inhalt aktualisieren, nachdem er genehmigt wurde:

-  Bearbeiten Sie die In-App Kauf Content-Projekt in Xcode.
-  Bump Versionsnummer einrichten.
-  Neu in iTunes Connect hochladen. Nachfolgende Einkäufer, erhalten automatisch die neueste Version, aber Benutzer mit noch die alte Version erhält keine Benachrichtigung. 
-  Ihre app ist verantwortlich für die Benachrichtigung der Benutzer, und wenn Sie eine neuere Version des Inhalts abrufen kann. Die app muss auch eine Funktion erstellen, die die neue Version heruntergeladen. Dies muss erfolgen, mit dem Feature zum Wiederherstellen des Speicher-Kits. 
-  Um festzustellen, ob eine neuere Version vorhanden, Sie ist können eine Funktion in Ihrer app das Abrufen von SKProducts (z. b. erstellen. derselbe Prozess, der zum Abrufen von Produktpreise verwendet wird), und vergleichen die ContentVersion-Eigenschaft. 

## <a name="purchasing-overview"></a>Erwerb von (Übersicht)

Lesen Sie vor dem Lesen dieses Abschnitts, die die vorhandene [In App-Käufe Dokumentation](~/ios/platform/in-app-purchasing/index.md).

Wird die Abfolge der Ereignisse, die auftritt, wenn ein Produkt mit gehosteten Inhalt verkauft und Download wird in diesem Diagramm veranschaulicht:

 [![](changes-to-storekit-images/image25.png "Die Abfolge von Ereignissen, die auftritt, wenn ein Produkt mit gehosteten Inhalt wird verkauft und herunterladen")](changes-to-storekit-images/image25.png#lightbox)

1.  Neue Produkte können in iTunes Connect mit gehosteten Inhalt aktiviert erstellt werden. Der tatsächliche Inhalt wird in Xcode separat erstellt wird, (als einfach als ziehen Dateien in einen Ordner "") und anschließend archiviert und hochgeladen iTunes (keine Codierung erforderlich ist). Jedes Produkt wird dann zur Genehmigung übermittelt wonach es käuflich erworben wird. Im Beispielcode diese Produkt-IDs hartcodiert sind, aber Inhalte mit Apple-hosting ist flexibler, wenn Sie die Liste der verfügbaren Produkt auf einem Remoteserver speichern, damit aktualisiert werden, wenn Sie neuen Produkten und Inhalt in iTunes Connect übermitteln. 
1.  Wenn der Benutzer ein Produkt kauft, wird eine Transaktion in der Warteschlange Zahlung für die Verarbeitung platziert. 
1.  Store-Kit leitet die Bestellung mit iTunes-Servern für die Verarbeitung. 
1.  Transaktion ist auf den iTunes-Servern (z. b. abgeschlossen. Kunden wird belastet) und eine Bestätigung an die app mit Produktinformationen angefügt werden, einschließlich gibt an, ob es herunterladbare ist zurückgegeben wird (und wenn dies der Fall ist, die Dateigröße und andere Metadaten). 
1.  Der Code sollte überprüfen, ob das Produkt Downloadble ist und wenn dies der Fall ist, führen Sie eine Inhaltsdownload-Anforderung der auch für die Zahlung-Warteschlange platziert wird. Store-Kit sendet diese Anforderung an die iTunes-Server. 
1.  Server gibt Inhaltsdatei Store Kit, die einen Rückruf zum Zurückgeben von Downloadstatus und Schätzungen für Ihren Code verbleibende Zeit bereitstellt. 
1.  Sobald der Vorgang abgeschlossen ist, erhalten Sie benachrichtigt und übergeben einen Speicherort für die im Cacheordner für die. 
1.  Der Code sollte kopieren Sie die Dateien und überprüfen sie, speichern Sie einem beliebigen Zustand, die Sie benötigen, denken Sie daran, dass das Produkt gekauft wurde. Nutzen Sie die Gelegenheit, den backup-Flag auf die neuen Dateien richtig festgelegt (Hinweis: Wenn sie von einem Server stammen und nie vom Benutzer bearbeitet werden, sollten Sie wahrscheinlich überspringen anschließende Sicherung, da der Benutzer immer von der Apple-Servern in Zukunft abgerufen werden kann). 
1.  Rufen Sie FinishTransaction. Dies ist wichtig, wie die Transaktion aus der Warteschlange Zahlung entfernt. Es ist auch wichtig, dass Sie FinishTransaction bis nicht aufrufen, nachdem Sie den Inhalt aus dem Cache-Verzeichnisses kopiert haben. Nachdem Sie FinishTransaction aufrufen, werden die zwischengespeicherten Dateien wahrscheinlich schnell gelöscht werden. 


## <a name="implementing-hosted-content-purchase"></a>Implementieren von gehosteten Inhalt Kauf

Lesen Sie die folgenden Informationen in Verbindung mit der vollständigen [In App-Einkäufe Dokumentation](~/ios/platform/in-app-purchasing/index.md). Die Informationen in diesem Dokument konzentriert sich auf die Unterschiede zwischen gehosteten Inhalt und die vorherige Implementierung.


### <a name="classes"></a>Klassen

Die folgenden Klassen wurden hinzugefügt oder geändert werden, damit Unterstützung gehosteten Inhalt in iOS 6:

-   **SKDownload** – neue Klasse, die einen Download darstellt. Die API ermöglicht für mehrere pro Produkt, jedoch zunächst nur eine implementiert wurde. 
-   **SKProduct** – neue Eigenschaften hinzugefügt: `Downloadable` , `ContentVersion` , `ContentLengths` Array. 
-   **SKPaymentTransaction** – neue Eigenschaft hinzugefügt: `Downloads` , enthält eine Auflistung von `SKDownload` Objekten zurück, wenn dieses Produkt zum Download verfügbaren Inhalte gehostet hat. 
-   **SKPaymentQueue** – neue Methode hinzugefügt: `StartDownloads` . Rufen Sie diese Methode mit `SKDownload` Objekte ihren gehosteten Inhalte abgerufen werden sollen. Herunterladen kann im Hintergrund ausgeführt. 
-   **SKPaymentTransactionObserver** – neue Methode: `UpdateDownloads` . Store-Kit ruft diese Methode mit Statusinformationen zur aktuellen Downloadvorgänge. 


Details des neuen `SKDownload` Klasse:

-   **Fortschritt** – ein Wert zwischen 0 und 1, die Sie verwenden können, um einen abgeschlossenen Indikator für den Benutzer anzuzeigen. Verwenden Sie keine Bearbeitung == 1 fest, um festzustellen, ob der Download abgeschlossen ist, überprüfen Sie Status == fertig gestellt. 
-   **TimeRemaining** – Schätzung der Download verbleibende Zeit, in Sekunden. -1 bedeutet, dass es noch Schätzung berechnet wird. 
-   **Status** – aktiv ist, warten, abgeschlossen, fehlgeschlagen, angehalten, abgebrochen. 
-   **ContentURL** – Speicherort, an denen der Inhalt auf dem Datenträger in ablegen wurde, die `Cache` Verzeichnis. NUR aufgefüllt, wenn der Download abgeschlossen wurde. 
-   **Fehler** – überprüfen Sie diese Eigenschaft, ob fehlgeschlagen ist. 


In diesem Diagramm werden die Interaktionen zwischen den Klassen im Beispielcode angezeigt (der Code, der speziell für gehosteten Inhalt Käufe wird grün angezeigt):

 [![](changes-to-storekit-images/image26.png "Gehosteten Inhalt Käufe in Grün in diesem Diagramm angezeigt wird")](changes-to-storekit-images/image26.png#lightbox)

Im Beispielcode, in dem diese Klassen verwendet wurden, wird im weiteren Verlauf dieses Abschnitts angezeigt:

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

Ändern Sie den vorhandenen `UpdatedTransactions` überschreiben, um die Überprüfung auf herunterladbare Inhalt, und rufen `StartDownloads` bei Bedarf:

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

Neue außer Kraft gesetzte Methode `UpdatedDownloads` wird unten gezeigt. Store-Kit ruft diese Methode nach dem `StartDownloads` wird ausgelöst, `UpdatedTransactions`. Diese Methode wird aufgerufen *mehrmals* unbestimmt Intervallen bereitstellen, mit denen Fortschritt herunterladen und dann erneut bei der Download abgeschlossen ist. Beachten Sie, dass die Methode nimmt ein Array aus `SKDownload` Objekte, damit Sie jeden Aufruf mit dem Status der in der Warteschlange mehrere Downloads bereitstellen kann. Entsprechend überprüft die Implementierung unten die Download-Status sind jeder Zeit und die entsprechende Aktion ausgeführt.

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
            Console.WriteLine ("Cancelled"); // TODO: UI?
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

Diese Klasse enthält eine neue Methode `SaveDownload` , die aufgerufen wird, nachdem jeden Download erfolgreich abgeschlossen wurde.

Der gehostete Inhalt wurden erfolgreich heruntergeladen und entzippt in wurde die `Cache` Verzeichnis. Die Struktur der. PKG-Datei muss alle Dateien gespeichert werden kann ein `Contents` Unterverzeichnis, damit Sie der folgenden Code innerhalb von Dateien extrahiert die `Contents` Unterverzeichnis.

Der Code durchläuft alle Dateien im Content-Paket und kopiert sie in der `Documents` Verzeichnis, in einem Unterordner mit dem Namen für die `ProductIdentifier`. Ruft schließlich `CompleteTransaction`, welche Aufrufe `FinishTransaction` So entfernen Sie die Transaktion aus der Warteschlange Zahlung.

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

Wenn `FinishTransaction` heißt der heruntergeladenen Dateien sind nicht mehr unbedingt in der `Cache` Verzeichnis. Alle Dateien kopiert werden sollen, vor dem Aufruf `FinishTransaction`.


## <a name="other-considerations"></a>Andere Überlegungen

Die oben genannte Beispielcode veranschaulicht, eine relativ einfache Implementierung der gehosteten Inhalt erwerben. Es gibt einige zusätzliche Punkte, die Sie berücksichtigen müssen:

### <a name="detecting-updated-content"></a>Erkennen von aktualisierten Inhalten

Es ist, zwar möglich, aktualisieren Sie Ihre gehosteten Content-Pakete bietet Store Kit keinen Mechanismus, um diese Updates an Benutzer zu verteilen, die bereits heruntergeladen und das Produkt gekauft haben. Zum Implementieren dieser Funktionalität kann Code die neue überprüfen `SKProduct.ContentVersion` Eigenschaft (wenn die `SKProduct` ist `Downloadable`) regelmäßig, und ermitteln, ob der Wert erhöht wird. Alternativ könnten Sie eine Pushbenachrichtigung System erstellen.

### <a name="installing-updated-content-versions"></a>Installieren des aktualisierte Inhaltsversionen

Im obigen Beispielcode lässt Dateikopiervorgang aus, wenn die Datei bereits vorhanden ist. Dies ist eine gute Idee nicht auf, wenn Sie neuere Versionen des Inhalts heruntergeladen werden unterstützen möchten.

Eine Alternative könnte sein, kopieren Sie den Inhalt in einen Ordner mit dem Namen für die Version, und verfolgen ist die aktuelle Version (z. b. in `NSUserDefaults` oder ablegen, wo Sie abgeschlossene Kaufdatensätze speichern).

### <a name="restoring-transactions"></a>Wiederherstellen von Transaktionen

Wenn `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` wird aufgerufen, im Store Kit gibt alle vorherige Transaktionen für den Benutzer zurück. Wenn sie eine große Anzahl von Elementen erworben haben oder verfügt jeder Bestellung sehr große Content-Pakete, konnte die Wiederherstellung als alles, was zum Download auf einmal in die Warteschlange ruft in eine viel Netzwerkverkehr führen.

Betrachten Sie das Nachverfolgen, ob ein Produkt aus des eigentlichen Downloads der zugehörigen Content-Paket separat erworben wurde.

### <a name="pausing-restarting-and-canceling-downloads"></a>Anhalten, neu zu starten und Abbrechen von Downloads

Obwohl der Code nicht diese Funktion gezeigt wird, ist es anhalten und neu starten, gehosteten Inhaltsdownloads möglich. Die `SKPaymentQueue.DefaultQueue` verfügt über Methoden für `PauseDownloads`, `ResumeDownloads` und `CancelDownloads`.

Wenn der Code ruft `FinishTransaction` für die Zahlung-Warteschlange gesendet werden kann der Download `Finished` und dann das Downloaden automatisch abgebrochen wird.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>Das Flag für die SKIP-Sicherung festlegen für den heruntergeladenen Inhalt

Apple iCloud Backup-Richtlinien wird empfohlen, sollte der nichtbenutzer-Inhalte, die problemlos von einem Server wiederhergestellt wird *nicht* werden gesichert (da es unnötigerweise iCloud-Speicher verwendet wird). Finden Sie in der [arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md) Dokumentation für Weitere Informationen zum Festlegen der backup-Attribut.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde in iOS6 zwei neue Funktionen von Store-Kit eingeführt: Erwerb iTunes und andere Inhalte innerhalb Ihrer app und, das Apple Server als host für in-app-Käufe. Diese Einführung sollten in Verbindung mit dem vorhandenen gelesen werden [In App-Käufe Dokumentation](~/ios/platform/in-app-purchasing/index.md) für vollständigere Abdeckung von Store-Kit-Funktionalität zu implementieren.

## <a name="related-links"></a>Verwandte Links

- [StoreKit (Beispiel)](https://developer.xamarin.com/samples/StoreKit/)
- [In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController-Klassenreferenz](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes Search-API-Referenz](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC-Video: Verkauf von Produkten mit Speicher-Kit](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
