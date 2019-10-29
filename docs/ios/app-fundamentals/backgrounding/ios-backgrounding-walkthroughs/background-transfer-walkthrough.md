---
title: Hintergrund Übertragung und nsurlsession in xamarin. IOS
description: Dieses Dokument enthält eine exemplarische Vorgehensweise, in der veranschaulicht wird, wie die Hintergrund Übertragung und nsurlsession verwendet wird, um den Download eines großen Images zu starten und den Download fortzusetzen, wenn die APP im Hintergrund platziert wird.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 6004598b215c85b70b057ff156c3a905a0879138
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010718"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Hintergrund Übertragung und nsurlsession in xamarin. IOS

Eine Hintergrund Übertragung wird durch das Konfigurieren eines Hintergrund `NSURLSession` und das Einreihen von Upload-oder Download Tasks initiiert. Wenn Aufgaben abgeschlossen sind, während die Anwendung mit einem Sicherungs Satz, einer Unterbrechung oder einem Abbruch abgeschlossen ist, wird die Anwendung von IOS benachrichtigt, indem der Abschluss Handler im *appdelegaten*der Anwendung aufgerufen wird. Dies wird in der folgenden Abbildung veranschaulicht:

 [![](background-transfer-walkthrough-images/transfer.png "A background transfer is initiated by configuring a background NSURLSession and enqueuing upload or download tasks")](background-transfer-walkthrough-images/transfer.png#lightbox)

Sehen wir uns an, wie das im Code aussieht.

## <a name="configuring-a-background-session"></a>Konfigurieren einer Hintergrund Sitzung

Erstellen Sie eine neue `NSUrlSession`, und konfigurieren Sie Sie mit einem `NSUrlSessionConfiguration`-Objekt, um eine Hintergrund Sitzung vorzunehmen.

Das Konfigurationsobjekt bestimmt, welche Aufgaben die Sitzung ausführen kann und welche Aufgaben Sie ausführen können.
Mit der `CreateBackgroundSessionConfiguration`-Methode konfigurierte Sitzungen werden in einem separaten Prozess ausgeführt, und es werden freigegebene (WiFi) Übertragungen ausgeführt, um die Daten und die Akku Lebensdauer beizubehalten
Das folgende Codebeispiel veranschaulicht das ordnungsgemäße Einrichten einer Hintergrund Übertragungs Sitzung mithilfe der `CreateBackgroundSessionConfiguration`-Methode und eines eindeutigen Zeichen folgen Bezeichners:

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

Nachdem wir nun eine Hintergrund Sitzung konfiguriert haben, beginnen wir mit der Aufgabe, die Übertragung zu verarbeiten. Wir können diese Aufgaben mit einer `NSUrlSessionDelegate` Instanz nachverfolgen, die als Sitzungs Delegat bezeichnet wird. Der Sitzungs Delegat ist dafür verantwortlich, eine beendete oder angehaltene Anwendung im Hintergrund zu reaktivieren, um die Authentifizierung, Fehler oder den Übertragungs Abschluss zu verarbeiten.

Eine `NSUrlSessionDelegate` stellt die folgenden grundlegenden Methoden zum Überprüfen des Übertragungs Status bereit:

- *Didfinisheventsforbackgroundsession* : Diese Methode wird aufgerufen, wenn alle Tasks abgeschlossen sind und die Übertragung abgeschlossen ist.
- *Didreceivechallenge* : wird aufgerufen, um Anmelde Informationen anzufordern, wenn eine Autorisierung erforderlich ist.
- *Didbecomeinvalidwitherror* : wird aufgerufen, wenn die `NSURLSession` ungültig wird.

Hintergrund Sitzungen erfordern speziellere Delegaten, abhängig von den Tasks, die ausgeführt werden. Hintergrund Sitzungen sind auf zwei Arten von Aufgaben beschränkt:

- *Aufgaben hochladen* : Aufgaben vom Typ `NSUrlSessionUploadTask` die `NSUrlSessionTaskDelegate` verwenden, die von `NSUrlSessionDelegate` erbt. Dieser Delegat stellt zusätzliche Methoden zum Nachverfolgen des uploadfortschritts, zum Verarbeiten der HTTP-Umleitung usw.
- *Tasks herunterladen* : Aufgaben vom Typ `NSUrlSessionDownloadTask` die `NSUrlSessionDownloadDelegate` verwenden, die von `NSUrlSessionTaskDelegate` erbt. Dieser Delegat stellt alle Methoden zum Hochladen von Aufgaben sowie Download spezifische Methoden bereit, um den Download Fortschritt zu verfolgen und zu ermitteln, wann eine Download Aufgabe fortgesetzt oder abgeschlossen wurde.

Der folgende Code definiert einen Task, der zum Herunterladen eines Bilds aus einer URL verwendet werden kann. Wir beginnen mit der Aufgabe, indem wir `CreateDownloadTask` in unserer Hintergrund Sitzung aufrufen und die URL-Anforderung übergeben:

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

Wenn Sie den Fortschritt einer Download Aufgabe ermitteln möchten, können Sie die `DidWriteData`-Methode außer Kraft setzen, um den Fortschritt zu verfolgen und sogar die Benutzeroberfläche zu aktualisieren. Aktualisierungen der Benutzeroberfläche werden sofort angezeigt, wenn sich die Anwendung im Vordergrund befindet oder wenn Sie auf den Benutzer wartet, wenn Sie die Anwendung das nächste Mal öffnen.

Die Sitzungs delegatapi bietet ein breites Toolkit für die Interaktion mit Aufgaben. Eine vollständige Liste der Sitzungs Delegatmethoden finden Sie in der `NSUrlSessionDelegate`-API-Dokumentation.

> [!IMPORTANT]
> Hintergrund Sitzungen werden in einem Hintergrund Thread gestartet, sodass alle Aufrufe zur Aktualisierung der Benutzeroberfläche explizit im UI-Thread ausgeführt werden müssen, indem `InvokeOnMainThread` aufgerufen wird, um das Beenden der app durch IOS zu vermeiden. 

## <a name="handling-transfer-completion"></a>Verarbeiten der Übertragungs Vervollständigung

Der letzte Schritt besteht darin, die Anwendung zu informieren, wenn alle der Sitzung zugeordneten Aufgaben abgeschlossen sind, und den neuen Inhalt zu verarbeiten.

Abonnieren Sie das `HandleEventsForBackgroundUrl`-Ereignis im *appdelegaten*. Wenn die Anwendung in den Hintergrund wechselt und eine Übertragungs Sitzung ausgeführt wird, wird diese Methode aufgerufen, und das System übergibt uns einen Abschluss Handler:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Wir verwenden den Abschluss Handler, um IOS zu informieren, wann die Verarbeitung der Anwendung abgeschlossen ist.

Denken Sie daran, dass eine Sitzung mehrere Aufgaben zum Verarbeiten einer Übertragung erzeugen kann. Wenn die letzte Aufgabe abgeschlossen ist, wird eine angehaltene oder beendete Anwendung im Hintergrund neu gestartet. Anschließend stellt die Anwendung die Verbindung mit dem `NSURLSession` mithilfe des eindeutigen Sitzungs Bezeichners erneut her und ruft `DidFinishEventsForBackgroundSession` für den Sitzungs Delegaten auf. Diese Methode bietet die Möglichkeit, neuen Inhalt zu verarbeiten, einschließlich der Aktualisierung der Benutzeroberfläche, um die Ergebnisse der Übertragung widerzuspiegeln:

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
