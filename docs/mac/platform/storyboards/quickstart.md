---
title: Storyboards-Schnellstart
description: "Abrufen gestarteten Gebäude MacOS Benutzeroberflächen mit Storyboards."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: fe3a93557509aba4b33b1470879cd2504ed0f2a2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="starting-a-new-storyboard-based-project"></a>Starten ein neues Storyboard-basiertes Projekt

Beginnen Sie als eine kurze Einführung zur Verwendung von Storyboards zum Definieren einer Xamarin.Mac-app-Benutzeroberfläche ein neues Xamarin.Mac-Projekt. Klicken Sie auf **Mac** > **App** > **Cocoa-App** und dann auf **Weiter**:

[![](quickstart-images/qs01.png "Hinzufügen einer neuen Kakao-App")](quickstart-images/qs01.png#lightbox)

Verwenden der **Anwendungsnamen** von `MacStoryboard` , und klicken Sie auf die **Weiter** Schaltfläche:

[![](quickstart-images/qs02.png "Die App-Name der Einstellung")](quickstart-images/qs02.png#lightbox)

Verwenden Sie den Standardnamen **Projektname** und **Projektmappenname** , und klicken Sie auf die **erstellen** Schaltfläche:

[![](quickstart-images/qs03.png "Die Namen von Projekt- und Projektmappendateien")](quickstart-images/qs03.png#lightbox)

In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen:

[![](quickstart-images/qs04.png "Bearbeiten das Storyboard in Xcode")](quickstart-images/qs04.png#lightbox)

Wie Sie oben sehen können, definiert die Standard-Storyboard der app-Menüleiste sowohl das Hauptfenster mit ein, View-Controller und Ansicht. Für unser Beispiel-app werden wir eine Benutzeroberfläche erstellen, die über ein Hauptfenster verfügt _Inhaltsansicht_ auf einer Seite und einen _Inspektor Ansicht_ in der zweiten.

Zu diesem Zweck müssen wir zunächst die standardmäßigen View Controller entfernt werden und die Sicht, die mit dem Storyboard von geliefert wird, wählen Sie diese im Benutzeroberflächen-Generator, und drücken die **löschen** Schlüssel:

[![](quickstart-images/qs05.png "Entfernen die Standard-Domänencontroller-Ansicht")](quickstart-images/qs05.png#lightbox)

Geben Sie als Nächstes `split` in der **Filter** Bereichs-, wählen Sie die vertikale Teilung-View-Controller, und ziehen Sie es auf die _Entwurfsoberfläche_:

[![](quickstart-images/qs06.png "Suchen für den Controller der geteilte Ansicht")](quickstart-images/qs06.png#lightbox)

Beachten Sie, dass der Domänencontroller automatisch zwei untergeordnete View Controller (und deren zugehörigen Sichten) enthalten, wired oben auf der linken und rechten Rand der Ansicht teilen. Um die geteilte Ansicht in das übergeordnete Fenster gleichwertig sind, drücken Sie die **Steuerelement** key, klicken Sie auf den Fenster-Controller (blauen Kreis in das Fenster des Controllers Frame), und ziehen Sie eine Zeile in der Split-View-Controller. Wählen Sie **Fensterinhalt** im Popupmenü:

[![](quickstart-images/qs07.png "Festlegen der Windows-Inhaltsansicht")](quickstart-images/qs07.png#lightbox)

Dadurch wird die zwei Benutzeroberflächenelement, das zusammen mit einem Segue verbunden:

[![](quickstart-images/qs08.png "Die Segue zwischen dem Fenster und der Inhalt")](quickstart-images/qs08.png#lightbox)

Wir möchten Platzieren einer Textansicht auf der linken Seite der geteilten Ansicht und den verfügbaren Bereich automatisch eingetragen wird, beim Ändern der Größe des Fensters oder der Ansicht teilen. View-Controller angefügt wird, zur geteilten Ansicht oben ein Textansicht ziehen, und klicken Sie auf die **Pin** auto-Layout-Einschränkung (das zweite Symbol von rechts unten auf der Entwurfsoberfläche angezeigt).

[![](quickstart-images/qs09.png "Konfigurieren die Einschränkungen")](quickstart-images/qs09.png#lightbox)

Hier klicken wir alle vier der **i-Balken** Symbole, um das umgebende Feld am oberen Rand der Einschränkungen Popover und klicken Sie auf die **hinzufügen 4 Einschränkungen** Schaltfläche unten, um die erforderlichen Einschränkungen hinzufügen.

Wenn wir zu Visual Studio für Mac zurück, und führen Sie das Projekt, beachten Sie, dass der Textansicht automatisch wird an der linken Seite der geteilten Ansicht als das Fenster füllen oder die Teilung angepasst werden:

[![](quickstart-images/qs10.png "Ein Beispiel für die ausgeführte app")](quickstart-images/qs10.png#lightbox)

Da wir werden auf der rechten Seite der geteilten Ansicht als Inspektor Bereich verwenden, möchten wir haben eine geringere Größe an, und reduziert werden kann. Zur Xcode zurückzukehren, und bearbeiten Sie die Anzeige für die rechte Seite, indem Sie sie in der Entwurfsoberfläche auswählen und dann auf die **Größe Inspektor**. Hier geben Sie einen **Breite** von `250`:

[![](quickstart-images/qs11.png "Festlegen der Breite")](quickstart-images/qs11.png#lightbox)

Wählen Sie dann das Split-Element, das rechts darstellt, legen Sie eine höhere **halten Priorität** , und klicken Sie auf die **Benutzer können reduzieren** Kontrollkästchen:

[![](quickstart-images/qs12.png "Bearbeiten die Priorität Betrieb")](quickstart-images/qs12.png#lightbox)

Wenn wir zurück zu Visual Studio für Mac und das Projekt jetzt ausführen, beachten Sie, dass die rechte Seite Dadurch bleiben die kleinere Größe und das Fenster geändert:

[![](quickstart-images/qs13.png "Ein Beispiel für die ausgeführte app")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Definieren einer Präsentation Segue

Layout Kegel der rechten Seite der geteilten Ansicht als eines Inspektors für Eigenschaften für den ausgewählten Text fungiert. Es müssen einige Steuerelemente in den Unteransicht Layout der Benutzeroberfläche des Inspektors ziehen. Für das letzte Steuerelement möchten wir eine Popover anzuzeigen, die dem Benutzer ermöglicht, aus vier vordefinierten Zeichenformate auswählen.

Eine Schaltfläche fügen wir der Inspektor und eine View-Controller auf die Entwurfsoberfläche. Resize View-Controller, um die Größe des sein müssen, dass wir unsere Popover und vier Schaltflächen hinzugefügt werden soll. Wir als Nächstes **Steuerelement** Schlüssel mit einem Klick auf die Schaltfläche in der Ansicht der Inspektor und ziehen Sie auf die View-Controller, die unsere Popover darstellen:

[![](quickstart-images/qs14.png "Ziehen zum Erstellen einer neuen segue")](quickstart-images/qs14.png#lightbox)

Über das Popupmenü wir wählen **Popover**: 

[![](quickstart-images/qs15.png "Segue auswählen")](quickstart-images/qs15.png#lightbox)

Schließlich wir die Segue in der Entwurfsoberfläche auswählen und Festlegen der **Edge bevorzugte** auf **Links**. Wir müssen ziehen Sie dann eine Zeile aus der **Anker Ansicht** auf die Schaltfläche, die die Popover zugeordnet werden soll:

[![](quickstart-images/qs16.png "Ziehen zum Erstellen einer neuen segue")](quickstart-images/qs16.png#lightbox)

Wenn es sich bei Visual Studio für Mac zurückgeben, führen Sie die app, und klicken Sie auf die **keine** Schaltfläche in der die Popover-Inspektor angezeigt wird:

[![](quickstart-images/qs17.png "Ein Beispiel für die Segue ausgeführt wird")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Erstellen von App-Einstellungen

Die meisten standard MacOS apps bieten eine _Voreinstellung Dialogfeld_ , mit dessen Hilfe des Benutzers mehrere Optionen zu definieren, die verschiedene Aspekte der Anwendung, z. B. Darstellung oder Benutzerkonten steuern.

Um eine standardmäßige Voreinstellung Dialogfenster zu definieren, ziehen Sie zunächst eine Registerkarte-View-Controller auf die Entwurfsoberfläche:

[![](quickstart-images/qs18.png "Bearbeiten das Storyboard in Xcode")](quickstart-images/qs18.png#lightbox)

Es wird wieder automatisch stammen, mit zwei untergeordneten View-Controller angeschlossen. Zum Beispiel an sich, eine Bezeichnung zu den einzelnen Sichten fügen wir, die intern center wird:

[![](quickstart-images/qs19.png "Die Einschränkungen festlegen")](quickstart-images/qs19.png#lightbox)

Als Nächstes wird im Fenster Voreinstellungen anzeigen, wenn der Benutzer auswählt möchten die **Einstellungen...**  Menüelement. Wählen Sie auf der Menüleiste das Menüelement Voreinstellungen **Steuerelement** Schlüssel Klick und ziehen Sie eine Zeile in unserer Registerkarte-View-Controller:

[![](quickstart-images/qs20.png "Ziehen zum Erstellen einer segue")](quickstart-images/qs20.png#lightbox)

Im Popupmenü wir wählen **modale** zum Anzeigen dieses Fensters als Modaldialogfeld:

[![](quickstart-images/qs21.png "Segue auswählen")](quickstart-images/qs21.png#lightbox)

Wenn wir unsere Änderungen an, zurück zu Visual Studio für Mac, speichern Sie die app auszuführen, und wählen Sie die **Einstellungen...**  Menüelement, unsere neue Voreinstellungen Dialogfeld wird angezeigt:

[![](quickstart-images/qs22.png "Ein Beispiel für die Segue ausgeführt wird")](quickstart-images/qs22.png#lightbox)

Stellen Sie möglicherweise fest, dass dies z. B. eine app standard MacOS Voreinstellung Dialogfenster aussieht. Um dieses Problem zu beheben, schließen Sie zwei Bilddateien in der Xamarin.Mac app `Resources` Ordner in der **Projektmappen-Explorer** und zurück zu Xcodes Benutzeroberflächen-Generator.

Wählen Sie die Registerkarte "-View-Controller und der Switch die **Stil** auf **Symbolleiste**: 

[![](quickstart-images/qs23.png "Die Registerkarte Balkenart festlegen")](quickstart-images/qs23.png#lightbox)

Wählen Sie die einzelnen Registerkarten, und geben Sie ihm eine **Bezeichnung** , und wählen Sie eines der Bilder für die Darstellung:

[![](quickstart-images/qs24.png "Konfigurieren jede Registerkarte in Xcode")](quickstart-images/qs24.png#lightbox)

Wenn wir unsere Änderungen an, zurück zu Visual Studio für Mac, speichern Sie die app auszuführen, und wählen Sie die **Einstellungen...**  Menüelement, z. B. eine app standard MacOS werden jetzt im Dialogfeld angezeigt:

[![](quickstart-images/qs25.png "Ein Beispiel des Fensters Voreinstellungen ausgeführt")](quickstart-images/qs25.png#lightbox)

Weitere Informationen finden Sie unter unsere [arbeiten mit Bildern](~/mac/app-fundamentals/image.md), [Menüs](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) und [Dialoge](~/mac/user-interface/dialog.md) Dokumentation.

## <a name="related-links"></a>Verwandte Links

- [MacStoryboard (Beispiel)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
