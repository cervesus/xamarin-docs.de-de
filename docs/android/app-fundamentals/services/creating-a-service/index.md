---
title: Erstellen eines Diensts
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 24d86827ab93dcf7dfc4da39c4a03a0a2805f332
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107429"
---
# <a name="creating-a-service"></a>Erstellen eines Diensts

Xamarin.Android-Dienste müssen zwei unverletzliche Regeln von Android-Dienste erfüllt sein:

* Sie müssen erweitern die [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* Sie müssen mit ergänzt werden, die [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Ein weiteres Erfordernis von Android-Dienste ist, dass sie in registriert werden, müssen die **"androidmanifest.xml"** und erhält einen eindeutigen Namen. Xamarin.Android wird der Dienst zum Zeitpunkt der Erstellung mit dem erforderlichen XML-Attribut automatisch in das Manifest registrieren.

Dieser Codeausschnitt ist das einfachste Beispiel für das Erstellen eines Diensts in Xamarin.Android ein, die diese beiden Anforderungen erfüllt:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Zum Zeitpunkt der Kompilierung wird Xamarin.Android den Dienst registrieren, indem Sie in den folgende XML-Element einfügen **"androidmanifest.xml"** (Beachten Sie, dass Xamarin.Android ein zufälliger Name für den Dienst generiert):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Es ist möglich, einen Dienst gemeinsam mit anderen Android-Anwendungen von _exportieren_ es. Dies geschieht durch Festlegen der `Exported` Eigenschaft für die `ServiceAttribute`. Wenn Sie einen Dienst exportieren die `ServiceAttribute.Name` Eigenschaft muss auch festgelegt werden, um einen öffentlichen aussagekräftigen Namen für den Dienst bereitzustellen. Dieser Codeausschnitt veranschaulicht, wie exportieren, und benennen Sie einen Dienst:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Die **"androidmanifest.xml"** -Element für diesen Dienst sieht dann etwa wie folgt:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Dienste verfügen über ihre eigenen Lebenszyklus mit Rückrufmethoden, die aufgerufen werden, da der Dienst erstellt wird. Genau die Methoden aufgerufen werden, hängt von den Typ des Diensts ab. Ein gestarteter Dienst muss andere Methoden als einen Dienst gebundene implementieren, während ein Hybrid-Diensts für einen Dienst gestartet und ein gebundener Dienst Rückrufmethoden implementieren muss. Diese Methoden sind alle Mitglieder der `Service` Klasse, die aufgerufen werden, welche Lebenszyklusmethoden bestimmt wie der Dienst gestartet wurde. Diese Methoden werden weiter unten ausführlicher erläutert.

Standardmäßig wird ein Dienst im gleichen Prozess wie eine Android-Anwendung gestartet. Es ist möglich, einen Dienst in einem eigenen Prozess starten, durch Festlegen der `ServiceAttribute.IsolatedProcess` Eigenschaft auf "true":

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Der nächste Schritt besteht, überprüfen Sie das Starten eines Diensts und uns dann zum Implementieren der drei verschiedene Arten von Diensten zu überprüfen.

> [!NOTE]
> Ein Dienst ausgeführt wird, auf den UI-Thread, sodass ist keine Arbeit sein ausgeführt, die die Benutzeroberfläche blockiert, der Dienst Threads verwenden muss, um die Arbeit zu erledigen.

## <a name="starting-a-service"></a>Starten eines Diensts

Die einfachste Möglichkeit zum Starten eines Diensts unter Android wird zum Verteilen einer `Intent` enthält Metadaten zur Identifizierung der Dienst gestartet werden soll. Es gibt zwei verschiedene Arten der Intent-Elemente, die zum Starten eines Diensts verwendet werden können:

-   **Expliziter Intent** &ndash; ein _expliziter Intent_ erkennt genau welcher Dienst verwendet werden soll, um eine bestimmte Aktion abzuschließen. Ein expliziter Intent kann als Buchstabe betrachtet werden, die eine bestimmte Adresse verfügt. Android wird die Absicht an den Dienst weiterzuleiten, die explizit angegeben wird. Dieser Codeausschnitt ist ein Beispiel der Verwendung expliziten Intent zum Starten eines Diensts wird aufgerufen, `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Impliziter Intent** &ndash; diese Art von Absicht lose identifiziert die Aktion an, dass der Benutzer ausführen möchte, aber die genaue Dienst zum Abschließen dieser Aktion unbekannt ist. Ein impliziter Intent-Objekt kann ein Buchstabe, der "To Whom It Mai Problem..." kommunikationsattribut betrachtet werden.
    Android wird untersuchen Sie den Inhalt über den Zweck und bestimmen, ob es ein vorhandener Dienst, der die Absicht entspricht.

    Ein _Zielfilter_ wird verwendet, um die impliziter Intent mit einem registrierten Dienst übereinstimmen. Ein intent-Filter ist ein XML-Element, das hinzugefügt wird **"androidmanifest.xml"** enthält die erforderlichen Metadaten können von einem Dienst mit impliziter Intent entsprechen.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Wenn Android mehr als eine mögliche Entsprechung für ein impliziter Intent enthält, kann dann es der Benutzer die Auswahl die Komponente, die Aktion zu behandeln Fragen:

![Screenshot des ein Dialogfeld zur Klärung](images/creating-a-service-01.png "Screenshot eines Dialogfelds zur mehrdeutigkeitsvermeidung")

> [!IMPORTANT]
> Ab Android 5.0 (AP-Ebene 21) kann ein impliziter Intent zum Starten eines Diensts verwendet werden.

Nach Möglichkeit sollten Anwendungen expliziter Intent-Elemente verwenden, um einen Dienst zu starten. Ein impliziter Intent fordert nicht für einen bestimmten Dienst zu &ndash; ist eine Anforderung für einen Dienst auf dem Gerät installiert, die die Anforderung zu verarbeiten. Diese Anforderung nicht eindeutig kann führen, in dem falschen Dienst Behandlung der Anforderung oder einer anderen app unnötig starten (wodurch den Druck für Ressourcen auf dem Gerät erhöht wird).

Wie die Absicht weitergeleitet wird, hängt von der Art des Diensts und erläutert im Detail weiter unten in den Leitfäden, die für jeden Dienst spezifische.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Erstellen eine Zielfilter für implizite Intents

Um einen Dienst ein impliziter Intent zuzuordnen, muss eine Android-app einige Metadaten zum Identifizieren der Funktionen des Diensts bereitstellen. Diese Metadaten erfolgt über _Beabsichtigter Filter_. Beabsichtigter Filter enthalten einige Informationen, z. B. eine Aktion oder einen Typ von Daten, die in ein Intent-Objekt zum Starten eines Diensts vorhanden sein müssen. In Xamarin.Android wird im Zielfilter registriert **"androidmanifest.xml"** werden, indem einen Dienst mit der [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Der folgende Code fügt z. B. einen beabsichtigten Filter mit der Aktion zugeordnete `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Dies führt zu einem Eintrag in einbezogen werden die **"androidmanifest.xml"** Datei &ndash; einen Eintrag, der mit der Anwendung auf eine Weise, die analog zu den im folgenden Beispiel wird verpackt wird:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Betrachten wir mit den Grundlagen eines Xamarin.Android-Diensts aus dem Weg die unterschiedliche Untertypen von Diensten im Detail.


## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
