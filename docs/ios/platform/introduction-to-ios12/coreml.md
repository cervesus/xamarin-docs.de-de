---
title: Core ML 2 in Xamarin.iOS
description: Dieses Dokument beschreibt die Updates von Core ML verfügbar als Teil der e/a 12. Insbesondere überprüft er leistungsverbesserungen, die der neue batchvorhersage-API zugeordnet.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/15/2018
ms.openlocfilehash: 50d59f0b6ff2133c5870d84a1d740547768116e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61398844"
---
# <a name="core-ml-2-in-xamarinios"></a>Core ML 2 in Xamarin.iOS

Core ML ist eine Machine learning-Technologie, die auf iOS, MacOS, TvOS und WatchOS verfügbar. Sie ermöglicht apps, die zum treffen von Vorhersagen basierend auf Machine learning-Modelle.

In iOS 12 enthält Core ML für eine API-Batchverarbeitung. Diese API wird von Core ML effizienter und enthält leistungsverbesserungen in Szenarien, in denen ein Modell verwendet wird, um eine Folge von Vorhersagen zu machen.

## <a name="sample-app-marshabitatcoremltimer"></a>Beispiel-app: MarsHabitatCoreMLTimer

Um Batch-Prognosen mit Core ML zu demonstrieren, sehen Sie sich die [MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer) Beispiel-app. Dieses Beispiel verwendet einen Core ML-Modell trainiert, um die Kosten der Erstellung einer Habitat auf dem Mars, Vorhersagen auf Basis verschiedener Eingaben: Anzahl der nachfragegeboten, Anzahl der Treibhäusern und Anzahl von morgen.

Die Codeausschnitte in diesem Dokument stammen aus diesem Beispiel.

## <a name="generate-sample-data"></a>Generieren von Beispieldaten

In `ViewController`, der Beispiel-app `ViewDidLoad` Methodenaufrufe `LoadMLModel`, lädt das enthalten Core ML-Modell:

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

Die Beispiel-app erstellt dann 100.000 `MarsHabitatPricerInput` Objekte, die als Eingabe für sequenzielle Vorhersagen von Core ML verwendet. Jedes generierte Beispiel hat es sich um einen zufälligen Wert für die Anzahl der nachfragegeboten, die Anzahl der Treibhäusern und die Anzahl von morgen festgelegt:

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

Durch Tippen auf eine der drei Schaltflächen der app führt zwei Sequenzen von Vorhersagen: eins mit einer `for` Schleife und ein anderes mit dem neuen Batch `GetPredictions` Methode, die in iOS-12 eingeführt:

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## <a name="for-loop"></a>for-Schleife

Die `for` Schleife Version des Tests durchläuft naiv die angegebene Anzahl von Eingaben Aufrufen [ `GetPrediction` ](xref:CoreML.MLModel.GetPrediction*) für jeden und das Ergebnis wird verworfen. Die Methode Timeout an, wie lange es dauert, um die Vorhersagen zu treffen:

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="getpredictions-new-batch-api"></a>GetPredictions (neues Batch-API)

Die Batch-Version des Tests erstellt eine `MLArrayBatchProvider` Objekt aus dem Eingabearray (da dies ein erforderlicher Eingabeparameter für ist das `GetPredictions` Methode), erstellt ein [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
Objekt, das verhindert, dass Vorhersage Berechnungen für die CPU beschränkt und verwendet die `GetPredictions` -API, um die Vorhersagen, verwerfen erneut das Ergebnis abzurufen:

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="results"></a>Ergebnisse

Sowohl für Simulator Gerät `GetPredictions` abgeschlossen ist schneller als die schleifenbasierte Core ML-Vorhersagen.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer)
- [Neuerungen in Core ML, Teil 1 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [Neuerungen in Core ML, Teil 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Einführung in Core ML in Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [Core ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [Verwenden von Core ML-Modellen](https://developer.apple.com/machine-learning/build-run-models/)
