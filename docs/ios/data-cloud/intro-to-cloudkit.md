---
title: CloudKit in Xamarin.iOS
description: Dieses Dokument beschreibt das Arbeiten mit CloudKit in Xamarin.iOS. Es bietet eine Übersicht über CloudKit und erläutert, wie es, den CloudKit der Einfachheit halber-API, Skalierbarkeit, Benutzerkonten und Entwicklungs-und produktionsumgebungen zu ermöglichen.
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/11/2016
ms.openlocfilehash: daea27472ac7c0578c1cfd79ebd96428212fafb3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61165589"
---
# <a name="cloudkit-in-xamarinios"></a>CloudKit in Xamarin.iOS

Das CloudKit-Framework wird der Entwicklung von Anwendungen, iCloud Zugriff optimiert. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit, erhalten Benutzer eine Ebene der Anonymität durch Gewähren des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne Weitergabe persönlicher Informationen.

Entwickler können clientseitige Anwendungen konzentrieren und iCloud nicht erforderlich, serverseitige Anwendungslogik schreiben können. CloudKit bietet, Authentifizierung, private und öffentliche Datenbanken und strukturierten Daten und Asset-Storage-Dienste.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel vorgestellten Schritte ausführen:

-  **Xcode und iOS SDK** – Xcode und iOS 8 Apple APIs müssen installiert und konfiguriert werden, auf dem Computer des Entwicklers.
-  **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und auf dem Benutzergerät konfiguriert werden soll.
-  **iOS 8 Gerät** – ein iOS-Gerät mit der neuesten Version von iOS 8 für das Testen.

## <a name="what-is-cloudkit"></a>Was ist CloudKit?

CloudKit lässt sich die Entwicklerzugriff auf die iCloud-Server zu gewähren. Sie bildet die Grundlage für iCloud Drive und iCloud-Fotomediathek. CloudKit werden sowohl auf Mac OS X und Apple iOS-Geräte.

 [![](intro-to-cloudkit-images/image1.png "Unterstützung für CloudKit auf Mac OS X und Apple iOS-Geräten")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit verwendet die iCloud-Konto-Infrastruktur. Wenn ein Benutzer ein iCloud-Konto auf dem Gerät angemeldet ist, verwendet CloudKit ihre ID zur Identifizierung des Benutzers. Wenn kein Konto verfügbar ist, wird eingeschränkten schreibgeschützten Zugriff bereitgestellt werden.

CloudKit unterstützt sowohl das Konzept, Datenbanken für öffentliche und private. Öffentliche-Datenbanken bieten eine "mehr Licht bei" aller Daten, die der Benutzer Zugriff hat. Private Datenbanken sollen die privaten Daten für einen bestimmten Benutzer gebunden.

CloudKit unterstützt sowohl strukturierte als auch Massenimport von Daten. Es ist in großen Dateitransfers nahtlos verarbeiten kann. CloudKit übernimmt effizient übertragen großer Dateien in und aus der iCloud-Server im Hintergrund den Entwickler für andere Aufgaben freigibt.

> [!NOTE]
> Es ist wichtig zu beachten, dass CloudKit eine _Transport Technologie_. Sie bieten jedoch keinen Persistenz; Es kann nur eine Anwendung zum Senden und Empfangen von Informationen von den Servern effizient.

Während ich dies schreibe, ist Apple anfänglich CloudKit kostenlos ein hohes Limit für sowohl Bandbreite und die Speicherkapazität bereit. Für größere Projekte oder Anwendungen mit einer großen Benutzerbasis hat Apple haben, dass es sich bei eine erschwingliche Preise Skalierungsgruppe bereitgestellt wird.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Aktivieren von CloudKit in einer Xamarin-Anwendung

Vor eine Xamarin-Anwendung das CloudKit-Framework verwenden kann, die Anwendung muss ordnungsgemäß bereitgestellt werden entsprechend den Details in der [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) und [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Führungslinien

1.  Öffnen Sie das Projekt in Visual Studio für Mac oder Visual Studio.
2.  In der **Projektmappen-Explorer**öffnen die **"Info.plist"** Datei, und stellen Sie sicher die **Bündel-ID** entspricht, die in definiert wurde **App-ID**erstellt, da Sie Teil der Bereitstellung einrichten:
 
    [![](intro-to-cloudkit-images/image26a.png "Geben Sie die Bündel-ID")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  Führen Sie einen Bildlauf zum unteren Rand der **"Info.plist"** und wählen Sie **aktiviert Background Modes**, **Speicherortaktualisierungen** und **Remotebenachrichtigungen**:

    [![](intro-to-cloudkit-images/image27a.png "Wählen Sie aktiviert Hintergrundmodi, Speicherortaktualisierungen und Remotebenachrichtigungen")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  Mit der rechten Maustaste in des iOS-Projekts in der Projektmappe, und wählen **Optionen**.
5.  Wählen Sie **iOS Bundle-Signierung**, wählen die **Entwickleridentität** und **Bereitstellungsprofil** oben erstellt haben.
6.  Stellen Sie sicher die **"Entitlements.plist"** enthält **iCloud aktivieren** , **Schlüssel-Wert-Speicher** und **CloudKit** .
7.  Stellen Sie sicher die **ortsunabhängigen Container** für die Anwendung vorhanden ist, (wie oben erstellt haben). Ein Beispiel: `iCloud.com.your-company.CloudKitAtlas`
8.  Speichern Sie die Änderungen in der Datei.


Mit diesen Einstellungen vorhanden ist die Anwendung nun auf die CloudKit-Framework-APIs zugreifen.

## <a name="cloudkit-api-overview"></a>CloudKit-API – Übersicht

In diesem Artikel geht vor der Implementierung CloudKit in einer Xamarin iOS-Anwendung, die Grundlagen des Frameworks CloudKit, behandeln die beinhaltet die folgenden Themen:

1.  **Container** – isoliert Silos der iCloud-Kommunikation.
2.  **Datenbanken** – öffentlich und privat für die Anwendung verfügbar sind.
3.  **Datensätze** – der Mechanismus, der in der strukturierte Daten in und aus CloudKit verschoben werden.
4.  **Datensatz Zonen** – sind Gruppen von Datensätzen.
5.  **Aufzeichnen der IDs** – werden vollständig normalisiert und des spezifischen Speicherorts des Datensatzes darstellt.
6.  **Verweis** – Geben Sie über-und untergeordnete Beziehungen zwischen den entsprechenden Datensätzen in einer bestimmten Datenbank.
7.  **Assets** – für die Datei der große, unstrukturierte Daten in iCloud hochgeladen und einen bestimmten Datensatz zugeordnet werden können.


### <a name="containers"></a>Container

Eine bestimmte Anwendung auf einem iOS-Gerät ausgeführt wird, wird immer ausgeführt. Seite zusammen, andere Anwendungen und Dienste auf diesem Gerät. Die Anwendung wird auf dem Clientgerät Silos oder Sandbox in irgendeiner Form sein. In einigen Fällen Hierbei handelt es sich um eine literale Sandbox, und in anderen Fällen wird die Anwendung sich einfach mit eigenen Arbeitsspeicher Speicherplatz ist.

Das Konzept eine Clientanwendung und Ausführen von anderen Clients getrennt ist sehr leistungsfähig und bietet die folgenden Vorteile:

1.  **Sicherheit** – eine Anwendung verursacht keine Konflikte mit anderen Client-apps oder das Betriebssystem selbst.
1.  **Stabilität** – Wenn die Client-Anwendung abstürzt, andere apps des Betriebssystems nicht möglich.
1.  **Datenschutz** – jede Clientanwendung eingeschränkter Zugriff auf die persönlichen Informationen, die innerhalb des Geräts gespeichert.


CloudKit wurde entwickelt, bieten die gleichen Vorteile, wie die oben genannten aus, und sie für die Arbeit mit Cloud-basierter anwenden:

 [![](intro-to-cloudkit-images/image31.png "Kommunikation CloudKit-Anwendungen mithilfe von Containern")](intro-to-cloudkit-images/image31.png#lightbox)

Wie bei der Anwendung, also der mehrere auf dem Gerät ausgeführt wird der Anwendung-Kommunikation mit der iCloud-1-von-n. Jeder dieser abweichender Kommunikationspfad Silos werden als Container bezeichnet.

Container werden verfügbar gemacht, in CloudKit Framework über die `CKContainer` Klasse. Standardmäßig finden Sie eine Anwendung für einen Container und diesem Container getrennt werden die Daten für diese Anwendung. Dies bedeutet, dass mehrere Anwendungen können Informationen mit dem gleichen iCloud-Konto speichern, aber diese Informationen niemals vermischt werden werden.

Die containerisierung von iCloud-Daten kann auch CloudKit um Benutzerinformationen zu kapseln. Auf diese Weise können die Anwendung einige eingeschränkten Zugriff auf das iCloud-Konto und die Benutzerinformationen in, während Sie weiterhin den Schutz und die Sicherheit des Benutzers gespeichert.

Container werden vollständig vom Entwickler der Anwendung über das WWDR-Portal verwaltet. Der Namespace des Containers ist global für alle Apple-Entwickler, daher der Container nicht nur Anwendungen eines bestimmten Entwicklers muss eindeutig sein, sondern für alle Apple-Entwickler und Anwendungen.

Apple empfiehlt reverse-DNS-Notation verwenden, beim Erstellen des Namespace für die Anwendungscontainer. Beispiel:

```csharp
iCloud.com.company-name.application-name
```

Während der Container, in der Standardeinstellung gebunden 1: 1 für eine bestimmte Anwendung sind, können sie zwischen Anwendungen freigegeben werden. Damit mehrere Anwendungen auf einem einzelnen Container koordinieren können. Eine einzelne Anwendung kann auch mit mehreren Containern sprechen.

### <a name="databases"></a>Databases

Eine der Hauptfunktionen CloudKit ist Datenmodell einer Anwendung und die Replikation dieses Modell bis zu der iCloud-Server. Einige Informationen richtet sich des Benutzers, die sie erstellt haben, Weitere Informationen sind öffentliche Daten, die von einem Benutzer für die öffentliche Verwendung (z. B. eine Restaurantkritik) erstellt werden konnte oder möglicherweise Informationen, die der Entwickler für die Anwendung veröffentlicht hat. In beiden Fällen die Zielgruppe ist nicht nur einen einzelnen Benutzer, jedoch ist eine Community von Benutzern.

 [![](intro-to-cloudkit-images/image32.png "Diagramm der CloudKit-Container")](intro-to-cloudkit-images/image32.png#lightbox)

In einem Container ist zuerst und vor allem die Öffentliche Datenbank. Dies ist, in dem alle Informationen des öffentlichen lebt und mingles zusammen. Darüber hinaus stehen mehrere private einzeldatenbanken für jeden Benutzer der Anwendung.

Bei Ausführung auf einem iOS-Gerät wird die Anwendung nur Zugriff auf die Informationen für den gegenwärtig angemeldeten iCloud-Benutzer sein. Daher wird die Anwendungsansicht des Containers wie folgt lauten:

 [![](intro-to-cloudkit-images/image33.png "Die Anwendungsansicht des Containers")](intro-to-cloudkit-images/image33.png#lightbox)

Es kann nur der öffentliche und der private Datenbank verknüpft ist, mit dem iCloud-aktuell angemeldeten Benutzer angezeigt.

Datenbanken werden zur Verfügung gestellt, in CloudKit Framework über die `CKDatabase` Klasse. Jede Anwendung verfügt über Zugriff auf zwei Datenbanken: die Öffentliche Datenbank und der private.

Der Container ist der anfängliche Einstiegspunkt in CloudKit. Der folgende Code kann Zugriff auf die öffentliche und private Datenbank aus der Anwendung Standardcontainer verwendet werden:

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

Hier sind die Unterschiede zwischen der Datenbank-Datentypen:

||Öffentliche Datenbank|Private-Datenbank|
|---|--- |--- |
|**Datentyp**|Freigegebene Daten|Daten des aktuellen Benutzers|
|**Quota**|Klicken Sie auf der Entwickler Kontingent berücksichtigt|Klicken Sie auf das Kontingent des Benutzers berücksichtigt|
|**Standardberechtigungen**|World lesbar|Benutzer lesbar|
|**Berechtigungen bearbeiten**|iCloud-Dashboard-Rollen über eine Benutzerdatensatz-Klasse Ebene|Nicht zutreffend|

### <a name="records"></a>Datensätze

Container enthalten, die Datenbanken und in Datenbanken sind Datensätze. Datensätze sind der Mechanismus, der in dem strukturierte Daten in und aus CloudKit verschoben werden:

 [![](intro-to-cloudkit-images/image34.png "Container enthalten, die Datenbanken und in Datenbanken sind Datensätze")](intro-to-cloudkit-images/image34.png#lightbox)

Datensätze werden verfügbar gemacht, in CloudKit Framework über die `CKRecord` -Klasse, die Schlüssel-Wert-Paare umschließt. Eine Instanz eines Objekts in einer Anwendung ist gleichbedeutend mit einem `CKRecord` in CloudKit. Darüber hinaus jede `CKRecord` besitzt einen Datensatztyp, der die Klasse eines Objekts entspricht.

Datensätze sind ein just-in-Time-Schema, sodass die Daten cloudkit beschrieben werden, bevor für die Verarbeitung übergeben wird. Ab diesem Punkt können CloudKit die Informationen interpretiert und verarbeitet die Logistik für das Speichern und Abrufen des Datensatzes.

Die `CKRecord` Klasse unterstützt auch eine Breite Palette von Metadaten. Ein Datensatz enthält beispielsweise Informationen zu dem es erstellt wurde und dem Benutzer, die sie erstellt haben. Ein Datensatz enthält auch Informationen wann es zuletzt geändert wurde und der Benutzer, der sie geändert werden.

Datensätze enthalten, das Konzept eines Tags ändern. Dies ist eine frühere Version von einer Version eines bestimmten Datensatzes. Das Change-Tag wird als eine Möglichkeit, leicht ermitteln, ob der Client und dem Server die gleiche Version eines bestimmten Datensatzes haben verwendet.

Wie bereits erwähnt, `CKRecords` Umschließen von Schlüssel-Wert-Paaren, und daher die folgenden Arten von Daten in einem Datensatz gespeichert werden können:

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


Zusätzlich zu den einzelnen Wert kann ein Datensatz eine homogene Array mit beliebigen der oben aufgeführten Typen enthalten.

Der folgende Code kann verwendet werden, um einen neuen Datensatz erstellen und in einer Datenbank zu speichern:

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

Datensätze nicht alleine vorhanden, in einer bestimmten Datenbank – Gruppen von Datensätzen vorhanden zusammen in einer Zone Datensatz. Datensatz-Zonen können als Tabellen in einer herkömmlichen relationalen Datenbanken betrachtet werden:

 [![](intro-to-cloudkit-images/image35.png "Gruppen von Datensätzen vorhanden zusammen in einer Zone Datensatz.")](intro-to-cloudkit-images/image35.png#lightbox)

Es können mehrere Datensätze in einer bestimmten Zone für den Datensatz und mehrere Datensatz Zonen innerhalb einer bestimmten Datenbank vorhanden sein. Jede Datenbank enthält einen Datensatz Standardzone:

 [![](intro-to-cloudkit-images/image36.png "Jede Datenbank enthält einen Datensatz Standardzone und eine benutzerdefinierte Zone")](intro-to-cloudkit-images/image36.png#lightbox)

Dies ist, in dem Datensätze werden standardmäßig gespeichert werden. Darüber hinaus können benutzerdefinierte Datensatz-Zonen erstellt werden. Notieren Sie Zonen darstellen, die die Basis Granularität, an welche atomische Commits und Änderungsnachverfolgung erfolgt.

## <a name="record-identifiers"></a>Datensatzbezeichner

Datensatz-IDs werden als ein Tupel dargestellt, die ein Client mit diesen beiden bereitgestellt werden, Eintragsname und die Zone, in der der Eintrag vorhanden ist. Datensatz-IDs weisen folgende Merkmale auf:

-  Sie werden von der Clientanwendung erstellt.
-  Sie werden vollständig normalisiert und des spezifischen Speicherorts des Datensatzes darstellt.
-  Durch den Namen des Eintrags die eindeutige ID eines Datensatzes in einer fremden Datenbank zugewiesen haben, können sie verwendet werden, lokale Datenbanken zu überbrücken, die nicht in CloudKit gespeichert werden.


Wenn Entwickler neue Datensätze erstellen, können sie auswählen, um eine Datensatz-ID übergeben. Wenn eine Datensatz-ID nicht angegeben ist, wird eine UUID wird, automatisch erstellt und zugewiesen werden, auf den Eintrag.

Wenn Entwickler neue Datensatz-IDs erstellen, können sie die Datensatz-Zone anzugeben, die jeden Datensatz angehören soll. Wenn keine Angabe erfolgt, wird der Datensatz-Standardzone verwendet werden.

Datensatz-IDs werden zur Verfügung gestellt, in CloudKit Framework über die `CKRecordID` Klasse. Der folgende Code kann verwendet werden, um einen neuen Datensatz-ID zu erstellen:

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Verweise

Verweise bieten Beziehungen zwischen den entsprechenden Datensätzen in einer bestimmten Datenbank:

 [![](intro-to-cloudkit-images/image37.png "Geben Sie Beziehungen zwischen den entsprechenden Datensätzen in einer bestimmten Datenbank verweisen")](intro-to-cloudkit-images/image37.png#lightbox)

Im obigen Beispiel besitzen Eltern untergeordnete Elemente, so, dass das untergeordnete Element über einen untergeordneten Datensatz des übergeordneten Datensatzes ist. Die Beziehung wird aus dem untergeordneten Datensatz mit dem übergeordneten Datensatz und wird als bezeichnet ein *wieder Verweis*.

Verweise werden verfügbar gemacht, in CloudKit Framework über die `CKReference` Klasse. Sie sind eine Möglichkeit, durch den iCloud-Server, die die Beziehung zwischen den Datensätzen zu verstehen.

Verweise bereitstellen, die Mechanismen hinter Cascading gelöscht wird. Wenn ein übergeordneter Datensatz aus der Datenbank gelöscht wird, werden alle untergeordneten Datensätze (wie angegeben in einer Beziehung) automatisch aus der Datenbank ebenfalls gelöscht.

> [!NOTE]
> Verbleibende Zeiger sind eine Möglichkeit, beim Verwenden von CloudKit. Beispielsweise kann dem Zeitpunkt, an die Anwendung verfügt über eine Liste der Zeiger auf Datensätze abgerufen, einen Datensatz ausgewählt und dann aufgefordert, für den Datensatz, der Datensatz vorhanden nicht mehr in der Datenbank. Eine Anwendung muss codiert werden, um diese Situation ordnungsgemäß zu behandeln.

Zwar nicht erforderlich, werden wieder Verweise bevorzugt, bei der Arbeit mit dem CloudKit-Framework. Apple hat das System, um dies den effizientesten Typ des Verweises, optimiert.

Wenn Sie einen Verweis zu erstellen, kann der Entwickler enthalten einen Eintrag, der bereits im Arbeitsspeicher vorhanden ist oder einen Verweis auf eine Datensatz-ID erstellen. Wenn Sie mit der eine Datensatz-ID und den angegebenen Verweis in der Datenbank noch nicht gab, würde ein Verbleibend Zeiger erstellt werden.

Im folgenden finden ein Beispiel zum Erstellen eines Verweises mit einem bekannten Datensatz:

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Objekte

Objekte können für eine Datei vom große, unstrukturierte Daten in iCloud hochgeladen und einen bestimmten Datensatz zugeordnet werden:

 [![](intro-to-cloudkit-images/image38.png "Ressourcen für eine Datei vom große, unstrukturierte Daten in iCloud hochgeladen und einen bestimmten Datensatz zugeordnet werden können")](intro-to-cloudkit-images/image38.png#lightbox)

Auf dem Client eine `CKRecord` erstellt wird, beschreibt die Datei, die auf den iCloud-Server hochgeladen werden. Ein `CKAsset` wird erstellt, um die Datei enthalten, und klicken Sie mit der beschreibende Eintrag verknüpft ist.

Wenn die Datei an den Server hochgeladen wird, wird der Datensatz in der Datenbank eingefügt, und die Datei in eine spezielle Massenspeicherung-Datenbank kopiert wird. Es wird ein Link zwischen dem Zeiger und die hochgeladene Datei erstellt.

Ressourcen werden verfügbar gemacht, in CloudKit Framework über die `CKAsset` Klasse und werden verwendet, um große, unstrukturierte Daten zu speichern. Da der Entwickler nie möchte, die große, unstrukturierte Daten im Arbeitsspeicher befinden, werden Objekte mithilfe von Dateien auf dem Datenträger implementiert.

Ressourcen sind im Besitz Datensätze, wodurch die Assets aus der iCloud-verwenden den Datensatz als Zeiger abgerufen werden sollen. Können Sie auf diese Weise die Server Garbage Sammeln von Assets, wenn der Datensatz, der Besitzer das Medienobjekt ist, gelöscht wird.

Da `CKAssets` dienen Behandeln von großen Dateien, werden mit Apple CloudKit effizient hochladen und Herunterladen von Assets.

Der folgende Code kann zum Erstellen eines Medienobjekts und ordnen sie den Datensatz verwendet werden:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Wir haben nun alle grundlegenden Objekte in CloudKit behandelt. Container werden zugeordnet, um Anwendungen und Datenbanken enthalten. Datenbanken enthalten Datensätze, die in einer Gruppe in Datensatz Zonen und Datensatz-IDs verweist. Über-/ unterordnungsbeziehungen werden zwischen den Datensätzen, die Verweise definiert. Schließlich können große Dateien hochgeladen und, die Datensätze mithilfe von Ressourcen zugeordnet sind.

## <a name="cloudkit-convenience-api"></a>CloudKit bequeme API

Apple bietet zwei verschiedene API-Sätze für die Arbeit mit CloudKit:

-  **-API von Operational** – jede einzelne Funktion von CloudKit bietet. Für komplexere Anwendungen bietet diese API eine präzisere Kontrolle über CloudKit.
-  **Bequeme API** – bietet einen allgemeinen, vorkonfiguriert, dass ein Teil CloudKit-Funktionen. Es bietet eine praktische und einfachen Zugriff Lösung einschließlich CloudKit-Funktionen in einer iOS-Anwendung.


Der Einfachheit halber-API ist in der Regel die beste Wahl für die meisten Anwendungen für iOS und Apple empfiehlt es ab. Im restlichen Teil dieses Abschnitts wird der Einfachheit halber-API Folgendes behandelt:

-  Speichern eines Datensatzes.
-  Beim Abrufen eines Datensatzes aus.
-  Aktualisieren eines Datensatzes.


### <a name="common-setup-code"></a>Allgemeine Setupcode

Bevor Sie beginnen mit der CloudKit der Einfachheit halber-API, es gibt standardeinrichtung Code erforderlich. Ändern die Anwendung zunächst `AppDelegate.cs` Datei, und stellen sie wie folgt aussehen:

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

Der obige Code gibt die öffentlichen und privaten CloudKit-Datenbanken als Verknüpfungen zu ihnen erleichtern, mit der Rest der Anwendung arbeiten.

Als Nächstes fügen Sie den folgenden Code, um jede Sicht oder den Container anzeigen, die CloudKit verwenden möchten:

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Dadurch wird eine Verknüpfung zum Abrufen der `AppDelegate` und Zugriff auf die zuvor erstellten öffentliche und private Datenbank-Verknüpfungen.

Mit diesem Code werden werfen wir einen Blick auf die CloudKit der Einfachheit halber-API in einer Xamarin iOS 8-Anwendung implementieren.

### <a name="saving-a-record"></a>Speichern eines Datensatzes

Verwenden das Muster, das oben angeführte, bei der Erörterung von Datensätzen, wird der folgende Code einen neuen Datensatz erstellen und verwenden die der Einfachheit halber-API auf um die Öffentliche Datenbank zu speichern:

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

Drei Dinge zu den obigen Code zu beachten:

1.  Durch Aufrufen der `SaveRecord` Methode der `PublicDatabase`, die Entwickler müssen nicht angeben, wie die Daten werden gesendet, welcher Zone er usw. geschrieben wird. Der Einfachheit halber-API ist für alle diese Details selbst zuständig.
1.  Der Aufruf asynchron ist, und stellt eine Rückrufroutine bereit, bei Beendigung des Aufrufs, entweder mit Erfolg oder Fehler. Wenn der Aufruf fehlschlägt, wird eine Fehlermeldung bereitgestellt werden.
1.  CloudKit bietet keine lokalen Speicher/Persistenz. Es ist nur ein Übertragungsmedium. Also, wenn eine Anforderung gesendet wird, um einen Datensatz zu speichern, sofort an die iCloud-Server gesendet.


> [!NOTE]
> Aufgrund der "" Verluste der mobile die Netzwerkkommunikation, in denen Verbindungen werden ständig gelöscht wird oder unterbrochen, einer der ersten Aspekte, die der Entwickler beim Arbeiten mit CloudKit Fehlerbehandlung ist treffen muss.

### <a name="fetching-a-record"></a>Abrufen eines Datensatzes

Verwenden Sie mit einem Eintrag erstellt und gespeichert wurde erfolgreich auf dem Server iCloud den folgenden Code, um den Datensatz abrufen:

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

Genau wie in den Datensatz gespeichert, der obige Code ist asynchron, einfach und erfordert gute Fehlerbehandlung.

### <a name="updating-a-record"></a>Aktualisieren eines Datensatzes

Nach dem ein Datensatz aus der iCloud-Server abgerufen wurde, kann der folgende Code verwendet werden, ändern Sie den Eintrag, und speichern Sie die Änderungen in der Datenbank:

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

Die `FetchRecord` Methode der `PublicDatabase` gibt eine `CKRecord` , wenn der Aufruf erfolgreich war. Klicken Sie dann die Anwendung ändert den Eintrag und ruft `SaveRecord` erneut aus, um die Änderungen zurück in die Datenbank geschrieben.

In diesem Abschnitt hat die typischen Zyklus gezeigt, den eine Anwendung bei der Arbeit mit der CloudKit der Einfachheit halber-API verwenden. Die Anwendung wird Datensätze in iCloud zu speichern, rufen diese Datensätze aus der iCloud-, ändern Sie die Datensätze und diese Änderungen zurück in iCloud speichern.

## <a name="designing-for-scalability"></a>Aus Gründen der Skalierbarkeit entwerfen

Bisher wurde in diesem Artikel erläutert das Speichern und Abrufen von gesamte Objektmodell einer Anwendung aus der iCloud-Server, jedes Mal, wenn es vor sich geht, mit dem gearbeitet werden. Dieser Ansatz gut mit einer kleinen Menge von Daten und eine sehr kleine Benutzerbasis funktioniert, zwar Skalierung es keine gute bei basieren die Menge an Informationen und/oder den Benutzer erhöht.

### <a name="big-data-tiny-device"></a>Big Data und winziges Gerät

Weitere beliebte in einer Anwendung ist, je mehr Daten in der Datenbank und weniger möglich ist, einen Cache der gesamten Daten auf dem Gerät verfügen. Die folgenden Verfahren können verwendet werden, um dieses Problem zu lösen:

-  **Behalten Sie die großen Datenmengen in der Cloud** – CloudKit wurde entworfen, um große Datenmengen effizient zu verarbeiten.
-  **Client sollte einen Slice von Daten nur anzeigen** – Herunterfahren auf das absolute Minimum, behandeln jede Aufgabe zu einem bestimmten Zeitpunkt erforderlichen Daten.
-  **Client-Ansichten können ändern,** : Da jeder Benutzer verfügt über unterschiedliche Einstellungen, die der Slice von Daten, die angezeigt wird, Benutzer ändern kann und der Benutzer die einzelnen kann Ansicht der angegebenen Slices unterschiedlich sein.
-  **Client verwendet die Abfragen, um den Standpunkt konzentrieren** – Abfragen können Benutzer an eine kleine Teilmenge eines größeren Datasets, die vorhanden ist, in der Cloud.


### <a name="queries"></a>Abfragen

Wie bereits erwähnt, können Abfragen den Entwickler eine kleine Teilmenge der dem größeren Dataset auswählen, die in der Cloud vorhanden ist. Abfragen werden zur Verfügung gestellt, in CloudKit Framework über die `CKQuery` Klasse.

Eine Abfrage kombiniert drei verschiedene Dinge: einen Datensatztyp ( `RecordType`), ein Prädikat ( `NSPredicate`) und optional einen Sortierdeskriptor ( `NSSortDescriptors`). CloudKit unterstützt die meisten `NSPredicate`.

#### <a name="supported-predicates"></a>Unterstützten Prädikate

CloudKit unterstützt die folgenden Typen von `NSPredicates` beim Arbeiten mit Abfragen:


1. Übereinstimmende Datensätze, in dem die Namen gleich einem Wert in einer Variablen gespeichert ist:
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. Ermöglicht es, zugeordnet werden, um einen dynamischen Wert basiert, damit der Schlüssel nicht zu, die zum Zeitpunkt der Kompilierung bekannt sein:
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. Übereinstimmende Datensätze, in dem der Datensatz der Wert größer als der angegebene Wert ist:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Übereinstimmende Datensätze, in dem der Datensatz-Speicherort innerhalb von 100 Metern der angegebenen Position ist:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit unterstützt eine Suche mit Token. Dieser Aufruf erstellt zwei Token – eines für `after` und ein anderes für `session`. Es gibt einen Datensatz zurück, die diese beiden Token enthält:
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. Unterstützt die CloudKit zusammengesetzte verknüpften mit Prädikaten die `AND` Operator.
    
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

Zunächst erstellt sie ein Prädikat zum Auswählen von Datensätzen, die mit einen angegebenen Namen übereinstimmen. Anschließend erstellt er eine Abfrage, die Datensätze des angegebenen Datensatztyps auswählen, die dem Prädikat entsprechen.

#### <a name="performing-a-query"></a>Durchführen einer Abfrage

Nachdem eine Abfrage erstellt wurde, verwenden Sie folgenden Code, um die Abfrage auszuführen und die zurückgegebenen Datensätze zu verarbeiten:

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

Der obige Code nimmt die Abfrage, die weiter oben erstellten und für die Öffentliche Datenbank ausgeführt. Da kein Datensatz Zeitzone angegeben ist, werden alle Zonen durchsucht. Wenn keine Fehler aufgetreten ist, ein Array von `CKRecords` wird zurückgegeben, mit den Parametern der Abfrage übereinstimmt.

Die Möglichkeit, Informationen zu Abfragen vorzustellen, ist, dass sie Umfragen, und stellen eine tolle über große Datasets aufteilen. Abfragen sind jedoch aufgrund der folgenden Gründe nicht gut geeignet für große, größtenteils statische Datasets:

-  Es handelt sich negativ auf die Akkulaufzeit des Geräts.
-  Sie sind schlecht für Netzwerkdatenverkehr.
-  Sie sind negativ auf die benutzerfreundlichkeit, da die Informationen, die sie finden Sie unter durch wie oft die Anwendung die Datenbank Abfragen, ist begrenzt ist. Benutzer erwarten heute Pushbenachrichtigungen, man bei jeder Änderung.


### <a name="subscriptions"></a>Abonnements

Beim Umgang mit großen, größtenteils statische Datasets wird die Abfrage nicht auf dem Clientgerät ausgeführt werden soll, sollte auf dem Server im Auftrag des Clients ausgeführt. Die Abfrage sollte im Hintergrund ausgeführt und nachdem jeden einzelnen Datensatz gespeichert haben, ob durch das aktuelle Gerät oder einem anderen Gerät, die durch berühren der gleichen Datenbank ausgeführt werden soll.

Schließlich sollte eine Pushbenachrichtigung gesendet werden, auf jedes Gerät, auf die Datenbank angefügt wird, wenn die serverseitige Abfrage ausgeführt wird.

Abonnements werden verfügbar gemacht, in CloudKit Framework über die `CKSubscription` Klasse. Sie kombinieren, einem Datensatz vom Typ ( `RecordType`), ein Prädikat ( `NSPredicate`) und ein Apple-Pushbenachrichtigungsdienst ( `Push`).

> [!NOTE]
> CloudKit Pushvorgänge sind leicht erweitert werden, da sie eine Nutzlast mit CloudKit Informationen wie etwa den Push enthalten Ursache ausgeführt.

#### <a name="how-subscriptions-work"></a>Funktionsweise von Abonnements

Vor der Implementierung von Abonnements in C# code, werfen wir einen kurzen Überblick über die Funktionsweise von Abonnements:

 [![](intro-to-cloudkit-images/image39.png "Einen Überblick über die Funktionsweise von Abonnements")](intro-to-cloudkit-images/image39.png#lightbox)

Das obige Diagramm zeigt typische Abonnement wie folgt aus:

1.  Das Client-Gerät erstellt ein neues Abonnement mit den Bedingungen, die das Abonnement und eine Pushbenachrichtigung gesendet, die gesendet wird, tritt der Trigger ausgelöst wird.
2.  Das Abonnement ist an die Datenbank gesendet, in dem sie auf die Auflistung von vorhandenen Abonnements hinzugefügt.
3.  Ein zweites Gerät erstellt einen neuen Datensatz und speichert diesen Datensatz in der Datenbank.
4.  Die Datenbank sucht, durch die Liste der Abonnements, um festzustellen, ob der neue Datensatz ihre Bedingungen entspricht.
5.  Wenn eine Übereinstimmung gefunden wird, wird die Pushbenachrichtigung an das Gerät gesendet, die das Abonnement mit Informationen über den Datensatz registriert, die ausgelöst wurde.


Mit diesem Wissen vorhanden ist sehen wir uns das Erstellen von Abonnements in einer Xamarin.IOS 8-Anwendung.

#### <a name="creating-subscriptions"></a>Erstellen von Abonnements

Der folgende Code kann zum Erstellen eines Abonnements verwendet werden:

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

Zunächst erstellt sie ein Prädikat, das die Bedingung zum Auslösen des Abonnements enthält. Als Nächstes wird das Abonnement für einen bestimmten Datensatztyp erstellt und die Möglichkeit, wenn der Trigger überprüft wird. Schließlich definiert es den Typ der Benachrichtigung, die auftreten, wenn das Abonnement ausgelöst wird, und sie das Abonnement fügt aus.

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

Verwenden der Einfachheit halber-API, der Aufruf ist asynchron, einfach und bietet einfache Fehlerbehandlung.

#### <a name="handling-push-notifications"></a>Behandlung von Pushbenachrichtigungen

Wenn der Entwickler zuvor Apple Push Notifications (APS) verwendet, sollten die Verarbeitung von Benachrichtigungen von CloudKit generiert vertraut sein.

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

Der obige Code fragt CloudKit, um die Benutzerinformationen in einer Benachrichtigung CloudKit zu analysieren. Als Nächstes werden die Informationen zur Warnung extrahiert. Klicken Sie abschließend der Typ der Benachrichtigung wird getestet, und die Benachrichtigung wird entsprechend behandelt wird.

In diesem Abschnitt wurde gezeigt, wie beantworten Sie die Big Data, ein winziges Gerät Problem über Abfragen und Abonnements angezeigt. Die Anwendung wird lassen Sie die umfangreiche Daten in der Cloud, und verwenden Sie diese Technologien, um die Ansichten in diesem Dataset enthalten.

## <a name="cloudkit-user-accounts"></a>CloudKit-Benutzerkonten

Wie zu Beginn dieses Artikels erwähnt, basiert die CloudKit auf die vorhandene Infrastruktur für die iCloud. Im folgende Abschnitt behandelt, ausführlich wie Konten für Entwickler mit der CloudKit-API verfügbar gemacht werden.

### <a name="authentication"></a>Authentifizierung

Die erste Überlegung ist die Authentifizierung beim Umgang mit Benutzerkonten. CloudKit unterstützt die Authentifizierung über die iCloud-aktuell angemeldeten Benutzer auf dem Gerät. Authentifizierung findet im Hintergrund und wird von iOS verarbeitet. Auf diese Weise müssen Entwickler nicht die Details der Implementierung einer Authentifizierung kümmern. Sie testen nur um festzustellen, ob ein Benutzer angemeldet ist.

### <a name="user-account-information"></a>Benutzerkontoinformationen

CloudKit bietet die folgenden Benutzerinformationen für dem Entwickler:

-  **Identität** – eine Möglichkeit, die den Benutzer eindeutig identifiziert.
-  **Metadaten** – die Möglichkeit zum Speichern und Abrufen von Informationen zu Benutzern.
-  **Datenschutz** – alle Informationen in eine bewusste Datenschutz-Manor behandelt wird. "Nothing" ist verfügbar, es sei denn, damit der Benutzer zugestimmt hat.
-  **Ermittlung** – bietet Benutzern die Möglichkeit, seine Freunde zu ermitteln, die die gleiche Anwendung verwenden.


Als Nächstes werden wir diese Themen im Detail betrachten.

#### <a name="identity"></a>Identität

Wie bereits erwähnt, bietet CloudKit eine Möglichkeit für die Anwendung aus, um einen bestimmten Benutzer eindeutig zu identifizieren:

 [![](intro-to-cloudkit-images/image40.png "Eindeutig sich ein bestimmter Benutzer")](intro-to-cloudkit-images/image40.png#lightbox)

Es ist eine Clientanwendung, die auf Geräte eines Benutzers und alle Datenbanken innerhalb des Containers CloudKit bestimmte Benutzer Private ausgeführt. Die Clientanwendung wird mit einem der bestimmten Benutzer verknüpft werden. Dies basiert auf den Benutzer, der in iCloud lokal auf dem Gerät angemeldet ist.

Da diese aus der iCloud-stammen, ist eine umfassende Store von Benutzerinformationen zu sichern. Und da iCloud tatsächlich den Container hostet, können sie Benutzer korrelieren. In der obigen Grafik, die der Benutzer, dessen iCloud-Konto `user@icloud.com` für den aktuellen Client verknüpft ist.

In Abständen von Containern ist eine eindeutige, nach dem Zufallsprinzip generierte Benutzer-ID erstellt und iCloud-Konto des Benutzers (e-Mail-Adresse) zugeordnet. Dieser Benutzer-ID für die Anwendung zurückgegeben, und kann verwendet werden, in keiner Weise, die der Entwickler Ihr als angemessen erscheint.

> [!NOTE]
> Verschiedene Anwendungen, die ausgeführt wird, auf dem gleichen Gerät für den gleichen iCloud müssen unterschiedliche IDs, da sie unterschiedliche CloudKit Container verbunden sind.

Der folgende Code Ruft die CloudKit-Benutzer-ID für den derzeit angemeldeten in iCloud-Benutzer auf dem Gerät:

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

Der obige Code fordert den CloudKit-Container, die ID des aktuell angemeldeten Benutzers angeben. Da diese Informationen aus der iCloud-Server stammen, wird der Aufruf asynchron ist, und Fehlerbehandlung ist notwendig.

#### <a name="metadata"></a>Metadaten

Jeder Benutzer in CloudKit hat es sich um bestimmte Metadaten, die sie beschreiben. Diese Metadaten werden als ein Datensatz CloudKit dargestellt:

 [![](intro-to-cloudkit-images/image41.png "Jeder Benutzer in CloudKit hat bestimmten Metadaten, die sie beschreiben")](intro-to-cloudkit-images/image41.png#lightbox)

Suchen Sie in der privaten Datenbank für einen bestimmten Benutzer von einem Container vorhanden, wird ein Datensatz, die dieser Benutzer definiert ist. Es gibt viele Benutzerdatensätze in der öffentlichen Datenbank, eine für jeden Benutzer des Containers. Eines der folgenden verfügen eine Datensatz-ID, die den derzeit angemeldeten Benutzers Datensatz-ID entspricht.

Benutzerdatensätze in der öffentlichen Datenbank sind weltweit lesbar. Sie werden zum größten Teil, wie ein normales Eintrag behandelt, und weisen den Typ `CKRecordTypeUserRecord`. Diese Einträge werden vom System reserviert und stehen nicht für Abfragen.

Verwenden Sie den folgenden Code ein Benutzerdatensatz den Zugriff auf:

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

Der obige Code werden die zurückzugebenden Benutzerdatensatz für den Benutzer, die ID ist, wir über Zugriff auf, Öffentliche Datenbank gefragt. Da diese Informationen aus der iCloud-Server stammen, wird der Aufruf asynchron ist, und Fehlerbehandlung ist notwendig.

#### <a name="privacy"></a>Datenschutz

CloudKit war entwerfen, wird standardmäßig zum Schutz der Daten des aktuell angemeldeten Benutzers. Keine personenbezogene Informationen über den Benutzer wird standardmäßig verfügbar gemacht. Es gibt einige Fälle, in dem die Anwendung nur begrenzte Informationen über den Benutzer erforderlich sind.

In diesen Fällen kann die Anwendung anfordern, dass der Benutzer diese Informationen offen legen. Ein Dialogfeld wird für den Benutzer auffordert, um sich für das Verfügbarmachen von Kontoinformationen angezeigt werden.

#### <a name="discovery"></a>Suche

Vorausgesetzt, dass die Benutzer als erfolgter Anmeldung auf die Anwendung den Zugriff auf ihre Kontoinformationen der Benutzer eingeschränkt, können sie für andere Benutzer der Anwendung ermittelt werden:

 [![](intro-to-cloudkit-images/image42.png "Ein Benutzer kann für andere Benutzer der Anwendung erkennbar sein.")](intro-to-cloudkit-images/image42.png#lightbox)

Kommuniziert der Client-Anwendung in einen Container, und der Container iCloud, den Zugriff auf Benutzer Informationen kommuniziert. Der Benutzer kann eine e-Mail-Adresse angeben und die Ermittlung kann zum Abrufen von Informationen über den Benutzer wieder verwendet werden. Optional können die Benutzer-ID auch verwendet werden, um Informationen über den Benutzer zu ermitteln.

CloudKit bietet auch eine Möglichkeit, Informationen über alle Benutzer zu ermitteln, die Freunde des derzeit angemeldeten möglicherweise für Benutzer von iCloud durch Abfragen der gesamten Adressbuch. Der Prozess CloudKit Abrufen des Benutzers Kontakt Buch und e-Mail-Adressen verwendet, um festzustellen, ob es andere Benutzer der Anwendung, die diese Adressen übereinstimmen.

Dadurch kann es sich um die Anwendung des Benutzers Kontakt Buch zu nutzen, ohne den Zugriff darauf bereitstellen oder den Benutzer auffordert, den Zugriff auf die Kontakte zu genehmigen. Zu keinem Zeitpunkt ist die Kontaktinformationen für die Anwendung zur Verfügung gestellt, nur den CloudKit-Prozess Zugriff hat.

Zusammenfassend lässt sich sagen, stehen drei verschiedene Arten von Eingaben für die Ermittlung der Benutzer zur Verfügung:

-  **Datensatz-ID des Benutzers** – Ermittlung kann für die Benutzer-ID des derzeit angemeldeten erfolgen in CloudKit-Benutzer.
-  **E-Mail des Benutzers** – der Benutzer kann eine e-Mail-Adresse angeben und kann für die Ermittlung verwendet werden.
-  **Wenden Sie sich an Buch** – Adressbuch des Benutzers kann verwendet werden, um Benutzer der Anwendung zu ermitteln, die die gleiche e-Mail-Adresse in ihre Kontakte aufgeführt.


Ermittlung von Benutzerrichtlinien gibt Folgendes zurück:

-  **Datensatz-ID des Benutzers** – die eindeutige ID eines Benutzers in die Öffentliche Datenbank.
-  **Erste und letzte Name** – wie in der öffentlichen Datenbank gespeichert.


Diese Informationen werden nur für Benutzer zurückgegeben werden, die sich entschieden, in der Ermittlung.

Im folgende Code werden Informationen zu in iCloud auf dem Gerät angemeldeten Benutzers zu ermitteln:

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

In diesem Abschnitt haben wir die vier wichtigsten Bereiche des Zugriffs auf dem Konto eines Benutzers behandelt, die CloudKit für eine Anwendung bereitstellen kann. Vom Erhalt von Identität und die Metadaten des Benutzers, zu den Datenschutzrichtlinien, die in CloudKit und schließlich die Möglichkeit zum Ermitteln von anderen Benutzern der Anwendung integriert werden.

## <a name="the-development-and-production-environments"></a>Die Entwicklungs- und Produktionsumgebungen

CloudKit stellt separate Umgebungen für Entwicklungs- und Produktionsumgebungen für Record-Typen und Daten von einer Anwendung bereit. Die Entwicklungsumgebung ist eine flexiblere, die nur für Mitglieder eines Entwicklungsteams verfügbar ist. Wenn eine Anwendung ein neues Feld zu einem Datensatz fügt und diesen Datensatz in der Entwicklungsumgebung speichert, aktualisiert der Server automatisch die Schemainformationen an.

Der Entwickler kann diese Funktion verwenden, so ändern Sie ein Schema während der Entwicklung, Zeit zu sparen. Ein Nachteil ist, nach einem Datensatz ein Feld hinzugefügt wurde, der Datentyp, der diesem Feld zugeordneten programmgesteuert geändert werden kann. Um ein Feld vom Typ zu ändern, muss der Entwickler löschen Sie das Feld in [CloudKit Dashboard](https://icloud.developer.apple.com/dashboard/) und erneut mit den neuen Typ hinzufügen.

Vor der Bereitstellung der Anwendungs an, der Entwickler kann ihre Schemas und Daten zu migrieren Umgebung für die Produktion mit **CloudKit Dashboard**. Wenn Sie mit der Produktionsumgebung ausführen zu können, wird verhindert, dass der Server eine Anwendung aus programmgesteuert eine Änderung des Schemas. Entwickler kann weiterhin Änderungen mit **CloudKit Dashboard** aber versucht, einen Datensatz in der Produktionsumgebung führen zu Fehlern Felder hinzufügen.

> [!NOTE]
> Der iOS Simulator funktioniert nur mit der **Entwicklungsumgebung**. Wenn der Entwickler ist bereit zum Testen einer Anwendung in einem **Produktionsumgebung**, ein physisches iOS-Gerät ist erforderlich.


## <a name="shipping-a-cloudkit-enabled-app"></a>Versand einer CloudKit-fähigen App

Vor dem Versand einer Anwendung, CloudKit verwendet, müssen sie so konfiguriert werden, soll die **CloudKit Produktionsumgebung** oder die Anwendung wird von Apple abgelehnt werden.

Führen Sie folgende Schritte aus:

1. In Visual Studio für Ma, kompilieren Sie die Anwendung für die **Version** > **iOS-Gerät**: 

    [![](intro-to-cloudkit-images/shipping01.png "Kompilieren Sie die Anwendung zur Veröffentlichung")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. Von der **erstellen** , wählen Sie im Menü **Archiv**: 

    [![](intro-to-cloudkit-images/shipping02.png "Wählen Sie Archiv aus")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. Die **Archiv** wird erstellt und in Visual Studio für Mac angezeigt: 

    [![](intro-to-cloudkit-images/shipping03.png "Das Archiv wird erstellt und angezeigt werden")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Starten Sie **Xcode**.
5. Von der **Fenster** , wählen Sie im Menü **Organisator**: 

    [![](intro-to-cloudkit-images/shipping04.png "Wählen Sie Organisator")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Wählen Sie die Anwendung Archiv aus, und klicken Sie auf die **exportieren...**  Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping05.png "Archivieren von der Anwendung")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. Wählen Sie eine Methode für den Export, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping06.png "Wählen Sie eine Methode für den export")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Wählen Sie die **Entwicklungsteam** aus der Dropdown-Liste und klicken Sie auf die **auswählen** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping07.png "Wählen Sie das Entwicklungsteam aus der Dropdown-Liste")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Wählen Sie **Produktion** aus der Dropdown-Liste und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping08.png "Produktion in der Dropdownliste auswählen.")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. Überprüfen Sie die Einstellung und klicken Sie auf die **exportieren** Schaltfläche: 

    [![](intro-to-cloudkit-images/shipping09.png "Überprüfen Sie die Einstellung")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Wählen Sie einen Speicherort aus, um die Anwendung generieren `.ipa` Datei.

Der Prozess ist ähnlich für die Anwendung direkt in iTunes Connect übermitteln, klicken Sie einfach auf die **senden...**  Schaltfläche und nicht den Export... nach der Auswahl eines Archivs im Planer-Fenster.

## <a name="when-to-use-cloudkit"></a>Verwenden von CloudKit

Wie wir in diesem Artikel gesehen haben, bietet es sich bei CloudKit um eine einfache Möglichkeit für eine Anwendung zum Speichern und Abrufen von Informationen aus der iCloud-Server. Davon abgesehen CloudKit veralteter oder nicht als veraltet markiert die vorhandenen Tools oder Frameworks.

### <a name="use-cases"></a>Anwendungsfälle

Die folgenden Anwendungsfälle tragen den Entwickler entscheiden, wann eine bestimmte iCloud-Framework oder Technologie verwendet:

-  **iCloud-Schlüssel-Wert-Store** – asynchron kleine Menge Daten auf dem neuesten Stand hält und eignet sich hervorragend für die Arbeit mit den Anwendungsvoreinstellungen. Es ist jedoch für eine sehr kleine Menge an Informationen eingeschränkt.
-  **iCloud Drive** : basiert auf vorhandenen iCloud-Dokumente-APIs und bietet eine einfache API, um unstrukturierte Daten aus dem Dateisystem zu synchronisieren. Es bietet einen vollständige offlinebereitstellung Cache unter Mac OS X und eignet sich hervorragend für dokumentanwendungen.
-  **iCloud Kerndaten** – können Sie Daten zwischen aller Geräte des Benutzers repliziert werden. Die Daten sind Single user- oder sich hervorragend für Keeping private, strukturierte Daten synchronisiert.
-  **CloudKit** – stellt die öffentlichen Daten sowohl Struktur bereit und Massenimport und ist in der Lage, sowohl für große Datasets als auch für große unstrukturierte Dateien zu verarbeiten. Die gebunden, die dem Benutzer die iCloud-Konto, und bietet Client geleitet, die Datenübertragung.


Halten diese Anwendungsfälle, denken Sie daran, der Entwickler sollten Auswählen der richtigen iCloud-Technologie für sowohl den aktuellen erforderliche Anwendungsfunktionen bereitstellen, und geben Sie gute Skalierbarkeit für zukünftiges Wachstum.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden eine kurze Einführung in CloudKit API behandelt. Es hat gezeigt, wie zum Bereitstellen und konfigurieren eine Xamarin.IOS-Anwendung zum Verwenden von CloudKit. Es wurden die Funktionen der Einfachheit halber CloudKit-API behandelt. Kann anzeigen zum Entwerfen einer CloudKit-fähige Anwendung, für die Skalierbarkeit, die mithilfe von Abfragen und Abonnements. Und schließlich hat es die Benutzerkonto-Informationen, die zu einer Anwendung von CloudKit verfügbar gemacht, wird angezeigt.

## <a name="related-links"></a>Verwandte Links

- [CloudKitAtlas (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Erstellen eines bereitstellungsprofils](~/ios/get-started/installation/device-provisioning/index.md)
