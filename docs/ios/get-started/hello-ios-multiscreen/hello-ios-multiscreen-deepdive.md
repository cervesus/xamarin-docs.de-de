---
title: 'Ausführliche Erläuterungen: Hallo, iOS Multiscreen'
description: In diesem Dokument wird ein tieferer Einblick in die erweiterte Phoneword-Anwendung gewährt, mit weiterer Berücksichtigung des Model-View-Controllers, der iOS-Navigation und anderen Konzepten der iOS-Entwicklung.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: e8b3881db99d569008ce1290f81891f1b3b183d7
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563809"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Ausführliche Erläuterungen: Hallo, iOS Multiscreen

In der exemplarischen Vorgehensweise zum Schnellstart von Hallo, iOS Multiscreen haben Sie eine Xamarin.iOS-Multiscreen-Anwendung erstellt und ausgeführt. In dieser Anleitung erhalten Sie nun tiefere Einblicke in die Navigation und Architektur von iOS.

In diesem Leitfaden wird das Muster *Modell – Ansicht – Controller* (Model, View, Controller; MVC) eingeführt und seine Rolle in der iOS-Architektur und -Navigation veranschaulicht.
Außerdem wird der Navigationscontroller ausführlich beschrieben und seine Verwendung für eine unkomplizierte Navigation in iOS erläutert.

## <a name="model-view-controller-mvc"></a>Model View Controller-Muster (MVC)

Im [Hallo, iOS](~/ios/get-started/hello-ios/index.md)-Tutorial wurde erklärt, dass iOS-Anwendungen nur ein *Fenster* besitzen und dass Ansichtscontroller dafür zuständig sind, ihre *Hierarchien der Inhaltsansicht* in dieses Fenster zu laden. In der zweiten exemplarischen Vorgehensweise zu Phoneword wurde der Anwendung ein zweiter Bildschirm hinzugefügt und Daten (eine Liste mit Telefonnummern) zwischen den beiden Bildschirmen übergeben. Dies wird im folgenden Diagramm veranschaulicht:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Dieses Diagramm veranschaulicht die Übergabe von Daten zwischen zwei Bildschirmen")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

Im Beispiel wurden Daten im ersten Bildschirm erfasst, anschließend vom ersten an den zweiten Ansichtscontroller übergeben und schließlich im zweiten Bildschirm angezeigt. Diese Trennung von Bildschirmen, Ansichtscontrollern und Daten folgt dem *MVC*-Muster. In den nächsten Abschnitten werden die Vorteile des Musters, seine Komponenten und seine Verwendung in der Phoneword-Anwendung beschrieben.

### <a name="benefits-of-the-mvc-pattern"></a>Vorteile des MVC-Musters

Model View Controller ist ein *Entwurfsmuster*, d.h. eine wiederverwendbare architektonische Lösung für ein häufiges Problem oder einen Anwendungsfall im Code. MVC stellt eine Architektur für Anwendungen mit einer *grafischen Benutzeroberfläche (GUI)* dar. Es weist Objekten in der Anwendung eine von drei Rollen zu: *Modell* (Daten- oder Anwendungslogik), *Ansicht* (Benutzeroberfläche) und *Controller* (CodeBehind). Das folgende Diagramm veranschaulicht die Beziehungen zwischen den drei Bestandteilen des MVC-Musters und dem Benutzer:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Dieses Diagramm veranschaulicht die Beziehungen zwischen den drei Bestandteilen des MVC-Musters und dem Benutzer")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

Das MVC-Muster ist hilfreich, da es die verschiedenen Teile einer GUI-Anwendung logisch trennt und die Wiederverwendung von Codes und Ansichten vereinfacht. Nachfolgend werden die drei Rollen ausführlicher betrachtet.

> [!NOTE]
> Das MVC-Muster entspricht in etwa der Struktur von ASP.NET-Seiten oder WPF-Anwendungen. In diesen Beispielen stellt die Ansicht die für die Beschreibung der Benutzeroberfläche zuständige Komponente dar, die der ASPX-Seite (HTML) in ASP.NET oder der XAML-Seite in einer WPF-Anwendung entspricht. Der Controller ist die für die Verwaltung der Ansicht zuständige Komponente, die dem CodeBehind in ASP.NET oder WPF entspricht.

### <a name="model"></a>Modell

Das Modellobjekt ist in der Regel eine anwendungsspezifische Darstellung von Daten, das in der Ansicht angezeigt oder eingegeben werden soll. Das Modell wird oft nur vage definiert. In der **Phoneword_iOS**-App ist das Modell beispielsweise die Liste mit Telefonnummern (dargestellt als Liste von Zeichenfolgen). Bei einer plattformübergreifenden Anwendung kann die Freigabe des **PhonewordTranslator**-Code für die iOS- und Android-Anwendungen ausgewählt werden. Auch dieser freigegebene Code kann das Modell darstellen.

MVC ist von der *Datenpersistenz* und dem *Zugriff* auf das Modell völlig unabhängig. Das heißt in anderen Worten, dass MVC weder das Aussehen der Daten noch die Art ihrer Speicherung berücksichtigt, sondern nur ihre *Darstellung*. So können Sie die Daten beispielsweise in einer SQL-Datenbank speichern, in einem Cloudspeicher aufbewahren oder einfach eine `List<string>` verwenden. Für MVC ist nur die Darstellung der Daten im Muster enthalten.

In einigen Fällen kann die Modellkomponente von MVC auch leer sein. Sie können beispielsweise Ihrer App statische Seiten hinzufügen, in denen erklärt wird, wie das Telefonkonvertierungsprogramm funktioniert, warum es erstellt wurde und welche Kontaktmöglichkeiten es zum Melden von Fehlern gibt. Diese App-Bildschirme werden dann zwar mithilfe von Ansichten und Controllern erstellt, doch sie enthalten keine echten Modelldaten.

> [!NOTE]
> In manchen Dokumentationen bezieht sich die Modellkomponente des MVC-Musters auf das gesamte Back-End der Anwendung und nicht nur auf die auf der Benutzeroberfläche angezeigten Daten. In diesem Leitfaden wird allerdings eine moderne Interpretation des Ausdrucks Modell verwendet. Die Unterscheidung ist jedoch nicht von großer Bedeutung.

### <a name="view"></a>Ansicht

Die Ansicht ist die für das Rendern der Benutzeroberfläche zuständige Komponente. Auf nahezu allen Plattformen, die das MVC-Muster verwenden, besteht die Benutzeroberfläche aus einer Hierarchie von Ansichten. Die Ansicht in MVC entspricht einer Ansichtshierarchie mit einer einzigen Ansicht an der Spitze der Hierarchie (der Stammansicht) und einer beliebigen Anzahl von untergeordneten Ansichten (den Unteransichten). In iOS entspricht die Hierarchie der Inhaltsansicht des Bildschirms der Ansichtskomponente in MVC.

### <a name="controller"></a>Controller

Das Controllerobjekt ist die alles miteinander verbindende Komponente, die in iOS durch den `UIViewController` dargestellt wird. Der Controller entspricht dem Unterstützungscode für einen Bildschirm oder für eine Gruppe von Ansichten. Der Controller ist für das Abhören von Benutzeranforderungen und das Zurückgeben der entsprechenden Ansichtshierarchie zuständig. Er hört die Anforderungen der Ansicht ab (Anklicken von Schaltflächen, Texteingabe usw.) und führt die entsprechende Verarbeitung, Ansichtsänderung oder erneutes Laden der Ansicht aus. Der Controller ist auch für das Erstellen oder Abrufen des Modells aus einem beliebigen Sicherungsdatenspeicher der Anwendung sowie für das Auffüllen der Ansicht mit den Daten zuständig.

Controller können auch andere Controller verwalten. Ein Controller kann beispielsweise einen weiteren Controller laden, wenn ein anderer Bildschirm angezeigt werden soll. Oder er verwaltet einen Stapel von Controllern, um deren Reihenfolge und Übergange zu überwachen. Im nächsten Abschnitt wird ein besonderer Typ des iOS-Ansichtscontrollers eingeführt, der andere Controller verwaltet: der *Navigationscontroller*.

## <a name="navigation-controller"></a>Navigationscontroller

In der Phoneword-Anwendung wurde ein Navigationscontroller zur Verwaltung der Navigation zwischen mehreren Bildschirmen verwendet. Er ist ein spezialisierter `UIViewController`, der durch die `UINavigationController`-Klasse dargestellt wird. Der Navigationscontroller verwaltet nicht eine einzelne Hierarchie der Inhaltsansicht, sondern andere Ansichtscontroller sowie seine eigene spezielle Hierarchie der Inhaltsansicht in Form einer Navigationssymbolleiste. Diese enthält einen Titel, eine Zurück-Schaltfläche und weitere optionale Funktionen.

Der Navigationscontroller wird häufig in iOS-Anwendungen verwendet und stellt die Navigation für wesentliche iOS-Anwendungen bereit, wie z.B. für die App **Einstellungen**. Dies wird im folgenden Screenshot veranschaulicht:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Der Navigationscontroller stellt die Navigation für iOS-Anwendungen wie die hier angezeigte App „Einstellungen“ bereit.")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Der Navigationscontroller erfüllt drei Hauptaufgaben:

-  **Bereitstellen von Hooks für die Vorwärtsnavigation**: Der Navigationscontroller verwendet eine hierarchische Navigationsmetapher, bei der die Hierarchien der Inhaltsansicht auf einen *Navigationsstapel* *gepusht* werden. Ein Navigationsstapel ähnelt einem Kartenstapel, von dem nur die oberste Karte, wie im folgenden Diagramm veranschaulicht, sichtbar ist:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Dieses Diagramm veranschaulicht die Navigation als Kartenstapel")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **Bereitstellen einer optionalen Zurück-Schaltfläche**: Wird ein neues Element auf dem Navigationsstapel abgelegt, wird in der Titelleiste automatisch eine *Zurück-Schaltfläche* angezeigt, mit der der Benutzer rückwärts navigieren kann. Durch Drücken der Schaltfläche „Zurück“ wird der aktuelle Ansichtscontroller vom Navigationsstapel *per Pop entfernt*, und es wird die vorherige Hierarchie der Inhaltsansicht im Fenster geladen:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Dieses Diagramm veranschaulicht das „Herunternehmen“ einer Karte vom Stapel")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Bereitstellen einer Titelleiste**: Der obere Teil des Navigationscontrollers wird *Titelleiste* genannt. Sie zeigt, wie im folgenden Diagramm dargestellt, den Titel des Ansichtscontroller an:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "In der Titelleiste wird der Titel des Ansichtscontrollers angezeigt.")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)

### <a name="root-view-controller"></a>Stammansichtscontroller

Da ein Navigationscontroller keine Hierarchien der Inhaltsansicht verwaltet, kann er keine eigenen Inhalte anzeigen.
Stattdessen wird ein Navigationscontroller einem *Stammansichtscontroller* zugeordnet:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Ein Navigationscontroller ist einem Stammansichtscontroller zugeordnet.")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Der Stammansichtscontroller stellt den ersten Ansichtscontroller im Stapel des Navigationscontrollers dar. Die Hierarchie der Inhaltsansicht des Stammansichtscontrollers ist die erste Hierarchie der Inhaltsansicht, die im Fenster geladen wird. Wenn die vollständige Anwendung auf dem Stapel des Navigationscontrollers abgelegt werden soll, können Sie, wie schon zuvor in der Phoneword-Anwendung, den Sourceless Segue zum Navigationscontroller verschieben und den Ansichtscontroller des ersten Bildschirms als Stammansichtscontroller festlegen:

 [![](hello-ios-multiscreen-deepdive-images/06.png "„Sourceless Segue“ legt den Ansichtscontroller des ersten Bildschirms als Stammansichtscontroller fest")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Zusätzliche Navigationsoptionen

Der Navigationscontroller wird häufig zur Navigation in iOS verwendet, doch er ist nicht die einzige Möglichkeit. Ein [Registerkartenleistencontroller](~/ios/user-interface/controls/creating-tabbed-applications.md) kann z.B. eine Anwendung in verschiedene funktionale Bereiche aufteilen, und ein [Controller für geteilte Ansichten](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) kann zum Erstellen von Master-/Detailansichten verwendet werden. Wenn Sie Navigationscontroller mit diesen anderen Navigationsparadigmen kombinieren, können Sie Inhalt in iOS auf viele unterschiedliche Arten präsentieren und durch diesen navigieren.

## <a name="handling-transitions"></a>Behandeln von Übergängen

In der exemplarischen Vorgehensweise zu Phoneword wurde der Übergang zwischen zwei Ansichtscontrollern auf zwei unterschiedliche Arten behandelt: mit einem Storyboard-Segue und programmgesteuert. Diese beiden Optionen werden im folgenden Abschnitt ausführlicher betrachtet.

### <a name="prepareforsegue"></a>PrepareForSegue

Durch das Hinzufügen eines Segues mit einer **Anzeigen**-Aktion zum Storyboard wird iOS angewiesen, den zweiten Ansichtscontroller auf den Stapel des Navigationscontrollers zu pushen:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Segue-Typ aus einer Dropdownliste auswählen")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Für einen einfachen Übergang zwischen zwei Bildschirmen ist das Hinzufügen eines Segues zum Storyboard ausreichend. Wenn Daten zwischen Ansichtscontrollern übergeben werden sollen, müssen Sie die `PrepareForSegue`-Methode außer Kraft setzen und die Daten selbst verarbeiten:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

Direkt vor dem Übergang wird `PrepareForSegue` von iOS aufgerufen und der im Storyboard erstellte Segue an die Methode übergeben.
An dieser Stelle müssen Sie den Zielansichtscontroller des Segues manuell festlegen. Der folgende Code ruft ein Handle für den Zielansichtscontroller auf und wandelt ihn in die richtige Klasse um, in diesem Fall „CallHistoryController“:

```csharp
CallHistoryController callHistoryController = segue.DestinationViewController as CallHistoryController;
```

Anschließend wird die Liste mit Telefonnummern (das Modell) vom `ViewController` an den `CallHistoryController` übergeben, indem die `PhoneHistory`-Eigenschaft des `CallHistoryController` auf die Liste der gewählten Rufnummern festgelegt wird:

```csharp
callHistoryController.PhoneNumbers = PhoneNumbers;
```

Der vollständige Code für die Übergabe von Daten mithilfe eines Segues lautet wie folgt:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController as CallHistoryController;

    if (callHistoryController != null) {
         callHistoryController.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>Navigation ohne Segues

Der Übergang vom ersten Ansichtscontroller zum zweiten funktioniert im Code genauso wie mit einem Segue, jedoch müssen verschiedene Schritte manuell durchgeführt werden.
Verwenden Sie als Erstes `this.NavigationController`, um einen Verweis auf den Navigationscontroller abzurufen, dessen Stapel aktuell ist. Verwenden Sie anschließend die `PushViewController`-Methode des Navigationscontrollers, um den nächsten Ansichtscontroller manuell auf dem Stapel zu pushen. Dabei werden der Ansichtscontroller und eine Option zur Animation des Übergangs (auf `true` festgelegt) übergeben.

Der folgende Code behandelt den Übergang zwischen Phoneword-Bildschirm und Anrufliste-Bildschirm:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Vor dem Übergang zum nächsten Ansichtscontroller müssen Sie diesen manuell aus dem Storyboard instanziieren. Rufen Sie hierzu `this.Storyboard.InstantiateViewController` auf, und übergeben Sie die Storyboard-ID des `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Anschließend wird die Liste mit Telefonnummern (das Modell) vom `ViewController` an den `CallHistoryController` übergeben, indem die `PhoneHistory`-Eigenschaft des `CallHistoryController` auf die Liste der gewählten Rufnummern festgelegt wird. Dieser Vorgang entspricht der Vorgehensweise des Übergangs mit einem Segue:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Der vollständige Code für den programmgesteuerten Übergang lautet wie folgt:

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Zusätzliche in Phoneword eingeführte Konzepte

Die Phoneword-Anwendung enthält weitere Konzepte, die jedoch nicht in diesem Leitfaden behandelt werden. Dies sind z.B. folgende Konzepte:

-  **Automatisches Erstellen des Ansichtscontrollers**: Wenn Sie im **Eigenschaftenpad** einen Klassennamen für den Ansichtscontroller eingeben, wird vom iOS-Designer überprüft, ob diese Klasse vorhanden ist, und anschließend die Sicherungsklasse des Ansichtscontrollers generiert. Weitere Informationen zu dieser und anderen iOS-Designer-Funktionen finden Sie im Leitfaden [Introduction to the iOS Designer](~/ios/user-interface/designer/introduction.md) (Einführung in den iOS-Designer).
-  **Tabellenansichtscontroller**: Der `CallHistoryController` ist ein Tabellenansichtscontroller. Ein Tabellenansichtscontroller enthält eine Tabellenansicht, das häufigste Tool zum Anzeigen von Layout und Daten in iOS. Tabellen sind jedoch nicht Teil dieses Leitfadens. Weitere Informationen zu Tabellenansichtscontrollern finden Sie im Leitfaden [Arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md).
-   **Storyboard-ID**: Durch das Festlegen der Storyboard-ID wird eine Ansichtscontroller-Klasse in Objective-C erstellt, die den CodeBehind für den Ansichtscontroller im Storyboard enthält. Suchen Sie mit der Storyboard-ID die Objective-C-Klasse, und instanziieren Sie den Ansichtscontroller im Storyboard. Weitere Informationen zu Storyboard-IDs finden Sie im Leitfaden [Introduction to Storyboards](~/ios/user-interface/storyboards/index.md) (Einführung in Storyboards).

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch, Sie haben die iOS-Multiscreen-Anwendung fertiggestellt!

In diesem Handbuch wird das MVC-Muster eingeführt und zum Erstellen einer Anwendung mit mehreren Bildschirmen verwendet. Außerdem werden Navigationscontroller und deren Rolle bei der iOS-Navigation behandelt. Mit diesem soliden Grundwissen können Sie jetzt mit der Entwicklung Ihrer eigenen Xamarin.iOS-Anwendungen beginnen.

Erfahren Sie nun mehr über das Erstellen von plattformübergreifenden Anwendungen mit Xamarin in den Leitfäden [Introduction to Mobile Development](~/cross-platform/get-started/introduction-to-mobile-development.md) (Einführung in die Entwicklung mobiler Anwendungen) und [Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) (Erstellen von plattformübergreifenden Anwendungen).

## <a name="related-links"></a>Verwandte Links

- [Hallo, iOS (Beispiel)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS-Eingaberichtlinien](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS-Bereitstellungsportal](https://developer.apple.com/ios/manage/overview/index.action)
