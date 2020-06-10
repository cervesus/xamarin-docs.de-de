---
title: Aktivitätslebenszyklus
description: Aktivitäten sind ein grundlegender Baustein von Android-Anwendungen und können in einer Reihe unterschiedlicher Zustände vorhanden sein. Der Aktivitäts Lebenszyklus beginnt mit der Instanziierung und endet mit der Zerstörung und umfasst viele Zustände dazwischen. Wenn sich der Zustand einer Aktivität ändert, wird die entsprechende Lifecycle-Ereignismethode aufgerufen, die die Aktivität der bevorstehenden Zustandsänderung benachrichtigt und die Ausführung von Code für die Anpassung an diese Änderung zulässt. In diesem Artikel wird der Lebenszyklus von Aktivitäten untersucht, und es wird erläutert, wie sich eine Aktivität bei den einzelnen Zustandsänderungen als Teil einer gut verhaltenen, zuverlässigen Anwendung verhält.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: b9404e8a54899d3af6e6534fc9157e78139ca7d1
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568726"
---
# <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

_Aktivitäten sind ein grundlegender Baustein von Android-Anwendungen und können in einer Reihe unterschiedlicher Zustände vorhanden sein. Der Aktivitäts Lebenszyklus beginnt mit der Instanziierung und endet mit der Zerstörung und umfasst viele Zustände dazwischen. Wenn sich der Zustand einer Aktivität ändert, wird die entsprechende Lifecycle-Ereignismethode aufgerufen, die die Aktivität der bevorstehenden Zustandsänderung benachrichtigt und die Ausführung von Code für die Anpassung an diese Änderung zulässt. In diesem Artikel wird der Lebenszyklus von Aktivitäten untersucht, und es wird erläutert, wie sich eine Aktivität bei den einzelnen Zustandsänderungen als Teil einer gut verhaltenen, zuverlässigen Anwendung verhält._

## <a name="activity-lifecycle-overview"></a>Übersicht über Aktivitäts Lebenszyklus

Aktivitäten sind ein ungewöhnliches Programmier Konzept für Android. Bei der herkömmlichen Anwendungsentwicklung gibt es normalerweise eine statische Main-Methode, die zum Starten der Anwendung ausgeführt wird. Bei Android ist es jedoch anders. Android-Anwendungen können über jede registrierte Aktivität innerhalb einer Anwendung gestartet werden. In der Praxis verfügen die meisten Anwendungen nur über eine bestimmte Aktivität, die als Einstiegspunkt für die Anwendung angegeben wird. Wenn jedoch eine Anwendung abstürzt oder vom Betriebssystem beendet wird, kann das Betriebssystem versuchen, die Anwendung bei der letzten geöffneten Aktivität oder an einer beliebigen Stelle im vorherigen Aktivitäts Stapel neu zu starten.
Außerdem hält das Betriebssystem möglicherweise Aktivitäten an, wenn Sie nicht aktiv sind, und gibt Sie frei, wenn nicht genügend Arbeitsspeicher verfügbar ist. Es muss sorgfältig vorgegangen werden, damit die Anwendung ihren Zustand ordnungsgemäß wiederherstellen kann, wenn eine Aktivität neu gestartet wird. Dies gilt insbesondere dann, wenn diese Aktivität von Daten aus vorherigen Aktivitäten abhängig ist.

Der Aktivitäts Lebenszyklus wird als eine Sammlung von Methoden implementiert, die das Betriebssystem während des Lebenszyklus einer Aktivität aufruft. Diese Methoden ermöglichen es Entwicklern, die Funktionalität zu implementieren, die erforderlich ist, um die Anforderungen an die Zustands-und Ressourcenverwaltung Ihrer Anwendungen zu erfüllen.

Es ist äußerst wichtig, dass der Anwendungsentwickler die Anforderungen der einzelnen Aktivitäten analysiert, um zu bestimmen, welche Methoden durch den Aktivitäts Lebenszyklus implementiert werden müssen. Wenn dies nicht der Fall ist, kann dies zu Anwendungs Instabilität, abstürzen, Ressourcen-Bloat und möglicherweise sogar zugrunde liegenden Betriebssystem Instabilität führen.

In diesem Kapitel wird der Aktivitäts Lebenszyklus ausführlich erläutert, einschließlich:

- Aktivitäts Zustände
- Lebenszyklusmethoden
- Beibehalten des Zustands einer Anwendung

Dieser Abschnitt enthält auch eine exemplarische Vorgehensweise, die praktische Beispiele [für die effiziente](~/android/app-fundamentals/activity-lifecycle/saving-state.md) Speicherung des Zustands während des Aktivitäts Lebenszyklus bereitstellt. Am Ende dieses Kapitels sollten Sie einen Einblick in den Aktivitäts Lebenszyklus und seine Unterstützung in einer Android-Anwendung haben.

## <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

Der Android-Aktivitäts Lebenszyklus besteht aus einer Sammlung von Methoden, die innerhalb der Aktivitäts Klasse verfügbar gemacht werden und dem Entwickler ein Resource Management-Framework zur Verfügung stellen. Dieses Framework ermöglicht es Entwicklern, die Anforderungen an die eindeutige Zustands Verwaltung der einzelnen Aktivitäten innerhalb einer Anwendung zu erfüllen und die Ressourcenverwaltung ordnungsgemäß zu verarbeiten.

### <a name="activity-states"></a>Aktivitäts Zustände

Das Android-Betriebssystem unterscheidet Aktivitäten basierend auf deren Status. Dadurch können Android Aktivitäten ermitteln, die nicht mehr verwendet werden, und das Betriebssystem kann Speicher und Ressourcen freigeben. Das folgende Diagramm veranschaulicht die Zustände, die eine Aktivität während ihrer Lebensdauer durchlaufen kann:

[![Diagramm der Aktivitäts Zustände](images/image1-sml.png)](images/image1.png#lightbox)

Diese Zustände können wie folgt in vier Hauptgruppen unterteilt werden:

1. *Aktiv oder wird ausgeführt* &ndash; Aktivitäten werden als aktiv betrachtet oder ausgeführt, wenn Sie sich im Vordergrund befinden, auch bekannt als oberster Rand des Aktivitäts Stapels. Dies wird als die höchste Prioritäts Aktivität in Android angesehen und wird daher nur von dem Betriebssystem in Extremsituationen abgebrochen, z. b. wenn die Aktivität versucht, mehr Speicher zu verwenden, als auf dem Gerät verfügbar ist, da dies dazu führen könnte, dass die Benutzeroberfläche nicht mehr reagiert.

1. *Angehalten* &ndash; Wenn das Gerät in den Standbymodus wechselt oder eine Aktivität weiterhin sichtbar ist, aber teilweise durch eine neue, nicht vollständige oder transparente Aktivität ausgeblendet ist, wird die Aktivität als angehalten betrachtet. Angehaltene Aktivitäten sind immer noch aktiv, d. h., Sie behalten alle Zustands-und Element Informationen bei und bleiben an den Fenster-Manager angefügt. Dies wird als die zweite Aktivität mit der höchsten Priorität in Android angesehen und wird daher nur vom Betriebssystem abgebrochen, wenn durch das Beenden dieser Aktivität die Ressourcenanforderungen erfüllt werden, die erforderlich sind, um die aktive/aktive Aktivität stabil und reaktionsfähig zu halten.

1. *Beendet/Rück basiert* &ndash; Aktivitäten, die von einer anderen Aktivität vollständig verdeckt werden, werden als beendet oder im Hintergrund betrachtet.
    Beendete Aktivitäten versuchen immer noch, ihre Zustands-und Element Informationen so lange wie möglich beizubehalten, aber beendete Aktivitäten werden als niedrigste Priorität der drei Zustände betrachtet. Daher werden die Aktivitäten in diesem Zustand vom Betriebssystem zuerst abgebrochen, um die Ressourcenanforderungen von Aktivitäten mit höherer Priorität zu erfüllen.

1. *Neu gestartet* &ndash; Es ist möglich, dass eine Aktivität, die sich an einem beliebigen Ort befindet, im Lebenszyklus angehalten wird, damit Sie von Android aus dem Arbeitsspeicher entfernt wird. Wenn der Benutzer zur Aktivität zurück navigiert, muss er neu gestartet, im zuvor gespeicherten Zustand wieder hergestellt und dann dem Benutzer angezeigt werden.

### <a name="activity-re-creation-in-response-to-configuration-changes"></a>Neuerstellung von Aktivitäten als Reaktion auf Konfigurationsänderungen

Damit die Dinge komplizierter werden, löst Android einen weiteren Schraubendreher in der Mischung namens Konfigurationsänderungen aus. Konfigurationsänderungen sind schnelle Aktivitäts Zerstörung/Neuerstellungs Zyklen, die auftreten, wenn die Konfiguration einer Aktivität geändert wird, z. b. wenn das Gerät [gedreht](~/android/app-fundamentals/handling-rotation.md) wird (und die Aktivität im Quer-oder Hochformat neu erstellt werden muss), wenn die Tastatur angezeigt wird (und die Größe der Aktivität angezeigt wird), oder wenn das Gerät in einem Dock platziert wird.

Konfigurationsänderungen führen weiterhin zu denselben Aktivitäts Zustandsänderungen, die beim Beenden und Neustarten einer Aktivität auftreten würden. Um jedoch sicherzustellen, dass eine Anwendung reaktionsfähig ist und bei Konfigurationsänderungen gut funktioniert, ist es wichtig, dass Sie so schnell wie möglich behandelt werden. Daher verfügt Android über eine bestimmte API, die verwendet werden kann, um den Zustand während der Konfigurationsänderungen beizubehalten.
Dies wird später im Abschnitt Verwalten des [Zustands im gesamten Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) behandelt.

### <a name="activity-lifecycle-methods"></a>Aktivitäts Lebenszyklus-Methoden

Das-Android SDK und, das xamarin. Android-Framework, bietet ein leistungsfähiges Modell zum Verwalten des Zustands von Aktivitäten in einer Anwendung. Wenn sich der Zustand einer Aktivität ändert, wird die Aktivität vom Betriebssystem benachrichtigt, das bestimmte Methoden für diese Aktivität aufruft. Im folgenden Diagramm werden diese Methoden im Zusammenhang mit dem Aktivitäts Lebenszyklus veranschaulicht:

[![Flussdiagramm des Aktivitäts Lebenszyklus](images/image2-sml.png)](images/image2.png#lightbox)

Als Entwickler können Sie Zustandsänderungen behandeln, indem Sie diese Methoden in einer Aktivität überschreiben. Es ist jedoch wichtig zu beachten, dass alle Lebenszyklus Methoden im UI-Thread aufgerufen werden und das Betriebssystem nicht mehr auf der Benutzeroberfläche ausgeführt werden kann, z. b. durch Ausblenden der aktuellen Aktivität, Anzeigen einer neuen Aktivität usw. Daher sollte der Code in diesen Methoden so kurz wie möglich sein, um eine optimale Leistung einer Anwendung zu gewährleisten. Alle Tasks mit langer Ausführungszeit sollten in einem Hintergrund Thread ausgeführt werden.

Betrachten wir jede dieser Lebenszyklus Methoden und deren Verwendung:

#### <a name="oncreate"></a>OnCreate

[OnCreate](xref:Android.App.Activity.OnCreate*) ist die erste Methode, die aufgerufen wird, wenn eine Aktivität erstellt wird.
`OnCreate`wird immer überschrieben, um Start Initialisierungen auszuführen, die möglicherweise für eine Aktivität erforderlich sind, z. b.:

- Erstellen von Sichten
- Initialisieren von Variablen
- Binden statischer Daten an Listen

`OnCreate`nimmt einen [Bundle](xref:Android.OS.Bundle) -Parameter an. Hierbei handelt es sich um ein Wörterbuch zum Speichern und übergeben von Zustandsinformationen und Objekten zwischen Aktivitäten, wenn das Paket nicht NULL ist. Dies bedeutet, dass die Aktivität neu gestartet wird und den Zustand der vorherigen Instanz wiederherstellen soll. Der folgende Code veranschaulicht das Abrufen von Werten aus dem Bündel:

```csharp
protected override void OnCreate(Bundle bundle)
{
   base.OnCreate(bundle);

   string intentString;
   bool intentBool;

   if (bundle != null)
   {
      intentString = bundle.GetString("myString");
      intentBool = bundle.GetBoolean("myBool");
   }

   // Set our view from the "main" layout resource
   SetContentView(Resource.Layout.Main);
}
```

Nach `OnCreate` Abschluss des Vorgangs wird von Android aufgerufen `OnStart` .

#### <a name="onstart"></a>OnStart

[OnStart](xref:Android.App.Activity.OnStart) wird immer vom System aufgerufen, nachdem `OnCreate` abgeschlossen wurde. Aktivitäten können diese Methode überschreiben, wenn Sie eine bestimmte Aufgabe direkt vor dem sichtbar machen einer Aktivität ausführen müssen, wie z. b. das Aktualisieren aktueller Werte von Sichten innerhalb der Aktivität. Android ruft `OnResume` unmittelbar nach dieser Methode auf.

#### <a name="onresume"></a>Onresume

Das System ruft [onresume](xref:Android.App.Activity.OnResume) auf, wenn die Aktivität bereit ist, mit dem Benutzer zu interagieren.
Aktivitäten sollten diese Methode überschreiben, um folgende Aufgaben auszuführen:

- Hochskalieren von Frameraten (eine gängige Aufgabe bei der Entwicklung von spielen)
- Starten von Animationen
- Lauschen auf GPS-Updates
- Anzeigen von relevanten Warnungen oder Dialogfeldern
- Externe Ereignishandler verknüpfen

Der folgende Code Ausschnitt zeigt beispielsweise, wie die Kamera initialisiert wird:

```csharp
public void OnResume()
{
    base.OnResume(); // Always call the superclass first.

    if (_camera==null)
    {
        // Do camera initializations here
    }
}
```

`OnResume`ist wichtig, da jeder Vorgang, der in ausgeführt wird `OnPause` , in nicht ausgeführt werden soll `OnResume` , da es sich dabei um die einzige Lebenszyklus Methode handelt, die nach dem `OnPause` Wiederholen der Aktivität ausgeführt wird.

#### <a name="onpause"></a>OnPause

[OnPause](xref:Android.App.Activity.OnPause) wird aufgerufen, wenn das System im Begriff ist, die Aktivität in den Hintergrund zu versetzen, oder wenn die Aktivität teilweise verdeckt wird. Aktivitäten sollten diese Methode überschreiben, wenn Folgendes erforderlich ist:

- Nicht gespeicherte Änderungen an persistenten Daten übertragen

- Andere Objekte, die Ressourcen verbrauchen, zerstören oder bereinigen

- Fortsetzen von Frameraten und Anhalten von Animationen

- Aufheben der Registrierung externer Ereignishandler oder Benachrichtigungs Handler (d. h. solche, die an einen Dienst gebunden sind). Dies muss erreicht werden, um Arbeitsspeicher Verluste zu verhindern.

- Ebenso müssen, wenn die Aktivität Dialogfelder oder Warnungen angezeigt hat, mit der-Methode bereinigt werden `.Dismiss()` .

Der folgende Code Ausschnitt gibt beispielsweise die Kamera frei, da Sie von der Aktivität nicht verwendet werden kann, während Sie angehalten wurde:

```csharp
public void OnPause()
{
    base.OnPause(); // Always call the superclass first

    // Release the camera as other activities might need it
    if (_camera != null)
    {
        _camera.Release();
        _camera = null;
    }
}
```

Es gibt zwei mögliche Lebenszyklus Methoden, die nach aufgerufen werden `OnPause` :

1. `OnResume`wird aufgerufen, wenn die Aktivität wieder in den Vordergrund gesetzt werden soll.
1. `OnStop`wird aufgerufen, wenn die Aktivität im Hintergrund platziert wird.

#### <a name="onstop"></a>OnStop

[Onstopps](xref:Android.App.Activity.OnStop) wird aufgerufen, wenn die Aktivität für den Benutzer nicht mehr sichtbar ist. Dies geschieht, wenn eine der folgenden Aktionen auftritt:

- Eine neue Aktivität wird gestartet und deckt diese Aktivität ab.
- Eine vorhandene Aktivität wird in den Vordergrund gestellt.
- Die Aktivität wird zerstört.

`OnStop`kann nicht immer in Situationen mit geringem Arbeitsspeicher aufgerufen werden, z. b. wenn Android für Ressourcen verhungert wird und den Hintergrund der Aktivität nicht ordnungsgemäß durchläuft. Aus diesem Grund sollten Sie sich nicht darauf verlassen, dass `OnStop` aufgerufen wird, wenn eine Aktivität für die Zerstörung vorbereitet wird. Die nächsten Lebenszyklus Methoden, die nach diesem Vorgang aufgerufen werden können `OnDestroy` , sind, wenn die Aktivität entfernt wird, oder `OnRestart` Wenn die Aktivität zurückgeht, um mit dem Benutzer zu interagieren.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](xref:Android.App.Activity.OnDestroy) ist die abschließende Methode, die für eine Aktivitäts Instanz aufgerufen wird, bevor Sie zerstört und vollständig aus dem Arbeitsspeicher entfernt wird. In Extremsituationen kann Android den Anwendungsprozess beenden, der die Aktivität gehostet, was dazu führt, dass der Vorgang `OnDestroy` nicht aufgerufen wird. Die meisten Aktivitäten implementieren diese Methode nicht, da die meisten Bereinigungs-und heruntergefahren in den `OnPause` Methoden und vorgenommen wurden `OnStop` . Die- `OnDestroy` Methode wird in der Regel außer Kraft gesetzt, um Ressourcen mit langer Ausführungszeit zu bereinigen, die Ressourcen nicht Ein Beispiel hierfür sind Hintergrundthreads, die in gestartet wurden `OnCreate` .

Es werden keine Lebenszyklus Methoden aufgerufen, nachdem die Aktivität zerstört wurde.

#### <a name="onrestart"></a>Onrestart

[Onrestart](xref:Android.App.Activity.OnRestart) wird aufgerufen, nachdem die Aktivität beendet wurde, bevor Sie erneut gestartet wird. Ein gutes Beispiel hierfür wäre, wenn der Benutzer die Start Schaltfläche drückt, während eine Aktivität in der Anwendung angezeigt wird. In diesem Fall `OnPause` `OnStop` werden Methoden aufgerufen, und die Aktivität wird in den Hintergrund verschoben, aber nicht zerstört. Wenn der Benutzer die Anwendung dann mithilfe des Task-Managers oder einer ähnlichen Anwendung wiederherstellen soll, ruft Android die- `OnRestart` Methode der-Aktivität auf.

Es gibt keine allgemeinen Richtlinien für die Art der Logik, die in implementiert werden sollte `OnRestart` . Der Grund hierfür `OnStart` ist, dass immer aufgerufen wird, unabhängig davon, ob die Aktivität erstellt oder neu gestartet wird. Daher sollten alle Ressourcen, die für die Aktivität erforderlich sind, in anstelle von initialisiert werden `OnStart` `OnRestart` .

Die nächste Lebenszyklus Methode, die nach aufgerufen wird, ist `OnRestart` `OnStart` .

### <a name="back-vs-home"></a>Zurück im Vergleich zu Home

Viele Android-Geräte haben zwei unterschiedliche Schaltflächen: die Schaltfläche "zurück" und die Schaltfläche "Home". Ein Beispiel hierfür finden Sie im folgenden Screenshot von Android 4.0.3:

[![Schaltflächen zurück und Start](images/image4-sml.png)](images/image4.png#lightbox)

Es gibt einen geringfügigen Unterschied zwischen den beiden Schaltflächen, obwohl Sie anscheinend die gleiche Auswirkung haben, dass Sie eine Anwendung im Hintergrund platzieren. Wenn ein Benutzer auf die Schaltfläche "zurück" klickt, wird Android mitgeteilt, dass Sie mit der-Aktivität abgeschlossen sind. Android zerstört die Aktivität. Wenn der Benutzer dagegen auf die Start Schaltfläche klickt, wird die Aktivität nur im Hintergrund abgelegt &ndash; .

<a name="Managing_State_Throughout_the_Lifecycle"></a>

## <a name="managing-state-throughout-the-lifecycle"></a>Verwalten des Zustands im gesamten Lebenszyklus

Wenn eine Aktivität beendet oder zerstört wird, bietet das System die Möglichkeit, den Zustand der Aktivität zu speichern, um Sie später zu aktivieren.
Dieser gespeicherte Zustand wird als Instanzstatus bezeichnet. Android bietet drei Optionen zum Speichern des instanzzustands während des Aktivitäts Lebenszyklus:

1. Speichern primitiver Werte in einem `Dictionary` als [Paket bezeichneten Paket](xref:Android.OS.Bundle) , das Android zum Speichern des Zustands verwendet.

1. Erstellen einer benutzerdefinierten Klasse, die komplexe Werte wie z. b. Bitmaps enthalten soll. Android verwendet diese benutzerdefinierte Klasse zum Speichern des Zustands.

1. Der Lebenszyklus der Konfigurationsänderung wird umgangen, und es wird davon ausgegangen, dass der Zustand in der Aktivität beibehalten werden soll.

In dieser Anleitung werden die ersten beiden Optionen behandelt.

### <a name="bundle-state"></a>Bündel Status

Die primäre Option zum Speichern des Instanzstatus ist die Verwendung eines Schlüssel-Wert-Wörterbuch Objekts, das als [Bündel](xref:Android.OS.Bundle)bezeichnet wird.
Wenn eine Aktivität erstellt wird, die der `OnCreate` Methode als Parameter übergeben wird, kann dieses Bündel verwendet werden, um den Instanzzustand wiederherzustellen. Es wird nicht empfohlen, ein Bündel für komplexere Daten zu verwenden, die nicht schnell oder einfach in Schlüssel/Wert-Paare (z. b. Bitmaps) serialisiert werden können. Stattdessen sollte Sie für einfache Werte wie Zeichen folgen verwendet werden.

Eine-Aktivität stellt Methoden bereit, die beim Speichern und Abrufen des Instanzstatus im Bündel helfen sollen:

- [Onsaveinstancestate](xref:Android.App.Activity.OnSaveInstanceState*) &ndash; Dies wird von Android aufgerufen, wenn die Aktivität zerstört wird. Aktivitäten können diese Methode implementieren, wenn Sie Schlüssel-Wert-Zustands Elemente persistent speichern müssen.

- [Onrestoreinstancestate](xref:Android.App.Activity.OnRestoreInstanceState*) &ndash; Dies wird nach Abschluss der- `OnCreate` Methode aufgerufen und bietet eine weitere Möglichkeit für eine Aktivität, ihren Zustand nach Abschluss der Initialisierung wiederherzustellen.

Im folgenden Diagramm wird veranschaulicht, wie diese Methoden verwendet werden:

[![Flussdiagramm zu Bündel Zuständen](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>Onsaveinstancestate

[Onsaveinstancestate](xref:Android.App.Activity.OnSaveInstanceState*) wird aufgerufen, wenn die Aktivität angehalten wird. Er empfängt einen Paket Parameter, in dem die Aktivität seinen Zustand speichern kann. Wenn ein Gerät eine Konfigurationsänderung hat, kann eine Aktivität das `Bundle` übergebenen-Objekt verwenden, um den Aktivitäts Zustand durch Überschreiben beizubehalten `OnSaveInstanceState` . Beachten Sie z. B. folgenden Code:

```csharp
int c;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  this.SetContentView (Resource.Layout.SimpleStateView);

  var output = this.FindViewById<TextView> (Resource.Id.outputText);

  if (bundle != null) {
    c = bundle.GetInt ("counter", -1);
  } else {
    c = -1;
  }

  output.Text = c.ToString ();

  var incrementCounter = this.FindViewById<Button> (Resource.Id.incrementCounter);

  incrementCounter.Click += (s,e) => {
    output.Text = (++c).ToString();
  };
}
```

Der obige Code erhöht eine Ganzzahl mit dem Namen `c` , wenn auf eine Schaltfläche `incrementCounter` mit dem Namen geklickt `TextView` wird `output` Wenn eine Konfigurationsänderung auftritt, z. b. wenn das Gerät rotiert wird, verliert der obige Code den Wert von `c` , da der `bundle` `null` wie in der folgenden Abbildung dargestellt wäre:

[![Anzeige zeigt keinen vorherigen Wert an](images/07-sml.png)](images/07.png#lightbox)

Um den Wert von `c` in diesem Beispiel beizubehalten, kann die-Aktivität überschreiben `OnSaveInstanceState` und den Wert im Paket speichern, wie unten dargestellt:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

Wenn das Gerät nun in eine neue Ausrichtung gedreht wird, wird die ganze Zahl im Paket gespeichert und mit der folgenden Zeile abgerufen:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> Es ist wichtig, immer die Basis Implementierung von aufzurufen `OnSaveInstanceState` , damit der Status der Ansichts Hierarchie ebenfalls gespeichert werden kann.

##### <a name="view-state"></a>Ansichts Zustand

`OnSaveInstanceState`Das Überschreiben ist ein geeigneter Mechanismus zum Speichern vorübergehender Daten in einer Aktivität über Richtungsänderungen hinweg, wie z. b. den Counter im obigen Beispiel. Die Standard Implementierung von übernimmt jedoch das `OnSaveInstanceState` Speichern vorübergehender Daten in der Benutzeroberfläche für jede Ansicht, sofern jeder Ansicht eine ID zugewiesen ist. Angenommen, eine Anwendung verfügt über ein `EditText` in XML definiertes-Element wie folgt:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

Da dem `EditText` -Steuerelement eine `id` zugewiesen ist, werden die Daten nach wie vor angezeigt, wenn der Benutzer einige Daten eingibt und das Gerät rotiert:

[![Daten werden im Querformat beibehalten](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>Onrestoreinstancestate

" [Onrestoreinstancestate](xref:Android.App.Activity.OnRestoreInstanceState*) " wird nach "" aufgerufen `OnStart` . Sie bietet eine Aktivität, die die Möglichkeit hat, jeden Zustand wiederherzustellen, der zuvor in einem Paket während der vorherigen gespeichert wurde `OnSaveInstanceState` . Dies ist das gleiche Bündel, das auch für bereitgestellt wird `OnCreate` .

Der folgende Code veranschaulicht, wie der Status in wieder hergestellt werden kann `OnRestoreInstanceState` :

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

Diese Methode bietet eine gewisse Flexibilität, wenn der Zustand wieder hergestellt werden soll. Manchmal ist es besser, zu warten, bis alle Initialisierungen abgeschlossen sind, bevor der Instanzstatus wieder hergestellt wird. Außerdem möchte eine Unterklasse einer vorhandenen Aktivität möglicherweise nur bestimmte Werte aus dem Instanzzustand wiederherstellen. In vielen Fällen ist es nicht erforderlich, außer Kraft zu setzen `OnRestoreInstanceState` , da die meisten Aktivitäten den Zustand mithilfe des für bereitgestellten Pakets wiederherstellen können `OnCreate` .

Ein Beispiel für das Speichern des Zustands mithilfe von finden Sie unter Exemplarische Vorgehensweise `Bundle` [: Speichern des Aktivitäts Zustands](saving-state.md).

#### <a name="bundle-limitations"></a>Bündel Einschränkungen

Obwohl `OnSaveInstanceState` es einfach ist, vorübergehende Daten zu speichern, gelten einige Einschränkungen:

- Sie wird nicht in allen Fällen aufgerufen. Beispielsweise führt das Drücken der **Startseite** oder **zurück** zum Beenden einer Aktivität nicht dazu, dass `OnSaveInstanceState` aufgerufen wird.

- Das `OnSaveInstanceState` an über gegebene Bündel ist nicht für große Objekte (z. b. Bilder) konzipiert. Im Fall von großen Objekten ist das Speichern des Objekts aus [onretainnonconfigurationinstance](xref:Android.App.Activity.OnRetainNonConfigurationInstance) vorzuziehen, wie unten erläutert.

- Daten, die mithilfe des Pakets gespeichert wurden, werden serialisiert. Dies kann zu Verzögerungen führen.

Der Bündel Status ist für einfache Daten nützlich, die nicht viel Arbeitsspeicher verwenden, wohingegen *nicht-Konfigurations-Instanzdaten* für komplexere Daten oder Daten, die aufwendig abgerufen werden können, wie z. b. von einem Webdienst-oder einer komplizierten Datenbankabfrage, nützlich sind. Nicht konfigurationsinstanzdaten werden bei Bedarf in einem Objekt gespeichert. Im nächsten Abschnitt wird die `OnRetainNonConfigurationInstance` Beibehaltung komplexer Datentypen durch Konfigurationsänderungen erläutert.

### <a name="persisting-complex-data"></a>Beibehalten komplexer Daten

Zusätzlich zum Beibehalten von Daten im Paket unterstützt Android auch das Speichern von Daten durch Überschreiben von [onretainnonconfigurationinstance](xref:Android.App.Activity.OnRetainNonConfigurationInstance) und Zurückgeben einer Instanz von `Java.Lang.Object` , die die Daten enthält, die persistent gespeichert werden sollen. Die Verwendung von zum Speichern des Zustands hat zwei Hauptvorteile `OnRetainNonConfigurationInstance` :

- Das von zurückgegebene-Objekt `OnRetainNonConfigurationInstance` ist gut mit größeren, komplexeren Datentypen verfügbar, da der Arbeitsspeicher dieses Objekt beibehält.

- Die `OnRetainNonConfigurationInstance` -Methode wird bei Bedarf und nur bei Bedarf aufgerufen. Dies ist wirtschaftlicher als die Verwendung eines manuellen Caches.

`OnRetainNonConfigurationInstance`Die Verwendung von eignet sich für Szenarien, in denen es aufwendig ist, die Daten mehrmals abzurufen, z. b. in Webdienst aufrufen. Sehen Sie sich beispielsweise den folgenden Code an, der Twitter durchsucht:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);
    SearchTwitter ("xamarin");
  }

  public void SearchTwitter (string text)
  {
    string searchUrl = String.Format("http://search.twitter.com/search.json?" + "q={0}&rpp=10&include_entities=false&" + "result_type=mixed", text);

    var httpReq = (HttpWebRequest)HttpWebRequest.Create (new Uri (searchUrl));
    httpReq.BeginGetResponse (new AsyncCallback (ResponseCallback), httpReq);
  }

  void ResponseCallback (IAsyncResult ar)
  {
    var httpReq = (HttpWebRequest)ar.AsyncState;

    using (var httpRes = (HttpWebResponse)httpReq.EndGetResponse (ar)) {
      ParseResults (httpRes);
    }
  }

  void ParseResults (HttpWebResponse httpRes)
  {
    var s = httpRes.GetResponseStream ();
    var j = (JsonObject)JsonObject.Load (s);

    var results = (from result in (JsonArray)j ["results"] let jResult = result as JsonObject select jResult ["text"].ToString ()).ToArray ();

    RunOnUiThread (() => {
      PopulateTweetList (results);
    });
  }

  void PopulateTweetList (string[] results)
  {
    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
  }
}
```

Mit diesem Code werden Ergebnisse aus dem Web als JSON formatiert, analysiert und dann die Ergebnisse in einer Liste dargestellt, wie im folgenden Screenshot zu sehen:

[![Auf dem Bildschirm angezeigte Ergebnisse](images/06-sml.png)](images/06.png#lightbox)

Wenn eine Konfigurationsänderung auftritt, z. b. Wenn ein Gerät gedreht wird, wiederholt der Code den Vorgang. Um die ursprünglich abgerufenen Ergebnisse wiederzuverwenden und nicht unnötige, redundante Netzwerk Aufrufe zu verursachen, können wir verwenden, `OnRetainNonconfigurationInstance` um die Ergebnisse zu speichern, wie unten dargestellt:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  TweetListWrapper _savedInstance;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    var tweetsWrapper = LastNonConfigurationInstance as TweetListWrapper;

    if (tweetsWrapper != null) {
      PopulateTweetList (tweetsWrapper.Tweets);
    } else {
      SearchTwitter ("xamarin");
    }

    public override Java.Lang.Object OnRetainNonConfigurationInstance ()
    {
      base.OnRetainNonConfigurationInstance ();
      return _savedInstance;
    }

    ...

    void PopulateTweetList (string[] results)
    {
      ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
      _savedInstance = new TweetListWrapper{Tweets=results};
    }
}
```

Wenn das Gerät nun gedreht wird, werden die ursprünglichen Ergebnisse aus der- `LastNonConfiguartionInstance` Eigenschaft abgerufen. In diesem Beispiel bestehen die Ergebnisse aus einer, die `string[]` tweets enthält. Da `OnRetainNonConfigurationInstance` erfordert, dass eine `Java.Lang.Object` zurückgegeben wird, `string[]` wird der in eine Klasse umschließt, die Unterklassen `Java.Lang.Object` wie unten dargestellt:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

Wenn Sie z. b. versuchen, eine `TextView` als das von zurückgegebene-Objekt zu verwenden `OnRetainNonConfigurationInstance` , wird die-Aktivität wie im folgenden Code veranschaulicht:

```csharp
TextView _textView;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  var tv = LastNonConfigurationInstance as TextViewWrapper;

  if(tv != null) {
    _textView = tv;
    var parent = _textView.Parent as FrameLayout;
    parent.RemoveView(_textView);
  } else {
    _textView = new TextView (this);
    _textView.Text = "This will leak.";
  }

  SetContentView (_textView);
}

public override Java.Lang.Object OnRetainNonConfigurationInstance ()
{
  base.OnRetainNonConfigurationInstance ();
  return _textView;
}
```

In diesem Abschnitt haben wir gelernt, wie Sie einfache Zustandsdaten mit dem beibehalten `Bundle` und komplexere Datentypen mit beibehalten `OnRetainNonConfigurationInstance` .

## <a name="summary"></a>Zusammenfassung

Der Android-Aktivitäts Lebenszyklus bietet ein leistungsfähiges Framework für die Zustands Verwaltung von Aktivitäten in einer Anwendung, aber es kann schwierig sein, zu verstehen und zu implementieren. In diesem Kapitel wurden die verschiedenen Zustände vorgestellt, die eine Aktivität während ihrer Lebensdauer durchlaufen kann, sowie die Lebenszyklus Methoden, die diesen Zuständen zugeordnet sind. Als nächstes wurde eine Anleitung bereitgestellt, welche Art von Logik für jede dieser Methoden ausgeführt werden sollte.

## <a name="related-links"></a>Verwandte Links

- [Verarbeiten der Drehung](~/android/app-fundamentals/handling-rotation.md)
- [Android-Aktivität](xref:Android.App.Activity)
