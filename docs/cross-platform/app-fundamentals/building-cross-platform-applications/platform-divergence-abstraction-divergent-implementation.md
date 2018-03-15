---
title: 'Teil 4: Umgang mit mehreren Plattformen'
ms.topic: article
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 21cd08ad2eb9c78ba1bcd6b31400a38266c65e51
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Teil 4: Umgang mit mehreren Plattformen

## <a name="handling-platform-divergence-amp-features"></a>Behandlung von Plattform Abweichungen &amp; Funktionen

Abweichungen nicht nur auf ein Problem 'Cross-Platform'. Geräte für "dieselbe" Plattform haben verschiedene Funktionen (insbesondere die Vielzahl von Android-Geräte, die verfügbar sind). Die offensichtliche und grundlegende ist Bildschirmgröße allerdings andere Geräteattribute können variieren und erfordern eine Anwendung überprüfen für bestimmte Funktionen und Verhalten sich anders basierend auf deren Vorhandensein (oder fehlen).

Dies bedeutet, dass alle Anwendungen müssen Ausstiegs Funktionalität zurecht, sonst ein uninteressant, niedrigste gemeinsamen Nenner Funktionsgruppe vorhanden. Xamarin umfassende Integration in die systemeigene SDKs für jede Plattform können Anwendungen profitieren von Clientplattform-spezifische Funktionalität, damit es sinnvoll, Entwerfen von apps, die Funktionen zu verwenden.

Finden Sie die Dokumentation Plattformfunktionen eine Übersicht über die Unterschiede zwischen der Plattformen Funktionalität.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Beispiele für die Plattform Abweichungen

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Grundlegende Elemente, die über Plattformen hinweg vorhanden sind.

Es gibt einige Eigenschaften von mobilen Anwendungen, die universellen sind.
Dies sind auf höherer Ebene Konzepte, die in der Regel gilt auch für alle Geräte und können daher bilden die Grundlage für Ihre Anwendung entwerfen:

-  Funktionsauswahl über Registerkarten oder Menüs
-  Listen der Daten und Durchführen eines Bildlaufs
-  Einzelnen Sichten der Daten
-  Bearbeiten die einzelne Sichten der Daten
-  Navigieren zurück


Beim Entwerfen Ihrer Fluss auf hoher Ebene Bildschirm können Sie eine gemeinsame Benutzeroberfläche für diese Konzepte definieren.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>Clientplattform-spezifische Attribute

Zusätzlich zu den grundlegenden Elemente, die auf allen Plattformen vorhanden sind, müssen Sie auf Adresse Key Plattformunterschiede im Entwurf. Möglicherweise müssen Sie diese Unterschiede sollten (und Schreiben von Code speziell für die Behandlung):

-   **Bildschirm Größen** – einige Plattformen (z. B. iOS und früheren Versionen von Windows Phone) haben standardisiert Bildschirmgrößen, die relativ einfach, ausgerichtet sind. Android-Geräte haben eine große Vielfalt von Bildschirmdimensionen, die Unterstützung in Ihre Anwendung mehr Aufwand.
-   **Navigation Metaphern** – unterscheiden Sie sich über Plattformen (z. b. Hardware 'zurück' Schaltfläche Panorama UI-Steuerelement) und innerhalb von Plattformen (Android 2 und 4, iPhone und iPad).
-   **Tastaturen** – einige Android-Geräte sind physische Tastaturen, während andere nur eine Software-Tastatur verfügen. Code, der erkennt, wenn eine Soft-Tastatur Teil des Bildschirms verdeckt ist muss empfindlich gegenüber diesen unterschieden werden.
-   **Touch- und Gesten** – Betriebssystem Geste Recognition variiert die Unterstützung für, insbesondere in älteren Versionen für jedes Betriebssystem. Frühere Versionen von Android haben sehr eingeschränkte Unterstützung für Touch-Operationen, d. h., dass ältere Geräte unterstützen separaten Code erfordern
-   **Pushbenachrichtigungen** – es gibt verschiedene Funktionen/Implementierungen auf jeder Plattform (z. b. Live-Kacheln unter Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Geräte-spezifischer Funktionen

Bestimmen Sie, was die minimale, für die Anwendung erforderlichen Funktionen werden müssen; oder wenn entscheiden, welche zusätzlichen Funktionen auf jeder Plattform nutzen. Code wird erforderlich sein, um zu erkennen Features und Funktionen deaktivieren oder bieten alternativen (z. b. eine Alternative zum geografischen Standort könnte darin bestehen, den Benutzer aus, geben Sie einen Speicherort, oder wählen Sie aus einer Zuordnung lassen):

-   **Kamera** – Funktionen unterscheidet sich auf Geräten verfügbar sind: Einige Geräte besitzen keiner Kamera, andere haben beide vorne und hinten gerichteten Kameras. Einige Kameras werden können videoaufzeichnung.
-   **GeoLocation und Zuordnungen** – Unterstützung für GPS oder Wi-Fi-Speicherort ist nicht auf allen Geräten vorhanden. Apps müssen auch für die verschiedenen Vertrauensebenen Genauigkeit zu erfüllen, die von jeder Methode unterstützt wird.
-   **Beschleunigungsmesser, Gyroskop und Kompass** – diese Funktionen werden häufig in nur eine Auswahl an Geräten auf jeder Plattform gefunden, damit apps müssen fast immer ein Fallback bereitstellen, wenn die Hardware nicht unterstützt wird.
-   **Twitter und Facebook** – nur "integrierte" iOS5 und iOS6 bzw. Unter früheren Versionen und anderen Plattformen müssen Sie eigene Authentifizierungsfunktionen bereitzustellen und direkt mit jedem Schnittstelle Services-API.
-   **NEAR Field Communications (NFC)** – (einige) nur für Android-Telefone (zum Zeitpunkt der Verfassung).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Umgang mit Abweichungen Plattform

Es gibt zwei unterschiedliche Ansätze für die Unterstützung von mehreren Plattformen aus der gleichen Codebasis, jeweils über einen eigenen Satz von vor- und Nachteile.

-   **Plattform Abstraktion** – für BusinessRules-Muster bietet eine einheitliche Zugriff über Plattformen hinweg und die bestimmten Plattform-Implementierungen in eine einheitliche API abstrahiert.
-   **Voneinander abweichende Implementierung** – Aufrufen von bestimmten Plattform verfügt über unterschiedliche Implementierungen über architektonische Tools, z. B. Schnittstellen und Vererbung oder bedingte Kompilierung.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Plattformabstraktion

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Abstraktion

Mit Schnittstellen oder Basisklassen im freigegebenen Code definiert und implementiert oder im plattformspezifischen Projekte erweitert. Schreiben, und Erweitern von freigegebenem Code mit der Klassenabstraktionen sind besonders auf Portable Klassenbibliotheken geeignet, da sie eine begrenzte Teilmenge von Framework zur Verfügung haben und können nicht Compilerdirektiven zur Unterstützung der plattformspezifischen codeverzweigungen enthalten.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Schnittstellen

Verwendung von Schnittstellen ermöglicht Clientplattform-spezifische Klassen implementieren, die noch in Ihren freigegebenen Bibliotheken gemeinsamen Code nutzen übergeben werden kann.

Die Schnittstelle ist in den freigegebenen Code definiert und in der freigegebenen Bibliothek als Parameter oder Eigenschaft übergeben.

Die Clientplattform-spezifische Anwendungen können implementieren Sie die Schnittstelle und dennoch Code "Verarbeitung" gemeinsam nutzen.

 **Vorteile**

Die Implementierung kann verweisen auch plattformspezifische externen Bibliotheken und plattformspezifischen Code enthalten.

 **Nachteile**

Zum Erstellen und übergeben Implementierungen in den freigegebenen Code vorliegt. Wenn die Schnittstelle tief in den freigegebenen Code verwendet wird Endes dann sie über mehrere Methodenparameter übergeben, oder andernfalls durch die Aufrufkette weitergegeben. Wenn der freigegebene Code viele unterschiedliche Schnittstellen verwendet müssen dann diese alle erstellt und legen Sie im freigegebenen Code an einer beliebigen Stelle.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Vererbung

Der freigegebene Code konnte abstrakte oder virtuelle Klassen, die erweitert werden konnte in ein oder mehrere plattformspezifischen Projekte implementieren. Dies ist vergleichbar mit Schnittstellen, doch einige Verhaltensweisen, die bereits implementiert. Es gibt verschiedene Blickpunkte, ob Schnittstellen oder Vererbung Entwurf besser geeignet sind: insbesondere weil c# nur einfache Vererbung ermöglicht es kann diktieren die Möglichkeit, Ihre APIs zukünftig entworfen werden können. Verwenden Sie Vererbung mit Vorsicht.

Die vor- und Nachteile von Schnittstellen gelten gleichermaßen, Vererbung, mit den zusätzlichen Vorteil, den die Basisklasse, kann einige Implementierungscode (z. B. eine gesamte Plattform agnostisch Implementierung, die optional erweitert werden kann) enthalten.

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Finden Sie unter der [Xamarin.Forms](~/xamarin-forms/get-started/index.md) Dokumentation.


### <a name="plug-in-cross-platform-functionality"></a>Plug-in-Cross-Platform-Funktionen

Sie können auch plattformübergreifende apps auf konsistente Weise mit-Plug-Ins erweitern.

Aus verknüpften unsere [-Plug-Ins Github](https://github.com/xamarin/plugins), sind den meisten Erweiterungen Open Source-Projekte (in der Regel für die Installation über Nuget verfügbar), mit denen Sie implementieren eine Vielzahl von Clientplattform-spezifische Funktionalität aus Akkustatus Einstellungen mit einer gemeinsame-API, der einfach im Xamarin-Plattform und Xamarin.Forms-apps nutzen.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Andere plattformübergreifende Bibliotheken

Es gibt eine Reihe von 3rd Party-Bibliotheken, die verfügbar sind, die Funktionalität über Plattformen hinweg bereitstellen:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Fachsprache** (für Lokalisierung) -  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (für XNA-Spiele) -  [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) und seine Vorläufer [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Voneinander abweichende Implementierung

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Bedingte Kompilierung

Es gibt einige Situationen, in dem freigegebene Code immer noch anders funktionieren auf jeder Plattform, die möglicherweise den Zugriff auf Klassen oder Funktionen, die sich anders verhalten. Bedingter Kompilierung funktioniert am besten mit freigegebenen Asset-Projekten, in denen die gleichen Quelldatei in mehreren Projekten referenziert wird, die unterschiedliche Symbole definiert haben.

Definieren Sie die Xamarin-Projekte immer `__MOBILE__` also "true" für IOS- und Android-Anwendung-Projekte (den doppelte Unterstrich vor und nach der Korrektur auf diese Symbole beachten).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

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

<a name="Android" />

##### <a name="android"></a>Android

Code, der nur in Clientanwendungen Xamarin.Android kompiliert werden sollte, kann Folgendes verwenden.

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Jede API-Version definiert auch eine neuen Compiler-Direktive, damit Codes können Sie Funktionen hinzufügen, neuere APIs angesprochen werden. Jede API-Ebene enthält die "unteren" Level-Symbole. Diese Funktion ist nicht sehr nützlich für die Unterstützung mehrerer Plattformen; in der Regel die `__ANDROID__` Symbol ausreichen.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Es ist kein derzeit ein integriertes Symbol für Xamarin.Mac, aber Sie können Ihren eigenen Mac app-Projekt hinzufügen **Optionen > Erstellen > Compiler** in der **Symbole** , oder bearbeiten Sie die **csproj**  Datei, und fügen dort (z. B. `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Windows Phone-apps definiert zwei Symbole – `WINDOWS_PHONE` und `SILVERLIGHT` –, um Code für die Plattform als Ziel verwendet werden kann. Diese verfügen nicht über die Unterstriche wie die Xamarin-Plattform-Symbole umschlossen werden.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Bedingte Kompilierung

Ein einfaches Beispiel Fallstudie bedingte Kompilierung wird der Speicherort für die Datenbankdatei SQLite festlegen. Die drei Plattformen haben leicht abweichende Anforderungen für den Dateispeicherort angeben:

-   **iOS** : Apple bevorzugt nichtbenutzer-Daten in einem bestimmten Speicherort (Bibliotheksverzeichnis) platziert werden sollen, aber es ist keine Konstante System für dieses Verzeichnis. Plattformspezifischen Code ist erforderlich, um den richtigen Pfad zu erstellen.
-   **Android** – zurückgegebenes Systempfad `Environment.SpecialFolder.Personal` ist ein akzeptabler Speicherort zum Speichern der Datei.
-   **Windows Phone-** – die isolierten Speichermechanismus lässt sich nicht auf einen vollständigen Pfad angegeben werden, nur einen relativen Pfad und Dateinamen.
-   **Universelle Windows-Plattform** – verwendet `Windows.Storage` APIs.

Der folgende Code bedingten Kompilierung verwendet, um sicherzustellen, dass die `DatabaseFilePath` für jede Plattform richtig ist:

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

Das Ergebnis ist eine Klasse, die erstellt und auf allen Plattformen, platzieren die SQLite-Datenbankdatei an einem anderen Speicherort auf jeder Plattform verwendet werden kann.

