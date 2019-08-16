---
title: Hintergrund Übertragung und nsurlsession in xamarin. IOS
description: Dieses Dokument enthält eine exemplarische Vorgehensweise, in der veranschaulicht wird, wie die Hintergrund Übertragung und nsurlsession verwendet wird, um den Download eines großen Images zu starten und den Download fortzusetzen, wenn die APP im Hintergrund platziert wird.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a0c659904be2f6755ff4a32853e141ee8572e839
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521247"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Hintergrund Übertragung und nsurlsession in xamarin. IOS

Eine Hintergrund Übertragung wird durch Konfigurieren eines Hintergrunds `NSURLSession` und Einreihen von Upload-oder Download Tasks initiiert. Wenn Aufgaben abgeschlossen sind, während die Anwendung mit einem Sicherungs Satz, einer Unterbrechung oder einem Abbruch abgeschlossen ist, wird die Anwendung von IOS benachrichtigt,indem der Abschluss Handler im appdelegaten der Anwendung aufgerufen wird. Dies wird in der folgenden Abbildung veranschaulicht:

 [![](background-transfer-walkthrough-images/transfer.png "Eine Hintergrund Übertragung wird durch Konfigurieren einer nsurlsession-Hintergrund Sitzung und Einreihen von Upload-oder Download Aufgaben in die Warteschlange initiiert.")](background-transfer-walkthrough-images/transfer.png#lightbox)

Sehen wir uns an, wie das im Code aussieht.

## <a name="configuring-a-background-session"></a>Konfigurieren einer Hintergrund Sitzung

Um eine Hintergrund Sitzung zu erstellen, erstellen Sie `NSUrlSession` eine neue, und konfigurieren `NSUrlSessionConfiguration` Sie Sie mithilfe eines-Objekts.

Das Konfigurationsobjekt bestimmt, welche Aufgaben die Sitzung ausführen kann und welche Aufgaben Sie ausführen können.
Mit der `CreateBackgroundSessionConfiguration` -Methode konfigurierte Sitzungen werden in einem separaten Prozess ausgeführt, und es werden freigegebene (WiFi) Übertragungen durchgeführt, um die Daten und die Akku Lebensdauer
Das folgende Codebeispiel veranschaulicht das ordnungsgemäße Einrichten einer Hintergrund Übertragungs Sitzung mithilfe `CreateBackgroundSessionConfiguration` der-Methode und eines eindeutigen Zeichen folgen Bezeichners:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

Abgesehen von einem Konfigurationsobjekt erfordert eine Sitzung auch einen Sitzungs Delegaten und eine Warteschlange.
Die Warteschlange bestimmt die Reihenfolge, in der die Aufgaben ausgeführt werden. Der Sitzungs Delegat verketstet den Übertragungsprozess und behandelt die Authentifizierung, das Caching und andere Sitzungs bezogene Probleme.

## <a name="working-with-tasks-and-delegates"></a>Arbeiten mit Aufgaben und Delegaten

Nachdem wir nun eine Hintergrund Sitzung konfiguriert haben, beginnen wir mit der Aufgabe, die Übertragung zu verarbeiten. Wir können diese Aufgaben mithilfe einer `NSUrlSessionDelegate` Instanz, die als Sitzungs Delegat bezeichnet wird, nachverfolgen. Der Sitzungs Delegat ist dafür verantwortlich, eine beendete oder angehaltene Anwendung im Hintergrund zu reaktivieren, um die Authentifizierung, Fehler oder den Übertragungs Abschluss zu verarbeiten.

Eine `NSUrlSessionDelegate` stellt die folgenden grundlegenden Methoden zum Überprüfen des Übertragungs Status bereit:

- *Didfinisheventsforbackgroundsession* : Diese Methode wird aufgerufen, wenn alle Tasks abgeschlossen sind und die Übertragung abgeschlossen ist.
- *Didreceivechallenge* : wird aufgerufen, um Anmelde Informationen anzufordern, wenn eine Autorisierung erforderlich ist.
- *Didbecomeinvalidwitherror* : wird aufgerufen, `NSURLSession` wenn das ungültig wird.


Hintergrund Sitzungen erfordern speziellere Delegaten, abhängig von den Tasks, die ausgeführt werden. Hintergrund Sitzungen sind auf zwei Arten von Aufgaben beschränkt:

- *Aufgaben hochladen* : Aufgaben des Typs `NSUrlSessionUploadTask` verwenden den `NSUrlSessionTaskDelegate` , der von `NSUrlSessionDelegate` erbt. Dieser Delegat stellt zusätzliche Methoden zum Nachverfolgen des uploadfortschritts, zum Verarbeiten der HTTP-Umleitung usw.
- `NSUrlSessionDownloadDelegate` *Tasks herunterladen* : `NSUrlSessionTaskDelegate` verwenden Sie den, der von erbt. `NSUrlSessionDownloadTask` Dieser Delegat stellt alle Methoden zum Hochladen von Aufgaben sowie Download spezifische Methoden bereit, um den Download Fortschritt zu verfolgen und zu ermitteln, wann eine Download Aufgabe fortgesetzt oder abgeschlossen wurde.


Der folgende Code definiert einen Task, der zum Herunterladen eines Bilds aus einer URL verwendet werden kann. Wir beginnen mit der Aufgabe, indem `CreateDownloadTask` wir in unserer Hintergrund Sitzung aufrufen und die URL-Anforderung übergeben:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Als Nächstes erstellen wir einen neuen Sitzungs Download Delegaten, um alle Download Tasks in dieser Sitzung nachzuverfolgen:

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Wenn Sie den Fortschritt einer Download Aufgabe ermitteln möchten, können Sie die `DidWriteData` -Methode überschreiben, um den Fortschritt zu verfolgen und sogar die Benutzeroberfläche zu aktualisieren. Aktualisierungen der Benutzeroberfläche werden sofort angezeigt, wenn sich die Anwendung im Vordergrund befindet oder wenn Sie auf den Benutzer wartet, wenn Sie die Anwendung das nächste Mal öffnen.

Die Sitzungs delegatapi bietet ein breites Toolkit für die Interaktion mit Aufgaben. Eine vollständige Liste der Sitzungs Delegatmethoden finden Sie in `NSUrlSessionDelegate` der API-Dokumentation.

> [!IMPORTANT]
> Hintergrund Sitzungen werden in einem Hintergrund Thread gestartet, sodass alle Aufrufe zur Aktualisierung der Benutzeroberfläche explizit im UI-Thread ausgeführt werden müssen, `InvokeOnMainThread` indem aufgerufen wird, um das Beenden der app durch IOS zu vermeiden. 


## <a name="handling-transfer-completion"></a>Verarbeiten der Übertragungs Vervollständigung

Der letzte Schritt besteht darin, die Anwendung zu informieren, wenn alle der Sitzung zugeordneten Aufgaben abgeschlossen sind, und den neuen Inhalt zu verarbeiten.

Abonnieren Siedas `HandleEventsForBackgroundUrl` Ereignis im appdelegaten. Wenn die Anwendung in den Hintergrund wechselt und eine Übertragungs Sitzung ausgeführt wird, wird diese Methode aufgerufen, und das System übergibt uns einen Abschluss Handler:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Wir verwenden den Abschluss Handler, um IOS zu informieren, wann die Verarbeitung der Anwendung abgeschlossen ist.

Denken Sie daran, dass eine Sitzung mehrere Aufgaben zum Verarbeiten einer Übertragung erzeugen kann. Wenn die letzte Aufgabe abgeschlossen ist, wird eine angehaltene oder beendete Anwendung im Hintergrund neu gestartet. Anschließend stellt die Anwendung `NSURLSession` mithilfe des eindeutigen Sitzungs Bezeichners erneut eine Verbindung mit her, und ruft für den Sitzungs Delegaten `DidFinishEventsForBackgroundSession` auf. Diese Methode bietet die Möglichkeit, neuen Inhalt zu verarbeiten, einschließlich der Aktualisierung der Benutzeroberfläche, um die Ergebnisse der Übertragung widerzuspiegeln:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Nachdem die Verarbeitung neuer Inhalte abgeschlossen ist, wird der Abschluss Handler aufgerufen, um dem System mitzuteilen, dass es sicher ist, eine Momentaufnahme der Anwendung zu erstellen und wieder in den Standbymodus zurückzukehren:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

In dieser exemplarischen Vorgehensweise haben wir die grundlegenden Schritte zur Implementierung des Background Transfer Service in ios 7 behandelt.



## <a name="related-links"></a>Verwandte Links

- [Einfache Hintergrund Übertragung (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
