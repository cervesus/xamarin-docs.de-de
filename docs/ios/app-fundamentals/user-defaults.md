---
title: Arbeiten mit Benutzer Standardwerten in xamarin. IOS
description: In diesem Artikel wird die Verwendung von NSUserDefaults zum Speichern von Standardeinstellungen in einer xamarin IOS-APP oder-Erweiterung behandelt. Er beschreibt NSUserDefaults auf hoher Ebene und erläutert, wie Werte gelesen und geschrieben werden.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: c65a06b8f2a04eda669b2d741135538fa8a583f2
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571726"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Arbeiten mit Benutzer Standardwerten in xamarin. IOS

_In diesem Artikel wird die Verwendung von nsuserdefault zum Speichern von Standardeinstellungen in einer xamarin. IOS-APP oder-Erweiterung behandelt._

`NSUserDefaults`Mit der-Klasse können IOS-apps und-Erweiterungen Programm gesteuert mit dem systemweiten Standardsystem interagieren. Mithilfe des Standardsystems kann der Benutzer das Verhalten oder das Formatieren einer APP so konfigurieren, dass die Einstellungen (basierend auf dem Entwurf der APP) erfüllt werden. Beispielsweise zur Darstellung von Daten in Metriken im Vergleich zu kaiserlichen Messungen oder zum Auswählen eines bestimmten UI-Designs.

Bei Verwendung mit App-Gruppen `NSUserDefaults` bietet auch eine Möglichkeit, zwischen Apps (oder Erweiterungen) innerhalb einer bestimmten Gruppe zu kommunizieren.

<a name="About-User-Defaults"></a>

## <a name="about-user-defaults"></a>Informationen zu Benutzer Standards

Wie bereits erwähnt, können Benutzer Standardwerte ( `NSUserDefaults` ) einer APP (oder Erweiterung) hinzugefügt und zum Bereitstellen konfigurierbarer Optionen verwendet werden, die der Endbenutzer ändern kann, um das Aussehen oder den Betrieb der App zur Laufzeit anzupassen.

Wenn Ihre APP zum ersten Mal ausgeführt wird, `NSUserDefaults` liest die Schlüssel und Werte aus der Datenbank mit den Benutzer Standardwerten der APP und speichert Sie im Arbeitsspeicher zwischen, um zu vermeiden, dass die Datenbank bei jedem erforderlichen Wert geöffnet und gelesen wird. 

> [!IMPORTANT]
> Apple empfiehlt nicht mehr, dass der Entwickler die- `Synchronize` Methode aufruft, um den in-Memory-Cache direkt mit der Datenbank zu synchronisieren. Stattdessen wird es automatisch in regelmäßigen Abständen aufgerufen, um den in-Memory-Cache mit der Standarddatenbank eines Benutzers synchron zu halten.

Die- `NSUserDefaults` Klasse enthält verschiedene Hilfsmethoden zum Lesen und Schreiben von Einstellungs Werten für allgemeine Datentypen wie z. b. String, Integer, float, booleschen und URLs. Andere Datentypen können mithilfe `NSData` von archiviert und dann in die Datenbank für Benutzer Standardwerte gelesen oder geschrieben werden. Weitere Informationen finden Sie im [Programmier Handbuch für Einstellungen und Einstellungen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)von Apple.

<a name="Accessing-the-Shared-NSUserDefaults-Instance"></a>

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Zugreifen auf die freigegebene NSUserDefaults-Instanz 

Die Standard Instanz für freigegebene Benutzer ermöglicht den Zugriff auf die Benutzer Standardwerte für den aktuellen Benutzer des Geräts. Wenn das freigegebene Default-Objekt nicht vorhanden ist, wird es beim ersten Zugriff erstellt und mit den folgenden Informationen initialisiert:

- Ein, `NSArgumentDomain` das aus den aus der aktuellen App analysierten Standardwerten besteht.
- Die Bundle-Bezeichnerdomäne der app.
- Eine, `NSGlobalDomain` die aus den von allen apps gemeinsam genutzten Standardwerten besteht.
- Eine separate Domäne für jede bevorzugte Sprache des Benutzers.
- Ein `NSRegistrationDomain` mit einer Reihe von temporären Standardwerten, die von der APP geändert werden können, um sicherzustellen, dass die Suche immer erfolgreich ist.

Verwenden Sie den folgenden Code, um auf die Standard Instanz der freigegebenen Benutzer zu zugreifen:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance"></a>

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Zugreifen auf eine APP-Gruppe NSUserDefaults-Instanz

Wie bereits erwähnt, kann mithilfe von App-Gruppen `NSUserDefaults` verwendet werden, um zwischen Apps (oder Erweiterungen) innerhalb einer bestimmten Gruppe zu kommunizieren. Zunächst müssen Sie sicherstellen, dass die APP-Gruppe und die erforderlichen App-IDs im Abschnitt **Zertifikate, Bezeichner & profile** im [IOS dev Center](https://developer.apple.com/devcenter/ios/) ordnungsgemäß konfiguriert wurden und in der Entwicklungsumgebung installiert wurden.

Als nächstes müssen Ihre APP-und/oder Erweiterungsprojekte eine gültige APP-ID aufweisen, die oben erstellt wurde, und die `Entitlements.plist` Datei muss in das App-Bündel eingeschlossen werden, wobei die APP-Gruppen aktiviert und angegeben werden.

Da all diese Einstellungen vorhanden sind, können Sie mit dem folgenden Code auf die Benutzer Standardwerte der freigegebenen App-Gruppe zugreifen:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

`group.com.xamarin.todaysharing`Dabei ist die APP-Gruppe, die in **Zertifikaten erstellt wurde, Bezeichner & profile** , auf die Sie zugreifen möchten. Weitere Informationen finden Sie in der Dokumentation zu [App-Gruppenfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

<a name="Reading-Default-Values"></a>

## <a name="reading-default-values"></a>Lesen von Standardwerten

Nachdem Sie auf die gewünschte Standarddatenbank zugegriffen haben, können Sie Werte aus den Standardeinstellungen mithilfe von Schlüssel-Wert-Paaren und verschiedenen Hilfsmethoden lesen, die auf der Art der gelesenen Daten basieren:

- `ArrayForKey`-Gibt ein Array von `NSObjects` für den angegebenen Schlüsselwert zurück.
- `BoolForKey`-Gibt einen booleschen Wert für den angegebenen Schlüssel zurück.
- `DataForKey`-Gibt ein- `NSData` Objekt für den angegebenen Schlüssel zurück.
- `DictionaryForKey`-Gibt einen `NSDictionary` für den angegebenen Schlüssel zurück.
- `DoubleForKey`-Gibt einen Double-Wert für den angegebenen Schlüssel zurück.
- `FloatForKey`-Gibt einen float-Wert für den angegebenen Schlüssel zurück.
- `IntForKey`-Gibt einen ganzzahligen Wert für den angegebenen Schlüssel zurück.
- `StringArrayForKey`-Gibt ein Array von- `String` Objekten aus dem angegebenen Schlüsselwert zurück.
- `StringForKey`-Gibt einen Zeichen folgen Wert für den angegebenen Schlüssel zurück.
- `URLForKey`-Gibt einen `NSUrl` Wert für den angegebenen Schlüssel zurück.

Der folgende Code liest z. b. einen booleschen Wert aus den Benutzer Standardwerten:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values"></a>

## <a name="writing-default-values"></a>Schreiben von Standardwerten

Wie oben beschrieben, können Sie nach dem Zugriff auf die gewünschte Standarddatenbank des Benutzers Werte in die Standardwerte schreiben, indem Sie Schlüssel-Wert-Paare und verschiedene Hilfsmethoden basierend auf dem Typ der geschriebenen Daten verwenden:

- `SetBool`-Schreibt den angegebenen booleschen Wert in den angegebenen Schlüssel.
- `SetDouble`: Schreibt den angegebenen Double-Wert in den angegebenen Schlüssel.
- `SetFloat`: Schreibt den angegebenen float-Wert in den angegebenen Schlüssel.
- `SetString`-Schreibt den angegebenen Zeichen folgen Wert in den angegebenen Schlüssel.
- `SetURL`-Schreibt den angegebenen URL ( `NSUrl` )-Wert in den angegebenen Schlüssel.

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
> Wenn Ihre APP zum ersten Mal ausgeführt wird, `NSUserDefaults` liest die Schlüssel und Werte aus der Datenbank mit den Benutzer Standardwerten der APP und speichert Sie im Arbeitsspeicher zwischen, um zu vermeiden, dass die Datenbank bei jedem erforderlichen Wert geöffnet und gelesen wird.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die `NSUserDefaults` -Klasse behandelt und erläutert, wie Sie verwendet werden kann, um eine Reihe von Optionen bereitzustellen, mit denen der Endbenutzer ihre xamarin. IOS-App konfigurieren kann. Darüber hinaus wird die Verwendung von App-Gruppen für die Kommunikation zwischen einer Erweiterung und der übergeordneten APP oder zwischen apps in einer Gruppe abgedeckt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Programmier Handbuch zu Einstellungen und Einstellungen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
