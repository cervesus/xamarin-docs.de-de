---
title: Storyboards in Xamarin.Mac – Schnellstart
description: Dieses Dokument enthält eine Einführung Schnellstart zum Erstellen von MacOS-Benutzeroberflächen mit Storyboards in Xamarin.Mac. Es wird beschrieben, wie einem Segue und erstellt ein Fenster "Einstellungen".
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: 7f7d23a01a3c3c6567d6bab45d0abbfb078fb512
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033603"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Storyboards in Xamarin.Mac – Schnellstart

Beginnen Sie als eine kurze Einführung in mithilfe von Storyboards zum Definieren der Benutzeroberfläche einer Xamarin.Mac-app eines neuen Xamarin.Mac-Projekts. Klicken Sie auf **Mac** > **App** > **Cocoa-App** und dann auf **Weiter**:

[![](quickstart-images/qs01.png "Hinzufügen einer neuen Cocoa-App")](quickstart-images/qs01.png#lightbox)

Verwenden der **Anwendungsnamen** von `MacStoryboard` , und klicken Sie auf die **Weiter** Schaltfläche:

[![](quickstart-images/qs02.png "Der Name der App festlegen")](quickstart-images/qs02.png#lightbox)

Verwenden Sie die Standardeinstellung **Projektname** und **Projektmappenname** , und klicken Sie auf die **erstellen** Schaltfläche:

[![](quickstart-images/qs03.png "Die Namen von Projekt- und Projektmappendateien")](quickstart-images/qs03.png#lightbox)

In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Xcode Interface Builder zu öffnen:

[![](quickstart-images/qs04.png "Bearbeiten das Storyboard in Xcode")](quickstart-images/qs04.png#lightbox)

Wie Sie oben sehen können, definiert die Standard-Storyboard sowohl für der app Menüleiste als auch für das Hauptfenster mit View-Controller und Ansicht. Für unser Beispiel-app werden wir eine Benutzeroberfläche erstellen, die eine Main _Ansicht "Inhalt"_ auf einer Seite und ein _Prüfungsansicht_ in der Sekunde.

Zu diesem Zweck müssen wir zunächst das standardmäßige View Controller zu entfernen und die Ansicht, die mit dem Storyboard von geliefert wird, wählen Sie ihn in Interface Builder und auf die **löschen** Schlüssel:

[![](quickstart-images/qs05.png "Entfernen den Standard-View-controller")](quickstart-images/qs05.png#lightbox)

Geben Sie als Nächstes `split` in die **Filter** Bereich, wählen Sie die vertikale Controller für geteilte Ansicht, und ziehen Sie es auf die _Entwurfsoberfläche_:

[![](quickstart-images/qs06.png "Suchen nach den Controller für geteilte Ansicht")](quickstart-images/qs06.png#lightbox)

Beachten Sie, dass der Controller werden automatisch zwei untergeordnete View Controller (und deren zugehörigen Sichten), wired oben auf der linken und rechten Seite der geteilten Ansicht enthalten. Um die die geteilten Ansicht an dessen übergeordnetes Fenster zu verknüpfen, drücken Sie die **Steuerelement** Schlüssel, klicken Sie auf den Fenster-Controller (blauer Kreis im Fenster des Controllers Frame), und ziehen Sie eine Zeile in den Controller für geteilte Ansicht. Wählen Sie **Fensterinhalt** im Popupmenü:

[![](quickstart-images/qs07.png "Die Windows-Ansicht \"Inhalt\" festlegen")](quickstart-images/qs07.png#lightbox)

Dadurch wird die zwei Benutzeroberflächenelement, das zusammen mit einem Segue verbunden:

[![](quickstart-images/qs08.png "Der Segue zwischen dem Fenster und den Inhalt")](quickstart-images/qs08.png#lightbox)

Wir möchten, platzieren eine Textansicht, in der linken Seite der geteilten Ansicht und den verfügbaren Bereich automatisch auszufüllen, wenn entweder das Fenster oder der geteilten Ansicht geändert wird. Ziehen Sie eine Textansicht an die oberste Position Ansichtscontroller an der geteilten Ansicht, und klicken Sie auf die **Pin** auto-Layout-Einschränkung (das zweite Symbol von rechts unten auf der Entwurfsoberfläche).

[![](quickstart-images/qs09.png "Konfigurieren von Einschränkungen")](quickstart-images/qs09.png#lightbox)

Hier klicken wir alle vier der **i-Balken** Symbole für das umgebende Feld am oberen Rand im Popover Einschränkungen, und klicken Sie auf die **4 Einschränkungen hinzufügen** Schaltfläche unten, um die erforderlichen Einschränkungen hinzufügen.

Wenn wir zu Visual Studio für Mac zurück, und führen Sie das Projekt, beachten Sie, dass die Textansicht automatisch angepasst wird, um im linken Bildschirmbereich der geteilten Ansicht im Fenster auszufüllen, oder die Aufteilung geändert werden:

[![](quickstart-images/qs10.png "Ein Beispiel für die app ausgeführt wird")](quickstart-images/qs10.png#lightbox)

Da wir möchten als Inspektor Bereich rechts von der geteilten Ansicht verwenden, möchten wir haben eine kleinere Größe und zuzulassen, dass sie reduziert werden. Zurück zu Xcode, und bearbeiten Sie die Anzeige für die rechte Seite, indem Sie sie in der Entwurfsoberfläche auswählen und dann auf die **Größeninspektor**. Hier geben Sie einen **Breite** von `250`:

[![](quickstart-images/qs11.png "Festlegen der Breite")](quickstart-images/qs11.png#lightbox)

Wählen Sie dann das Split-Element, das der rechten Seite stellt eine höhere festgelegt **enthält Priorität** , und klicken Sie auf die **Benutzer können reduzieren** Kontrollkästchen:

[![](quickstart-images/qs12.png "Bearbeiten die Betriebs-Priorität")](quickstart-images/qs12.png#lightbox)

Wenn wir zurück zu Visual Studio für Mac und das Projekt jetzt ausführen, beachten Sie, dass die rechte Seite er hält kleiner wird Größe und das Fenster geändert:

[![](quickstart-images/qs13.png "Ein Beispiel für die app ausgeführt wird")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Definieren eine Präsentation Segue

Layout werden der rechten Seite der geteilten Ansicht, die als eines Inspektors für die Eigenschaften für den ausgewählten Text fungiert wir. Wir werden einige Steuerelemente auf die Ansicht im unteren Layout der Benutzeroberfläche des Inspektors ziehen. Das letzte Steuerelement möchten wir eine Popover anzuzeigen, die dem Benutzer ermöglicht, die aus vier vordefinierten Zeichenformate auswählen.

Wir fügen eine Schaltfläche des Inspektors und einen Ansichtscontroller für die Entwurfsoberfläche. Größe des View-Controller, um die Größe zu sein, dass unsere Popover und vier Schaltflächen hinzugefügt werden soll. Als Nächstes müssen wir **Steuerelement** Schlüssel, klicken Sie auf die Schaltfläche in der Prüfungsansicht und ziehen Sie in der View-Controller, die unsere Popover darstellt:

[![](quickstart-images/qs14.png "Ziehen zum Erstellen eines neuen segues")](quickstart-images/qs14.png#lightbox)

Klicken Sie im Popupmenü entscheiden wir uns **Popover**: 

[![](quickstart-images/qs15.png "Segue-Typ auswählen")](quickstart-images/qs15.png#lightbox)

Abschließend wir wählen Sie auf der Entwurfsoberfläche der Segue und legen Sie die **bevorzugt Edge** zu **Links**. Klicken Sie dann Ziehen wir eine Zeile aus der **Anker Ansicht** auf die Schaltfläche, die im Popover an angefügt werden sollen:

[![](quickstart-images/qs16.png "Ziehen zum Erstellen eines neuen segues")](quickstart-images/qs16.png#lightbox)

Wenn wir zu Visual Studio für Mac zurückgegeben wird, führen Sie die app, und klicken Sie auf die **keine** des Inspektors im Popover die Schaltfläche wird angezeigt:

[![](quickstart-images/qs17.png "Ein Beispiel der Segue ausgeführt wird")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Erstellen von App-Einstellungen

Der Großteil der standardmäßigen MacOS-apps bieten eine _Einstellung Dialogfeld_ , mit der den Benutzer mehrere Optionen zu definieren, die verschiedene Aspekte der app, wie z. B. die Darstellung oder Benutzerkonten steuern können.

Um ein Dialogfeld Einstellung Standardfenster zu definieren, ziehen Sie zuerst eine Registerkarte-View-Controller, auf die Entwurfsoberfläche:

[![](quickstart-images/qs18.png "Bearbeiten das Storyboard in Xcode")](quickstart-images/qs18.png#lightbox)

In diesem Fall wird dies automatisch stammen, mit zwei untergeordneten, mit der View Controller verbunden. Z. B. skizzieren, eine Bezeichnung zu den einzelnen Sichten fügen wir, die darin center wird:

[![](quickstart-images/qs19.png "Festlegen von Einschränkungen")](quickstart-images/qs19.png#lightbox)

Als Nächstes soll das Fenster "Einstellungen" angezeigt, wenn der Benutzer auswählt der **Einstellungen...**  Menüelement. Wählen Sie in der Menüleiste das Menüelement Voreinstellungen **Steuerelement** Schlüssel auf, und ziehen Sie eine Zeile in unserer Registerkarte-View-Controller:

[![](quickstart-images/qs20.png "Ziehen zum Erstellen eines segues")](quickstart-images/qs20.png#lightbox)

Im Popupmenü entscheiden wir uns **modale** dieses Fenster als Modaldialogfeld angezeigt:

[![](quickstart-images/qs21.png "Segue-Typ auswählen")](quickstart-images/qs21.png#lightbox)

Wenn wir unsere Änderungen, wechseln Sie zurück zur Visual Studio für Mac, speichern, führen Sie die app, und wählen die **Einstellungen...**  Menüelement, unsere neuen Einstellungen mit dem Dialogfeld wird angezeigt:

[![](quickstart-images/qs22.png "Ein Beispiel der Segue ausgeführt wird")](quickstart-images/qs22.png#lightbox)

Sie werden feststellen, dass dies nicht wie ein standard MacOS-app-Einstellung Dialogfenster aussieht. Um dieses Problem zu beheben, schließen Sie zwei Bilddateien in der Xamarin.Mac-app `Resources` Ordner in der **Projektmappen-Explorer** und kehren Sie zur Interface Builder von Xcode zurück.

Wählen Sie die Registerkarte-View-Controller und der Switch die **Stil** zu **Symbolleiste**: 

[![](quickstart-images/qs23.png "Der Standardstil der Statusleiste Registerkarte festlegen")](quickstart-images/qs23.png#lightbox)

Wählen Sie jede Registerkarte, und geben Sie ihm eine **Bezeichnung** , und wählen Sie eines der Images für die Darstellung:

[![](quickstart-images/qs24.png "Konfigurieren der einzelnen Registerkarten in Xcode")](quickstart-images/qs24.png#lightbox)

Wenn wir unsere Änderungen, wechseln Sie zurück zur Visual Studio für Mac, speichern, führen Sie die app, und wählen die **Einstellungen...**  Menüelement, wie ein standard MacOS-app werden jetzt im Dialogfeld angezeigt:

[![](quickstart-images/qs25.png "Ein Beispiel für die ausgeführten Fenster \"Einstellungen\"")](quickstart-images/qs25.png#lightbox)

Weitere Informationen finden Sie unserem [arbeiten mit Bildern](~/mac/app-fundamentals/image.md), [Menüs](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) und [Dialogfelder](~/mac/user-interface/dialog.md) Dokumentation.

## <a name="related-links"></a>Verwandte Links

- [MacStoryboard (Beispiel)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
