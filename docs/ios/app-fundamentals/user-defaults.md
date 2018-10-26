---
title: Arbeiten mit Standardeinstellungen für Benutzer in Xamarin.iOS
description: Dieser Artikel behandelt die Arbeit mit NSUserDefaults zum Speichern von Einstellungen der Standardrichtlinie in einer Xamarin iOS-app oder die Erweiterung. Es wird erläutert, wie das Lesen und Schreiben von Werten haben und NSUserDefaults auf hoher Ebene beschrieben wurde.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: 688db534d6c99a8fadb7535f0532f9c1e9564707
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120891"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Arbeiten mit Standardeinstellungen für Benutzer in Xamarin.iOS

_Dieser Artikel behandelt die Arbeit mit NSUserDefault Standardeinstellungen in einer Xamarin.iOS-App oder die Erweiterung zu speichern._


Die `NSUserDefaults` -Klasse bietet eine Möglichkeit für iOS-Apps und -Erweiterungen für die programmgesteuerte Interaktion mit dem standardmäßig eine systemweite-System. Verwenden Sie das System standardmäßig, kann der Benutzer der app-Verhalten oder die Formatierung, um ihre Einstellungen (basierend auf den Entwurf der app) zu erfüllen konfigurieren. Geben Sie beispielsweise Folgendes ein, um Daten im Metrik-Vs-Imperial Messungen darstellen oder von bestimmten UI Design auswählen.

Bei Verwendung mit App-Gruppen, `NSUserDefaults` bietet auch eine Möglichkeit für die Kommunikation zwischen apps (oder Erweiterungen) in einer bestimmten Gruppe.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>Informationen zu den Standardeinstellungen des Standardbenutzers

Wie oben, Standardeinstellungen für Benutzer (`NSUserDefaults`) kann eine App (oder die Erweiterung) hinzugefügt und verwendet, um konfigurierbare Optionen bereitzustellen, die der Endbenutzer zum Anpassen des Aussehens oder die Ausführung der app zur Laufzeit ändern können.

Wenn Ihre app zuerst ausgeführt wird, `NSUserDefaults` die Schlüssel und Werte aus der app Benutzer standardmäßig Datenbank liest und speichert sie in den Speicher, um zu vermeiden, öffnen und lesen die Datenbank jedes Mal ein Wert erforderlich ist. 

> [!IMPORTANT]
> Apple empfiehlt, die nicht mehr den Developer-Aufruf die `Synchronize` Methode, um in-Memory-Cache mit der Datenbank direkt zu synchronisieren. Stattdessen wird diese automatisch in regelmäßigen Abständen zu in-Memory-Cache mit der Datenbank eines Benutzers standardmäßig synchron aufgerufen werden.

Die `NSUserDefaults` -Klasse enthält verschiedene praktische Methoden zum Lesen und Schreiben Einstellungswerte für häufig verwendete Datentypen wie z. B.: Zeichenfolge "," ganze Zahl "," Float "," Boolean "und"-URLs. Andere Arten von Daten können archiviert werden, mithilfe von `NSData`, und Lesen aus oder in der Benutzerdatenbank standardmäßig geschrieben. Weitere Informationen finden Sie unter Apple [Vorlieben und Einstellungen Programming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Zugreifen auf die freigegebene NSUserDefaults-Instanz 

Die freigegebene standardmäßig Benutzerinstanz bietet Zugriff auf die Standardeinstellungen für den Benutzer, für den aktuellen Benutzer des Geräts. Wenn das Objekt freigegeben wird standardmäßig nicht vorhanden ist, wird es beim ersten Zugriff und initialisiert mit den folgenden Informationen erstellt:

- Ein `NSArgumentDomain` bestehend aus die Standardwerte aus der aktuellen app analysiert.
- Die app Bündel-ID-Domäne.
- Ein `NSGlobalDomain` bestehend aus die Standardwerte, die von allen apps gemeinsam verwendet werden.
- Eine separate Domäne für jede bevorzugte Sprache des Benutzers.
- Ein `NSRegistrationDomain` mit einem Satz von temporären Standardwerte, die von der app, um sicherzustellen, dass Suchvorgänge werden immer erfolgreich geändert werden kann.

Um die freigegebenen Benutzer-Standard-Instanz zuzugreifen, verwenden Sie den folgenden Code:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Zugreifen auf eine Instanz der App-Gruppe NSUserDefaults

Wie oben erwähnt, mithilfe von App-Gruppen, `NSUserDefaults` für die Kommunikation zwischen Apps (oder Erweiterungen) in einer bestimmten Gruppe verwendet werden können. Zunächst müssen Sie sicherstellen, dass die App-Gruppe und die erforderlichen App-IDs in ordnungsgemäß konfiguriert wurden die **Zertifikate, Bezeichner & Profile** Abschnitt [iOS Developer Center](https://developer.apple.com/devcenter/ios/) und installiert wurden in der Entwicklungsumgebung.

Als Nächstes Ihre App oder Erweiterungsbezogenen Projekte müssen jeweils eine der oben erstellten gültiger App-IDs und die `Entitlements.plist` Datei muss in das App-Bundle mit den App-Gruppen aktiviert und angegeben einbezogen werden.

Mit dieser All vorhanden kann die freigegebene App Gruppe Standardeinstellungen für Benutzer zugegriffen werden mithilfe des folgenden Codes:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Wo `group.com.xamarin.todaysharing` wird in die App-Gruppe erstellt **Zertifikate, Bezeichner & Profile** , die Sie zugreifen möchten. Weitere Informationen finden Sie unter den [App-Gruppen-Funktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Lesen Sie die Standardwerte

Nachdem Sie die gewünschte Standard-Datenbank für den Benutzer zugegriffen haben, können Sie die Werte von den Standardwerten, die mithilfe von Schlüssel/Wert-Paare und verschiedene praktische Methoden, die basierend auf den Typ des zu lesenden Daten lesen:

- `ArrayForKey` -Gibt ein Array von `NSObjects` für den angegebenen Schlüssel-Wert.
- `BoolForKey` -Gibt einen booleschen Wert für den angegebenen Schlüssel.
- `DataForKey` -Gibt ein `NSData` Objekt für den angegebenen Schlüssel.
- `DictionaryForKey` -Gibt ein `NSDictionary` für den angegebenen Schlüssel.
- `DoubleForKey` -Gibt einen double-Wert für den angegebenen Schlüssel.
- `FloatForKey` -Gibt einen Float-Wert für den angegebenen Schlüssel.
- `IntForKey` -Gibt einen ganzzahligen Wert für den angegebenen Schlüssel.
- `StringArrayForKey` -Gibt ein Array von `String` Objekte aus dem angegebenen Schlüssel-Wert.
- `StringForKey` -Gibt einen Zeichenfolgenwert für den angegebenen Schlüssel.
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

Genau wie beim oben genannten Werte lesen, nachdem Sie die gewünschte Benutzer standardmäßig Datenbank zugegriffen haben können Sie Werte auf die Standardwerte, die mithilfe von Schlüssel/Wert-Paare und verschiedene praktische Methoden, die basierend auf den Typ des zu schreibenden Daten schreiben:

- `SetBool` -Schreibt den angegebenen booleschen Wert dem angegebenen Schlüssel.
- `SetDouble` -Schreibt den angegebenen double-Wert dem angegebenen Schlüssel.
- `SetFloat` -Schreibt den angegebenen Float-Wert dem angegebenen Schlüssel an.
- `SetString` -Schreibt den angegebenen Zeichenfolgenwert, der dem angegebenen Schlüssel.
- `SetURL` – Schreibt die angegebene URL (`NSUrl`) Wert, der dem angegebenen Schlüssel.

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
> Wenn Ihre App zuerst ausgeführt wird, `NSUserDefaults` die Schlüssel und Werte aus der app Benutzer standardmäßig Datenbank liest und speichert sie in den Speicher, um zu vermeiden, öffnen und lesen die Datenbank jedes Mal ein Wert erforderlich ist.



<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt die `NSUserDefaults` -Klasse, und wie sie einen Satz von Optionen bereitstellen, die die Endbenutzer, zum Konfigurieren Ihrer Xamarin.iOS-App verwenden können verwendet werden kann. Darüber hinaus konnte damit die App-Gruppen für die Kommunikation zwischen einer Erweiterung und seine übergeordneten-App oder -apps in einer Gruppe verwenden.


## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Programmierleitfaden für Einstellungen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
