---
title: Aktivitätslebenszyklus
description: Aktivitäten sind ein wichtiger Baustein von Android-Anwendungen und sie können in einer Reihe von unterschiedlichen Zuständen annehmen. Der Aktivitätenlebenszyklus beginnt mit Instanziierung und endet mit Zerstörung und viele Status in der Zwischenzeit enthält. Bei einer statusänderung von eine Aktivität wird die entsprechende Lifecycle Ereignismethode aufgerufen, die Aktivität der bevorstehenden statusänderung zu benachrichtigen und zum Ausführen von Code Anpassung an diese Änderung ermöglicht. In diesem Artikel untersucht den Lebenszyklus von Aktivitäten und erläutert, die dafür verantwortlich, dass eine Aktivität bei jedem dieser Zustand Änderungen als Teil einer Anwendung gut konzipierte, zuverlässige verfügt.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: f35f3e59d8b669795ade3d370894e45866cea1ff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

_Aktivitäten sind ein wichtiger Baustein von Android-Anwendungen und sie können in einer Reihe von unterschiedlichen Zuständen annehmen. Der Aktivitätenlebenszyklus beginnt mit Instanziierung und endet mit Zerstörung und viele Status in der Zwischenzeit enthält. Bei einer statusänderung von eine Aktivität wird die entsprechende Lifecycle Ereignismethode aufgerufen, die Aktivität der bevorstehenden statusänderung zu benachrichtigen und zum Ausführen von Code Anpassung an diese Änderung ermöglicht. In diesem Artikel untersucht den Lebenszyklus von Aktivitäten und erläutert, die dafür verantwortlich, dass eine Aktivität bei jedem dieser Zustand Änderungen als Teil einer Anwendung gut konzipierte, zuverlässige verfügt._

## <a name="activity-lifecycle-overview"></a>Übersicht über die Aktivität-Lebenszyklus

Aktivitäten sind eine ungewöhnliche Programmierkonzept, die spezifisch für Android. Bei der herkömmlichen Entwicklung ist normalerweise eine statische main-Methode, die zum Starten der Anwendung ausgeführt wird. Mit Android unterscheiden sich jedoch Dinge; Android-Anwendungen können über jede registrierte Aktivität in einer Anwendung gestartet werden. In der Praxis haben die meisten Anwendungen nur eine bestimmte Aktivität, die als Einstiegspunkt der Anwendung angegeben wird. Jedoch, wenn eine Anwendung abstürzt, oder beendet wird vom Betriebssystem, können versuchen, das Betriebssystem der Anwendung am letzten Aktivität oder eine andere Stelle innerhalb der vorherigen Aktivität Stapel neu zu starten.
Das Betriebssystem kann darüber hinaus Aktivitäten anhalten, wenn sie nicht aktiv sind und sie wird jedoch wenig Arbeitsspeicher freigeben. Sorgfältiger Überlegung muss vorgenommen werden, um die Anwendung ordnungsgemäß Zustand wiederhergestellt werden, wenn eine Aktivität, insbesondere wenn neu gestartet wird, von denen Daten aus den vorherigen Aktivitäten Aktivität abhängig zu ermöglichen.

Der Aktivitätenlebenszyklus wird als eine Auflistung von Methoden der OS-Aufrufe im gesamten Lebenszyklus einer Aktivität implementiert. Diese Methoden ermöglichen Entwicklern die Funktionalität zu implementieren, die erforderlich sind, den Status und die Ressource Management-Anforderungen ihrer Anwendungen zu erfüllen.

Es ist äußerst wichtig für den Entwickler der Anwendung zum Analysieren der Anforderungen jeder Aktivität, um festzustellen, welche durch den Aktivitätenlebenszyklus verfügbar gemachten Methoden implementiert werden müssen. Dies versäumt, kann zu Anwendungsinstabilität, Abstürze Ressource Aufblähen und möglicherweise sogar zugrunde liegenden Betriebssystem Instabilität führen.

In diesem Kapitel wird den Aktivitätenlebenszyklus im Detail, einschließlich:

-  Status der Aktivität
-  Lebenszyklusmethoden
-  Beibehalten der Zustand einer Anwendung


Dieser Abschnitt enthält auch eine [Exemplarische Vorgehensweise](~/android/app-fundamentals/activity-lifecycle/saving-state.md) enthalten, die praktische Beispiele dazu, wie effizient Zustand während des Lebenszyklus der Aktivität zu speichern. Am Ende dieses Kapitels müssen Sie einen Überblick über den Lebenszyklus der Aktivität und wie es in einer Android-Anwendung unterstützt.

## <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

Android Aktivitäts-Lebenszyklus umfasst eine Auflistung von Methoden, die innerhalb der Activity-Klasse verfügbar gemacht, die den Entwickler mit einer Ressource Management Framework bereitstellen. Dieses Framework ermöglicht Entwicklern die eindeutigen Status Management erfüllen der einzelnen Aktivitäten in einer Anwendung und ressourcenverwaltung ordnungsgemäß verarbeiten.

### <a name="activity-states"></a>Status der Aktivität

Android-Betriebssysteme verhandelt Aktivitäten, die basierend auf ihren Zustand. Dadurch wird die Android-Aktivitäten zu identifizieren, die nicht mehr verwendet, sodass das Betriebssystem, Arbeitsspeicher und Ressourcen freizugeben. Das folgende Diagramm veranschaulicht die Zustände an, die eine Aktivität während seiner Lebensdauer durchlaufen kann:

[![Status-Aktivitätsdiagramm](images/image1-sml.png)](images/image1.png#lightbox)

Diese Zustände können wie folgt in 4 Hauptgruppen unterteilt werden:

1.  *Aktive oder ausgeführten* &ndash; Aktivitäten werden als aktiv angesehen oder ausgeführt wird, wenn sie in den Vordergrund sind auch als obere Ende des Stapels Aktivität bezeichnet. Dies ist die höchste Priorität-Aktivität im Android betrachtet und wird daher nur durch das Betriebssystem in Extremsituationen aufgehoben werden, z. B. als ob die Aktivität versucht, mehr Arbeitsspeicher als auf dem Gerät verfügbar wie dies die Benutzeroberfläche reagiert verursachen könnte.

1.  *Angehalten* &ndash; Wenn das Gerät wechselt in den Energiesparmodus oder eine Aktivität immer noch sichtbar, aber durch eine neue, nicht-Vollbild oder transparent Aktivität teilweise verdeckten ist, gilt die Aktivität wurde angehalten. Angehaltene Aktivitäten sind noch aktiv, d. h. sie alle Informationen über Status und Member verwalten und mit den Fenstermanager verbunden bleiben. Dies ist die zweite als höchste Priorität Aktivität in Android und daher wird nur vom Betriebssystem beendet, wenn das Entfernen dieser Aktivität die ressourcenanforderungen erforderlich, um die aktive/wird ausgeführt-Aktivität stabil und reaktionsfähig zu halten erfüllen.

1.  *Beendet/Backgrounded* &ndash; Aktivitäten, die von einer anderen Aktivität vollständig verdeckt sind gelten, beendet oder im Hintergrund.
    Beendete Aktivitäten versuchen weiterhin, deren Status und Member zu beibehalten, solange mögliche, aber beendete Aktivitäten werden als betrachtet die niedrigste Priorität der drei Zustände und das Betriebssystem wird als solche Aktivitäten in diesem Zustand zuerst an, um die Ressource erfüllen kill Anforderungen der höheren Priorität Aktivitäten.

1.  *Neustart* &ndash; es ist möglich, für eine Aktivität, die an einer beliebigen Stelle stammt im Lebenszyklus aus dem Arbeitsspeicher entfernt werden soll, von Android auf beendet wurde angehalten. Wenn navigiert zurück zum den Aktivitätstyp aus, den er neu gestartet werden muss, den zuvor gespeicherten Zustand wiederhergestellt, und klicken Sie dann dem Benutzer angezeigt.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>Erneute Erstellen der Aktivität als Reaktion auf Änderungen an der Konfiguration

Um weitere komplexe Fragen zu machen, löst Android eine weitere Schraubenschlüssel in der Mischung Konfigurationsänderungen aufgerufen. Konfigurationsänderungen werden schnelle Aktivität Zerstörung/erneute-creation Zyklen, die auftreten, wenn die Konfiguration einer Aktivität ändert, z. B. wenn das Gerät wird, [gedreht](~/android/app-fundamentals/handling-rotation.md) (und die Aktivität im Quer- oder Hochformat neu erstellt werden muss Modus), wenn die Tastatur angezeigt wird (und die Aktivität ist eine Möglichkeit, seine Größe selbst nach dargestellt), oder wenn das Gerät in einer andocken, u. a. befindet.

Änderungen an der Konfiguration verursacht weiterhin die gleichen Aktivitätszustand Änderungen, die beim Beenden und Neustarten einer Aktivität auftreten würden. Um sicherzustellen, dass eine Anwendung reaktionsfähig idealer und führt diese auch während der konfigurationsänderungen, ist es jedoch wichtig, dass sie so schnell wie möglich behandelt werden. Aus diesem Grund bietet Android eine bestimmte API, die zum Zustand beibehalten, während Änderungen an der Konfiguration verwendet werden kann.
Wir behandeln diese später in der [Status in der gesamten der Lebenszyklus verwalten](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) Abschnitt.

### <a name="activity-lifecycle-methods"></a>Aktivität Lebenszyklusmethoden

Android SDK und durch Erweiterung auch das Framework Xamarin.Android, geben Sie ein leistungsfähiges Modell für die Verwaltung des Status von Aktivitäten innerhalb einer Anwendung. Wenn eine Aktivität Zustand sich ändert, wird die Aktivität vom Betriebssystem, benachrichtigt der spezifische Methoden in der jeweiligen Aktivität aufruft. Das folgende Diagramm veranschaulicht diese Methoden in Beziehung zu den Lebenszyklus der Aktivität:

[![Flussdiagramm der Aktivität-Lebenszyklus](images/image2-sml.png)](images/image2.png#lightbox)

Als Entwickler können Sie Zustandsänderungen behandeln, durch das Überschreiben dieser Methoden innerhalb einer Aktivität. Es ist wichtig, beachten Sie jedoch, dass Lebenszyklusmethoden alle an der UI-Thread aufgerufen werden und das Betriebssystem blockiert verhindert das nächste Segment der UI-Arbeit, z. B. zum Ausblenden von der aktuellen Aktivität eine neue Aktivität usw. anzeigen. Daher sollte Code in diesen Methoden so kurz wie möglich ist, stellen eine Anwendung, die den Eindruck gut funktioniert. Lang andauernden Aufgaben sollten in einem Hintergrundthread ausgeführt werden.

Betrachten wir jede dieser Lebenszyklusmethoden und ihrer Verwendung:

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) ist die erste Methode, die aufgerufen werden, wenn eine Aktivität erstellt wird.
`OnCreate` wird immer überschrieben, um alle Start-Initialisierungen durchführen, die von einer Aktivität wie z. B. erforderlich sein kann:

-  Erstellen von Sichten
-  Initialisieren von Variablen
-  Binden von statischen Daten in Listen


`OnCreate` akzeptiert eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Parameter, der ein Wörterbuch zum Speichern und übergeben Zustandsinformationen und Objekten zwischen Aktivitäten, wenn das Paket nicht null ist, wird hiermit die Aktivität wird neu gestartet, und sollten sie den Zustand von Wiederherstellen der vorherige Instanz. Im folgende Code wird veranschaulicht, wie Werte aus dem Paket abgerufen werden:

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

Einmal `OnCreate` hat nicht mehr benötigen, Android angerufen `OnStart`.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) wird immer aufgerufen, durch das System nach `OnCreate` abgeschlossen ist. Aktivitäten können sie beliebige Rechte für bestimmte Aufgaben ausführen, bevor eine Aktivität wie z. B. das Aktualisieren der aktuellen Werte von Ansichten innerhalb der Aktivität angezeigt wird, müssen diese Methode überschreiben. Android angerufen `OnResume` unmittelbar nach dieser Methode.

#### <a name="onresume"></a>OnResume

Das System ruft [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) Wenn die Aktivität mit beginnen mit dem Benutzer interagieren ist.
Aktivitäten sollten diese Methode, um Aufgaben wie z. B. überschreiben:

-  Planungsansicht Frameraten (eine häufige Aufgabe im Spiel Gebäude)
-  Starten von Animationen
-  Warten auf GPS-updates
-  Alle relevanten Warnungen oder Dialogfeldern anzeigen
-  Externe Ereignishandler verknüpfen


Beispielsweise zeigt der folgende Codeausschnitt, wie die Kamera zu initialisieren:

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

`OnResume` ist wichtig, da jeder Vorgang, ist im durchgeführt wird, `OnPause` muss rückgängig gemacht in `OnResume`, da es die einzige Lifecycle Methode, die garantiert ist handelt, führen Sie nach dem `OnPause` , wenn die Aktivität wieder zum Leben zu bringen.

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) wird aufgerufen, wenn das System ist zum Speichern der Aktivitäts in den Hintergrund oder wenn die Aktivität teilweise verdeckt wird. Aktivitäten sollten diese Methode überschreiben, wenn dies erforderlich ist:

-   Übertragen Sie gehen nicht gespeicherte Änderungen an persistente Daten

-   Zerstören Sie oder bereinigen Sie andere Objekte, die Ressourcen zu verbrauchen.

-   Stufenförmige Frameraten und Anhalten von Fenstern

-   Aufheben der Registrierung externen Ereignishandler oder Benachrichtigungshandler (d. h. diejenigen, die an einen Dienst gebunden sind). Dies muss erfolgen, um die Aktivität Speicherverluste zu verhindern.

-   Ebenso, wenn die Aktivität Dialogfelder oder Warnungen angezeigt, sie müssen bereinigt werden mit der `.Dismiss()` Methode.

Beispielsweise der folgende Codeausschnitt die Kamera, freigeben, wenn die Aktivität vornehmen kann nicht davon bei angehaltener verwenden:

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

Es gibt zwei mögliche Lebenszyklusmethoden, die aufgerufen werden, nachdem `OnPause`:

1.  `OnResume` wird aufgerufen, wenn die Aktivität wird in den Vordergrund zurückgegeben werden sollen.
1.  `OnStop` wird aufgerufen, wenn die Aktivität im Hintergrund platziert wird.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) wird aufgerufen, wenn die Aktivität nicht mehr für den Benutzer sichtbar ist. Dies geschieht, wenn eines der folgenden Ereignisse eintritt:

-  Eine neue Aktivität gestartet wird, und wird von dieser Aktivität abdecken.
-  Eine vorhandene Aktivität wird in den Vordergrund gebracht wird.
-  Die Aktivität ist beschädigt.


`OnStop` möglicherweise nicht jedes Mal aufgerufen werden in den Arbeitsspeicherstatus niedrig zu halten Situationen, z. B. wenn Android für Ressourcen blockiert ist und die Aktivität kann nicht ordnungsgemäß im Hintergrund. Aus diesem Grund ist es am besten gar nicht abhängig `OnStop` erste aufgerufen, wenn eine Aktivität zum Löschen wird vorbereitet. Der nächsten Lebenszyklusmethoden, die aufgerufen werden können, nachdem dieses Objekt kann `OnDestroy` , wenn die Aktivität, also oder `OnRestart` , wenn die Aktivität wieder für die Interaktion mit dem Benutzer stammt.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) ist die letzte Methode, die für eine Aktivitätsinstanz aufgerufen wird, bevor sie zerstört und vollständig aus dem Arbeitsspeicher entfernt wurde. In Extremfällen kann Android brechen Sie den Anwendungsprozess, der die Aktivität gehostet wird, der verursacht `OnDestroy` nicht aufgerufen wird. Die meisten Aktivitäten nicht diese Methode implementieren, da die meisten bereinigen und Herunterfahren, in abgeschlossen wurde der `OnPause` und `OnStop` Methoden. Die `OnDestroy` -Methode überschrieben wird in der Regel zum Bereinigen von lang ausgeführt Ressourcen, die möglicherweise Ressourcenverluste. Ein Beispiel hierfür ist möglicherweise Hintergrundthreads, die gestartet wurden, im `OnCreate`.

Es werden keine Lebenszyklusmethoden wird aufgerufen, nachdem die Aktivität zerstört wurde.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) wird aufgerufen, nachdem die Aktivität beendet wurde, bevor er erneut gestartet wird. Ein gutes Beispiel hierfür wäre, wenn der Benutzer die Schaltfläche "Start", klicken Sie auf eine Aktivität in der Anwendung drückt. Wenn dies geschieht `OnPause` und dann `OnStop` Methoden aufgerufen werden, und die Aktivität wird in den Hintergrund verschoben, jedoch nicht zerstört wird. Wenn der Benutzer zum Wiederherstellen der Anwendung mithilfe des Task-Managers oder einer ähnlichen Anwendung Android aufruft, wären die `OnRestart` -Methode der Aktivität.

Es gibt keine allgemeinen Richtlinien für welche Art von Logik soll, in implementiert werden `OnRestart`. Grund hierfür ist, `OnStart` wird immer aufgerufen, unabhängig davon, ob die Aktivität erstellt wird oder neu gestartet wird, damit alle Ressourcen, die erforderlich sind, von der Aktivität soll, in initialisiert werden `OnStart`, anstatt `OnRestart`.

Die nächste Lifecycle-Methode wird aufgerufen, nachdem `OnRestart` werden `OnStart`.

### <a name="back-vs-home"></a>Sichern Sie die Visual Studio. Startseite

Viele Android-Geräte sind zwei unterschiedliche Schaltflächen: Schaltfläche "Zurück" und eine Schaltfläche "Start". Ein Beispiel hierfür kann im folgenden Screenshot von Android 4.0.3 angezeigt werden:

[![Schaltflächen zurück und Homepage](images/image4-sml.png)](images/image4.png#lightbox)

Besteht ein feine Unterschied zwischen den zwei Schaltflächen, obwohl sie haben die gleiche Auswirkung von Inkonsistenzen einer Anwendung im Hintergrund angezeigt werden. Klickt ein Benutzer auf die Schaltfläche "zurück", werden sie Android darüber informiert, dass sie die Aktivität abgeschlossen haben. Android wird die Aktivität dann gelöscht. Im Gegensatz dazu klickt der Benutzer die Schaltfläche "Start" die Aktivität ist lediglich abgelegt Hintergrund &ndash; Android wird die Aktivität nicht beenden.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>Verwalten des Zustands im gesamten Lebenszyklus

Wenn eine Aktivität beendet oder zerstört wird ermöglicht das System auf den Status der Aktivität für spätere Aktivierung zu speichern.
Diese gespeicherte Status wird als Instanzstatus bezeichnet. Android bietet drei Optionen zum Speichern des Instanzstatus während des Lebenszyklus der Aktivität:

1. Speichern von primitiven Werte in einer `Dictionary` genannt eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) , Android zum Speichern des Status verwenden möchten.

1. Komplexe Werte wie z. B. Bitmaps, erstellen eine benutzerdefinierte Klasse, die gespeichert werden. Diese benutzerdefinierte Klasse verwendet Android um Zustand zu speichern.

1. Umgehen des Lebenszyklus der Konfiguration ändern, und vollständige Verantwortung für die Zustandsverwaltung in der Aktivität.


Dieser Leitfaden behandelt die ersten beiden Optionen.



### <a name="bundle-state"></a>Bundle-Status

Die wichtigste Option zum Speichern des Status der Anwendungsinstanz ist die Verwendung von ein Schlüssel/Wert-Dictionary-Objekt als bezeichnet eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/).
Denken Sie daran: Wenn eine Aktivität erstellt wurde, in der `OnCreate` Methode ein Paket als Parameter übergeben wird, dieses Paket kann verwendet werden, um den Zustand der Instanz wiederherzustellen. Es wird nicht empfohlen, ein Paket für komplexere Daten zu verwenden, wird nicht schnell oder einfach serialisieren zu um Schlüssel/Wert-Paaren (z. B. Bitmaps); Stattdessen sollte für einfache Werte wie Zeichenfolgen verwendet werden.

Eine Aktivität enthält Methoden, um besser zu speichern und Abrufen des Instanzstatus im Paket:

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; Dies wird von Android aufgerufen, wenn die Aktivität zerstört wird. Aktivitäten können diese Methode implementieren, wenn sie alle Schlüssel-Wert-Statuselemente persistent zu speichernde.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; wird aufgerufen, nachdem die `OnCreate` Methode abgeschlossen ist, und bietet eine weitere Gelegenheit für eine Aktivität, um dessen Status wiederherzustellen, nachdem die Initialisierung abgeschlossen ist.

Das folgende Diagramm veranschaulicht, wie diese Methoden verwendet werden:

[![Flussdiagramm der Paket-Zustände](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) werden aufgerufen, wenn die Aktivität beendet wird. Sie erhalten einen Bundle-Parameter, dem die Aktivität ihren Status in gespeichert werden kann. Wenn ein Gerät eine Konfiguration ändert, können eine Aktivität die `Bundle` -Objekt, das übergeben wird, um durch Überschreiben der Aktivitätszustand beibehalten `OnSaveInstanceState`. Beachten Sie z. B. folgenden Code:

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

Der obige Code inkrementiert den Wert einer ganzen Zahl namens `c` beim Klicken auf eine Schaltfläche mit dem Namen `incrementCounter` geklickt wird, Anzeigen des Ergebnisses in einer `TextView` mit dem Namen `output`. Eine konfigurationsänderung Fall – z. B. wenn das Gerät gedreht wird – der obige Code verliert den Wert der `c` da die `bundle` wäre `null`, wie in der folgenden Abbildung dargestellt:

[![Anzeige wird vorherigen Wert nicht angezeigt werden.](images/07-sml.png)](images/07.png#lightbox)

Den Wert des beibehalten `c` in diesem Beispiel wird die Aktivität kann außer Kraft setzen `OnSaveInstanceState`, den Wert im Paket zu speichern, wie unten dargestellt:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

Jetzt, wenn das Gerät in die neue Ausrichtung gedreht wird, die ganze Zahl, die im Paket gespeichert ist und durch die Zeile abgerufen wird:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> Es ist wichtig, immer Aufruf auf die grundlegende Implementierung der `OnSaveInstanceState` , damit der Status der Hierarchie anzeigen auch gespeichert werden kann.



##### <a name="view-state"></a>Ansichtszustand

Überschreiben von `OnSaveInstanceState` geeigneten Mechanismus zum Speichern vorübergehender Daten in einer Aktivität über Ausrichtung ändert, z. B. den Indikator im obigen Beispiel ist. Allerdings die standardmäßige Implementierung des `OnSaveInstanceState` übernimmt speichern vorübergehende Daten in der Benutzeroberfläche für jede Ansicht, solange jede Ansicht eine ID zugewiesen wurde. Angenommen, eine Anwendung verfügt über eine `EditText` Element im XML-Code wie folgt definiert:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

Da die `EditText` Steuerelement verfügt über eine `id` zugewiesen, wenn der Benutzer einige Daten gibt und das Gerät dreht, die Daten werden weiterhin angezeigt, wie unten dargestellt:

[![Daten werden im Querformat beibehalten.](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) aufgerufen werden, nachdem `OnStart`. Es ermöglicht, dass einer Aktivität zu einem beliebigen Zustand wiederherstellen, die zuvor auf ein Paket während der vorherigen gespeicherten `OnSaveInstanceState`. Dies ist das gleiche Paket, die bereitgestellt wird `OnCreate`jedoch.

Im folgenden Code wird veranschaulicht, wie Status wiederhergestellt werden kann `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

Diese Methode ist vorhanden, um einige Flexibilität im Hinblick auf bereitstellen, wenn der Zustand wiederhergestellt werden soll. Manchmal ist es besser geeignet ist, warten, bis alle Initialisierungen vor dem Wiederherstellen der Instanzstatus fertig sind. Darüber hinaus kann eine Unterklasse von einer vorhandenen Aktivität nur bestimmte Werte aus der Instanzstatus wiederherstellen möchten. In vielen Fällen ist es nicht erforderlich, außer Kraft setzen `OnRestoreInstanceState`, da die meisten Aktivitäten mithilfe des Pakets bereitgestellt, um Zustand wiederherzustellen, können `OnCreate`.

Ein Beispiel für das Speichern von Status mit einer `Bundle`, finden Sie in der [Exemplarische Vorgehensweise – Speichern der Aktivitätszustand](saving-state.md).


#### <a name="bundle-limitations"></a>Bundle-Einschränkungen

Obwohl `OnSaveInstanceState` ist es einfach, um flüchtige Daten zu speichern, gelten einige Einschränkungen:

-   Es wird nicht in allen Fällen aufgerufen. Drücken Sie z. B. **Home** oder **wieder** zum Beenden einer Aktivität führt nicht dazu `OnSaveInstanceState` aufgerufen werden.

-   Übergeben Sie das Paket in `OnSaveInstanceState` ist nicht für große Objekte, z. B. Bilder konzipiert. Im Fall von großen Objekten Speichern des Objekts aus [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) ist zu bevorzugen, wie im folgenden erläutert.

-   Daten mithilfe des Pakets gespeichert werden serialisiert, dies kann zu Verzögerungen führen.

Bundle-Status eignet sich für einfache Datentypen, die nicht viel Arbeitsspeicher verwendet, wohingegen *nichtkonfigurationsdomänen für Instanzdaten* ist nützlich für komplexere Daten oder Daten, die abzurufenden aufwendig ist, z. B. einen Webdienstaufruf oder eine komplizierte Datenbankabfrage. Nach Bedarf, ruft Instanzdaten nicht-Konfiguration in einem Objekt gespeichert. Im nächste Abschnitt führt `OnRetainNonConfigurationInstance` als eine Möglichkeit, komplexe Datentypen über Änderungen an der Konfiguration beibehalten.


### <a name="persisting-complex-data"></a>Komplexe Daten beibehalten

Zusätzlich zum Beibehalten von Daten im Paket Android auch unterstützt das Speichern von Daten durch Überschreiben [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) und Rückgabe einer Instanz von einem `Java.Lang.Object` enthält die Daten beibehalten werden. Es gibt zwei Hauptvorteile der Verwendung von `OnRetainNonConfigurationInstance` Zustand speichern:

-   Das Objekt, das von `OnRetainNonConfigurationInstance` Arbeitsspeicher auf diesem Objekt besetzt gut mit größere und komplexere Datentypen durchgeführt.

-   Die `OnRetainNonConfigurationInstance` Methode wird aufgerufen, bei Bedarf, und dies nur bei Bedarf. Dies ist die ökonomischer als die Verwendung eines manuellen Caches.

Mit `OnRetainNonConfigurationInstance` wird für Szenarien, in denen es teuer, zum Abrufen der Daten mehrere Male ist, geeignet, wie Aufrufe des Webdiensts. Betrachten Sie beispielsweise den folgenden Code, mit dem Twitter gesucht:

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

Dieser Code Ruft die Ergebnisse aus dem Web als JSON formatierten ab, analysiert diese und präsentiert die Ergebnisse dann in einer Liste aus, wie im folgenden Screenshot gezeigt:

[![Auf dem Bildschirm angezeigten Ergebnisse](images/06-sml.png)](images/06.png#lightbox)

Tritt eine konfigurationsänderung – z. B., wenn ein Gerät gedreht wird - wird der Code der Prozess wiederholt. Zum wiederverwenden, die ursprünglich abgerufenen Ergebnisse und nicht dazu, dass unnötige, redundanten Netzwerke aufzurufen, verwenden wir `OnRetainNonconfigurationInstance` um die Ergebnisse zu speichern, wie unten dargestellt:

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

Nachdem die ursprünglichen Ergebnisse von abgerufen werden, wenn das Gerät gedreht wird, die `LastNonConfiguartionInstance` Eigenschaft. In diesem Beispiel besteht das Ergebnis aus einer `string[]` Tweets enthält. Da `OnRetainNonConfigurationInstance` erfordert, dass eine `Java.Lang.Object` zurückgegeben werden, wenn die `string[]` in einer Klasse, Unterklassen umschlossen `Java.Lang.Object`, wie unten dargestellt:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

Beispielsweise möchten, verwenden Sie eine `TextView` als das Objekt, das von `OnRetainNonConfigurationInstance` wird die Aktivität preisgeben, wie in den folgenden Code veranschaulicht:

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

In diesem Abschnitt haben wir gelernt, wie einfache Zustandsdaten mit beibehalten der `Bundle`, und die persist komplexerer Datentypen mit `OnRetainNonConfigurationInstance`.

## <a name="summary"></a>Zusammenfassung

Android Aktivitäts-Lebenszyklus bietet ein leistungsfähiges Framework für die Statusverwaltung aller Aktivitäten in einer Anwendung, aber es kann schwierig zu verstehen und zu implementieren. In diesem Kapitel eingeführt, die verschiedenen Zustände, die eine Aktivität durchlaufen kann, während seiner Lebensdauer als auch der Lebenszyklusmethoden, die die Länder zugeordnet sind. Als Nächstes wurde Anleitungen bereitgestellt, auf welche Art von Logik in jeder dieser Methoden ausgeführt werden soll.


## <a name="related-links"></a>Verwandte Links

- [Verarbeiten der Drehung](~/android/app-fundamentals/handling-rotation.md)
- [Android-Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/)
