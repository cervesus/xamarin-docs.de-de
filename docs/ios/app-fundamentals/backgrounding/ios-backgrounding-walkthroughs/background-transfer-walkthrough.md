---
title: Hintergrundübertragung und von NSURLSession in Xamarin.iOS
description: Dieses Dokument enthält eine exemplarische Vorgehensweise, die veranschaulicht, wie Sie hintergrundübertragung und von NSUrlSession zum Starten Sie des Downloads der ein großes Bild, und diesen Download fortgesetzt, wenn die app im Hintergrund platziert wird.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e525388290d92901e68e61f1ffa81866f5aac4d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392219"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Hintergrundübertragung und von NSURLSession in Xamarin.iOS

Eine hintergrundübertragung wird initiiert, konfigurieren Sie einen Hintergrund `NSURLSession` und einreihen hochladen oder Herunterladen von Aufgaben. Wenn Sie Aufgaben ausführen, während die Anwendung in diesem Fall, angehalten oder beendet wird, benachrichtigt iOS die Anwendung durch Aufrufen von den Abschlusshandler in der Anwendung *AppDelegate*. Das folgende Diagramm veranschaulicht dies in Aktion:

 [![](background-transfer-walkthrough-images/transfer.png "Eine hintergrundübertragung wird initiiert, indem Sie einen Hintergrund von NSURLSession konfigurieren und einreihen hochladen oder Herunterladen von Aufgaben")](background-transfer-walkthrough-images/transfer.png#lightbox)

Sehen wir uns an, wie im Code aussieht.

## <a name="configuring-a-background-session"></a>Konfigurieren eine Hintergrundsitzung

Um eine hintergrundsitzung zu machen, erstellen Sie ein neues `NSUrlSession` und konfigurieren sie mithilfe einer `NSUrlSessionConfiguration` Objekt.

Das Konfigurationsobjekt bestimmt, was die Sitzung möglich, und die Arten von Aufgaben, die sie ausführen kann.
Sitzungen, die konfiguriert wird, mit der `CreateBackgroundSessionConfiguration` Methode in einem separaten Prozess ausgeführt, und führen Sie zum Beibehalten von Daten sowie von längerer Akkulaufzeit discretionary (WLAN)-Übertragungen.
Im folgenden Codebeispiel wird veranschaulicht, korrekten Einrichtung der einer Hintergrund Übertragung-Sitzung unter Verwendung der `CreateBackgroundSessionConfiguration` -Methode und eines eindeutigen Zeichenfolgenbezeichners:

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

Abgesehen von ein Konfigurationsobjekt erfordert eine Sitzung auch ein Delegat für die Sitzung und einer Warteschlange.
Die Warteschlange bestimmt die Reihenfolge, in der die Aufgaben abgeschlossen werden. Der Delegat für die Sitzung chaperones des Übertragungsprozesses und führt die Authentifizierung, Zwischenspeichern und andere Sitzung-bezogene Probleme.

## <a name="working-with-tasks-and-delegates"></a>Arbeiten mit Aufgaben und Delegaten

Nun, dass wir eine hintergrundsitzung konfiguriert haben, starten wir Aufgaben, die Übertragung behandelt wird. Wir können mitverfolgen diese Aufgaben mit der ein `NSUrlSessionDelegate` Instanz ein Delegaten für die Sitzung mit der Bezeichnung. Der Delegat für die Sitzung ist für die Aktivierung einer Anwendung angehalten oder beendet wurden bzw. im Hintergrund Handle-Authentifizierung, Fehler oder Abschluss der Übertragung verantwortlich.

Ein `NSUrlSessionDelegate` bietet die folgenden grundlegenden Methoden zum Überprüfen des Übertragungsstatus:

-  *DidFinishEventsForBackgroundSession* – diese Methode wird aufgerufen, wenn alle Aufgaben abgeschlossen haben, und die Übertragung abgeschlossen ist.
-  *DidReceiveChallenge* - wird aufgerufen, um Benutzer bei der Autorisierung erforderlich ist.
-  *DidBecomeInvalidWithError* -wird aufgerufen, wenn die `NSURLSession` ungültig wird.


Hintergrund-Sitzungen erfordern spezialisiertere Delegaten je nach Art der Aufgaben, die ausgeführt werden. Hintergrund-Sitzungen werden auf zwei Arten von Aufgaben beschränkt:

-  *Hochladen von Aufgaben* -Aufgaben vom Typ `NSUrlSessionUploadTask` verwenden die `NSUrlSessionTaskDelegate` , erbt von `NSUrlSessionDelegate` . Dieser Delegat enthält, dass zusätzliche Methoden zum Nachverfolgen von Status, Handle HTTP-Umleitung und mehr hochladen.
-  *Herunterladen von Aufgaben* -Aufgaben vom Typ `NSUrlSessionDownloadTask` verwenden die `NSUrlSessionDownloadDelegate` , erbt von `NSUrlSessionTaskDelegate` . Dieser Delegat enthält, dass alle Methoden zum Hochladen von Aufgaben als auch Download-spezifische Methoden zum Überwachen des Downloadstatus und zu ermitteln, wann ein Download-Task fortgesetzt oder abgeschlossen wurde.


Der folgende Code definiert eine Aufgabe, die verwendet werden kann, um ein Image aus einer URL herunterladen. Wir durch Aufrufen der Aufgabe anstößt `CreateDownloadTask` auf unsere hintergrundsitzung, und klicken Sie auf die URL-Anforderung übergeben:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Als Nächstes erstellen wir einen neue Sitzung-Download-Delegaten zum Nachverfolgen aller Downloadtasks in dieser Sitzung:

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

Wenn wir den Fortschritt eines Herunterladevorgangs ermitteln möchten, können wir überschreiben die `DidWriteData` Methode, um den Status nachverfolgen und sogar die Benutzeroberfläche aktualisiert. Aktualisierungen der Benutzeroberfläche werden sofort angezeigt, wenn die Anwendung im Vordergrund ist, oder wird warten für den Benutzer beim nächsten Öffnen die Anwendung.

Die Sitzung-Delegat-API bietet eine umfassende-Toolkit für die Interaktion mit Aufgaben. Eine vollständige Liste der Sitzung Methoden delegieren, finden Sie in der `NSUrlSessionDelegate` -API-Dokumentation.

> [!IMPORTANT]
> Hintergrund-Sitzungen werden in einem Hintergrundthread gestartet, damit alle Aufrufe an die Aktualisierung der Benutzeroberfläche durch den Aufruf explizit im UI-Thread ausgeführt werden müssen `InvokeOnMainThread` iOS Beenden der app zu vermeiden. 


## <a name="handling-transfer-completion"></a>Behandlung von Übertragung abgeschlossen

Der letzte Schritt besteht darin, die Anwendung zu signalisieren, wenn alle Aufgaben im Zusammenhang mit der Sitzung abgeschlossen haben, und behandeln Sie den neuen Inhalt.

In der *AppDelegate*, abonnieren Sie den `HandleEventsForBackgroundUrl` Ereignis. Wenn die Anwendung in den Hintergrund, und eine übertragungssitzung ausgeführt wird, wird diese Methode wird aufgerufen, und das System übergibt uns einen Abschlusshandler:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Wir verwenden den Abschlusshandler können iOS kennen, wenn die Anwendung erfolgt Verarbeitung.

Denken Sie daran, dass eine Sitzung mehrere Aufgaben zum Verarbeiten einer Übertragung erstellen kann. Wenn die letzte Aufgabe abgeschlossen ist, wird eine Anwendung angehaltene oder beendete neu gestartet, in den Hintergrund. Klicken Sie dann die Anwendung die Verbindung zum die `NSURLSession` verwenden, die eindeutige Sitzungs-ID und ruft `DidFinishEventsForBackgroundSession` für den Delegaten für die Sitzung. Diese Methode ist die Anwendung die Möglichkeit, neue Inhalte, einschließlich der Aktualisierung der Benutzeroberfläche entsprechend die Ergebnissen der Übertragung zu verarbeiten:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Nachdem wir die Behandlung neuer Inhalt fertig sind, rufen wir den Abschlusshandler, um dem System mitzuteilen, dass es sicher ist, erstellen Sie eine Momentaufnahme der Anwendung, und wechseln zurück in den Ruhezustand handelt:

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

In dieser exemplarischen Vorgehensweise behandelt es die grundlegenden Schritte zum Implementieren von dem Hintergrundübertragungsdienst in iOS 7.



## <a name="related-links"></a>Verwandte Links

- [Einfache Hintergrundübertragung (Beispiel)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
