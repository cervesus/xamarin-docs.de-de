---
title: Cloudkit in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie mit cloudkit in xamarin. IOS arbeiten. Es bietet eine Übersicht über cloudkit und erläutert, wie Sie es, die cloudkit-Benutzerfreundlichkeit-API, Skalierbarkeit, Benutzerkonten sowie Entwicklungs-und Produktionsumgebungen aktivieren können.
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/11/2016
ms.openlocfilehash: af0765adb7e059bdc80c0b851b4bdcad8be0e3e4
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227831"
---
# <a name="cloudkit-in-xamarinios"></a>Cloudkit in xamarin. IOS

Das cloudkit-Framework optimiert die Entwicklung von Anwendungen, die auf icloud zugreifen. Dies umfasst das Abrufen von Anwendungsdaten und assetrechten sowie die Möglichkeit, Anwendungsinformationen sicher zu speichern. Dieses Kit bietet Benutzern eine Ebene der Anonymität, indem Sie den Zugriff auf Anwendungen mit ihren icloud-IDs ermöglicht, ohne persönliche Informationen zu teilen.

Entwickler können sich auf Ihre Client seitigen Anwendungen konzentrieren und es icloud nicht mehr benötigen, serverseitige Anwendungslogik zu schreiben. Cloudkit bietet Authentifizierung, private und öffentliche Datenbanken sowie strukturierte Daten-und Asset-Speicherdienste.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel beschriebenen Schritte auszuführen:

- **Xcode und das IOS SDK** – die Xcode-und IOS 8-APIs von Apple müssen auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac muss auf dem Benutzergerät installiert und konfiguriert werden.
- **IOS 8-Gerät** – ein IOS-Gerät, auf dem die neueste Version von IOS 8 zum Testen ausgeführt wird.

## <a name="what-is-cloudkit"></a>Was ist cloudkit?

Cloudkit ist eine Möglichkeit, dem Entwickler Zugriff auf die icloud-Server zu verschaffen. Es ist die Grundlage für icloud Drive und die icloud-Fotobibliothek. Cloudkit wird auf Mac OS X-und Apple IOS-Geräten unterstützt.

 [![](intro-to-cloudkit-images/image1.png "Unterstützung von cloudkit auf Mac OS X-und Apple IOS-Geräten")](intro-to-cloudkit-images/image1.png#lightbox)

Cloudkit verwendet die icloud-Konto Infrastruktur. Wenn ein Benutzer an einem icloud-Konto auf dem Gerät angemeldet ist, verwendet cloudkit seine ID, um den Benutzer zu identifizieren. Wenn kein Konto verfügbar ist, wird eingeschränkter Schreib geschützter Zugriff bereitgestellt.

Cloudkit unterstützt das Konzept der öffentlichen und privaten Datenbanken. Öffentliche Datenbanken stellen eine "Suppe" für alle Daten bereit, auf die der Benutzer Zugriff hat. Private Datenbanken dienen zum Speichern der privaten Daten, die an einen bestimmten Benutzer gebunden sind.

Cloudkit unterstützt sowohl strukturierte als auch Massendaten. Es ist in der Lage, große Dateiübertragungen nahtlos zu verarbeiten. Cloudkit kümmert sich um die effiziente Übertragung großer Dateien auf und von den icloud-Servern im Hintergrund, sodass der Entwickler sich auf andere Aufgaben konzentrieren kann.

> [!NOTE]
> Es ist wichtig zu beachten, dass cloudkit eine _Transport Technologie_ist. Sie bietet keine Persistenz. Sie ermöglicht es einer Anwendung nur, Informationen von den Servern effizient zu senden und zu empfangen.

Zum Zeitpunkt der Erstellung dieses Artikels stellt Apple das cloudkit anfänglich kostenlos mit einem hohen Grenzwert für die Bandbreite und die Speicherkapazität bereit. Bei größeren Projekten oder Anwendungen mit einer großen Benutzerbasis deutet Apple darauf hin, dass eine preisgünstige Preisskala bereitgestellt wird.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Aktivieren von cloudkit in einer xamarin-Anwendung

Bevor eine xamarin-Anwendung das cloudkit-Framework verwenden kann, muss die Anwendung ordnungsgemäß bereitgestellt werden, wie in den Handbüchern [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) und [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) beschrieben.

1. Öffnen Sie das Projekt in Visual Studio für Mac oder Visual Studio.
2. Öffnen Sie im **Projektmappen-Explorer**die Datei " **Info. plist** ", und stellen Sie sicher, dass die **Bündel** -ID mit der ID übereinstimmt, die in der im Rahmen der Einrichtung der Bereitstellung erstellten **App-ID** definiert wurde:

    [![](intro-to-cloudkit-images/image26a.png "Bündel-ID eingeben")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3. Scrollen Sie nach unten in der Datei **Info. plist** , und wählen Sie **aktivierte hintergrundmodi**, **Speicherort Aktualisierungen** und **Remote Benachrichtigungen**aus:

    [![](intro-to-cloudkit-images/image27a.png "Wählen Sie aktivierte hintergrundmodi, Speicherort Updates und Remote Benachrichtigungen aus.")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4. Klicken Sie mit der rechten Maustaste auf das IOS-Projekt in der Lösung, und wählen Sie **Optionen**
5. Wählen Sie **IOS-Bündel Signierung**aus, und wählen Sie die oben erstellte **Entwickler Identität** und das **Bereitstellungs Profil** .
6. Stellen Sie sicher, dass die Datei " **Berechtigungen. plist** " die Option **icloud** , **Key-Value Storage** und **cloudkit** umfasst.
7. Stellen Sie sicher, dass der **ubiquity-Container** für die Anwendung vorhanden ist (wie oben erstellt). Beispiel: `iCloud.com.your-company.CloudKitAtlas`
8. Speichern Sie die Änderungen in der Datei.


Wenn diese Einstellungen vorhanden sind, kann die Anwendung jetzt auf die cloudkit-Framework-APIs zugreifen.

## <a name="cloudkit-api-overview"></a>Übersicht über die cloudkit-API

Vor der Implementierung von cloudkit in einer xamarin IOS-Anwendung werden in diesem Artikel die Grundlagen des cloudkit-Frameworks erläutert, das die folgenden Themen umfasst:

1. **Container** – isolierte Silos der icloud-Kommunikation.
2. **Datenbanken** – öffentlich und privat sind für die Anwendung verfügbar.
3. **Datensätze** – der Mechanismus, bei dem strukturierte Daten in und aus cloudkit verschoben werden.
4. **Daten Satz Zonen** – sind Gruppen von Datensätzen.
5. **Daten Satz** Bezeichner – sind vollständig normalisiert und stellen den spezifischen Speicherort des Datensatzes dar.
6. **Verweis** – stellen Beziehungen zwischen übergeordneten und untergeordneten Datensätzen in einer bestimmten Datenbank bereit.
7. **Assets** – ermöglichen das Hochladen von Dateien mit großen, unstrukturierten Daten in die icloud und die Zuordnung zu einem bestimmten Datensatz.


### <a name="containers"></a>Container

Eine bestimmte Anwendung, die auf einem IOS-Gerät ausgeführt wird, wird immer neben anderen Anwendungen und Diensten auf diesem Gerät ausgeführt. Auf dem Client Gerät wird die Anwendung in irgendeiner Weise mit einem Silo oder einem Sandkasten versehen. In einigen Fällen handelt es sich hierbei um eine Literale Sandbox, und in anderen Fällen wird die Anwendung einfach in Ihrem eigenen Speicherbereich ausgeführt.

Das Konzept, eine Client Anwendung und deren Ausführung von anderen Clients getrennt zu machen, ist sehr leistungsstark und bietet die folgenden Vorteile:

1. **Sicherheit** – eine Anwendung kann sich nicht auf andere Client-Apps oder das Betriebssystem selbst stören.
1. **Stabilität** – wenn die Client Anwendung abstürzt, können andere apps des Betriebssystems nicht mehr übernommen werden.
1. **Datenschutz** – jede Client Anwendung hat eingeschränkten Zugriff auf die persönlichen Informationen, die auf dem Gerät gespeichert sind.


Cloudkit wurde entworfen, um die gleichen Vorteile wie die oben aufgeführten zu bieten, und wendet Sie auf die Verwendung cloudbasierter Informationen an:

 [![](intro-to-cloudkit-images/image31.png "Cloudkit-apps kommunizieren mithilfe von Containern")](intro-to-cloudkit-images/image31.png#lightbox)

Genau wie die Anwendung, die auf dem Gerät ausgeführt wird, ist die Kommunikation der Anwendung mit icloud 1-of-many. Jede dieser unterschiedlichen Kommunikations Silos wird als Container bezeichnet.

Container werden im cloudkit-Framework über die `CKContainer` -Klasse verfügbar gemacht. Standardmäßig kommuniziert eine Anwendung mit einem Container, und dieser Container trennt die Daten für diese Anwendung. Dies bedeutet, dass mehrere Anwendungen Informationen in demselben icloud-Konto speichern können. diese Informationen werden jedoch nie miteinander vermischt.

Die Containerisierung von icloud-Daten ermöglicht cloudkit auch das Kapseln von Benutzerinformationen. Auf diese Weise erhält die Anwendung begrenzten Zugriff auf das icloud-Konto und die darin gespeicherten Benutzerinformationen, während gleichzeitig der Schutz und die Sicherheit des Benutzers geschützt werden.

Container werden vollständig vom Entwickler der Anwendung über das wwdr-Portal verwaltet. Der Namespace des Containers ist für alle Apple-Entwickler Global, sodass der Container nicht nur für die Anwendungen eines bestimmten Entwicklers, sondern für alle Apple-Entwickler und-Anwendungen eindeutig sein darf.

Apple schlägt beim Erstellen des Namespace für Anwendungs Container die Verwendung der Reverse-DNS-Notation vor. Beispiel:

```csharp
iCloud.com.company-name.application-name
```

Während Container standardmäßig an eine bestimmte Anwendung gebunden sind, können Sie von allen Anwendungen gemeinsam genutzt werden. Daher können mehrere Anwendungen in einem einzelnen Container koordiniert werden. Eine einzelne Anwendung kann auch mit mehreren Containern kommunizieren.

### <a name="databases"></a>Databases

Eine der Hauptfunktionen von cloudkit besteht darin, das Datenmodell einer Anwendung und die Replikation für das Modell auf die icloud-Server zu übernehmen. Einige Informationen sind für den Benutzer gedacht, der ihn erstellt hat. andere Informationen sind öffentliche Daten, die von einem Benutzer für die öffentliche Verwendung (z. b. eine Restaurant Überprüfung) erstellt werden können, oder es kann sich um Informationen handeln, die der Entwickler für die Anwendung veröffentlicht hat. In beiden Fällen ist die Zielgruppe nicht nur ein einziger Benutzer, sondern eine Community von Menschen.

 [![](intro-to-cloudkit-images/image32.png "Cloudkit-Container Diagramm")](intro-to-cloudkit-images/image32.png#lightbox)

Innerhalb eines Containers ist die öffentliche Datenbank der erste und wichtigste. An dieser Stelle werden alle öffentlichen Informationen angezeigt. Außerdem gibt es mehrere einzelne private Datenbanken für jeden Benutzer der Anwendung.

Wenn die Anwendung auf einem IOS-Gerät ausgeführt wird, kann Sie nur auf die Informationen für den derzeit angemeldeten icloud-Benutzer zugreifen. Die Anwendungs Ansicht des Containers lautet also wie folgt:

 [![](intro-to-cloudkit-images/image33.png "Anwendungs Ansicht des Containers")](intro-to-cloudkit-images/image33.png#lightbox)

Es können nur die öffentliche Datenbank und die private Datenbank angezeigt werden, die mit dem derzeit angemeldeten icloud-Benutzer verknüpft sind.

Datenbanken werden im cloudkit-Framework über die `CKDatabase` -Klasse verfügbar gemacht. Jede Anwendung hat Zugriff auf zwei Datenbanken: die öffentliche Datenbank und die private.

Der Container ist der erste Einstiegspunkt in cloudkit. Der folgende Code kann verwendet werden, um über den Standard Container der Anwendung auf die öffentliche und private Datenbank zuzugreifen:

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

Im folgenden sind die Unterschiede zwischen den Datenbanktypen aufgeführt:

||Öffentliche Datenbank|Private Datenbank|
|---|--- |--- |
|**Datentyp**|Freigegebene Daten|Daten des aktuellen Benutzers|
|**Kontingent**|Im Kontingent des Entwicklers berücksichtigt|Im Benutzer Kontingent berücksichtigt|
|**Standard Berechtigungen**|Welt lesbar|Benutzer lesbar|
|**Bearbeiten von Berechtigungen**|icloud-dashboardrollen über eine Daten Satz-Klassenebene|N/V|

### <a name="records"></a>Datensätze

Container enthalten Datenbanken und innerhalb von Datenbanken Datensätze. Datensätze sind der Mechanismus, bei dem strukturierte Daten in und aus cloudkit verschoben werden:

 [![](intro-to-cloudkit-images/image34.png "Container enthalten Datenbanken und innerhalb von Datenbanken Datensätze.")](intro-to-cloudkit-images/image34.png#lightbox)

Datensätze werden im cloudkit-Framework über die `CKRecord` -Klasse verfügbar gemacht, die Schlüssel-Wert-Paare umschließt. Eine Instanz eines Objekts in einer Anwendung entspricht einem `CKRecord` in cloudkit. Außerdem verfügt jeder `CKRecord` über einen Daten Satz Typen, der der-Klasse eines Objekts entspricht.

Datensätze verfügen über ein Just-in-Time-Schema, sodass die Daten in cloudkit beschrieben werden, bevor Sie zur Verarbeitung übergeben werden. Von diesem Punkt aus werden die Informationen von cloudkit interpretiert und die Logistik zum Speichern und Abrufen des Datensatzes behandelt.

Die `CKRecord` -Klasse unterstützt auch eine breite Palette von Metadaten. Ein Datensatz enthält z. b. Informationen über den Zeitpunkt der Erstellung und den Benutzer, der ihn erstellt hat. Ein Datensatz enthält außerdem Informationen über den Zeitpunkt der letzten Änderung und den Benutzer, der die Änderung vorgenommen hat.

Datensätze enthalten das Konzept eines Änderungs Tags. Dies ist eine frühere Version einer Revision eines bestimmten Datensatzes. Das änderungstag dient als einfache Methode, um zu bestimmen, ob der Client und der Server über dieselbe Version eines bestimmten Datensatzes verfügen.

Wie bereits erwähnt, `CKRecords` können Sie Schlüssel-Wert-Paare einschließen, sodass die folgenden Datentypen in einem Datensatz gespeichert werden können:

1. `NSString`
1. `NSNumber`
1. `NSData`
1. `NSDate`
1. `CLLocation`
1. `CKReferences`
1. `CKAssets`


Zusätzlich zu den einzelnen Werttypen kann ein Datensatz ein homogenes Array von einem der oben aufgeführten Typen enthalten.

Der folgende Code kann verwendet werden, um einen neuen Datensatz zu erstellen und in einer-Datenbank zu speichern:

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>Daten Satz Zonen

Datensätze sind in einer bestimmten Datenbank nicht selbst vorhanden – Gruppen von Datensätzen sind innerhalb einer Daten Satz Zone nebeneinander vorhanden. Daten Satz Zonen können als Tabellen in herkömmlichen relationalen Datenbanken betrachtet werden:

 [![](intro-to-cloudkit-images/image35.png "In einer Daten Satz Zone sind Gruppen von Datensätzen vorhanden.")](intro-to-cloudkit-images/image35.png#lightbox)

Es können mehrere Datensätze innerhalb einer angegebenen Daten Satz Zone und mehrerer Daten Satz Zonen innerhalb einer bestimmten Datenbank vorhanden sein. Jede Datenbank enthält eine Standarddaten Satz Zone:

 [![](intro-to-cloudkit-images/image36.png "Jede Datenbank enthält eine Standarddaten Satz Zone und eine benutzerdefinierte Zone.")](intro-to-cloudkit-images/image36.png#lightbox)

Hier werden Datensätze standardmäßig gespeichert. Außerdem können benutzerdefinierte Daten Satz Zonen erstellt werden. Daten Satz Zonen stellen die Grundgenauigkeit dar, mit der atomarische Commits und Änderungsnachverfolgung ausgeführt werden.

## <a name="record-identifiers"></a>Daten Satz Bezeichner

Daten Satz Bezeichner werden als Tupel dargestellt, das sowohl den vom Client bereitgestellten Daten Satz Namen als auch die Zone enthält, in der der Datensatz vorhanden ist. Daten Satz Bezeichner weisen die folgenden Eigenschaften auf:

- Sie werden von der Client Anwendung erstellt.
- Sie sind vollständig normalisiert und stellen den jeweiligen Speicherort des Datensatzes dar.
- Durch Zuweisen der eindeutigen ID eines Datensatzes in einer fremd Datenbank zum Daten Satz Namen können Sie lokale Datenbanken, die nicht in cloudkit gespeichert sind, überbrücken.


Wenn Entwickler neue Datensätze erstellen, können Sie einen Daten Satz Bezeichner übergeben. Wenn kein Daten Satz Bezeichner angegeben wird, wird automatisch eine UUID erstellt und dem Datensatz zugewiesen.

Wenn Entwickler neue Daten Satz Bezeichner erstellen, können Sie festlegen, dass die Daten Satz Zonen, zu denen jeder Datensatz gehört, festgelegt werden sollen. Wenn kein Wert angegeben ist, wird die Standarddaten Satz Zone verwendet.

Daten Satz Bezeichner werden im cloudkit-Framework über die `CKRecordID` -Klasse verfügbar gemacht. Der folgende Code kann verwendet werden, um einen neuen Daten Satz Bezeichner zu erstellen:

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Verweise

Verweise stellen Beziehungen zwischen verknüpften Datensätzen in einer bestimmten Datenbank bereit:

 [![](intro-to-cloudkit-images/image37.png "Verweise stellen Beziehungen zwischen verknüpften Datensätzen in einer bestimmten Datenbank bereit.")](intro-to-cloudkit-images/image37.png#lightbox)

Im obigen Beispiel sind übergeordnete Elemente mit untergeordneten Elementen, sodass das untergeordnete Element ein untergeordneter Datensatz des übergeordneten Datensatzes ist. Die Beziehung wird vom untergeordneten Datensatz zum übergeordneten Datensatz geleitet und als *Rück Verweis*bezeichnet.

Verweise werden im cloudkit-Framework über die `CKReference` -Klasse verfügbar gemacht. Mit diesen Funktionen kann der icloud-Server die Beziehung zwischen Datensätzen verstehen.

Verweise stellen den Mechanismus hinter kaskadierenden Lösch Vorgängen bereit. Wenn ein übergeordneter Datensatz aus der Datenbank gelöscht wird, werden auch alle untergeordneten Datensätze (wie in einer Beziehung angegeben) automatisch aus der Datenbank gelöscht.

> [!NOTE]
> Verbleibende Zeiger sind bei der Verwendung von cloudkit eine Möglichkeit. Wenn die Anwendung z. b. eine Liste von Daten Satz Zeigern abgerufen hat, einen Datensatz ausgewählt und dann nach dem Datensatz gefragt hat, ist der Datensatz möglicherweise nicht mehr in der Datenbank vorhanden. Eine Anwendung muss so codiert sein, dass diese Situation ordnungsgemäß verarbeitet wird.

Beim Arbeiten mit dem cloudkit-Framework werden die Back-Verweise zwar nicht benötigt Apple hat das System optimiert, um dies als effizientesten Verweistyp zu gestalten.

Beim Erstellen eines Verweises kann der Entwickler entweder einen Datensatz bereitstellen, der bereits im Arbeitsspeicher vorhanden ist, oder einen Verweis auf eine Datensatz-ID erstellen. Wenn Sie einen Daten Satz Bezeichner verwenden und der angegebene Verweis nicht in der Datenbank vorhanden ist, würde ein Verb leibend Zeiger erstellt werden.

Im folgenden finden Sie ein Beispiel für das Erstellen eines Verweises auf einen bekannten Datensatz:

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Objekte

Mithilfe von Assets kann eine Datei mit großen, unstrukturierten Daten in die icloud hochgeladen und einem bestimmten Datensatz zugeordnet werden:

 [![](intro-to-cloudkit-images/image38.png "Mithilfe von Assets kann eine Datei mit großen, unstrukturierten Daten in die icloud hochgeladen und einem bestimmten Datensatz zugeordnet werden.")](intro-to-cloudkit-images/image38.png#lightbox)

Auf dem Client wird eine `CKRecord` erstellt, die die Datei beschreibt, die auf den icloud-Server hochgeladen wird. Eine `CKAsset` wird erstellt, die die Datei enthält und mit dem Datensatz verknüpft ist, der Sie beschreibt.

Wenn die Datei auf den Server hochgeladen wird, wird der Datensatz in der Datenbank abgelegt, und die Datei wird in eine spezielle Massenspeicher Datenbank kopiert. Zwischen dem Daten Satz Zeiger und der hochgeladenen Datei wird eine Verknüpfung erstellt.

Assets werden im cloudkit-Framework über die `CKAsset` -Klasse verfügbar gemacht und dienen zum Speichern großer, unstrukturierter Daten. Da der Entwickler keine großen, unstrukturierten Daten im Arbeitsspeicher haben will, werden Ressourcen mithilfe von Dateien auf dem Datenträger implementiert.

Assets sind im Besitz von Datensätzen, sodass die Assets aus icloud abgerufen werden können, indem der Datensatz als Zeiger verwendet wird. Auf diese Weise kann der Server Assets in den Garbage Collection-Speicher erfassen, wenn der Datensatz, der das Asset besitzt, gelöscht wird.

Da `CKAssets` für die Verarbeitung großer Datendateien vorgesehen ist, wurde das cloudkit von Apple entworfen, um die Assets effizient hochzuladen und herunterzuladen.

Der folgende Code kann verwendet werden, um ein Medienobjekt zu erstellen und es dem Datensatz zuzuordnen:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Wir haben jetzt alle grundlegenden Objekte in cloudkit abgedeckt. Container werden Anwendungen zugeordnet und enthalten Datenbanken. Datenbanken enthalten Datensätze, die in Daten Satz Zonen gruppiert sind und auf die von Daten Satz bezeichatoren verwiesen werden. Beziehungen zwischen übergeordneten und untergeordneten Elementen werden zwischen Datensätzen mit verweisen definiert. Zum Schluss können große Dateien hochgeladen und Datensätzen mithilfe von Assets zugeordnet werden.

## <a name="cloudkit-convenience-api"></a>Cloudkit-praktische API

Apple bietet zwei verschiedene API-Sätze für das Arbeiten mit cloudkit:

- **Operational API** – bietet alle einzelnen Features von cloudkit. Für komplexere Anwendungen bietet diese API eine fein abgestimmte Kontrolle über cloudkit.
- Praktische **API** – bietet eine allgemeine, vorkonfigurierte Teilmenge der cloudkit-Features. Es stellt eine bequeme, einfache Zugriffs Lösung für die cloudkit-Funktionalität in eine IOS-Anwendung bereit.


Die praktische API ist in der Regel die beste Wahl für die meisten IOS-Anwendungen, und Apple empfiehlt, damit zu beginnen. Im restlichen Teil dieses Abschnitts werden die folgenden Themen zur einfacheren API behandelt:

- Speichern eines Datensatzes.
- Abrufen eines Datensatzes.
- Aktualisieren eines Datensatzes.


### <a name="common-setup-code"></a>Allgemeiner Setup Code

Vor den ersten Schritten mit der cloudkit-praktische API ist ein standardmäßiger Setup Code erforderlich. Ändern Sie zunächst die `AppDelegate.cs` Datei der Anwendung, und führen Sie Sie wie folgt aus:

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

Der obige Code macht die öffentlichen und privaten cloudkit-Datenbanken als Verknüpfungen verfügbar, damit Sie im Rest der Anwendung einfacher arbeiten können.

Fügen Sie als nächstes den folgenden Code zu jedem Sicht-oder Ansichts Container hinzu, der cloudkit verwendet:

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Dadurch wird eine Verknüpfung hinzugefügt, um `AppDelegate` die zu erreichen und auf die oben erstellten öffentlichen und privaten Daten Bank Verknüpfungen zuzugreifen.

Wenn dieser Code vorhanden ist, sehen wir uns die Implementierung der cloudkit-praktische API in einer xamarin IOS 8-Anwendung an.

### <a name="saving-a-record"></a>Speichern eines Datensatzes

Mit dem oben dargestellten Muster bei der Erörterung von Datensätzen erstellt der folgende Code einen neuen Datensatz und verwendet die praktische API, um ihn in der öffentlichen Datenbank zu speichern:

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

Im obigen Code sind drei Punkte zu beachten:

1. Durch Aufrufen der `SaveRecord` -Methode `PublicDatabase`von muss der Entwickler nicht angeben, wie die Daten gesendet werden, in welche Zone Sie geschrieben wird usw. Die benutzerfreundliche API kümmert sich um alle diese Details.
1. Der-Rückruf ist asynchron und stellt eine Rückruf Routine bereit, wenn der-Befehl abgeschlossen wird, entweder mit Erfolg oder Fehler. Wenn der-Befehl fehlschlägt, wird eine Fehlermeldung bereitgestellt.
1. Cloudkit bietet keine lokale Speicherung/Persistenz. Es handelt sich hierbei nur um ein Übertragungsmedium. Wenn also eine Anforderung zum Speichern eines Datensatzes gestellt wird, wird er sofort an die icloud-Server gesendet.


> [!NOTE]
> Aufgrund der "verlustfreien" Art der Kommunikation mobiler Netzwerke, bei der Verbindungen ständig gelöscht oder unterbrochen werden, muss der Entwickler bei der Arbeit mit cloudkit die Fehlerbehandlung durchführen.

### <a name="fetching-a-record"></a>Abrufen eines Datensatzes

Wenn ein Datensatz erstellt und erfolgreich auf dem icloud-Server gespeichert wurde, verwenden Sie den folgenden Code zum Abrufen des Datensatzes:

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

Ebenso wie beim Speichern des Datensatzes ist der obige Code asynchron, einfach und erfordert eine hohe Fehlerbehandlung.

### <a name="updating-a-record"></a>Aktualisieren eines Datensatzes

Nachdem ein Datensatz von den icloud-Servern abgerufen wurde, kann der folgende Code verwendet werden, um den Datensatz zu ändern und die Änderungen wieder in der Datenbank zu speichern:

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

Die `FetchRecord` -Methode `PublicDatabase` des gibt einen `CKRecord` zurück, wenn der-Befehl erfolgreich ausgeführt wurde. Die Anwendung ändert dann den Datensatz und ruft `SaveRecord` erneut auf, um die Änderungen wieder in die Datenbank zu schreiben.

In diesem Abschnitt wurde der typische Zeitraum gezeigt, den eine Anwendung bei der Arbeit mit der cloudkit-API verwendet. Die Anwendung speichert Datensätze in der icloud, ruft diese Datensätze aus icloud ab, ändert die Datensätze und speichert diese Änderungen in icloud zurück.

## <a name="designing-for-scalability"></a>Entwerfen von Skalierbarkeit

Bisher wurde in diesem Artikel beschrieben, wie Sie das gesamte Objektmodell einer Anwendung von den icloud-Servern speichern und abrufen, und zwar jedes Mal, wenn Sie mit arbeiten. Dieser Ansatz funktioniert zwar gut mit einer kleinen Datenmenge und einer sehr kleinen Benutzerbasis, kann jedoch nicht gut skaliert werden, wenn sich die Menge der Informationen und/oder die Benutzerbasis vergrößert.

### <a name="big-data-tiny-device"></a>Big Data, kleines Gerät

Je beliebter eine Anwendung ist, desto mehr Daten in der Datenbank und desto weniger praktikabel ist es, einen Cache der gesamten Daten auf dem Gerät zu haben. Die folgenden Verfahren können verwendet werden, um dieses Problem zu beheben:

- **Behalten Sie die großen Daten in der Cloud** – cloudkit wurde für die effiziente Verarbeitung großer Daten entwickelt.
- Der **Client sollte nur einen Slice dieser Daten anzeigen** – das Minimum an Daten, die für die Verarbeitung von Aufgaben zu einem bestimmten Zeitpunkt benötigt werden.
- **Client Sichten können sich ändern** – da jeder Benutzer über unterschiedliche Einstellungen verfügt, kann sich der Slice der angezeigten Daten von Benutzer zu Benutzer ändern, und die individuelle Ansicht eines beliebigen Slice des Benutzers kann abweichen.
- **Der Client verwendet Abfragen, um den Standpunkt zu fokussieren** – mit Abfragen kann der Benutzer eine kleine Teilmenge eines größeren Datasets anzeigen, das in der Cloud vorhanden ist.


### <a name="queries"></a>Abfragen

Wie bereits erwähnt, ermöglichen Abfragen dem Entwickler, eine kleine Teilmenge des größeren Datasets auszuwählen, das in der Cloud vorhanden ist. Abfragen werden im cloudkit-Framework über die `CKQuery` -Klasse verfügbar gemacht.

Eine Abfrage kombiniert drei verschiedene Dinge: einen Daten Satz Typen `RecordType`(), ein Prädikat `NSPredicate`() und optional einen Sortierungs Deskriptor ( `NSSortDescriptors`). Cloudkit unterstützt die `NSPredicate`meisten von.

#### <a name="supported-predicates"></a>Unterstützte Prädikate

Cloudkit unterstützt die folgenden Typen `NSPredicates` von beim Arbeiten mit Abfragen:


1. Übereinstimmende Datensätze, bei denen der Name gleich einem in einer Variablen gespeicherten Wert ist:

    ```csharp
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```

2. Ermöglicht, dass die Übereinstimmung auf einem dynamischen Schlüsselwert basiert, damit der Schlüssel zum Zeitpunkt der Kompilierung nicht bekannt sein muss:

    ```csharp
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```

3. Übereinstimmende Datensätze, bei denen der Wert des Datensatzes größer ist als der angegebene Wert:

    ```csharp
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Übereinstimmende Datensätze, bei denen der Speicherort des Datensatzes innerhalb von 100 Meter des angegebenen Standorts liegt

    ```csharp
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. Cloudkit unterstützt eine tokenisierte Suche. Mit diesem-Befehl werden zwei Token erstellt: `after` eine für und `session`eine andere für. Es wird ein Datensatz zurückgegeben, der diese beiden Token enthält:

    ```csharp
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```

6. Cloudkit unterstützt Verbund Prädikate, `AND` die mithilfe des Operators verknüpft sind.

    ```csharp
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```



#### <a name="creating-queries"></a>Erstellen von Abfragen

Der folgende Code kann verwendet werden, um eine `CKQuery` in einer xamarin IOS 8-Anwendung zu erstellen:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

Zuerst wird ein Prädikat erstellt, um nur Datensätze auszuwählen, die einem bestimmten Namen entsprechen. Anschließend wird eine Abfrage erstellt, mit der Datensätze des angegebenen Daten Satz Typs ausgewählt werden, die dem Prädikat entsprechen.

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

Der obige Code führt die oben erstellte Abfrage aus und führt Sie für die öffentliche Datenbank aus. Da keine Daten Satz Zone angegeben ist, werden alle Zonen durchsucht. Wenn keine Fehler aufgetreten sind, wird ein `CKRecords` Array von zurückgegeben, das den Parametern der Abfrage entspricht.

Die Möglichkeit, Abfragen zu übernehmen, besteht darin, dass es sich um Umfragen handelt, die bei der slizierung durch große Datasets hervorragend sind. Abfragen sind jedoch aus folgenden Gründen für große, größtenteils statische Datasets nicht gut geeignet:

- Sie sind für die Akku Lebensdauer des Geräts ungeeignet.
- Sie sind für den Netzwerk Datenverkehr schlecht.
- Sie sind für den Benutzer nicht sichtbar, da die Informationen darauf beschränkt sind, wie oft die Anwendung die Datenbank abfragt. Benutzer erwarten heute Pushbenachrichtigungen, wenn sich etwas ändert.


### <a name="subscriptions"></a>Abonnements

Beim Umgang mit großen, größtenteils statischen Datasets sollte die Abfrage nicht auf dem Client Gerät ausgeführt werden, sondern auf dem-Server im Auftrag des Clients. Die Abfrage sollte im Hintergrund ausgeführt werden und nach jedem einzelnen Datensatz gespeichert werden, unabhängig davon, ob das aktuelle Gerät oder ein anderes Gerät dieselbe Datenbank berührt.

Zum Schluss sollte eine Pushbenachrichtigung an jedes Gerät gesendet werden, das an die Datenbank angefügt ist, wenn die serverseitige Abfrage ausgeführt wird.

Abonnements werden im cloudkit-Framework über die `CKSubscription` -Klasse verfügbar gemacht. Sie kombinieren einen Daten Satz `RecordType`Typen (), ein Prädikat ( `NSPredicate`) und eine Apple Push `Push`Notification ().

> [!NOTE]
> Cloudkit-Pushvorgänge werden etwas erweitert, da Sie eine Nutzlast enthalten, die cloudkit-spezifische Informationen enthält, wie z. b. die Ursache für den Push.

#### <a name="how-subscriptions-work"></a>Funktionsweise von Abonnements

Vor der Implementierung eines C# Abonnements im Code wird eine kurze Übersicht über die Funktionsweise von Abonnements angezeigt:

 [![](intro-to-cloudkit-images/image39.png "Eine Übersicht über die Funktionsweise von Abonnements")](intro-to-cloudkit-images/image39.png#lightbox)

Das obige Diagramm zeigt den typischen Abonnement Prozess wie folgt:

1. Das Client Gerät erstellt ein neues Abonnement mit dem Satz von Bedingungen, die das Abonnement auslöst, und einer Pushbenachrichtigung, die gesendet wird, wenn der-Triggervorgang auftritt.
2. Das Abonnement wird an die Datenbank gesendet, wo Sie der Sammlung vorhandener Abonnements hinzugefügt wird.
3. Ein zweites Gerät erstellt einen neuen Datensatz und speichert diesen Datensatz in der Datenbank.
4. Die Datenbank durchsucht die Liste der Abonnements, um festzustellen, ob der neue Datensatz mit einer ihrer Bedingungen übereinstimmt.
5. Wenn eine Entsprechung gefunden wird, wird die Pushbenachrichtigung an das Gerät gesendet, das das Abonnement mit Informationen zu dem Datensatz registriert hat, durch den der Trigger ausgelöst wurde.


Wenn dieses Wissen vorhanden ist, sehen wir uns das Erstellen von Abonnements in einer xamarin IOS 8-Anwendung an.

#### <a name="creating-subscriptions"></a>Erstellen von Abonnements

Der folgende Code kann verwendet werden, um ein Abonnement zu erstellen:

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

Zuerst wird ein Prädikat erstellt, das die Bedingung zum Auslösen des Abonnements bereitstellt. Anschließend wird das Abonnement für einen bestimmten Daten Satz Typen erstellt, und die Option wird festgelegt, wenn der-Vorgang getestet wird. Schließlich definiert Sie den Benachrichtigungstyp, der beim Auslösen des Abonnements auftritt, und fügt das Abonnement an.

### <a name="saving-subscriptions"></a>Speichern von Abonnements

Nachdem das Abonnement erstellt wurde, speichert der folgende Code es in der-Datenbank:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

Mithilfe der Easy-API ist der-Befehl asynchron, einfach und ermöglicht eine einfache Fehlerbehandlung.

#### <a name="handling-push-notifications"></a>Behandeln von Pushbenachrichtigungen

Wenn der Entwickler bereits Apple Push Notification (APS) verwendet hat, sollte der Prozess der Handhabung von Benachrichtigungen, die von cloudkit generiert werden, vertraut sein.

Überschreiben `AppDelegate.cs`Sie in der `ReceivedRemoteNotification` die-Klasse wie folgt:

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

Der obige Code fordert cloudkit auf, die userinfo in eine cloudkit-Benachrichtigung zu analysieren. Als nächstes werden Informationen zur Warnung extrahiert. Schließlich wird der Benachrichtigungstyp getestet, und die Benachrichtigung wird entsprechend behandelt.

In diesem Abschnitt wurde gezeigt, wie Sie die oben dargestellten Big Data-Probleme mit kleinen Geräten mithilfe von Abfragen und Abonnements beantworten können. Die Anwendung verlässt ihre großen Daten in der Cloud und verwendet diese Technologien, um Sichten in diesem Dataset bereitzustellen.

## <a name="cloudkit-user-accounts"></a>Cloudkit-Benutzerkonten

Wie zu Beginn dieses Artikels erwähnt, baut cloudkit auf der vorhandenen icloud-Infrastruktur auf. Im folgenden Abschnitt wird ausführlich erläutert, wie Konten einem Entwickler mithilfe der cloudkit-API zur Verfügung gestellt werden.

### <a name="authentication"></a>Authentifizierung

Beim Umgang mit Benutzerkonten muss zunächst die Authentifizierung durchgesetzt werden. Cloudkit unterstützt die Authentifizierung über den aktuell angemeldeten icloud-Benutzer auf dem Gerät. Die Authentifizierung erfolgt im Hintergrund und wird von IOS verarbeitet. Auf diese Weise müssen Entwickler sich keine Gedanken über die Details der Implementierung der Authentifizierung machen. Sie prüfen nur, ob ein Benutzer angemeldet ist.

### <a name="user-account-information"></a>Benutzerkontoinformationen

Cloudkit bietet den Entwicklern die folgenden Benutzerinformationen:

- **Identität** – eine Methode zur eindeutigen Identifizierung des Benutzers.
- **Metadaten** – die Möglichkeit, Informationen zu Benutzern zu speichern und abzurufen.
- **Datenschutz** – alle Informationen werden in einem Datenschutz Bewusstsein behandelt. Wenn der Benutzer nicht zugestimmt hat, wird nichts verfügbar gemacht.
- Ermittlung – bietet Benutzern die Möglichkeit, Ihre Freunde zu ermitteln, die dieselbe Anwendung verwenden.


Als nächstes sehen wir uns diese Themen ausführlich an.

#### <a name="identity"></a>Identität

Wie bereits erwähnt, bietet cloudkit eine Möglichkeit für die Anwendung, einen bestimmten Benutzer eindeutig zu identifizieren:

 [![](intro-to-cloudkit-images/image40.png "Eindeutige Identifizierung eines bestimmten Benutzers")](intro-to-cloudkit-images/image40.png#lightbox)

Es gibt eine Client Anwendung, die auf den Geräten eines Benutzers ausgeführt wird, und alle speziellen privaten Benutzer Datenbanken innerhalb des cloudkit-Containers. Die Client Anwendung wird mit einem dieser spezifischen Benutzer verknüpft. Dies basiert auf dem Benutzer, der lokal auf dem Gerät bei icloud angemeldet ist.

Da dies von icloud stammt, gibt es einen umfangreichen Sicherungs Speicher für Benutzerinformationen. Und da icloud tatsächlich den Container gehostet, kann er Benutzer korrelieren. In der obigen Grafik ist der Benutzer, dessen icloud `user@icloud.com` -Konto mit dem aktuellen Client verknüpft ist.

Auf Containerbasis wird eine eindeutige, zufällig generierte Benutzer-ID erstellt und dem icloud-Konto des Benutzers (e-Mail-Adresse) zugeordnet. Diese Benutzer-ID wird an die Anwendung zurückgegeben und kann in beliebiger Weise verwendet werden, die der Entwickler sieht.

> [!NOTE]
> Verschiedene Anwendungen, die auf demselben Gerät für denselben icloud-Benutzer ausgeführt werden, haben unterschiedliche Benutzer-IDs, da Sie mit verschiedenen cloudkit-Containern verbunden sind.

Mit dem folgenden Code wird die cloudkit-Benutzer-ID für den derzeit angemeldeten icloud-Benutzer auf dem Gerät abgerufen:

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

Der obige Code fordert den cloudkit-Container auf, die ID des aktuell angemeldeten Benutzers anzugeben. Da diese Informationen vom icloud-Server stammen, ist der-Rückruf asynchron, und es ist eine Fehlerbehandlung erforderlich.

#### <a name="metadata"></a>Metadaten

Jeder Benutzer in cloudkit verfügt über bestimmte Metadaten, die diese beschreiben. Diese Metadaten werden als cloudkit-Datensatz dargestellt:

 [![](intro-to-cloudkit-images/image41.png "Jeder Benutzer in cloudkit weist bestimmte Metadaten auf, die diese beschreiben.")](intro-to-cloudkit-images/image41.png#lightbox)

Wenn Sie in der privaten Datenbank nach einem bestimmten Benutzer eines Containers suchen, gibt es einen Datensatz, der diesen Benutzer definiert. In der öffentlichen Datenbank gibt es viele Benutzerdaten Sätze, eine für jeden Benutzer des Containers. Eine dieser Daten weist eine Datensatz-ID auf, die der Datensatz-ID des aktuell angemeldeten Benutzers entspricht.

Benutzerdaten Sätze in der öffentlichen Datenbank sind weltweit lesbar. Sie werden größtenteils als normaler Datensatz behandelt und weisen den Typ auf `CKRecordTypeUserRecord`. Diese Datensätze werden vom System reserviert und sind nicht für Abfragen verfügbar.

Verwenden Sie den folgenden Code, um auf einen Benutzerdaten Satz zuzugreifen:

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

Im obigen Code wird die öffentliche Datenbank aufgefordert, den Benutzerdaten Satz für den Benutzer zurückzugeben, von dem aus auf Sie zugegriffen wurde. Da diese Informationen vom icloud-Server stammen, ist der-Rückruf asynchron, und es ist eine Fehlerbehandlung erforderlich.

#### <a name="privacy"></a>Datenschutz

Cloudkit ist standardmäßig so konzipiert, dass der Datenschutz des aktuell angemeldeten Benutzers geschützt wird. Standardmäßig sind keine persönlich identifizierbaren Informationen über den Benutzer verfügbar. Es gibt Fälle, in denen die Anwendung eingeschränkte Informationen über den Benutzer benötigt.

In diesen Fällen kann die Anwendung anfordern, dass der Benutzer diese Informationen offen legt. Dem Benutzer wird ein Dialogfeld angezeigt, in dem Sie aufgefordert werden, seine Kontoinformationen verfügbar zu machen.

#### <a name="discovery"></a>Suche

Angenommen, der Benutzer hat sich angemeldet, um der Anwendung den eingeschränkten Zugriff auf die Benutzerkontoinformationen zu gestatten, Sie kann für andere Benutzer der Anwendung erkennbar sein:

 [![](intro-to-cloudkit-images/image42.png "Ein Benutzer kann anderen Benutzern der Anwendung auffallen.")](intro-to-cloudkit-images/image42.png#lightbox)

Die Client Anwendung kommuniziert mit einem Container, und der Container kommuniziert mit icloud, um auf Benutzerinformationen zuzugreifen. Der Benutzer kann eine e-Mail-Adresse angeben, und die Ermittlung kann verwendet werden, um Informationen über den Benutzer zu erhalten. Optional kann die Benutzer-ID auch verwendet werden, um Informationen über den Benutzer zu ermitteln.

Cloudkit bietet auch eine Möglichkeit, Informationen über alle Benutzer zu ermitteln, die als Freunde des aktuell angemeldeten icloud-Benutzers durch Abfragen des gesamten Adressbuchs auftreten können. Vom cloudkit-Prozess wird das Kontaktbuch des Benutzers abgerufen, und die e-Mail-Adressen werden verwendet, um festzustellen, ob andere Benutzer der Anwendung gefunden werden, die mit diesen Adressen zu vergleichen sind.

Dadurch kann die Anwendung das Kontaktbuch des Benutzers nutzen, ohne Zugriff darauf zu gewähren oder den Benutzer aufzufordern, den Zugriff auf die Kontakte zu genehmigen. Die Kontaktinformationen, die der Anwendung zur Verfügung gestellt werden, sind zu keinem Zeitpunkt verfügbar, nur der cloudkit-Prozess hat Zugriff.

Es gibt drei verschiedene Arten von Eingaben, die für die Benutzer Ermittlung verfügbar sind:

- **Benutzerdaten Satz-ID** – die Ermittlung kann anhand der Benutzer-ID des derzeit angemeldeten cloudkit-Benutzers durchgeführt werden.
- **E-Mail-Adresse des Benutzers** – der Benutzer kann eine e-Mail-Adresse angeben, die für die Ermittlung verwendet werden kann.
- **Kontaktbuch** – das Adressbuch des Benutzers kann verwendet werden, um Benutzer der Anwendung zu ermitteln, die dieselbe e-Mail-Adresse wie in Ihren Kontakten angegeben haben.


Von der Benutzer Ermittlung werden die folgenden Informationen zurückgegeben:

- **Benutzerdaten Satz-ID** : die eindeutige ID eines Benutzers in der öffentlichen Datenbank.
- **Vor-und Nachname** : wird in der öffentlichen Datenbank gespeichert.


Diese Informationen werden nur für Benutzer zurückgegeben, die die Ermittlung ausgewählt haben.

Mit dem folgenden Code werden Informationen zu dem Benutzer ermittelt, der derzeit bei icloud auf dem Gerät angemeldet ist:

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

Verwenden Sie den folgenden Code, um alle Benutzer im Kontaktbuch abzufragen:

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

In diesem Abschnitt haben wir die vier Hauptbereiche des Zugriffs auf das Konto eines Benutzers behandelt, das cloudkit für eine Anwendung bereitstellen kann. Das Ermitteln der Identität und Metadaten des Benutzers zu den Datenschutzrichtlinien, die in cloudkit integriert sind, und schließlich die Möglichkeit, andere Benutzer der Anwendung zu ermitteln.

## <a name="the-development-and-production-environments"></a>Die Entwicklungs-und Produktionsumgebungen

Cloudkit bietet separate Entwicklungs-und Produktionsumgebungen für die Daten Satz Typen und Daten einer Anwendung. Die Entwicklungsumgebung ist eine flexiblere Umgebung, die nur Mitgliedern eines Entwicklungsteams zur Verfügung steht. Wenn eine Anwendung ein neues Feld zu einem Datensatz hinzufügt und diesen Datensatz in der Entwicklungsumgebung speichert, aktualisiert der Server die Schema Informationen automatisch.

Der Entwickler kann diese Funktion verwenden, um während der Entwicklung Änderungen an einem Schema vorzunehmen, was Zeit spart. Ein Nachteil besteht darin, dass der Datentyp, der diesem Feld zugeordnet ist, nicht Programm gesteuert geändert werden kann, nachdem ein Feld zu einem Datensatz hinzugefügt wurde. Um den Typ eines Felds zu ändern, muss der Entwickler das Feld im [cloudkit-Dashboard](https://icloud.developer.apple.com/dashboard/) löschen und es erneut mit dem neuen Typ hinzufügen.

Vor dem Bereitstellen der Anwendung kann der Entwickler das Schema und die Daten mithilfe des **cloudkit-Dashboards**zur Produktionsumgebung migrieren. Bei der Ausführung für die Produktionsumgebung verhindert der Server, dass eine Anwendung das Schema Programm gesteuert ändert. Der Entwickler kann weiterhin Änderungen mit dem **cloudkit-Dashboard** vornehmen, aber versuche, einem Datensatz in der Produktionsumgebung Felder hinzuzufügen, führen zu Fehlern.

> [!NOTE]
> Der IOS-Simulator funktioniert nur mit der **Entwicklungsumgebung**. Wenn der Entwickler bereit ist, eine Anwendung in einer **Produktionsumgebung**zu testen, ist ein physisches IOS-Gerät erforderlich.


## <a name="shipping-a-cloudkit-enabled-app"></a>Versenden einer cloudkit-fähigen App

Vor dem Versand einer Anwendung, die cloudkit verwendet, muss Sie für die **cloudkit-Produktionsumgebung** konfiguriert werden, oder die Anwendung wird von Apple abgelehnt.

Führen Sie folgende Schritte aus:

1. Kompilieren Sie die Anwendung für das IOS- **Freigabe** > **Gerät**in Visual Studio für MA:

    [![](intro-to-cloudkit-images/shipping01.png "Kompilieren der Anwendung für die Veröffentlichung")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. Wählen Sie im Menü **Erstellen** die Option **Archive**:

    [![](intro-to-cloudkit-images/shipping02.png "Archiv auswählen")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. Das **Archiv** wird erstellt und in Visual Studio für Mac angezeigt:

    [![](intro-to-cloudkit-images/shipping03.png "Das Archiv wird erstellt und angezeigt.")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Starten Sie **Xcode**.
5. Wählen Sie im Menü **Fenster** die Option **Planer**aus:

    [![](intro-to-cloudkit-images/shipping04.png "Planer auswählen")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Wählen Sie das Archiv der Anwendung aus, und klicken Sie auf die Schaltfläche **exportieren...** :

    [![](intro-to-cloudkit-images/shipping05.png "Das Archiv der Anwendung")](intro-to-cloudkit-images/shipping05.png#lightbox)

7. Wählen Sie eine Export Methode aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![](intro-to-cloudkit-images/shipping06.png "Methode für den Export auswählen")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Wählen Sie in der Dropdown Liste das **Entwicklungs Team** aus, und klicken Sie auf die Schaltfläche **auswählen** :

    [![](intro-to-cloudkit-images/shipping07.png "Wählen Sie das Entwicklungs Team aus der Dropdown Liste aus.")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Wählen Sie in der Dropdown Liste **Production** aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![](intro-to-cloudkit-images/shipping08.png "Produktion in der Dropdown Liste auswählen")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. Prüfen Sie die Einstellung, und klicken Sie auf die Schaltfläche **exportieren** :

    [![](intro-to-cloudkit-images/shipping09.png "Überprüfen der Einstellung")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Wählen Sie einen Speicherort aus, um `.ipa` die resultierende Anwendungsdatei zu generieren.

Der Prozess ähnelt dem direkten Senden der Anwendung an iTunes Connect. Klicken Sie einfach auf die Schaltfläche " **Submit...** " anstatt auf "Exportieren"... Nachdem Sie im Planer-Fenster ein Archiv ausgewählt haben.

## <a name="when-to-use-cloudkit"></a>Verwendungszwecke von cloudkit

Wie wir in diesem Artikel gesehen haben, bietet cloudkit eine einfache Möglichkeit für eine Anwendung zum Speichern und Abrufen von Informationen von den icloud-Servern. Das heißt, dass cloudkit keines der vorhandenen Tools oder Frameworks veraltet oder als veraltet markiert.

### <a name="use-cases"></a>Anwendungsfälle

Die folgenden Anwendungsfälle helfen dem Entwickler bei der Entscheidung, wann ein bestimmtes icloud-Framework oder eine bestimmte Technologie verwendet werden sollte:

- **icloud Key-Value Store** – führt eine asynchrone Datenmenge auf dem neuesten Stand und eignet sich hervorragend für die Arbeit mit Anwendungseinstellungen. Allerdings ist es für eine sehr kleine Menge an Informationen eingeschränkt.
- **icloud Drive** – basiert auf den vorhandenen icloud Documents-APIs und bietet eine einfache API, um unstrukturierte Daten aus dem Dateisystem zu synchronisieren. Es bietet einen vollständigen Offline Cache auf Mac OS X und eignet sich hervorragend für Dokument zentrierte Anwendungen.
- **icloud Core Data** – ermöglicht das Replizieren von Daten zwischen allen Geräten des Benutzers. Bei den Daten handelt es sich um Einzelbenutzer und eignet sich hervorragend, um private, strukturierte Daten synchron zu halten.
- **Cloudkit** – stellt öffentliche Daten sowohl strukturiert als auch Massen bereit und ist in der Lage, sowohl großes Dataset als auch große unstrukturierte Dateien zu verarbeiten. Die ist an das icloud-Konto des Benutzers gebunden und ermöglicht eine Client gesteuerte Datenübertragung.


Wenn Sie diese Anwendungsfälle beachten, sollte der Entwickler die richtige icloud-Technologie auswählen, um die aktuelle erforderliche Anwendungs Funktionalität bereitzustellen und eine gute Skalierbarkeit für zukünftiges Wachstum bereitzustellen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine kurze Einführung in die cloudkit-API behandelt. Es wurde gezeigt, wie Sie eine xamarin IOS-Anwendung für die Verwendung von cloudkit bereitstellen und konfigurieren. Es wurden die Features der cloudkit-praktische API behandelt. Es wurde gezeigt, wie eine cloudkit-fähige Anwendung für die Skalierbarkeit mithilfe von Abfragen und Abonnements entworfen wird. Und schließlich wurden die Benutzerkontoinformationen angezeigt, die von cloudkit für eine Anwendung verfügbar gemacht werden.

## <a name="related-links"></a>Verwandte Links

- [Cloudkitatlas (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-cloudkitatlas)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Erstellen eines Bereitstellungs Profils](~/ios/get-started/installation/device-provisioning/index.md)
