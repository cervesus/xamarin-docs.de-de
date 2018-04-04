---
title: CloudKit
description: iCloud aktivieren APIs iOS 8-Anwendungen zum Speichern von Daten in icloud zulassen, mit Unterstützung für die automatische Synchronisierung über das Konto eines Benutzers an. Mithilfe von CloudKit ermöglicht Benutzern eine einfachen und nahtlose Handhabung auf iCloud-fähigen Geräten. Dieser Artikel behandelt die CloudKit in einer iOS 8-Anwendung, die mithilfe der Einfachheit halber-API zu aktivieren.
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: 33ceff4549e4afbb1e5fecf3bd380fdb9a3df5f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="cloudkit"></a>CloudKit

_iCloud aktivieren APIs iOS 8-Anwendungen zum Speichern von Daten in icloud zulassen, mit Unterstützung für die automatische Synchronisierung über das Konto eines Benutzers an. Mithilfe von CloudKit ermöglicht Benutzern eine einfachen und nahtlose Handhabung auf iCloud-fähigen Geräten. Dieser Artikel behandelt die CloudKit in einer iOS 8-Anwendung, die mithilfe der Einfachheit halber-API zu aktivieren._

Das Framework CloudKit optimiert der Entwicklung von Anwendungen, Zugriff iCloud. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch die Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit bietet Benutzern eine Ebene Anonymität durch Zulassen des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne die persönlichen Informationen gemeinsam nutzen.

Entwickler können darauf konzentrieren, ihre Client-Side-Anwendungen und iCloud nicht erforderlich, serverseitige Logik schreiben können. CloudKit bietet Authentifizierung, privaten und öffentlichen Datenbanken und strukturierte Daten und Asset-Speicherdienste.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, die in diesem Artikel vorgestellten Schritte ausführen:

-  **Xcode und das iOS SDK** – Xcode von Apple und iOS 8 APIs müssen installiert und auf dem Computer des Entwicklers konfiguriert werden.
-  **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und auf dem Benutzergerät konfiguriert werden.
-  **iOS 8 Gerät** – ein iOS-Gerät, das die aktuelle Version des iOS 8 für das Testen.

## <a name="what-is-cloudkit"></a>Was ist CloudKit?

CloudKit lässt sich der Entwicklerzugriff auf die iCloud-Server zu gewähren. Es bildet die Grundlage für iCloud Laufwerk und iCloud Foto-Bibliothek. CloudKit wird für Mac OS X und Apple iOS-Geräte unterstützt.

 [![](intro-to-cloudkit-images/image1.png "Wie CloudKit auf Mac OS X und Apple iOS-Geräten unterstützt wird")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit verwendet die iCloud-Konto-Infrastruktur. Wenn ein Benutzer eine iCloud-Konto auf dem Gerät angemeldet ist, verwendet CloudKit ihrer ID zur Identifizierung des Benutzers. Wenn kein Konto verfügbar ist, wird beschränkten schreibgeschützten Zugriff bereitgestellt werden.

CloudKit unterstützt sowohl das Konzept von öffentlichen und privaten Datenbanken. Public-Datenbanken bieten eine "Suppe" aller Daten, die der Benutzer Zugriff hat. Speichern die privaten Daten an einen bestimmten Benutzer gebunden werden private Datenbanken voneinander.

CloudKit unterstützt strukturierte und Massenimport von Daten. Es ist in der Behandlung von großen dateiübertragungen nahtlos. CloudKit übernimmt effizient übertragen großer Dateien zu und von iCloud Server im Hintergrund und Freigeben des Entwicklers auf andere Aufgaben konzentrieren.

> [!NOTE]
> Es ist wichtig zu beachten, dass CloudKit ist ein _Transport Technologie_. Es stellt keine Persistenz bereit; Es kann nur eine Anwendung zum Senden und Empfangen von Informationen von den Servern effizient.

Verfassung dieses Dokuments ist Apple anfänglich CloudKit kostenlos mit einem hohen Grenzwert auf sowohl Bandbreite und die Speicherkapazität bereitstellen. Für größere Projekte oder Anwendungen mit einer großen Benutzerbasis hat Apple mit, dass einen günstigen Tarif Skalierung bereitgestellt wird, wird dem Hinweis.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Aktivieren der CloudKit in einer Xamarin-Anwendung

Bevor eine Anwendung Xamarin CloudKit Framework nutzen kann, die Anwendung muss bereitgestellt sein, richtig finden Sie unter der [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) und [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Führungslinien

1.  Öffnen Sie das Projekt in Visual Studio für Mac oder Visual Studio.
2.  In der **Projektmappen-Explorer**öffnen die **"Info.plist"** Datei, und stellen Sie sicher der **Paket-ID** entspricht, die in definiert wurde **App-ID**erstellt als Teil der Bereitstellung einrichten:
 
    [![](intro-to-cloudkit-images/image26a.png "Geben Sie die Paket-ID")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  Führen Sie einen Bildlauf zum unteren Rand der **"Info.plist"** Datei, und wählen Sie **Hintergrundmodi aktiviert**, **Speicherort Updates** und **Remote Benachrichtigungen**:

    [![](intro-to-cloudkit-images/image27a.png "Wählen Sie aktiviert Hintergrundmodi Speicherort aktualisiert und Remote-Benachrichtigungen")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  Mit der rechten Maustaste in des iOS-Projekts in der Projektmappe, und wählen **Optionen**.
5.  Wählen Sie **iOS Bundle Signing**, wählen die **Developer Identität** und **Bereitstellungsprofil** oben erstellt.
6.  Sicherstellen der **Entitlements.plist** enthält **iCloud aktivieren** , **Schlüssel-Wert-Speicher** und **CloudKit** .
7.  Sicherstellen der **ortsunabhängigen Container** für die Anwendung vorhanden ist, (wie oben erstellt haben). Ein Beispiel: `iCloud.com.your-company.CloudKitAtlas`
8.  Speichern Sie die Änderungen in der Datei.


Mit diesen Einstellungen vorhanden kann die Anwendung jetzt CloudKit-Framework-APIs zuzugreifen.

## <a name="cloudkit-api-overview"></a>Übersicht über die CloudKit-API

In diesem Artikel geht vor der Implementierung CloudKit in einer Xamarin iOS-Anwendung, die Grundlagen von CloudKit Framework abdecken, der in den folgenden Themen enthalten:

1.  **Container** – Silos iCloud Kommunikation isoliert.
2.  **Datenbanken** – öffentliche und Private für die Anwendung verfügbar sind.
3.  **Datensätze** – der Mechanismus, der in der strukturierte Daten zu und von CloudKit verschoben werden.
4.  **Datensatz Zonen** – sind Gruppen von Datensätzen.
5.  **Aufzeichnen der IDs** – werden vollständig normalisiert und Darstellen des spezifischen Speicherorts des Datensatzes.
6.  **Verweis** – Geben Sie über-und untergeordneten Beziehungen zwischen den verknüpften Datensätzen in einer bestimmten Datenbank.
7.  **Bestand** – ermöglichen die Datei groß ist, nicht strukturierte Daten in die icloud hochgeladen und einen bestimmten Datensatz zugeordnet werden.


### <a name="containers"></a>Container

Eine bestimmte Anwendung, die auf einem iOS-Gerät ausgeführt wird immer ausgeführt zusammen clientseitigen andere Anwendungen und Dienste auf diesem Gerät. Auf dem Clientgerät geht die Anwendung isolierten oder in irgendeiner Form Sandbox sein. Hierbei handelt es sich um ein literal Sandkasten in einigen Fällen, und in anderen Fällen ist die Anwendung einfach darin ausgeführten ist eigenen Speicherbereich.

Das Konzept eine Clientanwendung und getrennt von anderen Clients ausführen, ist sehr leistungsfähig, und bietet folgende Vorteile:

1.  **Sicherheit** – eine Anwendung kann nicht in Konflikt mit anderen Client-apps oder das Betriebssystem selbst.
1.  **Stabilität** – Wenn die Clientanwendung stürzt ab, es können keine anderen apps des Betriebssystems entnehmen.
1.  **Datenschutz** – jeder Clientanwendung verfügt über eingeschränkten Zugriff auf die persönlichen Informationen, die innerhalb des Geräts gespeichert.


CloudKit wurde entworfen, die gleichen Vorteile bieten, wie die oben genannten, und diese mit dem Arbeiten mit Cloud-basierter anzuwenden:

 [![](intro-to-cloudkit-images/image31.png "CloudKit-apps kommunizieren mithilfe von Containern")](intro-to-cloudkit-images/image31.png#lightbox)

Genau wie die Anwendung, die sich daher ist die Anwendung die Kommunikation mit iCloud 1 von n eine von vielen auf dem Gerät ausgeführte. Jeder dieser anderen Kommunikationssilos werden als Container bezeichnet.

Container werden verfügbar gemacht, in CloudKit Framework über die `CKContainer` Klasse. Standardmäßig eine Anwendung kommuniziert mit einem Container und diesen Container seiner risikoanfälligkeit unterteilt die Daten für die jeweilige Anwendung. Dies bedeutet, dass mehrere Anwendungen können Informationen auf die gleiche iCloud-Konto speichern, aber diese Informationen nie vermischt werden werden.

Die Containerization iCloud-Daten kann auch CloudKit um Benutzerinformationen zu kapseln. Auf diese Weise die Anwendung einige beschränkten Zugriff auf die iCloud-Konto und die Benutzerinformationen gespeichert, während Sie weiterhin den Schutz der Datenschutz und die Sicherheit des Benutzers.

Container werden vollständig vom Entwickler der Anwendung über das Portal WWDR verwaltet. Der Namespace des Containers ist global für alle Apple-Entwickler, daher der Container nicht nur für bestimmte entwickleranwendungen eindeutig sein muss, sondern für alle Apple-Entwickler und Anwendungen.

Apple empfiehlt reverse-DNS-Notation verwenden, wenn Sie den Namespace für Anwendungscontainer zu erstellen. Beispiel:

```csharp
iCloud.com.company-name.application-name
```

Während der Container, standardmäßig gebunden 1: 1 für eine angegebene Anwendung sind, können sie für Anwendungen freigegeben werden. Damit mehrere Anwendungen auf einem einzelnen Container koordiniert werden können. Eine einzelne Anwendung kann auch mit mehreren Containern kommunizieren.

### <a name="databases"></a>Databases

Eine der Hauptfunktionen von CloudKit besteht darin Datenmodell für eine Anwendung und die Replikation dieses Modell bis zu dem iCloud-Server schalten. Einige Informationen richtet sich des Benutzers, die sie erstellt haben, andere sind öffentliche Daten, die von einem Benutzer für die öffentliche Verwendung (z. B. eine Restaurantkritik) erstellt werden konnte oder möglicherweise Informationen, die der Entwickler für die Anwendung veröffentlicht wurde. In beiden Fällen die Zielgruppe ist nicht nur ein einzelner Benutzer, jedoch ist eine Community von Benutzern.

 [![](intro-to-cloudkit-images/image32.png "CloudKit Container-Diagramm")](intro-to-cloudkit-images/image32.png#lightbox)

In einem Container wird zunächst die Öffentliche Datenbank. Dadurch werden alle Informationen des öffentlichen aktiv ist und mingles zusammen. Darüber hinaus sind mehrere einzelne private Datenbanken für jeden Benutzer der Anwendung.

Bei Ausführung auf einem iOS-Gerät wird die Anwendung nur Zugriff auf die Informationen für den gegenwärtig angemeldeten iCloud-Benutzer sein. Daher wird die Anwendungsansicht des Containers wie folgt lauten:

 [![](intro-to-cloudkit-images/image33.png "Die Anwendungsansicht des Containers")](intro-to-cloudkit-images/image33.png#lightbox)

Sie sehen nur der öffentliche und der private Datenbank, die dem aktuell angemeldeten iCloud-Benutzer zugeordnet.

Datenbanken werden verfügbar gemacht, in CloudKit Framework über die `CKDatabase` Klasse. Jede Anwendung verfügt über Zugriff auf zwei Datenbanken: der Öffentliche Datenbank und die privaten ein.

Der Container ist der anfängliche Einstiegspunkt in CloudKit. Der folgende Code kann verwendet werden, auf die öffentliche und private Datenbank aus der Anwendung Standard-Container zugreifen:

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

Dies sind die Unterschiede zwischen der Datenbank-Datentypen:

||Öffentliche Datenbank|Private-Datenbank|
|---|--- |--- |
|**Datentyp**|Gemeinsam genutzte Daten|Daten des aktuellen Benutzers|
|**Quota**|Auf der Hand des Entwicklers Kontingent berücksichtigt|Klicken Sie auf das Kontingent des Benutzers berücksichtigt|
|**Standardberechtigungen**|Lesbare World|Benutzer lesbar|
|**Berechtigungen bearbeiten**|iCloud Dashboard Rollen über eine Benutzerdatensatz-Klasse Ebene|Nicht zutreffend|

### <a name="records"></a>Datensätze

Container enthalten, Datenbanken und in Datenbanken sind Datensätze. Datensätze sind der Mechanismus, der in dem strukturierte Daten in und aus CloudKit verschoben werden:

 [![](intro-to-cloudkit-images/image34.png "Container enthalten, Datenbanken und in Datenbanken sind Datensätze")](intro-to-cloudkit-images/image34.png#lightbox)

Datensätze werden verfügbar gemacht, in CloudKit Framework über die `CKRecord` -Klasse, die Schlüssel-Wert-Paaren umschließt. Eine Instanz eines Objekts in einer Anwendung ist gleichbedeutend mit einem `CKRecord` in CloudKit. Darüber hinaus jede `CKRecord` besitzt einen Datensatztyp, was der Klasse eines Objekts entspricht.

Datensätze ist ein Just-in-Time-Schema aus, damit die Daten zu CloudKit beschrieben werden, bevor Sie zur Verarbeitung übergeben. Ab diesem Punkt CloudKit die Informationen interpretiert und verarbeitet die Logistik speichern und Abrufen des Datensatzes.

Die `CKRecord` -Klasse unterstützt auch eine Breite Palette von Metadaten. Ein Datensatz enthält z. B. Informationen zum Zeitpunkt der Erstellung und der Benutzer, der Sie erstellt. Ein Datensatz enthält auch Informationen wann es zuletzt geändert wurde und der Benutzer, der sie geändert.

Datensätze enthalten, das Konzept eines Tags ändern. Dies ist eine frühere Version von einer Version eines bestimmten Datensatzes. Das Change-Tag wird als eine ressourcenschonende Weise bestimmen, ob der Client und Server die gleiche Version eines Datensatzes haben verwendet.

Wie bereits erwähnt, `CKRecords` Umschließen von Schlüssel-Wert-Paare und daher die folgenden Typen von Daten in einem Datensatz gespeichert werden können:

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


Zusätzlich zu den einzelnen Wert kann ein Datensatz einem homogenen Array von den oben aufgeführten Typen enthalten.

Der folgende Code kann verwendet werden, um einen neuen Datensatz erstellen und speichern Sie sie in einer Datenbank:

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>Datensatz-Zonen

Datensätze sind nicht in einer bestimmten Datenbank selbst vorhanden – Gruppen von Datensätzen zusammen in einer Zone Datensatz vorhanden. Datensatz Zonen kann als Tabellen in einer herkömmlichen relationalen Datenbanken betrachtet werden:

 [![](intro-to-cloudkit-images/image35.png "Gruppen von Datensätzen vorhanden zusammen in einer Zone Datensatz.")](intro-to-cloudkit-images/image35.png#lightbox)

Es können mehrere Datensätze in einer bestimmten Datensatz Zone und mehrere Datensatz Zonen innerhalb einer bestimmten Datenbank vorhanden sein. Jede Datenbank enthält einen Datensatz Standardzone:

 [![](intro-to-cloudkit-images/image36.png "Jede Datenbank enthält einen Datensatz Standardzone und benutzerdefinierte Zone")](intro-to-cloudkit-images/image36.png#lightbox)

Dies ist, in dem Datensätze werden standardmäßig gespeichert. Darüber hinaus können benutzerdefinierte Datensatz Zonen erstellt werden. Notieren Sie die Zonen darstellen, die die Basis Granularität an welche Atomic Commits und Change Tracking erfolgt.

## <a name="record-identifiers"></a>Datensatz-IDs

Datensatz-IDs werden als ein Tupel dargestellt, sowohl mit einem Client bereitgestellt wird, Name des Ressourceneintrags und die Zone, in der der Eintrag vorhanden ist. Datensatz-IDs besitzen die folgenden Eigenschaften:

-  Sie werden von der Clientanwendung erstellt.
-  Sie werden vollständig normalisiert und Darstellen des spezifischen Speicherorts des Datensatzes.
-  Die eindeutige ID eines Datensatzes in einer fremden Datenbank der Name des Ressourceneintrags zuweisen, können sie verwendet, lokale Datenbanken zu überbrücken, die nicht in CloudKit gespeichert sind.


Wenn Entwickler neue Datensätze erstellen, können sie eine Datensatz-ID übergeben. Wenn eine Datensatz-ID nicht angegeben ist, wird eine UUID wird, automatisch erstellt und mit dem Datensatz zugeordnet.

Wenn Entwickler neue Datensatz-IDs erstellen, können sie die Zone Datensatz angeben, die jeden Datensatz gehören soll. Wenn keine Angabe erfolgt, wird der Datensatz-Standardzone verwendet werden.

Datensatz-IDs werden verfügbar gemacht, in CloudKit Framework über die `CKRecordID` Klasse. Der folgende Code kann verwendet werden, um einen neuen Datensatzbezeichner zu erstellen:

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Verweise

Verweise stellen Beziehungen zwischen den verknüpften Datensätzen in einer bestimmten Datenbank:

 [![](intro-to-cloudkit-images/image37.png "Verweise stellen Beziehungen zwischen den verknüpften Datensätzen in einer bestimmten Datenbank")](intro-to-cloudkit-images/image37.png#lightbox)

Im obigen Beispiel besitzen Eltern untergeordneten Elemente aus, damit, dass das untergeordnete Element über einen untergeordneten Datensatz des übergeordneten Datensatzes ist. Die Beziehung aus dem untergeordneten Datensatz mit dem übergeordneten Datensatz wechselt und wird als bezeichnet eine *wieder Verweis*.

Verweise werden verfügbar gemacht, in CloudKit Framework über die `CKReference` Klasse. Sie sind eine Möglichkeit, lassen die iCloud-Server, die die Beziehung zwischen den Datensätzen besser verstehen können.

Verweise stellen die Mechanismen hinter kaskadierende löscht. Wenn ein übergeordneter Datensatz aus der Datenbank gelöscht wird, werden jede untergeordnete Datensätze (wie in einer Beziehung angegeben) automatisch aus der Datenbank als auch gelöscht.

> [!NOTE]
> Zurückbleiben Zeiger sind eine Möglichkeit, wenn CloudKit verwenden. Beispielsweise kann nach der Zeit die Anwendung verfügt über eine Liste der Zeiger auf Datensätze abgerufen, einen Datensatz ausgewählt und dann für den Datensatz aufgefordert, der Datensatz nicht mehr in der Datenbank vorhanden. Eine Anwendung muss codiert werden, um diese Situation Datenbindungsvorgängen erfolgreich behandelt.

Zwar nicht erforderlich, werden wieder Verweise bevorzugt, bei der Arbeit mit dem CloudKit-Framework. Apple hat dies das effizientesten Typ des Verweises, der im System abgestimmt.

Beim Erstellen eines Verweises, der Entwickler kann entweder Geben Sie einen Datensatz, der bereits im Arbeitsspeicher befinden, oder erstellen einen Verweis auf eine Datensatz-ID ab. Wenn Sie mit der eine Datensatz-ID und der angegebene Verweis in der Datenbank noch nicht gab, würde ein Verbleibend Zeiger erstellt werden.

Im folgenden finden ein Beispiel zum Erstellen eines Verweises mit einem bekannten Datensatz:

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Objekte

Ressourcen können für eine Datei groß ist, nicht strukturierte Daten in die icloud hochgeladen und einen bestimmten Datensatz zugeordnet werden:

 [![](intro-to-cloudkit-images/image38.png "Ressourcen können für eine Datei groß ist, nicht strukturierte Daten in die icloud hochgeladen und einen bestimmten Datensatz zugeordnet werden")](intro-to-cloudkit-images/image38.png#lightbox)

Auf dem Client eine `CKRecord` erstellt wird, beschreibt die Datei, die auf dem iCloud-Server hochgeladen werden. Ein `CKAsset` wird erstellt, um die Datei enthalten und mit dem Datensatz, beschreiben sie verknüpft ist.

Wenn die Datei an den Server hochgeladen wird, wird der Datensatz in der Datenbank eingefügt und die Datei wird in eine spezielle Massenspeicherung Datenbank kopiert. Zwischen dem Zeiger und die hochgeladene Datei ist eine Verknüpfung erstellt.

Medienobjekte werden verfügbar gemacht, in CloudKit Framework über die `CKAsset` -Klasse und zum Speichern von großer, unstrukturierter Daten verwendet werden. Da der Entwickler nie große ist, nicht strukturierte Daten im Arbeitsspeicher haben möchte, werden Anlagen mithilfe von Dateien auf dem Datenträger implementiert.

Bestand gehören nach Datensätzen, wodurch der Bestand aus den Datensatz als Zeiger mit iCloud abgerufen werden sollen. Können Sie auf diese Weise den Server Garbage erfassen Ressourcen, wenn der Datensatz, der Besitzer das Medienobjekt ist, gelöscht wird.

Da `CKAssets` dienen Verarbeiten von großen Datendateien, Apple entwickelt CloudKit effizient hochladen und Herunterladen der Bestand.

Der folgende Code kann auf ein Medienobjekt erstellen und verknüpfen es mit dem Datensatz verwendet werden:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Wir haben jetzt alle grundlegenden Objekte innerhalb der CloudKit behandelt. Container werden zugeordnet, um Anwendungen und Datenbanken enthalten. Datenbanken enthalten Datensätze, die in den Datensatz Zonen gruppiert und Datensatz-IDs verweist. Parent-Child-Beziehungen werden zwischen den Datensätzen verwenden von Verweisen auf definiert. Schließlich können große Dateien hochgeladen und Datensätze mit Ressourcen zugeordnet sind.

## <a name="cloudkit-convenience-api"></a>CloudKit bequeme API

Apple bietet zwei verschiedene API-Sets für die Arbeit mit CloudKit:

-  **-API von Operational** – jede einzelne Funktion der CloudKit bietet. Für komplexere Anwendungen bietet diese API eine präzisere Kontrolle über CloudKit.
-  **Bequeme API** – bietet eine Untermenge common, vorkonfiguriert, dass der CloudKit-Funktionen. Es stellt eine praktische und einfache Lösung für einschließlich CloudKit Funktionen in einer iOS-Anwendung bereit.


Der Einfachheit halber-API wird in der Regel die beste Wahl für die meisten iOS-Anwendungen und Apple schlägt vor, sie ab. Im restlichen Teil dieses Abschnitts befasst sich mit die folgenden Themen der Vereinfachung-API:

-  Speichern einen Datensatz an.
-  Abrufen eines Datensatzes.
-  Aktualisieren eines Datensatzes.


### <a name="common-setup-code"></a>Allgemeine Setupcode

Bevor Sie beginnen mit der CloudKit halber-API, müssen Sie einige standardmäßige Setup-Code erforderlich vorhanden ist. Starten, indem Sie der Anwendungsverzeichnis `AppDelegate.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

Der obige Code stellt die öffentlichen und privaten CloudKit Datenbanken als Tastenkombinationen leichter mit dem Rest der Anwendung arbeiten.

Als Nächstes fügen Sie den folgenden Code, um jede Sicht oder den Container anzeigen, die/CloudKit verwendet:

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Dadurch wird eine Verknüpfung zum Abrufen der `AppDelegate` und Zugriff auf die oben erstellten öffentliche und private Datenbank-Verknüpfungen.

Mit diesem Code werden werfen wir einen Blick auf die CloudKit halber-API in einer Xamarin iOS 8-Anwendung implementieren.

### <a name="saving-a-record"></a>Speichern eines Datensatzes

Verwenden das Muster, das oben angeführte, bei der Betrachtung der Datensätze, wird der folgende Code einen neuen Datensatz erstellen und Verwenden der Einfachheit halber-API auf um die Öffentliche Datenbank zu speichern:

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Drei Dinge zu den oben aufgeführten Code zu beachten:

1.  Durch Aufrufen der `SaveRecord` Methode der `PublicDatabase`, der Entwickler muss keinen angeben, wie die Daten werden gesendet, welche Zone zu usw. geschrieben wird. Der Einfachheit halber-API ist alle diese Details selbst zuständig.
1.  Der Aufruf ist asynchron und bietet eine Abbruchroutine bei Beendigung des Aufrufs, entweder mit Erfolg oder Fehler. Wenn der Aufruf fehlschlägt, wird eine Fehlermeldung bereitgestellt werden.
1.  CloudKit bietet keine lokalen Speicher/Persistenz. Es ist nur eine Übertragung Mittel. Daher wird eine Anforderung gestellt wird, um einen Datensatz zu speichern, sofort an die iCloud-Server gesendet.


> [!NOTE]
> Aufgrund der "lossy" mobilen Netzwerkkommunikation, wobei Verbindungen werden permanent gelöscht wird oder unterbrochen, einer der ersten Aspekte, die der Entwickler beim Arbeiten mit CloudKit Fehlerbehandlung ist vornehmen muss.

### <a name="fetching-a-record"></a>Abrufen eines Datensatzes

Verwenden Sie mit einem Datensatz erstellt und auf dem Server iCloud erfolgreich gespeichert wurde den folgenden Code, um den Datensatz abzurufen:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Wie in den Datensatz gespeichert, der obige Code ist asynchron, einfach und erfordert gute Fehlerbehandlung.

### <a name="updating-a-record"></a>Aktualisieren eines Datensatzes

Nachdem Sie ein Datensatz in die iCloud-Server abgerufen wurde, kann der folgende Code verwendet werden, ändern Sie den Eintrag, und speichern Sie die Änderungen wieder in der Datenbank:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

Die `FetchRecord` Methode der `PublicDatabase` gibt eine `CKRecord` , wenn der Aufruf erfolgreich war. Klicken Sie dann die Anwendung ändert den Eintrag und ruft `SaveRecord` erneut aus, um die Änderungen wieder in die Datenbank geschrieben.

In diesem Abschnitt wurde die typische Zyklus angezeigt, den eine Anwendung bei der Arbeit mit der CloudKit halber-API verwenden. Die Anwendung wird Datensätze in iCloud zu speichern, diese Datensätze aus iCloud abrufen, ändern Sie die Datensätze und Speichern dieser Änderungen auf iCloud.

## <a name="designing-for-scalability"></a>Entwerfen für Skalierbarkeit

Bisher wurde in diesem Artikel erläutert, speichern und Abrufen von gesamte Objektmodell einer Anwendung von den Servern iCloud, jedes Mal, wenn er mit gearbeitet werden soll. Während dieser Ansatz gut für eine kleine Menge von Daten und eine sehr kleine Benutzerbasis eignet, ist es nicht gut skaliert Wenn basieren die Menge an Informationen und/oder der Benutzer steigt.

### <a name="big-data-tiny-device"></a>Big Data, kleine Gerät

Gängigeren einer Anwendung ist, je mehr Daten in der Datenbank, und desto weniger möglich ist auf einen Cache der gesamten Daten auf dem Gerät verfügen. Die folgenden Verfahren können verwendet werden, um dieses Problem zu lösen:

-  **Halten Sie die großen Datenmengen in der Cloud** – CloudKit wurde entworfen, um umfangreiche Daten effizient zu verarbeiten.
-  **Client sollte nur ein Segment der Daten anzeigen** – die absoluten Mindestanforderungen von Daten erforderlich sind, behandeln jede Aufgabe zu einem bestimmten Zeitpunkt heruntergefahren.
-  **Client-Ansichten können ändern,** – da jeder Benutzer verfügt über unterschiedliche Einstellungen, der Slice von Daten, die angezeigt wird, Benutzer ändern kann und des Benutzers einzelne kann Überblick über alle angegebenen Slice unterschiedlich sein.
-  **Client verwendet Abfragen, um den Blickpunkt konzentrieren** – Abfragen ermöglicht dem Benutzer eine kleine Teilmenge von einem größeren Dataset, das vorhanden ist in der Cloud anzeigen.


### <a name="queries"></a>Abfragen

Wie bereits erwähnt, können Abfragen den Entwickler eine kleine Teilmenge der größeren Dataset auswählen, die in der Cloud vorhanden ist. Abfragen werden verfügbar gemacht, in CloudKit Framework über die `CKQuery` Klasse.

Eine Abfrage kombiniert drei unterschiedliche Dinge: Datensatztyp ( `RecordType`), ein Prädikat ( `NSPredicate`) und optional eine Sortierdeskriptor ( `NSSortDescriptors`). CloudKit unterstützt die meisten `NSPredicate`.

#### <a name="supported-predicates"></a>Unterstützten Prädikate

CloudKit unterstützt die folgenden Typen von `NSPredicates` beim Arbeiten mit Abfragen:


1. Übereinstimmenden Datensätzen, in denen die Namen gleich einem Wert in einer Variablen gespeichert ist:
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. Ermöglicht das Abgleichen zum basieren auf eines dynamischen Schlüsselwert, so, dass der Schlüssel nicht zum Zeitpunkt der Kompilierung bekannt sein:
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. Übereinstimmenden Datensätzen, in dem der Datensatz-Wert größer als der angegebene Wert ist:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Übereinstimmenden Datensätzen, in dem Speicherort des Datensatzes innerhalb von 100 Metern des angegebenen Speicherorts ist:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit wird ein Token dargestellten durchsuchen unterstützt. Dieser Aufruf erstellt zwei Token, eine für `after` und ein anderes für `session`. Es wird einen Datensatz zurück, die diese zwei Token enthält:
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. Unterstützt das CloudKit zusammengesetzte mit verknüpften Prädikaten die `AND` Operator.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>Erstellen von Abfragen

Der folgende Code dient zum Erstellen einer `CKQuery` in einer Xamarin iOS 8-Anwendung:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

Zunächst erstellt er ein Prädikat zum Auswählen von Datensätzen, die mit einen angegebenen Namen übereinstimmen. Er erstellt dann eine Abfrage, die Datensätze des angegebenen Datensatztyps auswählen, die dem Prädikat entsprechen.

#### <a name="performing-a-query"></a>Ausführen einer Abfrage

Nachdem eine Abfrage erstellt wurde, verwenden Sie den folgenden Code, um die Abfrage auszuführen und die zurückgegebenen Datensätze zu verarbeiten:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

Der obige Code verwendet die oben erstellte Abfrage und für die Öffentliche Datenbank ausgeführt. Da kein Datensatz Zeitzone angegeben ist, werden alle Zonen durchsucht. Wenn keine Fehler aufgetreten sind, ein Array von `CKRecords` entsprechen die Parametern der Abfrage zurückgegeben werden.

Die Möglichkeit, Abfragen sind im Wesentlichen ist, dass sie abgerufen werden und sind hervorragend über große Datasets aufteilen. Abfragen sind jedoch aufgrund der folgenden Gründe nicht gut geeignet für umfangreiche, größtenteils statische Datasets:

-  Sie sind ungeeignet für die Akkulaufzeit des Geräts.
-  Sie sind ungeeignet für den Netzwerkverkehr.
-  Sie sind ungeeignet für Benutzer, da die Informationen, die Datenanzeige durch wie oft die Anwendung die Datenbank Abfragen ist begrenzt ist. Heute erwarten Benutzer Pushbenachrichtigungen aus, wenn etwas geändert wird.


### <a name="subscriptions"></a>Abonnements

Beim Umgang mit großen, größtenteils statische Datasets die Abfrage sollte auf dem Client-Gerät nicht ausgeführt werden, sollten sie auf dem Server im Auftrag des Clients ausführen. Die Abfrage im Hintergrund ausgeführt werden soll, und nach dem Speichern jedes einzelnen Datensatzes durch das aktuelle Gerät oder ein anderes Gerät durch berühren der gleichen Datenbank ausgeführt werden sollen.

Abschließend sollte eine Pushbenachrichtigung gesendet werden, auf jedem Gerät auf die Datenbank angefügt wird, wenn die serverseitige-Abfrage ausgeführt wird.

Abonnements werden verfügbar gemacht, in CloudKit Framework über die `CKSubscription` Klasse. Sie kombinieren Datensatztyp ( `RecordType`), ein Prädikat ( `NSPredicate`) und ein Apple Push Notification ( `Push`).

> [!NOTE]
> CloudKit Push-Vorgänge sind etwas erweitert werden, da sie eine Nutzlast enthalten, die bestimmte CloudKit-Informationen wie der Ursache die Clientpushinstallation durchgeführt werden soll.

#### <a name="how-subscriptions-work"></a>Funktionsweise von Abonnements

Bevor Sie Abonnements in C#-Code implementieren, werfen wir einen schnellen Überblick über die Funktionsweise von Abonnements:

 [![](intro-to-cloudkit-images/image39.png "Einen Überblick über die Funktionsweise von Abonnements")](intro-to-cloudkit-images/image39.png#lightbox)

Das obige Diagramm zeigt den typischen abonnierungsprozess wie folgt aus:

1.  Das Client-Gerät erstellt ein neues Abonnement, enthält den Satz von Bedingungen, die das Abonnement und eine Pushbenachrichtigung, die gesendet wird, tritt der Trigger ausgelöst wird.
2.  Das Abonnement ist an die Datenbank gesendet, wo sie auf die Auflistung von vorhandenen Abonnements hinzugefügt.
3.  Ein zweites Gerät wird einen neuer Datensatz erstellt und speichert diesen Datensatz in der Datenbank.
4.  Die Datenbank durchsucht die Liste der Abonnements, um festzustellen, ob der neue Datensatz ihre Bedingungen entspricht.
5.  Wenn eine Übereinstimmung gefunden wird, wird die Pushbenachrichtigung an das Gerät gesendet, die das Abonnement mit Informationen zu den Datensatz registriert, die zugrundeliegende ausgelöst werden soll.


Mit diesem Wissen direktes sehen wir uns das Erstellen von Abonnements in einer Xamarin iOS 8-Anwendung.

#### <a name="creating-subscriptions"></a>Erstellen von Abonnements

Der folgende Code dient zum Erstellen eines Abonnements

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

Zunächst erstellt er ein Prädikat, das die Bedingung zum Auslösen des Abonnements bereitstellt. Als Nächstes wird das Abonnement für einen bestimmten Typ von Datensatz erstellt, und die Möglichkeit, wenn der Trigger getestet wird. Schließlich wird den Typ der Benachrichtigung, die auftreten, wenn das Abonnement ausgelöst und es das Abonnement fügt definiert.

### <a name="saving-subscriptions"></a>Speichern von Abonnements

Mit dem Abonnement erstellt wird wird es im folgende Code in der Datenbank speichern:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

Mithilfe der API Vereinfachung der Aufruf asynchron, einfache, und bietet einfache Fehlerbehandlung.

#### <a name="handling-push-notifications"></a>Behandeln von Pushbenachrichtigungen

Der Entwickler zuvor Apple Push Notifications (APS) verwendet, sollte der Prozess für den Umgang mit Benachrichtigungen von CloudKit generierten vertraut sein.

In der `AppDelegate.cs`, überschreiben die `ReceivedRemoteNotification` -Klasse wie folgt:

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

Der obige Code fordert CloudKit, um die Benutzerinformationen in eine Benachrichtigung CloudKit zu analysieren. Als Nächstes werden die Informationen zur Warnung extrahiert. Klicken Sie abschließend der Typ der Benachrichtigung wird getestet, und die Benachrichtigung wird entsprechend behandelt wird.

In diesem Abschnitt wurde wie beantworten, die große Datenmengen, kleine Geräteproblem oben mithilfe von Abfragen und Abonnements dargestellt wird. Die Anwendung belassen die umfangreichen Daten in der Cloud, und diese Technologien zum Bereitstellen von Ansichten in diesem Dataset verwendet werden.

## <a name="cloudkit-user-accounts"></a>CloudKit-Benutzerkonten

Wie zu Beginn dieses Artikels erwähnt, basiert auf der vorhandenen Infrastruktur für iCloud CloudKit. Im folgende Abschnitt werden im Detail behandelt wie Konten für ein Entwickler mithilfe der CloudKit-API verfügbar gemacht werden.

### <a name="authentication"></a>Authentifizierung

Beim Umgang mit Benutzerkonten ist der erste Aspekt Authentifizierung an. CloudKit unterstützt die Authentifizierung über den derzeit angemeldeten iCloud-Benutzer auf dem Gerät. Authentifizierung erfolgt im Hintergrund und von iOS behandelt wird. Auf diese Weise müssen Entwickler nie Informationen zum Implementieren der Authentifizierung kümmern. Sie testen, nur um festzustellen, ob ein Benutzer angemeldet ist.

### <a name="user-account-information"></a>Benutzerkontoinformationen

CloudKit bietet die folgenden Informationen für Entwickler:

-  **Identität** – eine Möglichkeit, die den Benutzer eindeutig identifiziert.
-  **Metadaten** – die Möglichkeit zum Speichern und Abrufen von Informationen zu Benutzern.
-  **Datenschutz** – alle Informationen in eine bewusste Datenschutz-Manor behandelt wird. Nichts wird verfügbar gemacht, es sei denn, der Benutzer zugestimmt hat.
-  **Ermittlung** – ermöglicht Benutzern die Möglichkeit, ihre Freunde zu ermitteln, die die gleiche Anwendung verwenden.


Als Nächstes werden wir diese Themen detailliert betrachten.

#### <a name="identity"></a>Identität

Wie bereits erwähnt, bietet CloudKit eine Möglichkeit für die Anwendung zur eindeutigen Identifizierung ein gegebenen Benutzers an:

 [![](intro-to-cloudkit-images/image40.png "Eindeutig sich ein bestimmter Benutzer")](intro-to-cloudkit-images/image40.png#lightbox)

Es ist eine Clientanwendung, die auf Geräten des Benutzers und aller die jeweiligen Benutzer Private Datenbanken innerhalb des Containers CloudKit ausgeführt wird. Die Clientanwendung führt zu einem bestimmten Benutzer verknüpft sein. Dieser Wert basiert auf der Benutzer, die in der iCloud lokal auf dem Gerät angemeldet ist.

Da dies von iCloud stammt, ist eine umfangreiche Store Benutzerinformationen zu sichern. Und da iCloud tatsächlich den Container gehostet werden, können sie Benutzer korrelieren. In der Abbildung oben der Benutzer, dessen iCloud-Konto `user@icloud.com` mit den aktuellen Client verknüpft ist.

Eine eindeutige, nach dem Zufallsprinzip generierte Benutzer-ID ist auf der Grundlage einer Container vom Container erstellt und iCloud-Konto des Benutzers (e-Mail-Adresse) zugeordnet. Diese Benutzer-ID wird an die Anwendung zurückgegeben und können auf beliebige Weise der Entwickler Ermessen verwendet werden.

> [!NOTE]
> Verschiedene Anwendungen ausgeführt werden, auf dem gleichen Gerät für iCloud-Benutzer müssen verschiedene Benutzer-IDs aus, da mit unterschiedlichen CloudKit Containern verbunden sind.

Der folgende Code Ruft die CloudKit-Benutzer-ID für den angemeldeten in iCloud-Benutzer auf dem Gerät:

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

Der obige Code ist der CloudKit-Container, geben Sie die ID des gerade angemeldeten Benutzers gefragt werden. Da diese Informationen von iCloud Server stammt, wird der Aufruf asynchron ist, und Fehlerbehandlung ist erforderlich.

#### <a name="metadata"></a>Metadaten

Jeder Benutzer in CloudKit hat bestimmten Metadaten, die sie beschreiben. Diese Metadaten wird als Datensatz CloudKit dargestellt:

 [![](intro-to-cloudkit-images/image41.png "Jeder Benutzer in CloudKit hat bestimmten Metadaten, die sie beschreiben")](intro-to-cloudkit-images/image41.png#lightbox)

Suchen in der privaten Datenbank für einen bestimmten Benutzer, der einen Container vorhanden, wird ein Datensatz, die dieser Benutzer definiert ist. Es gibt viele Benutzerdatensätze in die Öffentliche Datenbank, eine für jeden Benutzer des Containers. Eines der folgenden verfügen Datensatz-ID, die dem aktuell angemeldeten Benutzers Datensatz-ID übereinstimmt

Benutzerdatensätze in der öffentlichen Datenbank sind World lesbar. Sie werden meistens, wie ein normales Datensatz behandelt, und weisen den Typ `CKRecordTypeUserRecord`. Diese Datensätze werden vom System reserviert und stehen nicht für Abfragen.

Verwenden Sie den folgenden Code ein Benutzerdatensatz Zugriff auf:

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

Der obige Code abfragt Benutzerdatensatzes für den Benutzer zurückgegeben werden, die ID es ist über Zugriff auf die Öffentliche Datenbank. Da diese Informationen von iCloud Server stammt, wird der Aufruf asynchron ist, und Fehlerbehandlung ist erforderlich.

#### <a name="privacy"></a>Datenschutz

CloudKit wurde entwerfen, wird standardmäßig zum Schutz der Privatsphäre des aktuell angemeldeten Benutzers. Standardmäßig ist keine persönlich identifizierbare Informationen über den Benutzer zur Verfügung gestellt. Es gibt einige Fälle, in denen die Anwendung begrenztem Umfang Informationen über den Benutzer erforderlich sind.

In diesen Fällen kann die Anwendung anfordern, dass der Benutzer diese Informationen offen zu legen. Ein Dialogfeld wird für den Benutzer bitten, opt-in verfügbar zu machen ihre Kontoinformationen angezeigt.

#### <a name="discovery"></a>Suche

Vorausgesetzt, dass die Benutzer als Filtertreiber-in für die Anwendung den Zugriff auf ihre Benutzerkontoinformationen beschränkt, können sie an andere Benutzer der Anwendung erkannt werden:

 [![](intro-to-cloudkit-images/image42.png "Ein Benutzer kann an andere Benutzer der Anwendung erkennbar sein.")](intro-to-cloudkit-images/image42.png#lightbox)

Kommuniziert der Client-Anwendung in einen Container, und der Container kommuniziert iCloud den Zugriff auf Benutzer Informationen. Der Benutzer kann eine e-Mail-Adresse angeben und Ermittlung kann verwendet werden, um das Abrufen von Informationen über den Benutzer zurück. Optional können die Benutzer-ID auch verwendet werden, um Informationen über den Benutzer zu ermitteln.

CloudKit bietet auch eine Möglichkeit, Informationen zu jeder Benutzer zu ermitteln, die Freunde des derzeit angemeldeten möglicherweise auf iCloud Benutzer durch die gesamte Adressbuch Abfragen. Der Prozess CloudKit Ziehen im Adressbuch für des Benutzers wenden Sie sich an und verwenden die e-Mail-Adressen, um festzustellen, ob andere Benutzer der Anwendung, die diesen Adressen entsprechen gefunden werden kann.

Dadurch kann die Anwendung des Benutzers Kontakt Buch nutzen, ohne den Zugriff darauf bereitstellen oder den Benutzer auffordert, den Zugriff auf die Kontakte zu genehmigen. Zu keinem Zeitpunkt ist die Kontaktinformationen für die Anwendung zur Verfügung gestellt, nur die CloudKit-Prozess, Zugriff hat.

Um Punkte aufzugreifen, stehen drei verschiedene Arten von Eingaben für die Benutzerermittlung zur Verfügung:

-  **Datensatz-ID des Benutzers** – Ermittlung kann erfolgen, für die Benutzer-ID des derzeit angemeldeten Benutzer CloudKit.
-  **Benutzer-e-Mail-Adresse** – der Benutzer kann eine e-Mail-Adresse angeben und für die Ermittlung verwendet werden.
-  **Wenden Sie sich an Buch** – Adressbuch des Benutzers zum Ermitteln von Benutzern der Anwendung, die dieselbe e-Mail-Adresse aufweisen, gemäß ihrer Kontakte verwendet werden kann.


Ermittlung von Benutzerrichtlinien gibt die folgende Informationen zurück:

-  **Datensatz-ID des Benutzers** -die eindeutige ID eines Benutzers in die Öffentliche Datenbank.
-  **Erste und letzte Name** – wie in der öffentlichen Datenbank gespeichert.


Diese Informationen werden nur für Benutzer zurückgegeben werden, die Filtertreiber in Discovery verfügen.

Im folgende Code wird mit Informationen zu den in icloud zulassen, auf dem Gerät derzeit angemeldeten Benutzers ermittelt:

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

Verwenden Sie den folgenden Code zum Abfragen aller Benutzer im Buch wenden Sie sich an:

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

In diesem Abschnitt haben wir die vier Hauptbereichen des Zugriffs auf dem Konto eines Benutzers behandelt, die CloudKit für eine Anwendung bereitstellen kann. Abrufen von Identität und die Metadaten des Benutzers, werden zu den Datenschutzrichtlinien, die in CloudKit und schließlich auch die Möglichkeit zum Ermitteln von anderen Benutzern der Anwendung integriert.

## <a name="the-development-and-production-environments"></a>Die Entwicklungs- und Produktionsumgebungen

CloudKit stellt separate Entwicklungs- und Produktionsumgebungen Umgebungen für einer Anwendungsverzeichnis Eintragstypen und Daten bereit. Der Entwicklungsumgebung ist eine flexiblere Umgebung, die nur für Mitglieder eines Entwicklungsteams verfügbar ist. Wenn eine Anwendung ein neues Feld zu einem Datensatz hinzugefügt und diesen Datensatz in der Entwicklungsumgebung speichert, aktualisiert der Server automatisch die Schemainformationen an.

Der Entwickler kann diese Funktion verwenden, so ändern Sie ein Schema während der Entwicklung, wodurch Zeit gespart werden. Ein Nachteil ist, nachdem ein Feld zu einem Datensatz hinzugefügt wurde, der Datentyp, die diesem Feld zugeordneten programmgesteuert geändert werden kann. Um den Typ des Felds zu ändern, muss der Entwickler löschen Sie das Feld in [CloudKit Dashboard](https://icloud.developer.apple.com/dashboard/) und mit den neuen Typ wieder hinzufügen.

Vor der Bereitstellung der Anwendung vom Entwickler kann ihre Schema und Daten zu migrieren-Umgebung für die Produktion verwenden **CloudKit Dashboard**. Bei der Ausführung für die Produktionsumgebung verhindert, dass der Server über eine Anwendung aus programmgesteuert das Schema geändert. Entwickler kann weiterhin Änderungen mit **CloudKit Dashboard** aber versucht, einen Datensatz in der Produktionsumgebung verursachen Fehler Felder hinzufügen.

> [!NOTE]
> Die iOS-Simulator eignet sich nur für die **Entwicklungsumgebung**. Wenn der Entwickler ist bereit zum Testen einer Anwendung in eine **Produktionsumgebung**, einem physischen iOS-Gerät ist erforderlich.


## <a name="shipping-a-cloudkit-enabled-app"></a>Eine CloudKit Shipping-fähiger Apps

Vor der Lieferung einer Anwendung, CloudKit verwendet, muss diese so konfiguriert werden, zum Ziel der **CloudKit Produktionsumgebung** oder die Anwendung wird von Apple abgelehnt werden.

Führen Sie folgende Schritte aus:

1. In Visual Studio für Ma, kompilieren Sie die Anwendung für **Release** > **iOS-Gerät**: 

    [![](intro-to-cloudkit-images/shipping01.png "Kompilieren Sie die Anwendung zur Freigabe")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. Aus der **erstellen** klicken Sie im Menü **Archiv**: 

    [![](intro-to-cloudkit-images/shipping02.png "Wählen Sie Archiv")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. Die **Archiv** wird erstellt und in Visual Studio für Mac angezeigt: 

    [![](intro-to-cloudkit-images/shipping03.png "Das Archiv wird erstellt und angezeigt werden")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Starten Sie **Xcode**.
5. Aus der **Fenster** klicken Sie im Menü **Planer**: 

    [![](intro-to-cloudkit-images/shipping04.png "Wählen Sie Planer")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Wählen Sie die Anwendung Archiv, und klicken Sie auf die **exportieren...**  Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping05.png "Die Anwendung Archiv")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. Wählen Sie eine Methode für den Export aus, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping06.png "Wählen Sie eine Methode für den export")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Wählen Sie die **Entwicklungsteam** aus der Dropdown-Liste und klicken Sie auf die **auswählen** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping07.png "Wählen Sie in der Dropdownliste das Entwicklungsteam")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Wählen Sie **Produktion** aus der Dropdown-Liste und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping08.png "Produktion in der Dropdownliste auswählen.")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. Überprüfen Sie die Einstellung aus und klicken Sie auf die **exportieren** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping09.png "Überprüfen Sie die Einstellung")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Wählen Sie einen Speicherort zum Generieren der resultierenden Anwendung `.ipa` Datei.

Der Prozess ist ähnlich wie für die Anwendung direkt in iTunes Connect senden, klicken Sie einfach auf die **senden...**  Schaltfläche anstelle des Exports... nach der Auswahl eines Archivs im Fenster organisieren.

## <a name="when-to-use-cloudkit"></a>CloudKit verwenden

Wie wir in diesem Artikel gesehen haben, bietet es sich bei CloudKit um eine einfache Möglichkeit für eine Anwendung zum Speichern und Abrufen von Informationen von den Servern iCloud. Nichtsdestotrotz CloudKit veraltet oder nicht als veraltet markiert, Änderungen an den vorhandenen Tools oder Frameworks.

### <a name="use-cases"></a>Anwendungsfälle

Die folgenden Anwendungsfälle sollte helfen, die den Entwickler entscheiden, wann eine bestimmte iCloud Framework oder eine Technologie zum Einsatz:

-  **iCloud Schlüsselwertspeicher** – asynchron kleine Menge an Daten auf dem neuesten Stand hält und eignet sich hervorragend für das Arbeiten mit Anwendungseinstellungen. Es ist jedoch für eine sehr kleine Menge an Informationen eingeschränkt.
-  **iCloud Laufwerk** – baut auf den vorhandenen iCloud Dokumente APIs und bietet eine einfache API zum Synchronisieren von unstrukturierter Daten aus dem Dateisystem. Er bietet einen vollständigen offline Cache auf Mac OS X und eignet sich hervorragend für Dokument zentrierten Anwendungen.
-  **iCloud Core Daten** – können Sie Daten zwischen aller Geräte des Benutzers repliziert werden. Die Daten sind Einzelbenutzermodus und hervorragend für die Aufbewahrung private, strukturierte Daten synchronisiert.
-  **CloudKit** – stellt öffentliche Daten sowohl Struktur und Massenkopieren und ist in der Lage, Behandeln von großen Datasets und große unstrukturierte Dateien. Seine gebunden, die dem Benutzer die iCloud-Konto und bietet Client geleitet, die Datenübertragung.


Behalten diese Anwendungsfälle Denken Sie daran, der Entwickler sollten die richtige iCloud-Technologie, die Funktionalität sowohl die aktuellen erforderliche Anwendung, und geben Sie gute Skalierbarkeit für zukünftiges Wachstum auswählen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine kurze Einführung in die CloudKit-API behandelt. Es zeigt, wie zum Bereitstellen und konfigurieren eine Xamarin iOS-Anwendung CloudKit verwenden. Es wurden die Funktionen der CloudKit halber-API behandelt. Es wurde anzeigen eine CloudKit entwerfen-fähige Anwendung, für die Skalierbarkeit, die mithilfe von Abfragen und Abonnements. Und schließlich verfügt die Benutzerkonto-Informationen, die für eine Anwendung durch CloudKit verfügbar gemacht wird angezeigt.

## <a name="related-links"></a>Verwandte Links

- [CloudKitAtlas (sample)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Erstellen ein Bereitstellungsprofil](~/ios/get-started/installation/device-provisioning/index.md)
