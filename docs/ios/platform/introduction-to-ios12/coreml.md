---
title: Core ml 2 in xamarin. IOS
description: In diesem Dokument werden die Updates für Core ml beschrieben, die als Teil von IOS 12 verfügbar sind. Insbesondere werden Leistungsverbesserungen im Zusammenhang mit der neuen Batch-Vorhersage-API untersucht.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/15/2018
ms.openlocfilehash: 7e22a095a51c2dca749cb1b17807a061d066d0c4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290303"
---
# <a name="core-ml-2-in-xamarinios"></a>Core ml 2 in xamarin. IOS

Core ml ist eine Machine Learning-Technologie, die unter IOS, macOS, tvos und watchos verfügbar ist. Sie ermöglicht apps das Treffen von Vorhersagen auf Grundlage von Machine Learning-Modellen.

In IOS 12 umfasst Core ml eine Batch Processing-API. Diese API macht Core ml effizienter und bietet Leistungsverbesserungen in Szenarios, in denen ein Modell verwendet wird, um eine Sequenz von Vorhersagen zu erstellen.

## <a name="sample-app-marshabitatcoremltimer"></a>Beispiel-App: MarsHabitatCoreMLTimer

Um Batch Vorhersagen mit Core ml zu veranschaulichen, sehen Sie sich die [marshabitatcoremltimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer) -Beispiel-APP an. Dieses Beispiel verwendet ein Core ml-Modell, das trainiert wird, um die Kosten für das Aufbauen eines Lebensraums auf Mars basierend auf verschiedenen Eingaben vorherzusagen: die Anzahl der Sonnen Bereiche, die Anzahl von Gewächshäusern und die Anzahl der Hektar.

Die Code Ausschnitte in diesem Dokument stammen aus diesem Beispiel.

## <a name="generate-sample-data"></a>Generieren von Beispieldaten

In `ViewController` `ViewDidLoad` ruft die`LoadMLModel`-Methode der Beispiel-App auf, die das enthaltene Core ml-Modell lädt:

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

Dann erstellt die Beispiel-APP 100.000 `MarsHabitatPricerInput` -Objekte, die als Eingabe für sequenzielle Core ml-Vorhersagen verwendet werden. Für jedes generierte Beispiel wird ein zufälliger Wert für die Anzahl der Sonnen Bereiche, die Anzahl der Gewächshäuser und die Anzahl der Hektar festgelegt:

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

Wenn Sie auf eine der drei Schaltflächen der APP tippen, werden zwei Sequenzen von vorher `for` sagen ausgeführt: eine mit einer Schleife und `GetPredictions` eine andere mit der neuen Batch-Methode, die in IOS 12 eingeführt wurde:

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

Die `for` Schleifen Version des Tests durchläuft die angegebene Anzahl von Eingaben, wobei für jeden aufgerufen [`GetPrediction`](xref:CoreML.MLModel.GetPrediction*) und das Ergebnis verworfen wird. Die Methode gibt an, wie lange es dauert, die Vorhersagen zu treffen:

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

## <a name="getpredictions-new-batch-api"></a>Getvorhersagen (neue Batch-API)

In der Batch Version des Tests wird ein `MLArrayBatchProvider` Objekt aus dem Eingabe Array erstellt (da es sich hierbei um einen erforderlichen Eingabe `GetPredictions` Parameter für die Methode handelt).[`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
ein Objekt, das verhindert, dass Vorhersage Berechnungen auf die CPU beschränkt werden, `GetPredictions` und verwendet die API zum Abrufen der Vorhersagen, wobei das Ergebnis erneut verworfen wird:

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

Sowohl auf dem Simulator als auch `GetPredictions` auf dem Gerät ist schneller abgeschlossen als die Schleifen basierten Core ml-Vorhersagen.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App – marshabitatcoremltimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
- [Neues in Core ml, Teil 1 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [Neues in Core ml, Teil 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Einführung in Core ml in xamarin. IOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [Core ml (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [Arbeiten mit Core ml-Modellen](https://developer.apple.com/machine-learning/build-run-models/)
