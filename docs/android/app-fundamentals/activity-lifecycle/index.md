---
title: Aktivitätslebenszyklus
description: Aktivitäten sind ein wesentlicher Baustein von Android-Anwendungen, und sie können in einer Reihe von anderen Zuständen vorhanden sein. Der aktivitätslebenszyklus beginnt mit der Instanziierung und endet mit Zerstörung und viele Status in der Zwischenzeit enthält. Wenn eine Aktivität den Status ändert, wird die entsprechende Lebenszyklus-Event-Methode aufgerufen, die Aktivität der bevorstehenden statusänderung zu benachrichtigen und zum Ausführen von Code zur Anpassung an diese Änderung ermöglicht. In diesem Artikel werden die Aktivitäten des Lebenszyklus untersucht und erläutert, die Verantwortung, dass eine Aktivität während dieser Zustandsänderungen als Teil einer Anwendung kaum und zuverlässige verfügt.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 3592a3027469cb9997d973db53d636ddea9e679d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61024270"
---
# <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

_Aktivitäten sind ein wesentlicher Baustein von Android-Anwendungen, und sie können in einer Reihe von anderen Zuständen vorhanden sein. Der aktivitätslebenszyklus beginnt mit der Instanziierung und endet mit Zerstörung und viele Status in der Zwischenzeit enthält. Wenn eine Aktivität den Status ändert, wird die entsprechende Lebenszyklus-Event-Methode aufgerufen, die Aktivität der bevorstehenden statusänderung zu benachrichtigen und zum Ausführen von Code zur Anpassung an diese Änderung ermöglicht. In diesem Artikel werden die Aktivitäten des Lebenszyklus untersucht und erläutert, die Verantwortung, dass eine Aktivität während dieser Zustandsänderungen als Teil einer Anwendung kaum und zuverlässige verfügt._

## <a name="activity-lifecycle-overview"></a>Lebenszyklus – Übersicht

Aktivitäten sind eine ungewöhnliche Programmierkonzept, die speziell für Android. Bei der herkömmlichen Anwendungsentwicklung besteht in der Regel eine statische main-Methode, die zum Starten der Anwendung ausgeführt wird. Mit Android unterscheiden sich jedoch Dinge. Android-Anwendungen können über alle registrierten Aktivitäten innerhalb einer Anwendung gestartet werden. In der Praxis haben die meisten Anwendungen nur eine bestimmte Aktivität, die als Einstiegspunkt der Anwendung angegeben wird. Jedoch, wenn eine Anwendung abstürzt, oder beendet wird vom Betriebssystem, können versuchen, das Betriebssystem die Anwendung auf der letzten Aktivität oder an anderer Stelle in der vorherigen Aktivität Stack neu zu starten.
Darüber hinaus kann das Betriebssystem Aktivitäten anhalten, wenn sie nicht aktiv sind, und diese freigeben, wenn es nicht genügend Arbeitsspeicher verfügt. Sorgfältiger Überlegung muss erfolgen, dass die Anwendung aus, um dessen Status ordnungsgemäß wiederherzustellen, die eine Aktivität, insbesondere dann, wenn neu gestartet wird, die Aktivität von Daten aus vorherigen Aktivitäten abhängen.

Der aktivitätslebenszyklus ist als eine Auflistung von Methodenaufrufen das Betriebssystem während des Lebenszyklus einer Aktivität implementiert. Diese Methoden ermöglichen Entwicklern, die die Funktionalität zu implementieren, die erforderlich sind, die Status und die Resource Management-Anforderungen ihrer Anwendungen zu erfüllen.

Es ist äußerst wichtig für den Entwickler der Anwendung zum Analysieren von der Anforderungen jeder Aktivität, um zu bestimmen, welche von der aktivitätslebenszyklus verfügbar gemachte Methoden implementiert werden müssen. Wenn dies nicht der Fall kann Anwendungsinstabilität, Abstürze, Ressource Überfrachtung und möglicherweise sogar zugrunde liegenden Betriebssystem Instabilität führen.

In diesem Kapitel wird der aktivitätslebenszyklus im Detail, einschließlich:

-  Aktivitätsstatus
-  Lebenszyklusmethoden
-  Der Zustand einer Anwendung beibehalten


Dieser Abschnitt enthält auch eine [Exemplarische Vorgehensweise](~/android/app-fundamentals/activity-lifecycle/saving-state.md) , die bieten praktische Beispiele zum Zustand während der aktivitätslebenszyklus effizient zu speichern. Am Ende dieses Kapitels benötigen Sie einen Überblick über den aktivitätslebenszyklus und wie es in einer Android-Anwendung unterstützt.

## <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

Der Lebenszyklus der Android-Aktivität umfasst eine Auflistung von Methoden, die innerhalb der Activity-Klasse verfügbar gemacht, die den Entwickler mit einer Resource Management-Framework bieten. Dieses Framework ermöglicht Entwicklern, die zum Erfüllen der verwaltungsanforderungen eindeutigen Zustand jeder Aktivität innerhalb einer Anwendung und ressourcenverwaltung ordnungsgemäß verarbeiten.

### <a name="activity-states"></a>Aktivitätsstatus

Das Android-Betriebssystem führt Vermittlungen Aktivitäten basierend auf ihren Status. Dadurch wird die Android-Aktivitäten zu identifizieren, die nicht mehr verwendet werden, sodass das Betriebssystem, Arbeitsspeicher und Ressourcen freizugeben. Das folgende Diagramm veranschaulicht die Zustände, die eine Aktivität während seiner Lebensdauer durchlaufen kann:

[![Status-Aktivitätsdiagramm](images/image1-sml.png)](images/image1.png#lightbox)

Diese Zustände können wie folgt in 4 Hauptgruppen unterteilt werden:

1.  *Aktiven bzw. ausgeführten* &ndash; Aktivitäten werden als aktiv oder ausgeführt wird, wenn sie in den Vordergrund, sind auch als der Anfang des Stapels Aktivität bezeichnet. Dies gilt, dass die höchste Priorität-Aktivität in Android und wird daher nur durch das Betriebssystem in extremen Fällen abgebrochen werden, z. B. als ob die Aktivität versucht, mit mehr Arbeitsspeicher als auf dem Gerät verfügbar wie Dadurch kann die Benutzeroberfläche reagiert.

1.  *Angehalten* &ndash; Wenn das Gerät wird in den Ruhezustand versetzt oder eine Aktivität immer noch sichtbar, sind jedoch durch eine neue, nicht-vergrößern oder transparent Aktivität teilweise verdeckten ist, gilt die Aktivität wurde angehalten. Angehaltene Aktivitäten sind aktiv, d. h. sie alle Informationen zu Status und die Member verwalten und mit dem Fenstermanager verbunden bleiben. Dies gilt als die zweite höchste Priorität-Aktivität in Android und als solche, wird nur vom Betriebssystem beendet werden, wenn diese Aktivität zu beenden die ressourcenanforderungen erforderlich, um die aktiven/aktiven-Aktivität zu halten, stabil und reaktionsfähige erfüllt.

1.  *Beendet/Backgrounded* &ndash; Aktivitäten, die von einer anderen Aktivität vollständig verdeckt sind gelten, beendet oder im Hintergrund.
    Beendete Aktivitäten versucht, noch ihren Status und Member zu beibehalten, solange mögliche, aber beendete Aktivitäten gelten die geringste Priorität für die drei Zustände und das Betriebssystem, Aktivitäten, die in diesem Zustand beendet, um auf die Ressource zu erfüllen die Anforderungen der höheren Priorität-Aktivitäten.

1.  *Neustart* &ndash; es ist möglich, für eine Aktivität, die an einer beliebigen Stelle aus im Lebenszyklus aus dem Arbeitsspeicher entfernt werden soll, von Android zu beendet wurde angehalten. Wenn der Benutzer navigiert, an die Aktivität, die sie neu gestartet werden muss, in den zuvor gespeicherten Zustand wiederhergestellt, und klicken Sie dann dem Benutzer angezeigt.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>Die erneute Erstellung Aktivität als Reaktion auf Änderungen an der Konfiguration

Um spielt eine Rolle, die komplizierter machen, löst Android eine weitere Schraubenschlüsselsymbol in der Mischung wird aufgerufen, Änderungen an der Konfiguration. Änderungen an der Konfiguration werden schnelle Aktivität Zerstörung/erneute-creation Zyklen, die auftreten, wenn die Konfiguration einer Aktivität ändert z. B. wenn das Gerät ist, [gedreht](~/android/app-fundamentals/handling-rotation.md) (und die Aktivität im Querformat oder Hochformat neu erstellt werden muss Modus), wenn die angezeigt wird (und die Aktivität ist eine Möglichkeit zum Ändern der Größe selbst angezeigt), oder wenn das Gerät in ein Dock, u. a. platziert wird.

Konfigurationsänderungen verursachen weiterhin die gleichen Aktivitätszustand Änderungen, die beim Beenden und Neustarten einer Aktivitäts auftreten würden. Um sicherzustellen, dass eine Anwendung ist mit der Reaktionsfähigkeit und auch bei Änderungen an der Konfiguration führt, ist es jedoch wichtig, dass sie so schnell wie möglich verarbeitet werden. Aus diesem Grund bietet Android eine bestimmte API, die verwendet werden kann, um den Zustand beizubehalten, während Änderungen an der Konfiguration.
Ich werde Dies wird später in der [Zustand während der Lebenszyklus verwalten](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) Abschnitt.

### <a name="activity-lifecycle-methods"></a>Aktivität Lebenszyklusmethoden

Das Android SDK und durch Erweiterung der Xamarin.Android-Framework, geben Sie ein leistungsfähiges Modell für die Verwaltung des Status von Aktivitäten innerhalb einer Anwendung. Bei einer Aktivität Zustand sich ändert, wird die Aktivität vom Betriebssystem benachrichtigt, die bestimmte Methoden auf die Aktivität aufruft. Das folgende Diagramm veranschaulicht diese Methoden in Beziehung zu der Aktivitätslebenszyklus an:

[![Aktivität-Lifecycle-Flussdiagramm](images/image2-sml.png)](images/image2.png#lightbox)

Als Entwickler können Sie Änderungen am Ansichtszustand behandeln, durch das Überschreiben dieser Methoden in einer Aktivität. Es ist wichtig, aber beachten Sie, dass alle Methoden, die im UI-Thread aufgerufen werden, und das Betriebssystem blockiert von der Durchführung des nächsten Teil der UI-Arbeit, z. B. durch das Ausblenden der aktuellen Aktivität, die angezeigt wird, eine neue Aktivität usw. Daher sollte Code in diesen Methoden so kurz wie möglich, eine Anwendung können Sie eine gute Leistung. Lang andauernde Aufgaben sollten in einem Hintergrundthread ausgeführt werden.

Betrachten Sie diese Lebenszyklusmethoden und ihre Verwendung:

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) ist die erste Methode aufgerufen werden, wenn eine Aktivität erstellt wird.
`OnCreate` wird immer überschrieben, um alle Start-Initialisierungen durchführen, die durch eine Aktivität wie z. B. möglicherweise erforderlich sind:

-  Erstellen von Ansichten
-  Initialisieren von Variablen
-  Binden von statischen Daten in Listen


`OnCreate` nimmt eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) -Parameter, der ist ein Wörterbuch zum Speichern und übergeben von Zustandsinformationen und Objekte zwischen Aktivitäten auf, wenn das Paket nicht null ist, wird hiermit die Aktivität wird neu gestartet und sollte es seinen Zustand aus Wiederherstellen der vorherige Instanz. Der folgende Code zeigt, wie Werte aus dem Paket abgerufen wird:

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

Einmal `OnCreate` wurde fertig gestellt wurde, Android ruft `OnStart`.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) wird immer aufgerufen, durch das System nach `OnCreate` abgeschlossen ist. Aktivitäten können diese Methode überschreiben, wenn eine beliebige Rechte für bestimmte Aufgaben ausführen, bevor eine Aktivität wie z. B. das Aktualisieren der aktuellen Werte von Ansichten innerhalb der Aktivität angezeigt wird. Android ruft `OnResume` unmittelbar nach dieser Methode.

#### <a name="onresume"></a>OnResume

Das System ruft [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) Wenn die Aktivität ist mit der Interaktion mit dem Benutzer beginnen.
Aktivitäten sollten diese Methode, um Aufgaben wie z. B. überschreiben:

-  Langsame Frameraten (eine häufige Aufgabe Spiele erstellen)
-  Starten von Animationen
-  Lauschen auf GPS-updates
-  Alle relevanten Warnungen oder Dialogfelder anzeigen
-  Verknüpfen Sie externer-Ereignishandler


Beispielsweise zeigt der folgende Codeausschnitt, wie die Kamera initialisiert werden:

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

`OnResume` ist wichtig, da jeder Vorgang, der ist geschieht im `OnPause` muss nicht Arbeit in `OnResume`, da es die einzige Lebenszyklusmethode, die garantiert ist, führen Sie nach der `OnPause` , wenn Sie die Aktivität wieder zum Leben.

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) wird aufgerufen, wenn das System ist dabei, platzieren Sie die Aktivität aus, in den Hintergrund oder wenn die Aktivität teilweise verdeckt wird. Aktivitäten sollten diese Methode überschreiben, wenn dies erforderlich:

-   Nicht gespeicherte Änderungen von persistenten Daten

-   Zerstören Sie oder bereinigen Sie andere Objekte, die Ressourcen zu verbrauchen.

-   Stufenförmige Frameraten und Anhalten von Animationen

-   Aufheben der Registrierung externer Ereignishandler oder benachrichtigungshandlern (d. h. diejenigen, die an einen Dienst gebunden sind). Dies muss erfolgen, um Aktivität Speicherverluste zu verhindern.

-   Auch wenn die Aktivität Dialogfelder oder Warnungen angezeigt hat, sie müssen bereinigt werden mit der `.Dismiss()` Methode.

Beispielsweise wird der folgende Codeausschnitt die Kamera, freigegeben, während die Aktivität vornehmen, kann nicht verwendet bei angehaltener skriptausführung:

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

Es gibt zwei mögliche Lebenszyklusmethoden, die aufgerufen werden, nach dem `OnPause`:

1.  `OnResume` wird aufgerufen, wenn die Aktivität ist in den Vordergrund zurückgegeben werden.
1.  `OnStop` wird aufgerufen, wenn die Aktivität im Hintergrund platziert wird.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) wird aufgerufen, wenn die Aktivität nicht mehr für den Benutzer sichtbar ist. Dies geschieht, wenn eine der folgenden Bedingungen zutrifft:

-  Eine neue Aktivität gestartet wird und Sie diese Aktivität behandelt wird.
-  Eine vorhandene Aktivität wird in den Vordergrund gesetzt wird.
-  Die Aktivität wird gelöscht.


`OnStop` unter Umständen nicht jedes Mal aufgerufen werden im Speicher knapp ist, z. B. wenn Android für Ressourcen blockiert wird, und kann nicht ordnungsgemäß im Hintergrund der Aktivitäts. Aus diesem Grund ist es am besten nicht abhängig `OnStop` bei der Vorbereitung einer Aktivitäts zur Zerstörung aufgerufen. Die nächste Lebenszyklusmethoden, die aufgerufen werden können, nachdem dieser Datensatz `OnDestroy` , wenn die Aktivität, verschwindet oder `OnRestart` , wenn die Aktivität wieder für die Interaktion mit der Benutzer stammt.

#### <a name="ondestroy"></a>onDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) ist die letzte Methode, die auf einer Aktivitätsinstanz aufgerufen wird, bevor es zerstört und vollständig aus dem Arbeitsspeicher entfernt wurde. In extremen Fällen beendet Android den Anwendungsprozess, der die Aktivität, was gehostet wird zu führt `OnDestroy` nicht aufgerufen wird. Die meisten Aktivitäten nicht diese Methode implementieren, da die meisten bereinigen und Herunterfahren erfolgt die `OnPause` und `OnStop` Methoden. Die `OnDestroy` Methode wird in der Regel überschrieben, um lange zu bereinigen Ressourcen ausgeführt, die möglicherweise Ressourcenverlust. Ein Beispiel hierfür ist möglicherweise Hintergrundthreads, die gestartet wurden, im `OnCreate`.

Es werden keine Lebenszyklusmethoden aufgerufen, nachdem die Aktivität zerstört wurde.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) wird aufgerufen, nachdem die Aktivität beendet wurde, bevor er erneut gestartet wird. Ein gutes Beispiel dafür wäre, wenn der Benutzer die Schaltfläche "Start", während Sie sich für eine Aktivität in der Anwendung drückt. Wenn dies geschieht `OnPause` und dann `OnStop` Methoden aufgerufen werden, und die Aktivität in den Hintergrund verschoben, aber nicht zerstört wird. Wenn der Benutzer dann zum Wiederherstellen der Anwendungs über den Task-Manager oder eine ähnliche Anwendung Android aufrufen, wird die `OnRestart` -Methode der Aktivität.

Es gibt keine allgemeingültigen Richtlinien für welche Art von Logik in implementiert werden sollte `OnRestart`. Grund hierfür ist, `OnStart` wird immer aufgerufen, unabhängig davon, ob die Aktivität erstellt wird oder neu gestartet wird, damit alle Ressourcen, die erforderlich sind, von der Aktivität soll, in initialisiert werden `OnStart`, statt `OnRestart`.

Die nächste Lebenszyklusmethode wird aufgerufen, nachdem `OnRestart` werden `OnStart`.

### <a name="back-vs-home"></a>Sichern Sie die Visual Studio. Startseite

Viele Android-Geräte sind zwei unterschiedliche Schaltflächen: eine Schaltfläche "Zurück" und eine Schaltfläche "Home". Ein Beispiel hierfür kann im folgenden Screenshot von Android 4.0.3 angezeigt werden:

[!["Zurück" und Schaltflächen der Startseite](images/image4-sml.png)](images/image4.png#lightbox)

Besteht ein feinen Unterschied zwischen den zwei Schaltflächen, auch wenn sie auf die gleiche Wirkung hat eine Anwendung im Hintergrund zu platzieren, angezeigt werden. Wenn ein Benutzer die Schaltfläche "zurück" klickt, sagen sie Android, dass sie mit der Aktivität ausgeführt werden. Android werden die Aktivität zerstört. Im Gegensatz dazu klickt der Benutzer die Schaltfläche "Start" die Aktivität nur befindet sich in den Hintergrund &ndash; Android wird die Aktivität nicht ebenfalls beendet.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>Verwalten des Status während des Lebenszyklus

Wenn eine Aktivität beendet oder zerstört wird ermöglicht das System den Status der Aktivität für spätere Aktivierung zu speichern.
Diese gespeicherte Zustand wird als Zustand der Instanz bezeichnet. Android bietet drei Optionen zum Speichern des Instanzstatus während des Lebenszyklus der Aktivität:

1. Speichern von primitiven Werten in einer `Dictionary` bekannt als eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) , Android, Zustand speichern verwenden.

1. Eine benutzerdefinierte Klasse erstellen, werden komplexe Werte wie Bitmaps enthalten. Android verwendet diese benutzerdefinierte Klasse, um Zustand zu speichern.

1. Umgehen der Lebenszyklus der Konfiguration ändern und die vollständige Verantwortung für die Zustandsverwaltung in der Aktivität.


Dieser Leitfaden behandelt die ersten beiden Optionen.



### <a name="bundle-state"></a>Bundle-Status

Die wichtigste Option zum Speichern des Instanzstatus ist die Verwendung der ein Schlüssel/Wert-Dictionary-Objekt als bezeichnet ein [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/).
Bedenken Sie, dass, wenn eine Aktivität erstellt, die `OnCreate` Methode ein Pakets als Parameter übergeben wird, dieses Paket kann verwendet werden, um den Zustand der Instanz wiederherzustellen. Es wird nicht empfohlen, ein Paket für eine komplexere Daten zu verwenden, wird Sie nicht schnell oder einfach serialisieren, um Schlüssel/Wert-Paaren (z. B. Bitmaps); Stattdessen sollte es für einfache Werte wie Zeichenfolgen verwendet werden.

Eine Aktivität bietet Methoden, um Hilfe zu speichern und Abrufen des Instanzstatus im Paket:

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; Dies wird von Android aufgerufen, wenn die Aktivität zerstört wird. Aktivitäten können diese Methode implementieren, wenn sie alle Elemente der Schlüssel/Wert-Zustand beibehalten müssen.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; wird aufgerufen, nachdem die `OnCreate` Methode abgeschlossen ist, und bietet eine weitere Verwendungsmöglichkeit für eine Aktivität, um dessen Status wiederherzustellen, nachdem die Initialisierung abgeschlossen ist.

Das folgende Diagramm veranschaulicht, wie diese Methoden verwendet werden:

[![Flussdiagramm der Bundle-Status](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) wird aufgerufen, wenn die Aktivität beendet wird. Sie erhalten einen Paket-Parameter, dem die Aktivität ihren Status in speichern kann. Wenn ein Gerät eine konfigurationsänderung auftritt, können eine Aktivität die `Bundle` -Objekt, das übergeben wird, um den Status der Aktivität durch Überschreiben beibehalten `OnSaveInstanceState`. Beachten Sie z. B. folgenden Code:

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

Der obige Code erhöht, eine ganze Zahl, die mit dem Namen `c` beim Klicken auf eine Schaltfläche mit dem Namen `incrementCounter` geklickt wird, Anzeigen des Ergebnisses in einer `TextView` mit dem Namen `output`. Eine konfigurationsänderung Fall – z. B. wenn das Gerät gedreht wird – der obige Code würde den Wert der verlieren `c` da die `bundle` wäre `null`, wie in der folgenden Abbildung gezeigt:

[![Anzeige wird vorherigen Wert nicht angezeigt werden.](images/07-sml.png)](images/07.png#lightbox)

Zur Beibehaltung des Werts der `c` in diesem Beispiel kann die Aktivität überschreiben `OnSaveInstanceState`, wird den Wert im Paket gespeichert, wie unten dargestellt:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

Nun, wenn das Gerät in die neue Ausrichtung gedreht wird, die ganze Zahl, die im Paket gespeichert ist und wird durch die Zeile abgerufen:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> Es ist wichtig, immer Aufruf der basisimplementierung der `OnSaveInstanceState` , damit der Zustand von der Hierarchie von Inhaltsansichten auch gespeichert werden kann.



##### <a name="view-state"></a>Ansichtszustand

Überschreiben von `OnSaveInstanceState` geeigneten Mechanismus zum Speichern vorübergehender Daten in einer Aktivität auf Änderungen der bildschirmausrichtung, z. B. der Indikator im obigen Beispiel ist. Allerdings die standardmäßige Implementierung des `OnSaveInstanceState` übernimmt speichern vorübergehende Daten in der Benutzeroberfläche für jede Ansicht, solange jede Ansicht eine ID zugewiesen wurde. Angenommen, eine Anwendung eine `EditText` Element im XML-Code wie folgt definiert:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

Da die `EditText` Steuerelement verfügt über eine `id` zugewiesen ist, wenn der Benutzer einige Daten gibt und das Gerät dreht, die Daten werden weiterhin angezeigt, wie unten dargestellt:

[![Daten werden im Querformat beibehalten.](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) werden aufgerufen, nachdem `OnStart`. Es stellt einer Aktivität die Möglichkeit, zu einem beliebigen Zustand wiederherstellen, die auf ein Paket zuvor, während der vergangenen gespeichert wurde `OnSaveInstanceState`. Dies ist das gleiche Paket, die bereitgestellt wird `OnCreate`jedoch.

Der folgende Code zeigt, wie Status wiederhergestellt werden kann `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

Diese Methode ist vorhanden, um einige Flexibilität im Hinblick auf bereitzustellen, wenn der Zustand wiederhergestellt werden soll. Manchmal ist es besser geeignet ist, warten, bis alle Initialisierungen vor dem Wiederherstellen der Instanzstatus fertig sind. Darüber hinaus sollten eine Unterklasse von einer vorhandenen Aktivität nur bestimmte Werte aus den Zustand der Instanz wiederherzustellen. In vielen Fällen ist es nicht erforderlich, außer Kraft setzen `OnRestoreInstanceState`, da die meisten Aktivitäten wiederherstellen können, verwenden das Paket bereitgestellt, um Status `OnCreate`.

Ein Beispiel für das Speichern des Zustands mittels einer `Bundle`, finden Sie in der [Exemplarische Vorgehensweise: Speichern der Aktivitätsstatus](saving-state.md).


#### <a name="bundle-limitations"></a>Bundle-Einschränkungen

Obwohl `OnSaveInstanceState` macht es einfach, um kurzlebige Daten zu speichern, es gelten einige Einschränkungen:

-   Es wird nicht in allen Fällen aufgerufen. Drücken Sie z. B. **Startseite** oder **wieder** zum Beenden einer Aktivitäts führen nicht `OnSaveInstanceState` aufgerufen wird.

-   Übergeben Sie das Paket in `OnSaveInstanceState` dient nicht für große Objekte, z. B. Bilder. Im Fall von großen Objekten Speichern des Objekts aus [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) ist zu bevorzugen, wie im folgenden erläutert.

-   Daten gespeichert, indem Sie das Paket werden serialisiert, dies kann zu Verzögerungen führen.

Bundle-Status eignet sich für einfache Daten, die nicht viel Arbeitsspeicher verwendet, während *ohne Konfigurationsaufwand Instanzdaten* ist nützlich für komplexere Daten oder Daten, die abzurufenden aufwendig ist, z. B. über einen Webdienstaufruf oder ein komplizierter Datenbankabfrage. Nicht-konfigurationsinstanz Daten nach Bedarf in einem Objekt gespeichert. Der nächste Abschnitt führt `OnRetainNonConfigurationInstance` als eine Möglichkeit, komplexere Datentypen über Änderungen an der Konfiguration beibehalten.


### <a name="persisting-complex-data"></a>Beibehalten von komplexen Daten

Zusätzlich zum Beibehalten von Daten in das Paket, Android unterstützt auch Speichern von Daten durch das Überschreiben von [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) und die Rückgabe einer Instanz von einem `Java.Lang.Object` enthält die Daten beibehalten werden. Es gibt zwei Hauptvorteile der Verwendung von `OnRetainNonConfigurationInstance` , Zustand speichern:

-   Das von zurückgegebene Objekt `OnRetainNonConfigurationInstance` gut für größere und komplexere Datentypen ausgeführt werden, da Speicher dieses Objekt behält.

-   Die `OnRetainNonConfigurationInstance` Methode ist bei Bedarf aufgerufen werden, und nur bei Bedarf. Dies ist wirtschaftlicher als die Verwendung eines manuellen Cache.

Mithilfe von `OnRetainNonConfigurationInstance` ist für Szenarien, in denen es teuer, zum Abrufen der Daten mehrere Male geeignet ist, z. B. Aufrufe des Webdiensts. Betrachten Sie beispielsweise den folgenden Code, mit dem Twitter gesucht:

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

Dieser Code Ruft die Ergebnisse aus dem Internet, die als JSON formatierten ab, analysiert sie und präsentiert die Ergebnisse dann in einer Liste aus, wie im folgenden Screenshot gezeigt:

[![Ergebnisse, die auf dem Bildschirm angezeigt](images/06-sml.png)](images/06.png#lightbox)

Tritt eine konfigurationsänderung – z. B. wenn ein Gerät gedreht wird - wird der Code der Prozess wiederholt. Um die ursprünglich abgerufenen Ergebnisse wiederverwenden, und nicht dazu, dass unnötige, redundante Netzwerkaufrufe, können wir `OnRetainNonconfigurationInstance` um die Ergebnisse zu speichern, wie unten dargestellt:

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

Nachdem die ursprünglichen Ergebnisse von abgerufen werden, wenn das Gerät gedreht wird, die `LastNonConfiguartionInstance` Eigenschaft. In diesem Beispiel besteht das Ergebnis aus einem `string[]` mit Tweets. Da `OnRetainNonConfigurationInstance` erfordert, dass eine `Java.Lang.Object` zurückgegeben werden, die `string[]` wird in einer Klasse, Unterklassen umschlossen `Java.Lang.Object`, wie unten dargestellt:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

Beispielsweise möchten, verwenden Sie eine `TextView` als das von zurückgegebene Objekt `OnRetainNonConfigurationInstance` verlieren die Aktivität, wie im folgenden Code dargestellt:

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

In diesem Abschnitt haben wir gelernt, wie einfache Daten mit beibehalten der `Bundle`, und speichern Sie eine komplexere Datentypen mit `OnRetainNonConfigurationInstance`.

## <a name="summary"></a>Zusammenfassung

Der Lebenszyklus der Android-Aktivität bietet ein leistungsfähiges Framework für die Zustandsverwaltung von Aktivitäten innerhalb einer Anwendung, aber es kann schwierig zu verstehen und zu implementieren sein. In diesem Kapitel eingeführt, die verschiedenen Zustände, die eine Aktivität durchlaufen kann, während seiner Lebensdauer als auch die Lebenszyklusmethoden, die diesen Status zugeordnet sind. Als Nächstes wurde Anleitungen, welche Art von Logik in einer dieser Methoden ausgeführt werden soll.


## <a name="related-links"></a>Verwandte Links

- [Verarbeiten der Drehung](~/android/app-fundamentals/handling-rotation.md)
- [Android Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)
