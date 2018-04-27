---
title: Einführung in DependencyService
description: Verständnis der Funktionsweise von DependencyService auf systemeigene Plattformfunktionen Zugriff
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 88821c5315fc338b5195e42ea4b2bc3e648e6ea1
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="introduction-to-dependencyservice"></a>Einführung in DependencyService

## <a name="overview"></a>Übersicht

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) ermöglicht apps Clientplattform-spezifische Funktionen von freigegebenem Code aufrufen. Diese Funktionen ermöglichen das Xamarin.Forms-apps, nichts zu tun, die eine systemeigene app durchführen können.

`DependencyService` ist ein Abhängigkeitskonfliktlöser. In der Praxis ist eine Schnittstelle definiert und `DependencyService` findet die korrekte Implementierung dieser Schnittstelle aus den verschiedenen plattformprojekten.

## <a name="how-dependencyservice-works"></a>Funktionsweise von DependencyService

Xamarin.Forms-apps benötigen drei Komponenten mit `DependencyService`:

- **Schnittstelle** &ndash; die erforderliche Funktionalität wird durch eine Schnittstelle im freigegebenen Code definiert.
- **Implementierung pro Plattform** &ndash; Klassen, die die Schnittstelle implementieren müssen auf jeder plattformprojekt hinzugefügt werden.
- **Registrierung** &ndash; jede implementierende Klasse muss registriert werden, mit `DependencyService` über ein Metadatenattribut. Registrierung ermöglicht `DependencyService` finden die implementierende Klasse, und stellen Sie es zur Laufzeit anstelle der Schnittstelle bereit.
- **Aufruf von DependencyService** &ndash; freigegebene Code muss explizit aufrufen `DependencyService` für Implementierungen der-Schnittstelle um Unterstützung bitten.

Beachten Sie, dass die Implementierungen für jede Plattform-Projekts in der Projektmappe bereitgestellt werden müssen. Plattformprojekte ohne Implementierungen werden zur Laufzeit fehlschlagen.

Die Struktur der Anwendung wird durch Folgendes Diagramm erläutert:

![](introduction-images/overview-diagram.png "DependencyService Anwendungsstruktur")

### <a name="interface"></a>Interface

Die Schnittstelle, die Sie entwerfen definieren Interaktion mit Clientplattform-spezifische Funktionen. Seien Sie vorsichtig, wenn Sie eine Komponente als eine Komponente oder ein NuGet-Paket die weiterzuleitenden entwickeln. API-Entwurf kann stellen oder ein Paket unterbrechen. Das folgende Beispiel gibt eine einfache Schnittstelle für sprechen, die bewirkt, dass die Implementierung für jede Plattform angepasst werden, ermöglicht Flexibilität bei der Angabe der Wörter zu sprechenden Text an:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implementierung plattformspezifischen

Sobald eine geeignete Benutzeroberfläche entworfen wurde, muss diese Schnittstelle in das Projekt für jede Plattform implementiert werden, die das Ziel. Z. B. die folgende Klasse implementiert die `ITextToSpeech` Schnittstelle auf iOS:

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

Jede Implementierung der Schnittstelle muss registriert werden `DependencyService` mit einem Metadatenattribut. Im folgende Code wird die Implementierung für iOS registriert:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Gesamtbild, sieht die Clientplattform-spezifische Implementierung:

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

Hinweis:, die die Registrierung auf Namespace-Ebene nicht auf Klassenebene ausgeführt wird.

#### <a name="universal-windows-platform-net-native-compilation"></a>Universelle Windows Plattform .NET Native Kompilierung

Führen Sie die uwp-Projekte, die die .NET Native Kompilierung-Option verwenden, sollte ein [etwas andere Konfiguration](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) Xamarin.Forms zu initialisieren. .NET native Kompilierung werden auch geringfügig Registrierung für Abhängigkeitsdienste erforderlich.

In der **App.xaml.cs** Datei, die manuell registrieren jedes Abhängigkeitsdienst definiert im uwp-Projekt mithilfe der `Register<T>` Methode, wie unten dargestellt:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Hinweis: die manuelle Registrierung mit `Register<T>` ist nur wirksam, die in Version builds mithilfe von .NET Native-Kompilierung. Wenn Sie diese Zeile weglassen, Debug-Builds weiterhin funktioniert, aber Releasebuilds schlägt fehl, Abhängigkeitsdienst geladen.

### <a name="call-to-dependencyservice"></a>Aufruf von DependencyService

Nachdem das Projekt mit der Implementierung für jede Plattform und eine gemeinsame Schnittstelle eingerichtet wurde, verwenden Sie `DependencyService` um die richtige Implementierung bei Laufzeit abzurufen:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` findet die korrekte Implementierung der Schnittstelle `T`.

### <a name="solution-structure"></a>Projektmappenstruktur

Die [UsingDependencyService Beispielprojektmappe](https://developer.xamarin.com/samples/UsingDependencyService/) unten für iOS und Android, mit der oben beschriebenen Änderungen am Code hervorgehoben ist.

 [![iOS und Android-Lösung](introduction-images/solution-sml.png "DependencyService Beispiel Projektmappenstruktur")](introduction-images/solution.png#lightbox "DependencyService Beispiel Projektmappenstruktur")

> [!NOTE]
> Sie **müssen** eine Implementierung in jedem plattformprojekt bereitstellen. Wenn keine Implementierung registriert ist, und klicken Sie dann die `DependencyService` konnte nicht aufgelöst werden die `Get<T>()` Methode zur Laufzeit.


## <a name="related-links"></a>Verwandte Links

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
