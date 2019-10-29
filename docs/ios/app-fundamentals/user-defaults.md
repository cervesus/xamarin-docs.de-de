---
title: Arbeiten mit Benutzer Standardwerten in xamarin. IOS
description: In diesem Artikel wird die Verwendung von NSUserDefaults zum Speichern von Standardeinstellungen in einer xamarin IOS-APP oder-Erweiterung behandelt. Er beschreibt NSUserDefaults auf hoher Ebene und erläutert, wie Werte gelesen und geschrieben werden.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 4706b483f8f3a104f54ea2bf451cb4e2fb209486
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009080"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Arbeiten mit Benutzer Standardwerten in xamarin. IOS

_In diesem Artikel wird die Verwendung von nsuserdefault zum Speichern von Standardeinstellungen in einer xamarin. IOS-APP oder-Erweiterung behandelt._

Die `NSUserDefaults`-Klasse bietet IOS-apps und-Erweiterungen die Möglichkeit, Programm gesteuert mit dem systemweiten Standardsystem zu interagieren. Mithilfe des Standardsystems kann der Benutzer das Verhalten oder das Formatieren einer APP so konfigurieren, dass die Einstellungen (basierend auf dem Entwurf der APP) erfüllt werden. Beispielsweise zur Darstellung von Daten in Metriken im Vergleich zu kaiserlichen Messungen oder zum Auswählen eines bestimmten UI-Designs.

Bei der Verwendung mit App-Gruppen bietet `NSUserDefaults` auch eine Möglichkeit, zwischen Apps (oder Erweiterungen) innerhalb einer bestimmten Gruppe zu kommunizieren.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>Informationen zu Benutzer Standards

Wie bereits erwähnt, können Benutzer Standardwerte (`NSUserDefaults`) einer APP (oder Erweiterung) hinzugefügt und zum Bereitstellen konfigurierbarer Optionen verwendet werden, die der Endbenutzer ändern kann, um das Aussehen oder den Betrieb der App zur Laufzeit anzupassen.

Wenn Ihre APP zum ersten Mal ausgeführt wird, liest `NSUserDefaults` die Schlüssel und Werte aus der Datenbank mit den Benutzer Standardwerten der APP und speichert Sie im Arbeitsspeicher zwischen, um zu vermeiden, dass die Datenbank bei jedem erforderlichen Wert geöffnet und gelesen wird. 

> [!IMPORTANT]
> Apple empfiehlt nicht mehr, dass der Entwickler die `Synchronize`-Methode aufruft, um den in-Memory-Cache direkt mit der Datenbank zu synchronisieren. Stattdessen wird es automatisch in regelmäßigen Abständen aufgerufen, um den in-Memory-Cache mit der Standarddatenbank eines Benutzers synchron zu halten.

Die `NSUserDefaults`-Klasse enthält verschiedene Hilfsmethoden zum Lesen und Schreiben von Einstellungs Werten für allgemeine Datentypen wie z. b. String, Integer, float, booleschen und URLs. Andere Datentypen können mit `NSData`archiviert und dann aus der Datenbank für Benutzer Standardwerte gelesen oder geschrieben werden. Weitere Informationen finden Sie im [Programmier Handbuch für Einstellungen und Einstellungen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)von Apple.

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Zugreifen auf die freigegebene NSUserDefaults-Instanz 

Die Standard Instanz für freigegebene Benutzer ermöglicht den Zugriff auf die Benutzer Standardwerte für den aktuellen Benutzer des Geräts. Wenn das freigegebene Default-Objekt nicht vorhanden ist, wird es beim ersten Zugriff erstellt und mit den folgenden Informationen initialisiert:

- Eine `NSArgumentDomain`, die aus den aus der aktuellen App analysierten Standardwerten besteht.
- Die Bundle-Bezeichnerdomäne der app.
- Eine `NSGlobalDomain`, die aus den von allen apps gemeinsam genutzten Standardwerten besteht.
- Eine separate Domäne für jede bevorzugte Sprache des Benutzers.
- Eine `NSRegistrationDomain` mit einer Reihe von temporären Standardwerten, die von der APP geändert werden können, um sicherzustellen, dass die Suche immer erfolgreich ist.

Verwenden Sie den folgenden Code, um auf die Standard Instanz der freigegebenen Benutzer zu zugreifen:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Zugreifen auf eine APP-Gruppe NSUserDefaults-Instanz

Wie oben bereits erwähnt, können `NSUserDefaults` mithilfe von App-Gruppen für die Kommunikation zwischen Apps (oder Erweiterungen) innerhalb einer bestimmten Gruppe verwendet werden. Zunächst müssen Sie sicherstellen, dass die APP-Gruppe und die erforderlichen App-IDs im Abschnitt **Zertifikate, Bezeichner & profile** im [IOS dev Center](https://developer.apple.com/devcenter/ios/) ordnungsgemäß konfiguriert wurden und in der Entwicklungsumgebung installiert wurden.

Als nächstes müssen Ihre APP-und/oder Erweiterungsprojekte eine gültige APP-ID aufweisen, die oben erstellt wurde, und die `Entitlements.plist` Datei muss in das App-Bündel eingeschlossen werden, wobei die APP-Gruppen aktiviert und angegeben werden.

Da all diese Einstellungen vorhanden sind, können Sie mit dem folgenden Code auf die Benutzer Standardwerte der freigegebenen App-Gruppe zugreifen:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Dabei ist `group.com.xamarin.todaysharing` die APP-Gruppe, die in **Zertifikaten erstellt wurde, Bezeichner & profile** , auf die Sie zugreifen möchten. Weitere Informationen finden Sie in der Dokumentation zu [App-Gruppenfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Lesen von Standardwerten

Nachdem Sie auf die gewünschte Standarddatenbank zugegriffen haben, können Sie Werte aus den Standardeinstellungen mithilfe von Schlüssel-Wert-Paaren und verschiedenen Hilfsmethoden lesen, die auf der Art der gelesenen Daten basieren:

- `ArrayForKey`: gibt ein Array von `NSObjects` für den angegebenen Schlüsselwert zurück.
- `BoolForKey`: gibt einen booleschen Wert für den angegebenen Schlüssel zurück.
- `DataForKey`: gibt ein `NSData` Objekt für den angegebenen Schlüssel zurück.
- `DictionaryForKey`: gibt eine `NSDictionary` für den angegebenen Schlüssel zurück.
- `DoubleForKey`: gibt einen Double-Wert für den angegebenen Schlüssel zurück.
- `FloatForKey`: gibt einen float-Wert für den angegebenen Schlüssel zurück.
- `IntForKey`: gibt einen ganzzahligen Wert für den angegebenen Schlüssel zurück.
- `StringArrayForKey`: gibt ein Array von `String` Objekten aus dem angegebenen Schlüsselwert zurück.
- `StringForKey`: gibt einen Zeichen folgen Wert für den angegebenen Schlüssel zurück.
- `URLForKey`: gibt einen `NSUrl` Wert für den angegebenen Schlüssel zurück.

Der folgende Code liest z. b. einen booleschen Wert aus den Benutzer Standardwerten:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Schreiben von Standardwerten

Wie oben beschrieben, können Sie nach dem Zugriff auf die gewünschte Standarddatenbank des Benutzers Werte in die Standardwerte schreiben, indem Sie Schlüssel-Wert-Paare und verschiedene Hilfsmethoden basierend auf dem Typ der geschriebenen Daten verwenden:

- `SetBool`: schreibt den angegebenen booleschen Wert in den angegebenen Schlüssel.
- `SetDouble`: schreibt den angegebenen Double-Wert in den angegebenen Schlüssel.
- `SetFloat`: schreibt den angegebenen float-Wert in den angegebenen Schlüssel.
- `SetString`: schreibt den angegebenen Zeichen folgen Wert in den angegebenen Schlüssel.
- `SetURL`: schreibt den angegebenen URL-Wert (`NSUrl`) in den angegebenen Schlüssel.

Der folgende Code schreibt z. b. einen booleschen Wert in die Benutzer Standardwerte:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> Wenn Ihre APP zum ersten Mal ausgeführt wird, liest `NSUserDefaults` die Schlüssel und Werte aus der Datenbank mit den Benutzer Standardwerten der APP und speichert Sie im Arbeitsspeicher zwischen, um zu vermeiden, dass die Datenbank bei jedem erforderlichen Wert geöffnet und gelesen wird.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die `NSUserDefaults`-Klasse behandelt und erläutert, wie Sie verwendet werden kann, um eine Reihe von Optionen bereitzustellen, die der Endbenutzer zum Konfigurieren ihrer xamarin. IOS-App verwenden kann. Darüber hinaus wird die Verwendung von App-Gruppen für die Kommunikation zwischen einer Erweiterung und der übergeordneten APP oder zwischen apps in einer Gruppe abgedeckt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Programmier Handbuch zu Einstellungen und Einstellungen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
