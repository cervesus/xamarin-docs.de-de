---
title: iOS-Erweiterungen in Xamarin.iOS
description: In diesem Dokument werden Erweiterungen beschrieben, bei denen es sich um Widgets handelt, die von IOS im Standardkontext wie innerhalb des Benachrichtigungs Centers dargestellt werden Es wird erläutert, wie eine Erweiterung erstellt und von der übergeordneten App aus kommuniziert wird.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 05/12/2020
ms.openlocfilehash: 540fd0180899f8eb7c1b171148be81c5d541613b
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997500"
---
# <a name="ios-extensions-in-xamarinios"></a>IOS-Erweiterungen in xamarin. IOS

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Erstellen von Erweiterungen in ios-Videos**

Erweiterungen, wie in ios 8 eingeführt, sind spezialisiert, `UIViewControllers` die von IOS in Standard Kontexten wie z. b. innerhalb des **Benachrichtigungs Centers**präsentiert werden, als benutzerdefinierte Tastaturtypen, die vom Benutzer angefordert werden, um spezialisierte Eingaben oder andere Kontexte auszuführen, wie z. b. die Bearbeitung eines Fotos, bei dem die Erweiterung spezielle Effektfilter

Alle Erweiterungen werden zusammen mit einer Container-App installiert (wobei beide Elemente mithilfe der vereinheitlichten 64-Bit-APIs geschrieben werden) und werden von einem bestimmten Erweiterungs Punkt in einer Host-App aktiviert. Und da Sie als Ergänzung zu vorhandenen Systemfunktionen verwendet werden, müssen Sie leistungsstark, schlank und robust sein.

## <a name="extension-points"></a>Erweiterungs Punkte

|Typ|Beschreibung|Erweiterungs Punkt|Host-App|
|--- |--- |--- |--- |
|Aktion|Spezieller Editor oder Viewer für einen bestimmten Medientyp|`com.apple.ui-services`|Any|
|Dokument Anbieter|Ermöglicht es der APP, einen Remote Dokument Speicher zu verwenden.|`com.apple.fileprovider-ui`|Apps, die einen [uidocumentpickerviewcontroller](xref:UIKit.UIDocumentPickerViewController) verwenden|
|Tastatur|Alternative Tastaturen|`com.apple.keyboard-service`|Any|
|Fotobearbeitung|Bearbeitung und Bearbeitung von Fotos|`com.apple.photo-editing`|Editor für Fotos. app|
|Teilen|Gibt Daten für soziale Netzwerke, Messaging Dienste usw. frei.|`com.apple.share-services`|Any|
|Heute|"Widgets", die auf dem Bildschirm "heute" oder im Notification Center angezeigt werden|`com.apple.widget-extensions`|Heute und Benachrichtigungs Center|

Zusätzliche Erweiterungs Punkte wurden in [IOS 10](~/ios/platform/introduction-to-ios10/index.md#app-extensions) und [IOS 12](~/ios/platform/introduction-to-ios12/index.md#notification-improvements)hinzugefügt. Die vollständige Tabelle aller unterstützten Typen finden Sie im [Programmier Handbuch](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW2)für die IOS-App-Erweiterung.

## <a name="limitations"></a>Einschränkungen

Erweiterungen weisen eine Reihe von Einschränkungen auf, von denen einige für alle Typen universell sind (z. b. Wenn kein Erweiterungstyp auf die Kameras oder das Mikrofon zugreifen kann), während andere Erweiterungs Typen bestimmte Einschränkungen hinsichtlich ihrer Verwendung aufweisen können (beispielsweise können benutzerdefinierte Tastaturen nicht für Felder mit sicheren Dateneingaben wie z. b. für Kenn Wörter verwendet werden).

Universelle Einschränkungen:

- Die Benutzeroberflächen-Frameworks von [Health Kit](~/ios/platform/healthkit.md) und [Event Kit](~/ios/platform/eventkit.md) sind nicht verfügbar.
- Erweiterungen können keine [erweiterten hintergrundmodi](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md) verwenden.
- Erweiterungen können nicht auf die Kameras oder das Mikrofon des Geräts zugreifen (obwohl Sie auf vorhandene Mediendateien zugreifen können).
- Erweiterungen können keine Air-Drop-Daten empfangen (obwohl Sie Daten über die luftablage übertragen können).
- [Uiaktionsheet](xref:UIKit.UIActionSheet) und [UIAlertView](xref:UIKit.UIAlertView) sind nicht verfügbar. Erweiterungen müssen [uialertcontroller](xref:UIKit.UIAlertController) verwenden
- Mehrere Mitglieder von [UIApplication](xref:UIKit.UIApplication) sind nicht verfügbar: [UIApplication. sharedapplikation](xref:UIKit.UIApplication.SharedApplication), [UIApplication. OpenURL](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl)), [UIApplication. beginignoringinteraktionevents](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) und [UIApplication. endignoringinteraktionevents.](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)
- IOS erzwingt eine Speicher Auslastungs Beschränkung von 16 MB für heutige Erweiterungen.
- Standardmäßig haben Tastatur Erweiterungen keinen Zugriff auf das Netzwerk. Dies wirkt sich auf das Debuggen auf dem Gerät aus (die Einschränkung wird im Simulator nicht erzwungen), da xamarin. IOS zum Funktionieren des Debuggens Netzwerk Zugriff benötigt. Es ist möglich, den Netzwerk Zugriff anzufordern, indem Sie den `Requests Open Access` Wert in der Info. plist-Datei des Projekts auf festlegen `Yes` . Weitere Informationen zu Einschränkungen bei der Tastatur Erweiterung finden Sie im [benutzerdefinierten Tastatur Handbuch](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) von Apple.

Informationen zu den einzelnen Einschränkungen finden Sie im [Programmier Handbuch zur APP-Erweiterung](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)von Apple.

## <a name="distributing-installing-and-running-extensions"></a>Verteilen, installieren und Ausführen von Erweiterungen

Erweiterungen werden innerhalb einer Container-App verteilt, die wiederum über den App Store übermittelt und verteilt wird. Die mit der APP verteilten Erweiterungen werden an diesem Punkt installiert, aber der Benutzer muss jede Erweiterung explizit aktivieren. Die verschiedenen Erweiterungs Typen werden auf unterschiedliche Weise aktiviert. einige erfordern, dass der Benutzer zur App " **Einstellungen** " navigiert und von dort aus aktiviert wird. Während andere zum Zeitpunkt der Verwendung aktiviert sind, z. b. das Aktivieren einer Freigabe Erweiterung beim Senden eines Fotos.

Die APP, in der die Erweiterung verwendet wird (in der der Benutzer auf den Erweiterungs Punkt stößt), wird als **Host-App**bezeichnet, da es sich um die APP handelt, die bei der Ausführung die Erweiterung hostet. Die APP, die die Erweiterung installiert, ist die **Container-App**, da Sie die APP ist, die die Erweiterung bei der Installation enthielt.  

In der Regel beschreibt die Container-APP die Erweiterung und führt den Benutzer durch den Prozess der Aktivierung.

## <a name="debug-and-release-versions-of-extensions"></a>Debug-und Releaseversionen von Erweiterungen

Arbeitsspeicher Limits für das Ausführen von App-Erweiterungen sind deutlich niedriger als die auf eine Vordergrund-App angewendeten Speicher Limits. Simulatoren, auf denen IOS ausgeführt wird, weisen weniger Einschränkungen auf, die auf Erweiterungen angewendet werden, und Sie können Ihre Erweiterung ohne Probleme ausführen. Wenn Sie jedoch dieselbe Erweiterung auf einem Gerät ausführen, kann dies zu unerwarteten Ergebnissen führen, z. a. wenn die Erweiterung abstürzen oder vom System aggressiv beendet wird. Stellen Sie daher sicher, dass Sie die Erweiterung auf einem Gerät erstellen und testen, bevor Sie es versenden.

Stellen Sie sicher, dass die folgenden Einstellungen auf das Container Projekt und alle referenzierten Erweiterungen angewendet werden:

1. Erstellen Sie ein Anwendungspaket in der **Releasekonfiguration** .
1. Legen Sie in den Einstellungen für das **IOS** -Buildprojekt die Option **Linker Verhalten** auf **nur Framework-sdker verknüpfen** oder **alle verknüpfen**fest.
1. Deaktivieren Sie in den **IOS-debugprojekteinstellungen** die Option **Debugging aktivieren** und **Profilerstellung aktivieren** .

## <a name="extension-lifecycle"></a>Erweiterungs Lebenszyklus

Eine Erweiterung kann so einfach wie ein einzelner [UIViewController](xref:UIKit.UIViewController) oder komplexere Erweiterungen sein, die mehrere Bildschirme der Benutzeroberfläche darstellen. Wenn der Benutzer auf _Erweiterungs Punkte_ stößt (z. b. bei der Freigabe eines Bilds), kann er aus den für diesen Erweiterungs Punkt registrierten Erweiterungen auswählen.

Wenn Sie eine der Erweiterungen ihrer App auswählen, wird Ihre `UIViewController` instanziiert und startet den normalen Lebenszyklus des Ansichts Controllers. Anders als bei einer normalen APP, die angehalten, aber nicht im allgemeinen beendet wird, wenn der Benutzer die Interaktion mit Ihnen beendet, werden die Erweiterungen geladen, ausgeführt und dann wiederholt beendet.

Erweiterungen können über ein [nsextensioncontext](xref:Foundation.NSExtensionContext) -Objekt mit Ihren Host-apps kommunizieren. Einige Erweiterungen verfügen über Vorgänge, die asynchrone Rückrufe mit den Ergebnissen empfangen. Diese Rückrufe werden für Hintergrundthreads ausgeführt, und die Erweiterung muss dies berücksichtigen. Verwenden Sie beispielsweise [NSObject. invokeonmainthread](xref:Foundation.NSObject.InvokeOnMainThread*) , wenn Sie die Benutzeroberfläche aktualisieren möchten. Weitere Informationen finden Sie weiter unten im Abschnitt [kommunizieren mit der Host-App](#communicating-with-the-host-app) .

Standardmäßig können Erweiterungen und Ihre Container-apps trotz der gemeinsamen Installation nicht miteinander kommunizieren. In einigen Fällen handelt es sich bei der Container-App im Grunde um einen leeren "Versand Container", dessen Zweck nach der Installation der Erweiterung erfüllt wird. Wenn dies jedoch der Fall ist, können die Container-APP und die Erweiterung Ressourcen aus einem gemeinsamen Bereich freigeben. Darüber hinaus kann eine **heutige Erweiterung** die Container-App zum Öffnen einer URL anfordern. Dieses Verhalten wird im Widget " [ereigniscountdownwidget](https://github.com/xamarin/ios-samples/tree/master/intro-to-extensions)" angezeigt.

## <a name="creating-an-extension"></a>Erstellen einer Erweiterung

Erweiterungen (und Ihre Container-Apps) müssen 64-Bit-Binärdateien und mithilfe der [vereinheitlichten](~/cross-platform/macios/unified/index.md)xamarin. IOS-APIs erstellt werden. Beim Entwickeln einer Erweiterung enthalten Ihre Lösungen mindestens zwei Projekte: die Container-APP und ein Projekt für jede Erweiterung, die der Container bereitstellt.

### <a name="container-app-project-requirements"></a>Container-App-Projektanforderungen

Die Container-APP, die zum Installieren der Erweiterung verwendet wird, muss die folgenden Anforderungen erfüllen:

- Es muss einen Verweis auf das Erweiterungsprojekt erhalten.   
- Dabei muss es sich um eine umfassende App handeln (muss gestartet und erfolgreich ausgeführt werden können), auch wenn Sie keine Möglichkeit bietet, eine Erweiterung zu installieren.
- Er muss über einen Bündel Bezeichner verfügen, der die Grundlage für die Bündel-ID des Erweiterungsprojekts ist (Weitere Informationen finden Sie im Abschnitt weiter unten).

### <a name="extension-project-requirements"></a>Erweiterungsprojekt Anforderungen

Außerdem gelten für das Projekt der Erweiterung die folgenden Anforderungen:

- Sie muss über eine Bündel-ID verfügen, die mit der Bündel-ID Ihrer Container-App beginnt. Wenn die Container-App beispielsweise über eine Bündel-ID von verfügt `com.myCompany.ContainerApp` , kann der Bezeichner der Erweiterung wie folgt lauten `com.myCompany.ContainerApp.MyExtension` :

  ![Bündel Bezeichner](extensions-images/bundleidentifiers.png)
- Er muss den Schlüssel `NSExtensionPointIdentifier` mit einem entsprechenden Wert (z. b. `com.apple.widget-extension` für ein **Today** Aktuelles Benachrichtigungs Center-Widget) in seiner `Info.plist` Datei definieren.
- Außerdem muss *entweder* der `NSExtensionMainStoryboard` Schlüssel oder der `NSExtensionPrincipalClass` Schlüssel in der `Info.plist` Datei mit einem geeigneten Wert definiert werden:
  - Verwenden `NSExtensionMainStoryboard` Sie den Schlüssel, um den Namen des Storyboards anzugeben, das die Hauptbenutzer Oberfläche für die Erweiterung (minus `.storyboard` ) darstellt. Beispielsweise `Main` für die `Main.storyboard` Datei.
  - Verwenden `NSExtensionPrincipalClass` Sie den Schlüssel, um die Klasse anzugeben, die beim Start der Erweiterung initialisiert wird. Der Wert muss dem **Registrierungs** Wert ihrer entsprechen `UIViewController` :

  ![Prinzipal Klassen Registrierung](extensions-images/registerandprincipalclass.png)

Bestimmte Erweiterungs Typen können zusätzliche Anforderungen haben. Beispielsweise muss die Prinzipal Klasse eines **heute** -oder **Notification Center** -Erweiterungs Diensts [incwidget-bereitstellen](xref:NotificationCenter.INCWidgetProviding)implementieren.

> [!IMPORTANT]
> Wenn Sie Ihr Projekt mit einer der von Visual Studio für Mac bereitgestellten Erweiterungs Vorlagen starten, werden die meisten (wenn nicht alle) diese Anforderungen von der Vorlage automatisch bereitgestellt und erfüllt.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In der folgenden exemplarischen Vorgehensweise erstellen Sie ein Widget für das **heutige** Beispiel, das den Tag und die Anzahl der verbleibenden Tage im Jahr berechnet:

[![Ein Beispiel für ein Widget, das den Tag und die Anzahl der verbleibenden Tage im Jahr berechnet.](extensions-images/carpediemscreenshot-sm.png)](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>Erstellen der Projekt Mappe

Gehen Sie folgendermaßen vor, um die erforderliche Lösung zu erstellen:

1. Erstellen Sie zunächst ein neues IOS-App-Projekt mit einer **einzelnen Ansicht** , und klicken Sie auf die Schaltfläche **weiter** :

    [![Erstellen Sie zunächst ein neues IOS-App-Projekt mit Einzelansicht, und klicken Sie auf die Schaltfläche Weiter.](extensions-images/today01.png)](extensions-images/today01.png#lightbox)
2. Nennen Sie das Projekt, `TodayContainer` und klicken Sie auf die Schaltfläche **weiter** :

    [![Nennen Sie das Projekt "tagcontainer", und klicken Sie auf "weiter"](extensions-images/today02.png)](extensions-images/today02.png#lightbox)
3. Überprüfen Sie den **Projektnamen** und den Projektmappennamen, und klicken Sie auf die Schaltfläche **Erstellen** , um die Lösung **SolutionName**

    [![Überprüfen Sie den Projektnamen und den Projektmappennamen, und klicken Sie auf die Schaltfläche "erstellen"](extensions-images/today03.png)](extensions-images/today03.png#lightbox)
4. Klicken Sie anschließend im **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und fügen Sie ein neues **IOS-Erweiterungs** Projekt aus der Vorlage für die **heutige Erweiterung** hinzu:

    [![Klicken Sie anschließend im Projektmappen-Explorer mit der rechten Maustaste auf die Projekt Mappe, und fügen Sie ein neues IOS-Erweiterungsprojekt aus der Vorlage für die heutige Erweiterung hinzu.](extensions-images/today04.png)](extensions-images/today04.png#lightbox)
5. Nennen Sie das Projekt, `DaysRemaining` und klicken Sie auf die Schaltfläche **weiter** :

    [![Nennen Sie das Projekt daysrest, und klicken Sie auf die Schaltfläche Weiter](extensions-images/today05.png)](extensions-images/today05.png#lightbox)
6. Überprüfen Sie das Projekt, und klicken Sie zum Erstellen auf die Schaltfläche **Erstellen**

    [![Überprüfen Sie das Projekt, und klicken Sie zum Erstellen auf die Schaltfläche erstellen](extensions-images/today06.png)](extensions-images/today06.png#lightbox)

Die resultierende Projekt Mappe sollte nun über zwei Projekte verfügen, wie hier gezeigt:

[![Die resultierende Projekt Mappe sollte nun über zwei Projekte verfügen, wie hier gezeigt.](extensions-images/today07.png)](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>Erstellen der Benutzeroberfläche für die Erweiterung

Als nächstes müssen Sie die Schnittstelle für Ihr **heute** -Widget entwerfen. Dies kann entweder mithilfe eines Storyboards oder durch Erstellen der Benutzeroberfläche im Code erfolgen. Beide Methoden werden im folgenden ausführlich beschrieben.

#### <a name="using-storyboards"></a>Verwenden von Storyboards

Gehen Sie folgendermaßen vor, um die Benutzeroberfläche mit einem Storyboard zu erstellen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die Datei des Erweiterungsprojekts, `Main.storyboard` um Sie für die Bearbeitung zu öffnen:

    [![Doppelklicken Sie auf die Datei "Main. Storyboard" der Erweiterungsprojekte, um Sie zur Bearbeitung zu öffnen.](extensions-images/today08.png)](extensions-images/today08.png#lightbox)
2. Wählen Sie die Bezeichnung aus, die der Benutzeroberfläche automatisch über die Vorlage hinzugefügt wurde, und **benennen** Sie Sie `TodayMessage` im **Eigenschaften-Explorer**auf der Registerkarte **Widget** :

    [![Wählen Sie die Bezeichnung aus, die der Benutzeroberfläche automatisch über die Vorlage hinzugefügt wurde, und benennen Sie Sie im Eigenschaften-Explorer auf der Registerkarte Widget mit dem Namen "".](extensions-images/today09.png)](extensions-images/today09.png#lightbox)
3. Speichern Sie die Änderungen am Storyboard.

#### <a name="using-code"></a>Verwenden von Code

Gehen Sie folgendermaßen vor, um die Benutzeroberfläche im Code zu erstellen:

1. Wählen Sie im **Projektmappen-Explorer**das Projekt **daysrestwert** aus, fügen Sie eine neue Klasse hinzu, und nennen Sie Sie `CodeBasedViewController` :

    [![Wählen Sie das daysrestprojekt aus, fügen Sie eine neue Klasse hinzu, und nennen Sie Sie "codebasedviewcontroller".](extensions-images/code01.png)](extensions-images/code01.png#lightbox)
2. Doppelklicken Sie in der **Projektmappen-Explorer**auf die Dateierweiterung, `Info.plist` um Sie für die Bearbeitung zu öffnen:

    [![Doppelklicken Sie auf Erweiterungen Info. plist-Datei, um Sie für die Bearbeitung zu öffnen.](extensions-images/code02.png)](extensions-images/code02.png#lightbox)
3. Wählen Sie die **Quell Ansicht** (unten auf dem Bildschirm) aus, und öffnen Sie den `NSExtension` Knoten:

    [![Wählen Sie unten auf dem Bildschirm die Quell Ansicht aus, und öffnen Sie den Knoten nsextension.](extensions-images/code03.png)](extensions-images/code03.png#lightbox)
4. Entfernen `NSExtensionMainStoryboard` Sie den Schlüssel, und fügen Sie einen `NSExtensionPrincipalClass` mit dem Wert hinzu `CodeBasedViewController` :

    [![Entfernen Sie den Schlüssel nsextensionmainstoryboard, und fügen Sie eine nsextensionprincipalclass mit dem Wert codebasedviewcontroller hinzu.](extensions-images/code04.png)](extensions-images/code04.png#lightbox)
5. Speichern Sie die Änderungen.

Bearbeiten Sie anschließend die `CodeBasedViewController.cs` Datei, und führen Sie Sie wie folgt aus:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
  [Register("CodeBasedViewController")]
  public class CodeBasedViewController : UIViewController, INCWidgetProviding
  {
    public CodeBasedViewController ()
    {
    }

    public override void ViewDidLoad ()
    {
      base.ViewDidLoad ();

      // Add label to view
      var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
        TextAlignment = UITextAlignment.Center
      };

      View.AddSubview (TodayMessage);

      // Insert code to power extension here...

    }
  }
}
```

Beachten Sie, dass mit `[Register("CodeBasedViewController")]` dem Wert übereinstimmt, den Sie für den obigen Wert angegeben haben `NSExtensionPrincipalClass` .

### <a name="coding-the-extension"></a>Codieren der Erweiterung

Nachdem die Benutzeroberfläche erstellt wurde, öffnen Sie entweder die- `TodayViewController.cs` oder die- `CodeBasedViewController.cs` Datei (basierend auf der-Methode, die zum Erstellen der Benutzeroberfläche verwendet wurde), ändern Sie die **viewDidLoad** -Methode, und machen Sie Sie wie folgt:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  // Calculate the values
  var dayOfYear = DateTime.Now.DayOfYear;
  var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
  var daysRemaining = 365 + leapYearExtra - dayOfYear;

  // Display the message
  if (daysRemaining == 1) {
    TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
  } else {
    TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
  }
}
```

Wenn Sie die Code basierte Benutzerschnittstellen Methode verwenden, ersetzen Sie den `// Insert code to power extension here...` Kommentar durch den oben aufgeführten neuen Code. Nachdem Sie die Basis Implementierung aufgerufen (und eine Bezeichnung für die Code basierte Version eingefügt haben), führt dieser Code eine einfache Berechnung durch, um den Tag des Jahres und die Anzahl der verbleibenden Tage zu erhalten. Anschließend wird die Meldung in der Bezeichnung ( `TodayMessage` ) angezeigt, die Sie im Benutzeroberflächen Design erstellt haben.

Beachten Sie, dass dieser Prozess dem normalen Prozess zum Schreiben einer App ähnelt. `UIViewController`Der Lebenszyklus einer Erweiterung hat denselben Lebenszyklus wie ein Ansichts Controller in einer APP, mit dem Unterschied, dass Erweiterungen nicht über hintergrundmodi verfügen und nicht angehalten werden, wenn der Benutzer Sie nicht mehr verwendet. Stattdessen werden Erweiterungen wiederholt initialisiert, und die Zuordnung wird bei Bedarf aufgehoben.

### <a name="creating-the-container-app-user-interface"></a>Erstellen der Benutzeroberfläche der Container-App

In dieser exemplarischen Vorgehensweise wird die Container-App einfach als Methode zum Versenden und Installieren der Erweiterung verwendet und bietet keine eigenen Funktionen. Bearbeiten Sie die Datei "- `Main.storyboard` Dateityp", und fügen Sie Text hinzu, der die Funktion der Erweiterung definiert, und wie Sie Sie installieren:

[![Bearbeiten Sie die Datei "" von "" "" "" "" "" "" "" "" "" "" ".](extensions-images/today10.png)](extensions-images/today10.png#lightbox)

Speichern Sie die Änderungen am Storyboard.

### <a name="testing-the-extension"></a>Testen der Erweiterung

Um die Erweiterung im IOS-Simulator zu testen, führen Sie die Anwendung " **tagcontainer** " aus. Die Hauptansicht des Containers wird angezeigt:

[![Die Container Hauptansicht wird angezeigt.](extensions-images/run01.png)](extensions-images/run01.png#lightbox)

Klicken Sie anschließend auf die Schaltfläche **Startseite** im Simulator, wischen Sie vom oberen Bildschirmrand nach unten, um das **Benachrichtigungs Center**zu öffnen, wählen Sie die Registerkarte **heute** aus, und klicken Sie auf die Schaltfläche **Bearbeiten** :

[![Klicken Sie im Simulator auf die Start Schaltfläche, wischen Sie vom oberen Bildschirmrand nach unten, um das Benachrichtigungs Center zu öffnen, wählen Sie die Registerkarte heute aus, und klicken Sie auf Bearbeiten.](extensions-images/run02.png)](extensions-images/run02.png#lightbox)

Fügen Sie der Ansicht **heute** die Erweiterung **daysresthinzu** , und klicken Sie auf die Schaltfläche **done** :

[![Fügen Sie der Ansicht heute die Erweiterung daysresthinzu, und klicken Sie auf die Schaltfläche Done.](extensions-images/run03.png)](extensions-images/run03.png#lightbox)

Das neue Widget wird der Ansicht **heute** hinzugefügt, und die Ergebnisse werden angezeigt:

[![Das neue Widget wird der Ansicht heute hinzugefügt, und die Ergebnisse werden angezeigt.](extensions-images/run04.png)](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>Kommunizieren mit der Host-App

Die oben erstellte Beispiel Erweiterung kommuniziert nicht mit der zugehörigen Host-app (dem Bildschirm **heute** ). Wenn dies der Fall wäre, würde die [extensioncontext](xref:Foundation.NSExtensionContext) -Eigenschaft der-Klasse oder der-Klasse verwendet werden `TodayViewController` `CodeBasedViewController` .

Bei Erweiterungen, die Daten von Ihren Host-apps empfangen, werden die Daten in Form eines Arrays von [nsextensionitem](xref:Foundation.NSExtensionItem) -Objekten verwendet, die in der [inputitems](xref:Foundation.NSExtensionContext.InputItems) -Eigenschaft des [extensioncontext](xref:Foundation.NSExtensionContext) der der Erweiterung gespeichert sind `UIViewController` .

Andere Erweiterungen, wie z. b. die Erweiterungen zur Fotobearbeitung, unterscheiden sich möglicherweise zwischen dem Benutzer, der die Nutzung abschließt Dies wird über die [CompleteRequest](xref:Foundation.NSExtensionContext.CompleteRequest*) -Methode und die [CancelRequest](xref:Foundation.NSExtensionContext.CancelRequest*) -Methode der [extensioncontext](xref:Foundation.NSExtensionContext) -Eigenschaft an die Host-app zurück signalisiert.

Weitere Informationen finden Sie im [Programmier Handbuch zur APP-Erweiterung](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)von Apple.

## <a name="communicating-with-the-parent-app"></a>Kommunizieren mit der übergeordneten App

Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

- [Apple Watch Einstellungen](~/ios/watchos/app-fundamentals/settings.md).
- Frei [gegebene NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- Frei [gegebene Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files).

Weitere Informationen finden Sie im Abschnitt [App-Gruppen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) in unserer Dokumentation zum **Arbeiten mit Funktionen** .

## <a name="mobilecoreservices"></a>Mobilecoreservices

Verwenden Sie beim Arbeiten mit Erweiterungen einen URI (Uniform Type Identifier), um Daten zu erstellen und zu bearbeiten, die zwischen der APP, anderen apps und/oder Diensten ausgetauscht werden.

Die `MobileCoreServices.UTType` statische Klasse definiert die folgenden Hilfseigenschaften, die sich auf die Definitionen von Apple beziehen `kUTType...` :

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive`
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log`
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie`
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL`
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey`
- `kUTTypeVideo` - `Video`
- `kUTTypeVolume` - `Volume`
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

Sehen Sie sich folgendes Beispiel an:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Weitere Informationen finden Sie im Abschnitt [App-Gruppen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) in unserer Dokumentation zum **Arbeiten mit Funktionen** .

## <a name="precautions-and-considerations"></a>Vorsichtsmaßnahmen und Überlegungen

Erweiterungen haben erheblich weniger Arbeitsspeicher als apps. Es wird erwartet, dass Sie schnell und mit minimalem Eingriff an den Benutzer und die app ausgeführt werden, in der Sie gehostet werden. Eine Erweiterung sollte jedoch auch eine besondere, nützliche Funktion für die verarbeitende App mit einer Branding-Benutzeroberfläche bereitstellen, die es dem Benutzer ermöglicht, die Entwickler-oder Container-App der Erweiterung zu identifizieren, zu der er gehört.

Wenn diese strenge Anforderung erfüllt ist, sollten Sie nur Erweiterungen bereitstellen, die gründlich getestet und für die Leistung und den Speicherverbrauch optimiert wurden.

## <a name="summary"></a>Zusammenfassung

In diesem Dokument wurden die Erweiterungen, die Art der Erweiterungs Punkte und die bekannten Einschränkungen für eine Erweiterung durch IOS behandelt. Es wurde erläutert, wie Erweiterungen und der Erweiterungs Lebenszyklus erstellt, verteilt, installiert und ausgeführt werden. Es wurde eine exemplarische Vorgehensweise für das Erstellen eines einfachen **heutigen** Widgets bereitgestellt, das zwei Möglichkeiten zum Erstellen der Benutzeroberfläche des Widgets mithilfe von Storyboards oder Code anzeigt. Es wurde gezeigt, wie Sie eine Erweiterung im IOS-Simulator testen. Schließlich wurde kurz erläutert, wie die Kommunikation mit der Host-App erfolgt, und es gibt einige Vorsichtsmaßnahmen und Überlegungen, die beim Entwickeln einer Erweiterung ergriffen werden sollten.

## <a name="related-links"></a>Verwandte Links

- [Containerapp (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/intro-to-extensions)
- [Erstellen von Erweiterungen in xamarin. IOS (Video)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
- [Optimieren der Effizienz und Leistung einer IOS-App-Erweiterung](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW7)
