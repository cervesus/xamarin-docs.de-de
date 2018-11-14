---
title: 'Xamarin.Essentials: Einstellungen'
description: In diesem Dokument wird die Klasse „Preferences“ in Xamarin.Essentials beschrieben, die Anwendungseinstellungen in einem Schlüsselwertspeicher speichert. Behandelt werden die Verwendung der Klasse und die Datentypen, die gespeichert werden können.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: d50fe3853ab87d5bc14ac15a442140218a1b0fe0
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617552"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: Einstellungen

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **Preferences** unterstützt das Speichern von Anwendungseinstellungen in einem Schlüsselwertspeicher.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-preferences"></a>Verwenden von Einstellungen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

So speichern Sie in den Einstellungen einen Wert für einen bestimmten _Schlüssel_:

```csharp
Preferences.Set("my_key", "my_value");
```

So rufen Sie einen Wert aus den Einstellungen oder, sofern kein Wert festgelegt ist, einen Standardwert ab:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

So entfernen Sie den _Schlüssel_ aus den Einstellungen:

```csharp
Preferences.Remove("my_key");
```

So entfernen Sie alle Einstellungen:

```csharp
Preferences.Clear();
```

Neben diesen Methoden wird jeweils ein optionaler `sharedName` aufgenommen, der zum Erstellen zusätzlicher Container für die Einstellung verwendet werden kann. Die Besonderheiten bei der plattformspezifischen Implementierung sind weiter unten beschrieben.

## <a name="supported-data-types"></a>Unterstützte Datentypen

Die folgenden Datentypen werden in der Klasse **Preferences** unterstützt:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>Implementierungsdetails

`DateTime`-Werte werden im 64-Bit-Binärformat (lange ganze Zahl) mithilfe von zwei durch die Klasse `DateTime` definierten Methoden gespeichert: Die Methode [`ToBinary`](xref:System.DateTime.ToBinary) dient zum Codieren des `DateTime`-Werts, und die Methode [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) decodiert den Wert. In der Dokumentation zu diesen Methoden finden Sie eventuell notwendige Anpassungen, die an decodierten Werten vorgenommen werden müssen, wenn ein `DateTime`-Wert gespeichert wird, der kein UTC-Wert (Coordinated Universal Time) ist.

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="androidtabandroid"></a>[Android](#tab/android)

Alle Daten werden in [freigegebenen Einstellungen](https://developer.android.com/training/data-storage/shared-preferences.html) gespeichert. Wenn kein `sharedName` angegeben ist, werden die standardmäßigen freigegebenen-Einstellungen verwendet. Andernfalls wird der Name zum Abrufen einer **privaten** freigegebenen Einstellung mit dem angegebenen Namen verwendet.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/xamarin/ios/app-fundamentals/user-defaults) wird zum Speichern von Werten auf iOS-Geräten verwendet. Wenn kein `sharedName` angegeben ist, werden die `StandardUserDefaults` verwendet. Andernfalls wird der Name zum Erstellen eines neuen `NSUserDefaults` mit dem angegebenen Namen verwendet, der als `NSUserDefaultsType.SuiteName` verwendet wird.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) wird zum Speichern von Werten auf dem Gerät verwendet. Wenn kein `sharedName` angegeben ist, werden die `LocalSettings` verwendet. Andernfalls wird der Name zum Erstellen eines neuen Containers innerhalb von `LocalSettings` verwendet.

--------------

## <a name="persistence"></a>Persistenz

Das Deinstallieren der Anwendung führt dazu, dass alle _Einstellungen_ entfernt werden. Hierbei gibt es eine Ausnahme für Apps, die unter Android 6.0 (API-Ebene 23) oder höher ausgeführt werden (bzw. auf dieses Betriebssystem abzielen) und die [__automatische Sicherung__](https://developer.android.com/guide/topics/data/autobackup) verwenden. Dieses Feature ist standardmäßig aktiviert und sichert App-Daten einschließlich der __freigegebenen Einstellungen__, die von der **Preferences**-API genutzt werden. Sie können dieses Feature deaktivieren, indem Sie die in der Google-[Dokumentation](https://developer.android.com/guide/topics/data/autobackup) beschriebenen Schritte ausführen.

## <a name="limitations"></a>Einschränkungen

Beachten Sie beim Speichern einer Zeichenfolge, dass diese API zum Speichern kleiner Textmengen konzipiert wurde.  Die Leistung nimmt ggf. ab, wenn Sie versuchen, große Textmengen zu speichern.

## <a name="api"></a>API

- [Preferences-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Preferences-API-Dokumentation](xref:Xamarin.Essentials.Preferences)
