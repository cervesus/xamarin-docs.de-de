---
title: Einführung in die DependencyService
description: In diesem Artikel wird die Funktionsweise von Xamarin.Forms DependencyService Klasse zum Zugriff auf systemeigene Plattformfunktionen erläutert.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 28c6daa361b7de09a0d9332b21f1b6f75e035850
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "38995413"
---
# <a name="introduction-to-dependencyservice"></a>Einführung in die DependencyService

## <a name="overview"></a>Übersicht

[`DependencyService`](xref:Xamarin.Forms.DependencyService) erlaubt apps das plattformspezifische Funktionen über freigegebenen Code aufgerufen. Diese Funktionalität ermöglicht das Xamarin.Forms-apps, nichts tun, die eine native app ausführen können.

`DependencyService` ist ein Service Locator. In der Praxis ist eine Schnittstelle definiert und `DependencyService` sucht die richtige Implementierung dieser Schnittstelle aus den verschiedenen Plattform-Projekten.

> [!NOTE]
> In der Standardeinstellung die [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) nur löst plattformimplementierungen aus, die parameterlose Konstruktoren besitzen. Allerdings kann eine Abhängigkeit Auflösungsmethode in Xamarin.Forms eingefügt werden, die DI-Containern oder Factorymethoden verwendet, um die plattformimplementierungen zu beheben. Dieser Ansatz kann plattformimplementierungen zu beseitigen, die über Konstruktoren mit Parametern verwendet werden. Weitere Informationen finden Sie unter [abhängigkeitsauflösung in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

## <a name="how-dependencyservice-works"></a>Funktionsweise von DependencyService

Xamarin.Forms-apps benötigen Sie vier Komponenten zur Verwendung `DependencyService`:

- **Schnittstelle** &ndash; die erforderliche Funktionalität wird durch eine Schnittstelle in freigegebenem Code definiert.
- **Implementierung pro Plattform** &ndash; Klassen, die die Schnittstelle implementieren müssen jedes plattformprojekt hinzugefügt werden.
- **Registrierung** &ndash; jeder Klasse implementieren muss registriert werden, mit `DependencyService` über ein Metadatenattribut. Registrierung ermöglicht `DependencyService` die implementierende Klasse suchen und anstelle der Schnittstelle zur Laufzeit bereitstellen.
- **Aufrufen, um DependencyService** &ndash; freigegebene Code muss explizit aufrufen `DependencyService` , Implementierungen der Schnittstelle abzufragen.

Beachten Sie, dass Implementierungen für jede Plattform-Projekt in der Projektmappe bereitgestellt werden müssen. Plattform-Projekten ohne Implementierungen werden zur Laufzeit fehlschlagen.

Die Struktur der Anwendung wird mithilfe des folgenden Diagramms erläutert:

![](introduction-images/overview-diagram.png "DependencyService Anwendungsstruktur")

### <a name="interface"></a>Interface

Die Schnittstelle, die Sie entwerfen, wird die Interaktion mit plattformspezifischen Features definiert. Seien Sie vorsichtig, wenn Sie entwickeln eine Komponente als eine Komponente oder ein Nuget-Paket gemeinsam genutzt werden. API-Entwurf kann stellen oder ein Paket unterbrechen. Das folgende Beispiel gibt an, eine einfache Schnittstelle für sprechen-Text, der zu sprechende Flexibilität bei der Angabe der Wörter ermöglicht, sondern bleibt die Implementierung, die für die einzelnen Plattformen angepasst werden:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implementierung pro Plattform

Sobald eine geeignete Benutzeroberfläche entworfen wurde, muss diese Schnittstelle in das Projekt für jede Plattform implementiert werden, die das Ziel. Z. B. die folgende Klasse implementiert die `ITextToSpeech` Schnittstelle unter iOS:

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

Jede Implementierung der Schnittstelle registriert werden muss `DependencyService` mit einem Metadatenattribut. Der folgende Code registriert die Implementierung für iOS:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Technologien kombinieren, sieht die plattformspezifische Implementierung wie folgt aus:

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

Hinweis:, die auf Namespace-Ebene, nicht auf Klassenebene ist die Registrierung erfolgt.

#### <a name="universal-windows-platform-net-native-compilation"></a>Universelle Windows-Plattform .NET Native-Kompilierung

Führen Sie die UWP-Projekte, die die .NET Native-Kompilierung-Option verwenden sollten eine [etwas andere Konfiguration](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) beim Initialisieren von Xamarin.Forms. .NET native-Kompilierung erfordert etwas andere Registrierung auch für Abhängigkeitsdienste.

In der **"App.Xaml.cs"** Datei, manuell registrieren jedes Abhängigkeitsdienst definiert, in der UWP-Projekt mithilfe der `Register<T>` Methode, wie unten gezeigt:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Hinweis: die manuelle Registrierung mit `Register<T>` ist nur wirksam, die im Release-builds mithilfe von .NET Native-Kompilierung. Wenn Sie diese Zeile auslassen, Debug-Builds weiterhin funktionieren, aber Releasebuilds schlägt fehl, zum Laden des Dependency-Diensts.

### <a name="call-to-dependencyservice"></a>Aufrufen, um DependencyService

Nachdem das Projekt mit der eine allgemeine Schnittstelle und Implementierungen für jede Plattform eingerichtet wurde, verwenden Sie `DependencyService` um die richtige Implementierung bei Laufzeit abzurufen:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` sehen Sie die richtige Implementierung der Schnittstelle `T`.

### <a name="solution-structure"></a>Lösungsstruktur

Die [UsingDependencyService Beispielprojektmappe](https://developer.xamarin.com/samples/UsingDependencyService/) unten für iOS und Android, durch die oben beschriebenen codeänderungen markiert ist.

 [![iOS und Android-Lösung](introduction-images/solution-sml.png "DependencyService Beispiel Projektmappenstruktur")](introduction-images/solution.png#lightbox "DependencyService Beispiel Lösungsstruktur")

> [!NOTE]
> Sie **müssen** eine Implementierung in jedem plattformprojekt bereitstellen. Wenn keine Implementierung der Schnittstelle registriert ist, und klicken Sie dann die `DependencyService` kann nicht aufgelöst werden die `Get<T>()` Methode zur Laufzeit.

## <a name="related-links"></a>Verwandte Links

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
