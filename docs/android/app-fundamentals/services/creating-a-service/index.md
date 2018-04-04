---
title: Erstellen eines Diensts
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/01/2018
ms.openlocfilehash: d1e0fdb1c4b159b6db283d7b9b3be673b73a0ee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-service"></a>Erstellen eines Diensts

Xamarin.Android Dienste müssen zwei unverletzliche Regeln von Android Services unterliegen:

* Sie müssen erweitern die [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* Sie müssen ergänzt werden, mit der [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Eine andere Anforderung Android Dienste ist, dass sie in registriert werden müssen die **AndroidManifest.xml** außerdem einen eindeutigen Namen angegeben werden. Xamarin.Android wird den Dienst automatisch im Manifest während des Buildvorgangs mit dem erforderlichen XML-Attribut registriert.

Dieser Codeausschnitt ist das einfachste Beispiel zum Erstellen eines Diensts in Xamarin.Android, die diese beiden Anforderungen erfüllt:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Zum Zeitpunkt der Kompilierung wird Xamarin.Android den Dienst registrieren, indem Sie Räumen das folgende XML-Element in **AndroidManifest.xml** (Beachten Sie, dass Xamarin.Android einen zufälligen Namen für den Dienst generiert):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Es ist möglich, einen Dienst gemeinsam mit anderen Android-Anwendungen von _exportieren_ es. Dies geschieht durch Festlegen der `Exported` Eigenschaft auf die `ServiceAttribute`. Beim Exportieren eines Diensts die `ServiceAttribute.Name` Eigenschaft sollte auch festgelegt werden, um einen öffentlichen aussagekräftigen Namen für den Dienst bereitzustellen. Dieser Codeausschnitt veranschaulicht, wie exportieren, und benennen Sie einen Dienst:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Die **AndroidManifest.xml** Element für diesen Dienst wird dann wie folgt aussehen:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Dienste verfügen über eigene Lebenszyklus bei Rückrufmethoden, die aufgerufen werden, da der Dienst erstellt wird. Genau welche Methoden aufgerufen werden, hängt von den Typ des Diensts ab. Ein gestarteten Dienst muss verschiedene Lebenszyklusmethoden als Hintergrunddienst gebundenen implementieren, während ein hybriddienst für eine gestarteter Dienst und einen Dienst gebundene Rückrufmethoden implementieren muss. Diese Methoden sind alle Mitglieder der `Service` -Klasse; aufgerufen werden, welche Lebenszyklusmethoden bestimmt wie der Dienst gestartet wurde. Diese Lebenszyklusmethoden werden später im Detail besprochen.

Standardmäßig wird ein Dienst im gleichen Prozess wie einer Android-Anwendung gestartet. Es ist möglich, zum Starten eines Diensts in einem eigenen Prozess durch Festlegen der `ServiceAttribute.IsolatedProcess` Eigenschaft auf "true":

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Der nächste Schritt besteht, überprüfen Sie zum Starten eines Diensts, und fahren Sie dann zum Implementieren der drei verschiedenen Arten von Diensten zu überprüfen.

> [!NOTE]
> Ein Dienst ausgeführt wird, auf dem UI-Thread, daher ist keine Arbeiten durchgeführt werden, die blockiert, die der Benutzeroberflächenautomatisierungs des Diensts Threads verwendet werden muss, die Aufgaben auszuführen.

## <a name="starting-a-service"></a>Starten eines Diensts

Die grundlegendste Möglichkeit zum Starten eines Diensts in Android ist beim Verteilen einer `Intent` enthält Metadaten, um zu bestimmen, welcher Dienst gestartet werden soll. Es gibt zwei verschiedene Stile Intents, die zum Starten eines Diensts verwendet werden können:

-   **Explizite Absicht** &ndash; ein _explizite Absicht_ erkennt genau welchen Dienst verwendet werden soll, um eine bestimmte Aktion abzuschließen. Explizite Priorität kann der als Buchstabe betrachtet werden, die eine bestimmte Adresse verfügt. Android wird die Absicht an den Dienst weiterzuleiten, die explizit angegeben wird. Dieser Codeausschnitt ist ein Beispiel der Verwendung von expliziten Priorität zum Starten eines Diensts aufgerufen `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Implizite Absicht** &ndash; diese Art der Absicht lose identifiziert die Aktion, die ausgeführt werden soll, aber die genauen Service zum Abschließen dieser Aktion ist unbekannt. Implizite Priorität kann als ein Buchstabe, der "To Whom It Mai relevant..." adressiert betrachtet werden.
    Android Untersuchen des Inhalts der Zweck und Determin, wenn es ein vorhandener Dienst, der übereinstimmt, der die Absicht ist.

    Ein _beabsichtigte Filter_ wird verwendet, um bei der Zuordnung der implizite Absicht mit einem registrierten Dienst helfen. Ein Beabsichtigter Filter ist ein XML-Element, das hinzugefügt wird **AndroidManifest.xml** enthält die erforderlichen Metadaten können Sie einen Dienst mit der impliziten Priorität entsprechen.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Wenn Android mehr als eine mögliche Entsprechung für eine implizite Absicht verfügt, kann er den Benutzer zur Auswahl der Komponente, die Aktion zu behandeln Fragen:

![Screenshot eines Dialogfelds zur Klärung](images/creating-a-service-01.png "Screenshot eines Dialogfelds zur Klärung")

> [!IMPORTANT]
> Ab Android 5.0 (AP-Ebene 21) kann eine implizite Absicht zum Starten eines Diensts verwendet werden.

Wenn möglich, sollten Anwendungen explizite Intents verwenden, um einen Dienst zu starten. Implizite Priorität fordert nicht für einen bestimmten Dienst ans &ndash; ist eine Anforderung für einen Dienst auf dem Gerät installiert, um die Anforderung zu verarbeiten. Diese mehrdeutig Anforderung kann dazu führen, den falschen Dienst Behandlung der Anforderung oder eine andere app unnötig starten (wodurch der Druck nach Ressourcen auf dem Gerät erhöht wird).

Wie die Absicht verteilt ist hängt vom Typ des Diensts und wird später in den Handbüchern, die für jeden Dienst spezifische ausführlicher besprochen werden.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Erstellen einen beabsichtigten Filter für implizite Intents

Um einen Dienst mit einem impliziten Absicht zuzuordnen, muss eine Android-app einige Metadaten zum Identifizieren der Funktionen des Diensts angeben. Diese Metadaten werden vom bereitgestellt _beabsichtigte Filter_. Beabsichtigte Filter enthalten einige Informationen, z. B. eine Aktion oder einen Typ von Daten, die in einer Absicht zum Starten eines Diensts vorhanden sein müssen. In Xamarin.Android, wird der beabsichtigte Filter in registriert **AndroidManifest.xml** werden, indem einen Dienst mit der [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Der folgende Code fügt z. B. einen beabsichtigten Filter mit der Aktion zugeordnete `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Dadurch wird ein Eintrag in eingeschlossen werden die **AndroidManifest.xml** Datei &ndash; einen Eintrag, der mit der Anwendung auf eine Weise, die analog zu den im folgenden Beispiel wird verpackt wird:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Betrachten wir mit den Grundlagen eines Diensts Xamarin.Android aus dem Weg, die unterschiedliche Untertypen von Diensten im Detail.


## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
