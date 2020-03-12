---
title: 'Teil 4: Umgang mit mehreren Plattformen'
description: In diesem Dokument wird beschrieben, wie die Anwendungs Abweichung basierend auf Plattform oder Funktion behandelt wird. Es werden Bildschirmgröße, Navigations Metaphern, toucheingaben und Gesten, Pushbenachrichtigungen und Schnittstellen Paradigmen wie Listen und Registerkarten erläutert.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: c8b4dcbfbf65bc4059125404b0d20ed35fa31f29
ms.sourcegitcommit: ce4670de51e24116a944c778ee64585bd0aae0e1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79088925"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Teil 4: Umgang mit mehreren Plattformen

## <a name="handling-platform-divergence-amp-features"></a>Behandeln von Platt Form Abweichungen &amp; Features

Die Abweichung ist nicht nur ein plattformübergreifendes Problem. Geräte auf der gleichen Plattform verfügen über unterschiedliche Funktionen (insbesondere die Vielzahl von verfügbaren Android-Geräten). Der offensichtlichste und einfachste Wert ist die Bildschirmgröße, andere Geräte Attribute können jedoch variieren und erfordern, dass eine Anwendung bestimmte Funktionen prüft und sich je nach Anwesenheit (oder Abwesenheit) unterschiedlich verhält.

Dies bedeutet, dass alle Anwendungen eine ordnungsgemäße Verschlechterung der Funktionalität durchführen müssen, oder Sie stellen eine unattraktive, niedrigste und gemeinsame Nenner dar. Die Deep-Integration von xamarin in die nativen sdgs der einzelnen Plattformen ermöglicht Anwendungen, plattformspezifische Funktionen zu nutzen. Daher ist es sinnvoll, Apps für die Verwendung dieser Features zu entwerfen.

In der Dokumentation zu den Plattformfunktionen finden Sie eine Übersicht über die Unterschiede zwischen den Plattformen und Funktionen.

## <a name="examples-of-platform-divergence"></a>Beispiele für Platt Form Abweichungen

### <a name="fundamental-elements-that-exist-across-platforms"></a>Grundlegende Elemente, die plattformübergreifend vorhanden sind

Es gibt einige Merkmale von universellen mobilen Anwendungen.
Dabei handelt es sich um Konzepte höherer Ebene, die im Allgemeinen für alle Geräte zutreffen und daher die Grundlage für den Entwurf Ihrer Anwendung bilden:

- Funktionsauswahl über Registerkarten oder Menüs
- Listen der Daten und scrollen
- Einzelne Ansichten von Daten
- Bearbeiten einzelner Ansichten von Daten
- Navigieren zurück

Wenn Sie den bildschirmfluss auf hoher Ebene entwerfen, können Sie eine gängige Benutzer Darstellung auf diese Konzepte aufbauen.

### <a name="platform-specific-attributes"></a>Plattformspezifische Attribute

Zusätzlich zu den grundlegenden Elementen, die auf allen Plattformen vorhanden sind, müssen Sie die wichtigsten Platt Form Unterschiede in Ihrem Entwurf berücksichtigen. Sie müssen möglicherweise die folgenden Unterschiede in Erwägung gezogen (und Code speziell für die Behandlung von Code schreiben):

- **Bildschirmgrößen** – einige Plattformen (z. b. IOS und frühere Versionen von Windows Phone) verfügen über standardisierte Bildschirmgrößen, die relativ einfach zu erreichen sind. Android-Geräte verfügen über eine große Bandbreite an Bildschirm Abmessungen, bei denen mehr Aufwand für die Unterstützung von in Ihrer Anwendung erforderlich ist.
- **Navigations Metaphern** – unterscheiden sich plattformübergreifend (z. b. Schaltfläche "zurück" für Hardware, Panorama-UI-Steuerelement) und innerhalb von Plattformen (Android 2 und 4, iPhone vs iPad).
- **Tastaturen** – einige Android-Geräte verfügen über physische Tastaturen, während andere nur über eine Software Tastatur verfügen. Code, der erkennt, wenn eine weiche Tastatur einen Teil des Bildschirms verdeckt, muss für diese Unterschiede sensibel sein.
- **Toucheingabe und Gesten** – die Betriebssystemunterstützung für die Gestenerkennung variiert, insbesondere in älteren Versionen der einzelnen Betriebssysteme. Frühere Versionen von Android bieten sehr eingeschränkte Unterstützung für Berührungs Vorgänge, was bedeutet, dass die Unterstützung älterer Geräte unter Umständen separaten Code erfordert.
- **Pushbenachrichtigungen** – es gibt verschiedene Funktionen/Implementierungen auf jeder Plattform (z. b. Live-Kacheln unter Windows).

### <a name="device-specific-features"></a>Gerätespezifische Features

Bestimmen Sie, welche Mindestanforderungen für die Anwendung benötigt werden müssen. oder wenn Sie entscheiden, welche zusätzlichen Features auf den einzelnen Plattformen genutzt werden. Code ist erforderlich, um Features zu erkennen und Funktionen zu deaktivieren oder Alternativen anzubieten (z. b. eine Alternative zur georedunzierung könnte darin bestehen, dass der Benutzer einen Speicherort eingeben oder aus einer Karte auswählen kann:

- **Kamera** – unterscheidet sich von Geräten: einige Geräte verfügen nicht über eine Kamera, andere verfügen über Front-und rückwärts-Kameras. Einige Kameras sind in der Lage, Video aufzuzeichnen.
- Der Standort **& Maps** – Unterstützung für GPS oder WLAN ist nicht auf allen Geräten vorhanden. Apps müssen auch für die unterschiedlichen Genauigkeits Stufen geeignet sein, die von den einzelnen Methoden unterstützt werden.
- **Beschleunigungsmesser, Gyroskop und Compass** – diese Features sind häufig nur in einer Auswahl von Geräten auf jeder Plattform zu finden, sodass apps fast immer einen Fall Back bereitstellen müssen, wenn die Hardware nicht unterstützt wird.
- **Twitter und Facebook** – nur "integriert" auf iOS5 und iOS6. In früheren Versionen und anderen Plattformen müssen Sie Ihre eigenen Authentifizierungsfunktionen und-Schnittstellen direkt mit den einzelnen Dienst-APIs bereitstellen.
- **Near Field Communications (NFC)** – nur auf (einigen) Android-Telefonen (zum Zeitpunkt des Schreibens).

## <a name="dealing-with-platform-divergence"></a>Umgang mit Platt Form Abweichungen

Es gibt zwei unterschiedliche Ansätze zur Unterstützung mehrerer Plattformen aus derselben Codebasis, die jeweils über einen eigenen Satz von Vorteilen und Nachteile verfügen.

- **Platform Abstraktion** – Business Fassaden Pattern bietet einen einheitlichen Zugriff auf Plattformen und abstrahiert die einzelnen Platt Form Implementierungen in eine einzelne, einheitliche API.
- **Abweichende Implementierung** – Aufruf spezifischer Platt Form Features über abweichende Implementierungen über Architektur Tools wie Schnittstellen und Vererbung oder bedingte Kompilierung.

## <a name="platform-abstraction"></a>Plattformabstraktion

### <a name="class-abstraction"></a>Klassen Abstraktion

Verwenden von Schnittstellen oder Basisklassen, die im freigegebenen Code definiert und in plattformspezifischen Projekten implementiert oder erweitert wurden. Das Schreiben und Erweitern von frei gegebenem Code mit Klassen Abstraktionen eignet sich besonders für Portable Klassenbibliotheken, da Ihnen eine begrenzte Teilmenge des Frameworks zur Verfügung steht und keine Compilerdirektiven zur Unterstützung plattformspezifischer codebranches enthalten kann.

#### <a name="interfaces"></a>Schnittstellen

Durch die Verwendung von Schnittstellen können Sie plattformspezifische Klassen implementieren, die weiterhin an Ihre freigegebenen Bibliotheken weitergegeben werden können, um gemeinsamen Code zu nutzen.

Die-Schnittstelle wird im freigegebenen Code definiert und als Parameter oder Eigenschaft an die freigegebene Bibliothek übergeben.

Die plattformspezifischen Anwendungen können dann die-Schnittstelle implementieren und weiterhin freigegebenen Code nutzen, um ihn zu verarbeiten.

 **Vorteile**

Die-Implementierung kann plattformspezifischen Code enthalten und sogar plattformspezifische externe Bibliotheken referenzieren.

 **Nachteile**

Es müssen Implementierungen erstellt und in den freigegebenen Code übergeben werden. Wenn die Schnittstelle tief innerhalb des freigegebenen Codes verwendet wird, wird Sie schließlich über mehrere Methoden Parameter übergeben oder anderweitig durch die-Aufrufkette übertragen. Wenn für den freigegebenen Code viele verschiedene Schnittstellen verwendet werden, müssen Sie alle erstellt und im freigegebenen Code irgendwo festgelegt werden.

#### <a name="inheritance"></a>Vererbung

Mit dem freigegebenen Code können abstrakte oder virtuelle Klassen implementiert werden, die in einem oder mehreren plattformspezifischen Projekten erweitert werden könnten. Dies ähnelt der Verwendung von Schnittstellen, aber mit einem bereits implementierten Verhalten. Es gibt unterschiedliche Gesichtspunkte, ob Schnittstellen oder Vererbung eine bessere Entwurfs Entscheidung sind C# : insbesondere, da nur die einfache Vererbung zulässt, kann es die Art und Weise vorschreiben, wie Ihre APIs weiterentwickelt werden können. Verwenden Sie die Vererbung mit Vorsicht.

Die vor-und Nachteile von Schnittstellen gelten auch für die Vererbung, mit dem zusätzlichen Vorteil, dass die Basisklasse Implementierungs Code enthalten kann (z. b. eine gesamte plattformunabhängige Implementierung, die optional erweitert werden kann).

## <a name="xamarinforms"></a>Xamarin.Forms

Weitere Informationen finden Sie in der Dokumentation zu [xamarin. Forms](~/get-started/index.yml) .

### <a name="other-cross-platform-libraries"></a>Andere plattformübergreifende Bibliotheken

Diese Bibliotheken bieten auch plattformübergreifende Funktionen für C# Entwickler:

- [**Xamarin. Essentials**](~/essentials/index.md) – plattformübergreifende APIs für allgemeine Features.
- [**Skiasharp**](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) – plattformübergreifende 2D-Grafiken.

## <a name="conditional-compilation"></a>Bedingte Kompilierung

Es gibt einige Situationen, in denen der freigegebene Code auf jeder Plattform unterschiedlich funktionieren muss und möglicherweise auf Klassen oder Funktionen zugreift, die sich anders Verhalten. Die bedingte Kompilierung funktioniert am besten mit freigegebenen Asset-Projekten, bei denen in mehreren Projekten, für die verschiedene Symbole definiert sind, auf dieselbe Quelldatei verwiesen wird.

Xamarin-Projekte definieren immer `__MOBILE__`, die für IOS-und Android-Anwendungsprojekte zutreffen (Beachten Sie den doppelten unterstrich vor und nach der Behebung dieser Symbole).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

#### <a name="ios"></a>iOS

Xamarin. IOS definiert `__IOS__`, die Sie zum Erkennen von IOS-Geräten verwenden können.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Es gibt auch Überwachungs-und TV-spezifische Symbole:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

#### <a name="android"></a>Android

Code, der nur in xamarin. Android-Anwendungen kompiliert werden soll, kann Folgendes verwenden:

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Jede API-Version definiert auch eine neue Compilerdirektive, sodass Sie mit diesem Code Features hinzufügen können, wenn neuere APIs als Ziel verwendet werden. Jede API-Ebene enthält alle Symbole der Ebene "Lower". Diese Funktion ist für die Unterstützung mehrerer Plattformen nicht wirklich nützlich. in der Regel reicht das `__ANDROID__` Symbol aus.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

#### <a name="mac"></a>Mac

Xamarin. Mac definiert `__MACOS__`, die Sie zum Kompilieren nur für macOS verwenden können:

```csharp
#if __MACOS__
// macOS-specific code
#endif
```

#### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Verwenden Sie `WINDOWS_UWP`. Es gibt keine Unterstriche, die die Zeichenfolge wie die xamarin-Platt Form Symbole umschließen.

```csharp
#if WINDOWS_UWP
// UWP-specific code
#endif
```

#### <a name="using-conditional-compilation"></a>Verwenden der bedingten Kompilierung

Ein einfaches Beispiel für die bedingte Kompilierung besteht darin, den Datei Speicherort für die SQLite-Datenbankdatei festzulegen. Die drei Plattformen haben etwas andere Anforderungen zum Angeben des Dateispeicher Orts:

- **IOS** – Apple bevorzugt, dass sich Nichtbenutzer Daten an einem bestimmten Speicherort (dem Bibliotheksverzeichnis) befinden, es gibt jedoch keine System Konstante für dieses Verzeichnis. Der plattformspezifische Code ist erforderlich, um den korrekten Pfad zu erstellen.
- **Android** – der von `Environment.SpecialFolder.Personal` zurückgegebene Systempfad ist ein akzeptabler Speicherort zum Speichern der Datenbankdatei.
- **Windows Phone** – der isolierte Speichermechanismus lässt nicht zu, dass ein vollständiger Pfad angegeben wird, sondern nur ein relativer Pfad und Dateiname.
- **Universelle Windows-Plattform** – verwendet `Windows.Storage`-APIs.

Der folgende Code verwendet die bedingte Kompilierung, um sicherzustellen, dass die `DatabaseFilePath` für jede Plattform richtig ist:

```csharp
public static string DatabaseFilePath
{
    get
    {
        var filename = "TodoDatabase.db3";
#if SILVERLIGHT
        // Windows Phone 8
        var path = filename;
#else

#if __ANDROID__
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
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
        var path = Path.Combine(libraryPath, filename);
#endif
        return path;
    }
}
```

Das Ergebnis ist eine Klasse, die auf allen Plattformen erstellt und verwendet werden kann, wobei die SQLite-Datenbankdatei an einem anderen Speicherort auf jeder Plattform platziert wird.
