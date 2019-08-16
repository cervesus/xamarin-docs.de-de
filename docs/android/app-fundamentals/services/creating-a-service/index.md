---
title: Erstellen eines Diensts
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: d5b3f084be7adc664dcb52342af617788f4dde48
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526222"
---
# <a name="creating-a-service"></a>Erstellen eines Diensts

Xamarin. Android-Dienste müssen zwei unantastbare Regeln für Android-Dienste einhalten:

* Sie müssen den [`Android.App.Service`](xref:Android.App.Service)erweitern.
* Sie müssen mit dem [`Android.App.ServiceAttribute`](xref:Android.App.ServiceAttribute)versehen werden.

Eine weitere Anforderung für Android-Dienste besteht darin, dass Sie in der Datei " **androidmanifest. XML** " registriert werden müssen und einen eindeutigen Namen erhalten. Xamarin. Android registriert den Dienst bei der Buildzeit automatisch im Manifest, wobei das erforderliche XML-Attribut verwendet wird.

Dieser Code Ausschnitt ist das einfachste Beispiel für das Erstellen eines Dienstanbieter in xamarin. Android, der diese beiden Anforderungen erfüllt:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Zum Zeitpunkt der Kompilierung registriert xamarin. Android den Dienst, indem er das folgende XML-Element in " **androidmanifest. XML** " einfügt (Beachten Sie, dass xamarin. Android einen zufälligen Namen für den Dienst generiert hat):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Es ist möglich, einen Dienst gemeinsam mit anderen Android-Anwendungen zu verwenden, indem Sie ihn _exportieren_ . Dies wird erreicht, indem die `Exported` -Eigenschaft `ServiceAttribute`auf festgelegt wird. Wenn Sie einen Dienst exportieren, `ServiceAttribute.Name` sollte die-Eigenschaft auch so festgelegt werden, dass Sie einen aussagekräftigen öffentlichen Namen für den Dienst bereitstellt. Dieser Code Ausschnitt veranschaulicht, wie Sie einen Dienst exportieren und benennen:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Das Element " **androidmanifest. XML** " für diesen Dienst sieht dann in etwa wie folgt aus:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Dienste verfügen über einen eigenen Lebenszyklus mit Rückruf Methoden, die beim Erstellen des Diensts aufgerufen werden. Welche Methoden aufgerufen werden, richtet sich nach dem Diensttyp. Ein gestarteter Dienst muss verschiedene Lebenszyklus Methoden implementieren als ein gebundener Dienst, während ein Hybrid Dienst die Rückruf Methoden für einen gestarteten Dienst und einen gebundenen Dienst implementieren muss. Diese Methoden sind alle Member der `Service` -Klasse. die Art und Weise, wie der Dienst gestartet wird, bestimmt, welche Lebenszyklus Methoden aufgerufen werden. Diese Lebenszyklus Methoden werden später ausführlicher erläutert.

Standardmäßig wird ein Dienst im gleichen Prozess wie eine Android-Anwendung gestartet. Es ist möglich, einen Dienst in einem eigenen Prozess zu starten, indem `ServiceAttribute.IsolatedProcess` die-Eigenschaft auf "true" festgelegt wird:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Der nächste Schritt besteht darin, zu untersuchen, wie Sie einen Dienst starten und dann fortfahren, um zu erfahren, wie die drei verschiedenen Typen von Diensten implementiert werden.

> [!NOTE]
> Ein Dienst wird im UI-Thread ausgeführt. Wenn also eine Arbeit ausgeführt werden soll, die die Benutzeroberfläche blockiert, muss der Dienst Threads zum Ausführen der Arbeit verwenden.

## <a name="starting-a-service"></a>Starten eines Dienstanbieter

Die einfachste Möglichkeit, einen Dienst in Android zu starten, besteht darin, `Intent` einen zu senden, der Metadaten enthält, um zu ermitteln, welcher Dienst gestartet werden sollte. Es gibt zwei verschiedene Arten von Intents, die zum Starten eines dienstanzen verwendet werden können:

- **Explizite Absicht** Eine _explizite Absicht_ gibt genau an, welcher Dienst verwendet werden soll, um eine bestimmte Aktion abzuschließen. &ndash; Eine explizite Absicht kann als Buchstabe angesehen werden, der über eine bestimmte Adresse verfügt. Android leitet die Absicht an den Dienst weiter, der explizit identifiziert wird. Dieser Code Ausschnitt ist ein Beispiel für die Verwendung einer expliziten Absicht, einen Dienst `DownloadService`mit dem Namen zu starten:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

- **Implizite Absicht** &ndash; Diese Art der Absicht identifiziert lose den der Aktion, die der Benutzer ausführen möchte, aber der genaue Dienst zum Abschluss dieser Aktion ist unbekannt. Eine implizite Absicht kann als Buchstabe angesehen werden, der adressiert wird, dass es sich um ein Problem handelt.
    Android prüft den Inhalt der Absicht und ermittelt, ob ein vorhandener Dienst vorhanden ist, der mit der Absicht übereinstimmt.

    Ein beabsichtigter _Filter_ wird verwendet, um die implizite Absicht mit einem registrierten Dienst abzugleichen. Ein Intent Filter ist ein XML-Element, das der Datei " **androidmanifest. XML** " hinzugefügt wird, die die erforderlichen Metadaten enthält, um einen Dienst mit einer impliziten Absicht abzugleichen.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Wenn für Android mehr als eine mögliche Entsprechung für eine implizite Absicht vorliegt, wird der Benutzer möglicherweise aufgefordert, die Komponente auszuwählen, um die Aktion zu behandeln:

![Bildschirm Abbildung eines mehrdeutigkeits Dialogfelds](images/creating-a-service-01.png "Bildschirm Abbildung eines mehrdeutigkeits Dialogfelds")

> [!IMPORTANT]
> Ab Android 5,0 (AP-Level 21) kann eine implizite Absicht nicht verwendet werden, um einen Dienst zu starten.

Wenn möglich, sollten Anwendungen explizite Intents verwenden, um einen Dienst zu starten. Bei einer impliziten Absicht wird nicht angefordert, dass ein bestimmter Dienst &ndash; gestartet wird. es handelt sich um eine Anforderung für einen Dienst, der auf dem Gerät installiert ist, um die Anforderung zu verarbeiten. Diese mehrdeutige Anforderung kann dazu führen, dass der falsche Dienst die Anforderung verarbeitet oder eine andere APP unnötig startet (was den Druck für Ressourcen auf dem Gerät erhöht).

Die Art und Weise, wie die Absicht verteilt wird, hängt von der Art des Dienstanbieter ab und wird später in den Anleitungen zu den einzelnen Dienst Typen ausführlicher erläutert.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Erstellen eines Intent-Filters für implizite Intents

Um einen Dienst einer impliziten Absicht zuzuordnen, muss eine Android-App metadatendaten bereitstellen, um die Funktionen des Dienstanbieter zu identifizieren. Diese Metadaten werden von _beabsichtigten Filtern_bereitgestellt. Beabsichtigte Filter enthalten einige Informationen, z. b. eine Aktion oder einen Datentyp, die in einer Absicht vorhanden sein müssen, einen Dienst zu starten. In xamarin. Android wird der Intent-Filter in **androidmanifest. XML** registriert, indem ein Dienst mit dem [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute)versehen wird. Der folgende Code fügt z. b. einen Intent-Filter mit einer zugeordneten Aktion von `com.xamarin.DemoService`hinzu:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Dies führt dazu, dass in der Datei &ndash; " **androidmanifest. XML** " ein Eintrag enthalten ist, der mit der Anwendung in einer Weise analog zum folgenden Beispiel verpackt wird:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Mit den Grundlagen eines xamarin. Android-Diensts können wir die verschiedenen Untertypen von Diensten ausführlicher untersuchen.


## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](xref:Android.App.Service)
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute)
- [Android.App.Intent](xref:Android.Content.Intent)
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)
