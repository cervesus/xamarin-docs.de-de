---
title: "Exemplarische Vorgehensweise – mit Hintergrundübertragungsdienst und NSURLSession"
description: "In dieser exemplarischen Vorgehensweise verwenden wir den Hintergrundübertragungsdienst und NSURLSession-API zum Kickoff ein großes Bild, das weiterhin herunterladen, wenn die app im Hintergrund heruntergeladen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 015bce612f369797f0540a0cb55f71f420f007a2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-background-transfer-service-and-nsurlsession"></a>Exemplarische Vorgehensweise – mit Hintergrundübertragungsdienst und NSURLSession

_In dieser exemplarischen Vorgehensweise verwenden wir den Hintergrundübertragungsdienst und NSURLSession-API zum Kickoff ein großes Bild, das weiterhin herunterladen, wenn die app im Hintergrund heruntergeladen._

Eine hintergrundübertragung wird durch Konfigurieren von einem Hintergrund initiiert `NSURLSession` und einreihen hochladen oder Herunterladen von Aufgaben. Wenn Aufgaben abgeschlossen haben, während die Anwendung backgrounded, angehalten oder beendet wird, werden iOS die Anwendung durch Aufrufen der Abschlusshandler in der Anwendungsverzeichnis benachrichtigt *AppDelegate*. Das folgende Diagramm veranschaulicht diese in Aktion:

 [ ![](background-transfer-walkthrough-images/transfer.png "Eine hintergrundübertragung wird von einem Hintergrund NSURLSession konfigurieren initiiert und einreihen hochladen und Herunterladen von Aufgaben")](background-transfer-walkthrough-images/transfer.png)

Sehen wir uns an, wie dies im Code aussieht.

## <a name="configuring-a-background-session"></a>Konfigurieren einer Hintergrundsitzung

Um eine hintergrundsitzung zu machen, erstellen Sie ein neues `NSUrlSession` und konfigurieren Sie ihn mit einer `NSUrlSessionConfiguration` Objekt.

Das Konfigurationsobjekt bestimmt, was die Sitzung tun kann, und die Arten von Aufgaben, die sie ausführen.
Sitzungen konfiguriert wird, mithilfe der `CreateBackgroundSessionConfiguration` Methode in einem separaten Prozess ausgeführt wird, und discretionary (WLAN) Übertragungen zum Beibehalten von Daten und Akkulaufzeit ausführen.
Im folgenden Codebeispiel wird veranschaulicht, ordnungsgemäßen Einrichtung einer Sitzung mit Hintergrund Übertragung der `CreateBackgroundSessionConfiguration` -Methode und ein eindeutiger Zeichenfolgenbezeichner:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate, new NSOperationQueue());

}
```

Abgesehen von ein Konfigurationsobjekt erfordert eine Sitzung auch einen Delegaten für die Sitzung und einer Warteschlange.
Die Warteschlange bestimmt die Reihenfolge, in der die Aufgaben abgeschlossen werden. Der Delegat für die Sitzung chaperones des Übertragungsprozesses und verarbeitet die Authentifizierung, caching und andere Probleme in Bezug auf Sitzung.

## <a name="working-with-tasks-and-delegates"></a>Arbeiten mit Aufgaben und Delegaten

Nun, dass wir eine hintergrundsitzung konfiguriert haben, starten wir Aufgaben der Übertragung behandelt wird. Wir können Nachverfolgen von diese Aufgaben mit der ein `NSUrlSessionDelegate` Instanz den so genannten Stellvertreter Sitzung. Der Delegat für die Sitzung ist verantwortlich für eine beendete oder angehaltene Anwendung im Hintergrund Handle Authentifizierung, Fehler oder Beendigung der Übertragung der Ergebnisse.

Ein `NSUrlSessionDelegate` bietet die folgenden grundlegenden Methoden zum Überprüfen des Übertragungsstatus:

-  *DidFinishEventsForBackgroundSession* – diese Methode wird aufgerufen, wenn alle Aufgaben abgeschlossen haben, und die Übertragung abgeschlossen ist.
-  *DidReceiveChallenge* - wird aufgerufen, um Anforderung Anmeldeinformationen, wenn eine Autorisierung erforderlich ist.
-  *DidBecomeInvalidWithError* -wird aufgerufen, wenn die `NSURLSession` ungültig wird.


Hintergrund-Sitzungen erfordert spezialisiertere Delegaten abhängig von den Aufgaben, die ausgeführt werden. Hintergrund-Sitzungen werden auf zwei Arten von Aufgaben beschränkt:

-  *Hochladen von Aufgaben* -Aufgaben vom Typ `NSUrlSessionUploadTask` verwenden die `NSUrlSessionTaskDelegate` , erbt die `NSUrlSessionDelegate` . Dieser Delegat bietet zusätzliche Methoden zum Nachverfolgen von Status und die HTTP-Umleitung Handle hochladen.
-  *Herunterladen von Aufgaben* -Aufgaben vom Typ `NSUrlSessionDownloadTask` verwenden die `NSUrlSessionDownloadDelegate` , erbt die `NSUrlSessionTaskDelegate` . Dieser Delegat enthält, dass alle Methoden zum Hochladen, Aufgaben als auch Download-spezifischen Methoden zum Nachverfolgen von des Downloadstatus und bestimmen, wann ein Download-Task abgeschlossen ist oder wurde fortgesetzt.


Der folgende Code definiert eine Aufgabe, die verwendet werden kann, um ein Bild von einer URL herunterzuladen. Wir durch Aufrufen der Aufgabe Kickoff `CreateDownloadTask` auf unserer hintergrundsitzung und übergeben der URL-Anforderung:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Als Nächstes erstellen wir einen neue Sitzung Download Delegaten zum Nachverfolgen aller Downloadtasks in dieser Sitzung:

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

Wenn wir den Fortschritt einer Downloadaufgabe ermitteln möchten, können wir überschreiben die `DidWriteData` Methode zum Nachverfolgen des Fortschritts und sogar die Benutzeroberfläche aktualisiert. UI-Updates werden sofort angezeigt, wenn die Anwendung im Vordergrund oder wird Warten der Benutzer das nächste Mal die Anwendung zu öffnen.

Die Sitzung Delegaten-API ermöglicht eine umfassende Toolkit für die Interaktion mit Aufgaben. Eine vollständige Liste der Sitzung Methoden delegieren, beziehen sich auf die `NSUrlSessionDelegate` -API-Dokumentation.

> [!IMPORTANT]
> **Hinweis**: Hintergrund-Sitzungen werden in einem Hintergrundthread gestartet, damit alle Aufrufe zum Aktualisieren der Benutzeroberfläche durch Aufrufen explizit auf den UI-Thread ausgeführt werden müssen `InvokeOnMainThread` iOS Beenden der app zu vermeiden. 


## <a name="handling-transfer-completion"></a>Behandlung Übertragung Abschluss

Der letzte Schritt besteht, mit der Anwendung zu signalisieren, wenn alle Aufgaben im Zusammenhang mit der Sitzung abgeschlossen wurden, und behandeln Sie den neuen Inhalt.

In der *AppDelegate*, abonnieren Sie die `HandleEventsForBackgroundUrl` Ereignis. Wenn die Anwendung wechselt in den Hintergrund, und eine übertragungssitzung ausgeführt wird, diese Methode wird aufgerufen, und übergibt das System uns einen Abschlusshandler:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Wir wird den Abschlusshandler verwenden, damit Sie wissen, wann die Anwendung erfolgt iOS Verarbeitung.

Beachten Sie, dass eine Sitzung mehrere Aufgaben zum Verarbeiten einer Übertragung erzeugen kann. Nach Abschluss die vorhergehenden Aufgabe ist eine angehaltene oder beendete Anwendung erneut gestartet, in den Hintergrund. Klicken Sie dann die Anwendung die Verbindung zum der `NSURLSession` mithilfe der eindeutige Sitzungsbezeichner, und ruft `DidFinishEventsForBackgroundSession` für die Sitzung Delegaten. Diese Methode ist die Anwendung Möglichkeit, neue Inhalte, einschließlich der Aktualisierung der Benutzeroberfläche entsprechend der Ergebnisse der Übertragung zu verarbeiten:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Sobald wir Behandlung neuer Inhalt fertig sind, rufen wir den Abschlusshandler, damit das System weiß, dass es sicher ist, erstellen Sie eine Momentaufnahme der Anwendung, und wechseln zurück in den Ruhezustand ist:

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

In dieser exemplarischen Vorgehensweise behandelt die grundlegenden Schritte zum Implementieren der Hintergrundübertragungsdienst in iOS 7.



## <a name="related-links"></a>Verwandte Links

- [Einfache Hintergrundübertragung (Beispiel)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
