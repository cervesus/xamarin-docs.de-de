---
title: Einführung in DependencyService
description: In diesem Artikel wird die Funktionsweise der DependencyService-Klasse von Xamarin.Forms für den Zugriff auf native Plattformfeatures erläutert.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 3c8cc31c21f354b60001cefb919b51bf4d42da9f
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675015"
---
# <a name="introduction-to-dependencyservice"></a>Einführung in DependencyService

## <a name="overview"></a>Übersicht

[`DependencyService`](xref:Xamarin.Forms.DependencyService) erlaubt Apps, über freigegebenen Code plattformspezifische Funktionen aufzurufen. Mithilfe dieser Funktionen können Xamarin.Forms-Apps alle Schritte ausführen, die eine native App ausführen kann.

`DependencyService` ist ein Dienstlocator. In der Praxis wird eine Schnittstelle definiert und `DependencyService` sucht die richtige Implementierung dieser Schnittstelle aus den verschiedenen Plattformprojekten.

> [!NOTE]
> Standardmäßig löst [`DependencyService`](xref:Xamarin.Forms.DependencyService) nur Plattformimplementierungen auf, die parameterlose Konstruktoren besitzen. In Xamarin.Forms kann jedoch eine Methode für die Auflösung von Abhängigkeiten eingefügt werden, die für die Auflösung von Plattformimplementierungen einen Container für die Dependency Injection oder Factorymethoden verwendet. Dieser Ansatz kann zur Auflösung von Plattformimplementierungen verwendet werden, die über Konstruktoren mit Parametern verfügen. Weitere Informationen finden Sie unter [Auflösung von Abhängigkeiten in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

## <a name="how-dependencyservice-works"></a>Funktionsweise von DependencyService

Xamarin.Forms-Apps benötigen vier Komponenten für die Verwendung von `DependencyService`:

- **Schnittstelle**: Die erforderlichen Funktionen werden durch eine Schnittstelle in freigegebenem Code definiert.
- **Implementierung pro Plattform**: Klassen, welche die Schnittstelle implementieren, müssen zu den einzelnen Plattformprojekten hinzugefügt werden.
- **Registrierung**: Die einzelnen Implementierungsklassen müssen über ein Metadatenattribut bei `DependencyService` registriert werden. Durch die Registrierung kann `DependencyService` anstelle der Schnittstelle zur Laufzeit nach der Implementierungsklasse suchen und diese bereitstellen.
- **Aufrufen von DependencyService** &ndash; Freigegebener Code muss explizit `DependencyService` aufrufen, um Implementierungen der Schnittstelle abzufragen.

Beachten Sie, dass für jedes Plattformprojekt in Ihrer Projektmappe Implementierungen bereitgestellt werden müssen. Plattformprojekte ohne Implementierungen schlagen zur Laufzeit fehl.

Die Struktur der Anwendung wird durch das folgende Diagramm erläutert:

![](introduction-images/overview-diagram.png "Anwendungsstruktur von DependencyService")

### <a name="interface"></a>Interface

Die von Ihnen entworfene Schnittstelle definiert, wie Sie mit plattformspezifischen Funktionen interagieren. Seien Sie vorsichtig, wenn Sie eine Komponente entwickeln, die als Komponente oder NuGet-Paket freigegeben werden soll. Mit dem API-Entwurf kann ein Paket erstellt oder unterbrochen werden. Im folgenden Beispiel wird eine einfache Schnittstelle für das Sprechen von Text angegeben, die Flexibilität bei der Angabe der zu sprechenden Wörter ermöglicht. Die Implementierung bleibt für die einzelnen Plattformen jedoch angepasst:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implementierung nach Plattform

Sobald eine geeignete Schnittstelle entworfen wurde, muss diese Schnittstelle bei jeder Zielplattform in das Projekt implementiert werden. Die folgende Klasse implementiert beispielsweise die Schnittstelle `ITextToSpeech` unter iOS:

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>Registrierung

Die einzelnen Implementierungen der Schnittstelle müssen mit einem Metadatenattribut bei `DependencyService` registriert werden. Mit dem folgenden Code wird die Implementierungsdatei für iOS registriert:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Insgesamt sieht die plattformspezifische Implementierung wie folgt aus:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

Hinweis: Die Registrierung erfolgt auf Namespaceebene, nicht auf Klassenebene.

#### <a name="universal-windows-platform-net-native-compilation"></a>.NET Native-Kompilierung für die Universelle Windows-Plattform

UWP-Projekte, die die Option für die .NET Native-Kompilierung verwenden, sollten beim Initialisieren von Xamarin.Forms einer [etwas anderen Konfiguration](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) folgen. Für die .NET Native-Kompilierung ist zudem eine etwas andere Registrierung für Abhängigkeitsdienste erforderlich.

Registrieren Sie in der Datei **App.xaml.cs** die einzelnen Abhängigkeitsdienste, die im UWP-Projekt definiert sind, manuell mithilfe der `Register<T>`-Methode, wie im Folgenden dargestellt:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Hinweis: Die manuelle Registrierung mit `Register<T>` ist in Release-Builds nur unter Verwendung der .NET Native-Kompilierung wirksam. Wenn Sie diese Zeile auslassen, funktionieren Debug-Builds weiterhin, Release-Builds können den Abhängigkeitsdienst jedoch nicht laden.

### <a name="call-to-dependencyservice"></a>Aufrufen von DependencyService

Nachdem das Projekt mit einer allgemeinen Schnittstelle und Implementierungen für die einzelnen Plattformen eingerichtet wurde, können Sie über `DependencyService` zur Laufzeit die richtige Implementierung abzurufen:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` sucht nach der richtigen Implementierung der Schnittstelle `T`.

### <a name="solution-structure"></a>Projektmappenstruktur

Die [UsingDependencyService-Projektmappe](https://developer.xamarin.com/samples/UsingDependencyService/) wird nachfolgend für iOS und Android angezeigt. Darin sind die oben dargestellten Codeänderungen hervorgehoben.

 [![Projektmappe unter iOS und Android](introduction-images/solution-sml.png "Beispielhafte DependencyService-Projektmappenstruktur")](introduction-images/solution.png#lightbox "Beispielhafte DependencyService-Projektmappenstruktur")

> [!NOTE]
> Sie **müssen** in jedem Plattformprojekt eine Implementierung bereitstellen. Wenn keine Schnittstellenimplementierung registriert ist, kann `DependencyService` die `Get<T>()`-Methode zur Laufzeit nicht auflösen.

## <a name="related-links"></a>Verwandte Links

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
