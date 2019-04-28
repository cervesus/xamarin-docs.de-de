---
title: 'Teil 4: Umgang mit mehreren Plattformen'
description: Dieses Dokument beschreibt, wie Application abweichungen, die basierend auf der Plattform oder der Funktion behandelt wird. Es wird erläutert, Bildschirmgröße, Navigation Metaphern, Touch und Gesten, erste Schritte mit Pushbenachrichtigungen und Schnittstelle-Paradigmen wie z. B. Listen und Registerkarten.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8fb373aa5081c8937da5110bad47c3f0decf0126
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275538"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Teil 4: Umgang mit mehreren Plattformen

## <a name="handling-platform-divergence-amp-features"></a>Behandeln von Plattform Abweichungen &amp; Features

Abweichungen nicht nur ein "Cross-Platform"-Problem. Geräte, auf der "gleichen" Plattform haben unterschiedliche Funktionen (insbesondere die Vielzahl von Android-Geräte, die verfügbar sind). Die offensichtlichsten und am meisten basic ist die Bildschirmgröße, aber andere Geräteattribute können variieren und muss eine Anwendung bestimmte Funktionen zu prüfen und Verhalten sich unterschiedlich auf Basis ihres vorhanden (oder fehlen).

Dies bedeutet, dass alle Anwendungen müssen im Umgang mit sinnvolle Herabstufung der Funktionalität, andernfalls eine unschöne, niedrigste gemeinsamen Nenner Featuregruppe vorhanden. Xamarin Tiefe Integration mit der nativen SDKs für jede Plattform können Anwendungen nutzen plattformspezifische Funktionalität, sodass es sinnvoll, diese Funktionen-apps zu entwerfen.

Finden Sie unter der Dokumentation Plattformfunktionen eine Übersicht über die Unterschiede zwischen der Plattformen an der Funktionalität.

## <a name="examples-of-platform-divergence"></a>Beispiele für Abweichungen der Plattform

### <a name="fundamental-elements-that-exist-across-platforms"></a>Grundlegende Elemente, die plattformübergreifend vorhanden sind.

Es gibt einige Merkmale von mobilen Anwendungen, die universellen sind.
Dies sind die Konzepte auf höherer Ebene, die in der Regel "true", der alle Geräte und können daher bilden die Grundlage Ihrer Anwendung:

-  Funktionsauswahl über Registerkarten oder Menüs
-  Listen der Daten und Durchführen eines Bildlaufs
-  Einzelne Ansichten der Daten
-  Bearbeiten einzelne Ansichten der Daten
-  Zurück navigieren

Beim Entwerfen des Flows auf hoher Ebene Bildschirm können Sie eine übergreifende benutzerfreundlichkeit zu diesen Konzepten basieren.

### <a name="platform-specific-attributes"></a>Clientplattform-spezifische Attribute

Zusätzlich zur grundlegenden Elemente, die auf allen Plattformen vorhanden sind, müssen Sie an Adresse wichtige Unterschiede in Ihrem Entwurf. Möglicherweise müssen Sie diese Unterschiede berücksichtigen (und Schreiben von Code speziell für die Behandlung):

-   **Bildschirmgrößen** – einige Plattformen (wie iOS und früheren Versionen von Windows Phone) haben Bildschirmgrößen, die relativ einfach, die als Ziel sind standardisiert. Android-Geräte haben eine große Vielfalt von bildschirmabmessungen, die mehr Aufwand, die in Ihrer Anwendung unterstützen.
-   **Navigation Metaphern** – unterscheiden Sie sich auf Plattformen (z. b. Hardware "zurück"-Schaltfläche, Panorama-UI-Steuerelement) und innerhalb von Plattformen (Android 2 bis 4, iPhone und iPad).
-   **Tastaturen** – einige Android-Geräte sind physische Tastaturen, während andere nur über ein Software-Tastatur verfügen. Code, der erkennt, wenn eine Soft-Tastatur Teil des Bildschirms versteckt ist muss diese unterschieden werden.
-   **Berührung und Gesten** – insbesondere in älteren Versionen von jedem Betriebssystem variiert betriebssystemunterstützung für gestenerkennung. Frühere Versionen von Android haben sehr eingeschränkte Unterstützung für Touch-Vorgänge, was bedeutet, dass ältere Geräte unterstützen möglicherweise unterschiedlichen Code erfordern
-   **Senden von Pushbenachrichtigungen** – es gibt verschiedene Funktionen/Implementierungen auf jeder Plattform (z. b. Live-Kacheln für Windows).

### <a name="device-specific-features"></a>Gerätespezifische Funktionen

Bestimmen Sie, welche die für die Anwendung erforderlichen Features sein müssen; oder wenn festlegen, welche weiteren Features auf jeder Plattform nutzen. Code muss erkennen die Features und Funktionen zu deaktivieren oder bieten alternativen (z. b. eine Alternative zum geografischen Standort konnte, damit der Benutzer aus, geben Sie einen Speicherort aus, oder wählen Sie aus einer Zuordnung sein):

-   **Kamera** – Funktionalität, die auf Geräten unterscheidet sich: Einige Geräte keine Kamera, andere haben sowohl Front-End- und Rear Kameras. Einige Kameras sind funktionsfähig videoaufzeichnung.
-   **GeoLocation & Zuordnungen** – Unterstützung für GPS oder Wi-Fi-Speicherort ist nicht auf allen Geräten vorhanden. Apps müssen auch für die verschiedenen Vertrauensebenen Genauigkeit zu behandeln, die von jeder Methode unterstützt wird.
-   **Beschleunigungsmesser, Gyroskop und vom Kompass** – diese Features häufig befinden sich unter nur eine Auswahl an Geräten, die auf jeder Plattform, damit apps müssen fast immer ein Fallback bereitzustellen, wenn die Hardware nicht unterstützt wird.
-   **Twitter und Facebook** – nur "integrierte" iOS5 und iOS6 bzw. Unter früheren Versionen und anderen Plattformen müssen Sie Ihre eigenen Authentifizierungsfunktionen und eine Verbindung direkt mit einzelnen Services-API.
-   **NEAR Field Communications (NFC)** – (einige) nur auf Android-Telefone (zum Zeitpunkt der Verfassung).

## <a name="dealing-with-platform-divergence"></a>Umgang mit Abweichungen der Plattform

Es gibt zwei verschiedene Ansätze zur Unterstützung von mehreren Plattformen aus der gleichen Codebasis, jeweils einen eigenen Satz von vor- und Nachteile.

-   **Plattformabstraktion** – für BusinessRules-Muster bietet eine einheitliche Zugriff auf Plattformen und abstrahiert die plattformimplementierungen aus bestimmten in einer einzigen, einheitlichen API.
-   **Unterschiedliche Implementierung** : Aufruf von einer bestimmten Plattform Funktionen über unterschiedliche Implementierungen über die Architektur Tools wie Schnittstellen und Vererbung oder für die bedingte Kompilierung.

## <a name="platform-abstraction"></a>Plattformabstraktion

### <a name="class-abstraction"></a>Abstraktion der Kandidatenklassen

Mithilfe von Schnittstellen oder Basisklassen in den freigegebenen Code definiert und implementiert oder in den plattformspezifischen Projekten erweitert. Schreiben, und Erweitern von freigegebenem Code mit den Klassenabstraktionen ist besonders für Portable Klassenbibliotheken geeignet, da sie eine begrenzte Teilmenge der das Framework zur Verfügung haben und darf keine Compiler-Direktiven zur Unterstützung von plattformspezifischem codeverzweigungen enthalten.

#### <a name="interfaces"></a>Schnittstellen

Mithilfe der Schnittstellen, können Sie plattformspezifischen Klassen implementieren, die in gemeinsam genutzten Bibliotheken gemeinsamen Code nutzen weiterhin übergeben werden kann.

Die Schnittstelle ist in den freigegebenen Code definiert, und die freigegebene Bibliothek als Parameter oder Eigenschaft übergeben.

Die plattformspezifische Anwendungen können dann die Schnittstelle implementieren und dennoch nutzen freigegebenen Code "verarbeiten".

 **Vorteile**

Die Implementierung kann plattformspezifischen Code enthalten und auch plattformspezifische externe Bibliotheken zu verweisen.

 **Nachteile**

Wenn zum Erstellen und übergeben Implementierungen in den freigegebenen Code. Wenn die Schnittstelle, tief in den freigegebenen Code verwendet wird gewöhnlichen, klicken Sie dann über mehrere Parameter der Methode übergeben, oder andernfalls über der Aufrufkette weitergegeben. Wenn der freigegebene Code viele unterschiedliche Schnittstellen verwendet müssen dann diese alle erstellt und legen Sie im freigegebenen Code an einer beliebigen Stelle.

#### <a name="inheritance"></a>Vererbung

Der freigegebene Code könnten die abstrakte oder virtuelle-Klassen, die erweitert werden können implementieren, in ein oder mehrere plattformspezifische Projekte. Dies ist vergleichbar mit Schnittstellen jedoch mit einigen bereits implementiertes Verhalten. Es gibt verschiedene Standpunkte auf Schnittstellen oder Vererbung werden, ob eine bessere entwurfsentscheidung: besonders da C# -Code nur die einfache Vererbung ermöglicht es kann diktieren die Möglichkeit, Ihre APIs kann in Zukunft entworfen werden. Verwenden Sie Vererbung mit Vorsicht.

Die vor- und Nachteile von Schnittstellen gelten gleichermaßen, Vererbung, mit den zusätzlichen Vorteil, den die Basisklasse kann einigen Code zur Implementierung (z. B. eine gesamte Plattform unabhängig Implementierung, die optional erweitert werden kann) enthalten.

## <a name="xamarinforms"></a>Xamarin.Forms

Finden Sie unter den [Xamarin.Forms](~/get-started/index.yml) Dokumentation.

### <a name="other-cross-platform-libraries"></a>Andere plattformübergreifende Bibliotheken

Diese Bibliotheken bieten auch die plattformübergreifende Funktionalität für C# Entwickler:

- [**Xamarin.Essentials** ](~/essentials/index.md) – plattformübergreifende APIs für allgemeine Funktionen.
- [**SkiaSharp** ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) – plattformübergreifende 2D-Grafiken.

## <a name="conditional-compilation"></a>Bedingte Kompilierung

Es gibt einige Situationen, in dem freigegebene Code trotzdem nichts anders funktionieren auf jeder Plattform, die möglicherweise den Zugriff auf Klassen oder Funktionen, die sich anders verhalten. Für die bedingte Kompilierung funktioniert am besten bei Asset Projekten freigegeben ist, in derselben Quelldatei in mehreren Projekten verwiesen werden wird, die andere Symbole definiert haben.

Definieren Sie die Xamarin-Projekte immer `__MOBILE__` welche trifft für iOS und Android-Anwendungsprojekten (Beachten Sie die zwei Unterstriche vor und nach der Korrektur auf diese Symbole).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```
#### <a name="ios"></a>iOS

Xamarin.iOS definiert `__IOS__` die Sie verwenden können, um iOS-Geräten zu erkennen.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Es gibt auch überwachen und TV-spezifische Symbole:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

#### <a name="android"></a>Android

Code, der nur in Xamarin.Android-Anwendungen kompiliert werden soll kann Folgendes verwenden.

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Jede API-Version definiert auch eine neue Compilerdirektive, daher Code wie diesen können Sie Funktionen hinzufügen, wenn neuere APIs als Ziel verwendet werden. Jeder API-Ebene enthält die "niedriger" Level-Symbole. Dieses Feature ist nicht sehr nützlich für die Unterstützung von mehreren Plattformen; in der Regel die `__ANDROID__` Symbol ausreichen.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

#### <a name="mac"></a>Mac

Zurzeit ist kein integriertes Symbol für Xamarin.Mac, aber Sie können Ihren eigenen Mac-app-Projekt hinzufügen **Optionen > Erstellen > Compiler** in die **Symbole definieren** , oder bearbeiten Sie die **csproj**  Datei, und fügen dort (z. B. `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

#### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Verwenden Sie `WINDOWS_UWP`. Es gibt keine Unterstriche Zusammenhang mit der Zeichenfolge wie etwa die Xamarin-Plattform-Symbole.

```csharp
#if WINDOWS_UWP
// UWP-specific code
#endif
```

#### <a name="using-conditional-compilation"></a>Verwenden für die bedingte Kompilierung

Ein einfaches Beispiel-Fallstudie für die bedingte Kompilierung ist der Speicherort für die SQLite-Datenbank-Datei festlegen. Die drei Plattformen haben geringfügig unterschiedliche Anforderungen an den Dateispeicherort angeben:

-   **iOS** – Apple bevorzugt nichtbenutzer-Daten in einem bestimmten Speicherort (das Verzeichnis mit der Bibliothek) platziert werden, aber keine System-Konstante für dieses Verzeichnis vorhanden ist. Plattformspezifischen Code ist erforderlich, um den richtigen Pfad erstellen.
-   **Android** – zurückgegebenes Systempfad `Environment.SpecialFolder.Personal` ein zulässiger Speicherort zum Speichern der Datenbank ist.
-   **Windows Phone** – der isolierten Speichermechanismus lässt sich nicht auf einen vollständigen Pfad angegeben werden, nur einen relativen Pfad und Dateinamen.
-   **Universelle Windows-Plattform** – verwendet `Windows.Storage` APIs.

Der folgende Code für die bedingte Kompilierung verwendet, um sicherzustellen, dass die `DatabaseFilePath` für jede Plattform korrekt ist:

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

Das Ergebnis ist eine Klasse, die erstellt und auf allen Plattformen, platzieren die SQLite-Datenbank-Datei an einem anderen Speicherort auf jeder Plattform verwendet werden kann.
