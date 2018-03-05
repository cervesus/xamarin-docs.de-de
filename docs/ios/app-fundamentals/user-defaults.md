---
title: "Arbeiten mit Standardeinstellungen für Benutzer"
description: Dieser Artikel behandelt die Arbeit mit NSUserDefault Standardeinstellungen in einer Xamarin iOS-App oder eine Erweiterung zu speichern.
ms.topic: article
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: c4b2a103821bb18da4878cd37335faa899e910be
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-user-defaults"></a>Arbeiten mit Standardeinstellungen für Benutzer

_Dieser Artikel behandelt die Arbeit mit NSUserDefault Standardeinstellungen in einer Xamarin iOS-App oder eine Erweiterung zu speichern._


Die `NSUserDefaults` -Klasse bietet eine Möglichkeit für iOS-Apps und Erweiterungen für die programmgesteuerte Interaktion mit dem eine systemweite Standardeinstellung System. Verwenden Sie das System standardmäßig, kann der Benutzer einer Verhalten oder die app formatieren, um ihre Voreinstellungen (basierend auf den Entwurf der app) erfüllen konfigurieren. Geben Sie beispielsweise Folgendes ein, um das Darstellen von Daten in Vs Metrik das englische System oder Auswählen eines bestimmten UI-Designs.

Bei der App-Gruppen verwendet `NSUserDefaults` bietet auch eine Möglichkeit für die Kommunikation zwischen apps (oder Extensions) in einer bestimmten Gruppe.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>Informationen zu den Standardeinstellungen für Benutzer

Wie oben, Standardeinstellungen für Benutzer angegeben (`NSUserDefaults`) zu einer App (oder Erweiterung) hinzugefügt werden können und die konfigurierbare Optionen bereitgestellt, die der Benutzer ändern können, um die Darstellung oder der Vorgang der app zur Laufzeit anzupassen.

Wenn die app zuerst ausgeführt wird, `NSUserDefaults` liest die Schlüssel und Werte aus der app-Benutzerdatenbank wird standardmäßig und speichert sie in den Arbeitsspeicher zu vermeiden, öffnen und lesen die Datenbank jedes Mal ein Wert erforderlich ist. 

> [!IMPORTANT]
> **Hinweis**: Apple wird nicht mehr empfohlen, dass der Entwickler Aufrufen der `Synchronize` Methode, um in-Memory-Caches mit der Datenbank direkt zu synchronisieren. Stattdessen wird es automatisch in regelmäßigen Abständen zu in-Memory-Caches mit der Datenbank eines Benutzers Standardwerte synchronisieren aufgerufen werden.

Die `NSUserDefaults` Klasse enthält mehrere Hilfsmethoden zum Lesen und Schreiben von Einstellungswerte für häufig verwendete Datentypen wie z. B.: Zeichenfolge, ganze Zahl, "float", Boolean und URLs. Andere Typen von Daten können archiviert werden, mithilfe von `NSData`, die dann gelesen oder geschrieben werden, mit der Standard-Benutzerdatenbank. Weitere Informationen finden Sie in der Apple- [Präferenzen und Einstellungen Programmierhandbuch](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Den Zugriff auf den freigegebenen NSUserDefaults-Instanz 

Die freigegebene standardmäßig Benutzerinstanz bietet Zugriff auf die Standardeinstellungen für Benutzer, für den aktuellen Benutzer des Geräts. Wenn das Objekt freigegeben wird standardmäßig nicht vorhanden ist, wird es erstmalig erstellt, darauf zugegriffen wird, und initialisiert mit den folgenden Informationen:

- Ein `NSArgumentDomain` besteht die Standardwerte aus der aktuellen app analysiert.
- Die app-Paket-ID-Domäne.
- Ein `NSGlobalDomain` besteht die Standardwerte, die von allen apps gemeinsam verwendet werden.
- Einer separaten Domäne für jede bevorzugte Sprache des Benutzers.
- Ein `NSRegistationDomain` mit einem Satz von temporären Standardwerte, die von der app, um sicherzustellen, dass Suchvorgänge sind immer erfolgreich geändert werden kann.

Um die freigegebene standardmäßig Benutzerinstanz zuzugreifen, verwenden Sie den folgenden Code ein:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Zugreifen auf App NSUserDefaults Gruppeninstanz

Wie oben angegeben, App-Gruppen mit `NSUserDefaults` für die Kommunikation zwischen Apps (oder Extensions) in einer bestimmten Gruppe verwendet werden können. Sie müssen zunächst sicherstellen, dass die App-Gruppe und die erforderliche App-IDs in ordnungsgemäß konfiguriert wurde die **Zertifikate "," Bezeichner "und" Profile** Abschnitt [iOS Dev Center](https://developer.apple.com/devcenter/ios/) und installiert wurde auf in der Entwicklungsumgebung.

Als Nächstes Ihre App und/oder Erweiterung Projekte müssen einen gültigen App-IDs, die weiter oben erstellt werden, die die `Entitlements.plist` Datei hat die App-Gruppen aktiviert und angegeben und, dass es beim Abrufen des in der App-Bündel enthalten.

Mit dieser alle vorhanden können die Standardeinstellungen für den Benutzer der freigegebenen App-Gruppe mit dem folgenden Code zugegriffen werden:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Wobei `group.com.xamarin.todaysharing` wird die App-Gruppe erstellt **Zertifikate "," Bezeichner "und" Profile** , die Sie zugreifen möchten. Weitere Informationen finden Sie unter der [App Gruppenverwaltungsfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Lesen Sie die Standardwerte

Nachdem Sie die gewünschten Standard Benutzerdatenbank zugegriffen haben, können Sie die Werte von den Standardwerten mit Schlüssel/Wert-Paare und mehrere Hilfsmethoden, die basierend auf den Typ des zu lesenden Daten lesen:

- `ArrayForKey` -Gibt ein Array von `NSObjects` für den angegebenen Schlüsselwert.
- `BoolForKey` -Gibt einen booleschen Wert für den angegebenen Schlüssel zurück.
- `DataForKey` -Gibt ein `NSData` Objekt für den angegebenen Schlüssel.
- `DictionaryForKey` -Gibt ein `NSDictionary` für den angegebenen Schlüssel.
- `DoubleForKey` -Gibt einen double-Wert für den angegebenen Schlüssel zurück.
- `FloatForKey` -Gibt einen Float-Wert für den angegebenen Schlüssel zurück.
- `IntForKey` -Gibt einen ganzzahligen Wert für den angegebenen Schlüssel zurück.
- `StringArrayForKey` -Gibt ein Array von `String` Objekte aus dem angegebenen Schlüssel-Wert.
- `StringForKey` -Gibt einen Zeichenfolgenwert für den angegebenen Schlüssel zurück.
- `URLForKey` -Gibt ein `NSUrl` Wert für den angegebenen Schlüssel.

Beispielsweise würde der folgende Code einen booleschen Wert aus der Standardeinstellungen für Benutzer lesen:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Schreiben von Standardwerten

Lesen Sie die oben genannten Werte, nachdem Sie die gewünschte Benutzer standardmäßig Datenbank zugegriffen haben können Sie Werte auf die Standardwerte, die mithilfe von Schlüssel/Wert-Paare und mehrere Hilfsmethoden, die basierend auf den Typ des zu schreibenden Daten schreiben:

- `SetBool` -Schreibt den angegebenen booleschen Wert auf den angegebenen Schlüssel.
- `SetDouble` -Schreibt die angegebenen double-Wert dem angegebenen Schlüssel.
- `SetFloat` -Schreibt den angegebenen Float-Wert auf den angegebenen Schlüssel.
- `SetString` -Schreibt die angegebene Zeichenfolge auf den angegebenen Schlüssel.
- `SetURL` -Schreibt die angegebene URL (`NSUrl`) Wert, der dem angegebenen Schlüssel.

Beispielsweise würde der folgende Code einen booleschen Wert auf die Standardeinstellungen für Benutzer schreiben:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> **Hinweis:** Wenn zuerst die App ausgeführt wird, `NSUserDefaults` liest die Schlüssel und Werte aus der app-Benutzerdatenbank wird standardmäßig und speichert sie in den Arbeitsspeicher zu vermeiden, öffnen und lesen die Datenbank jedes Mal ein Wert erforderlich ist.



<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt hat die `NSUserDefaults` Klasse und wie sie verwendet werden kann, um eine Reihe von Optionen bereit, mit denen die Endbenutzer können Ihre Xamarin.iOS App konfigurieren. Darüber hinaus behandelt sie mithilfe von App-Gruppen, um zwischen einer Erweiterung und ihre übergeordnete App oder zwischen apps in einer Gruppe zu kommunizieren.


## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Benutzereinstellungen und Vorlieben Programmierhandbuch](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
