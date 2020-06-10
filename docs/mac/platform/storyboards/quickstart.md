---
title: Storyboards in xamarin. Mac – Schnellstart
description: Dieses Dokument enthält eine kurze Einführung in das aufbauen von macOS-Benutzeroberflächen mit Storyboards in xamarin. Mac. Es wird beschrieben, wie Sie einen-Abschnitt erstellen und ein Einstellungsfenster erstellen.
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 007d835b71c16a51cf5f6aca3c9fe1edd5be0034
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572389"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Storyboards in xamarin. Mac – Schnellstart

Als kurze Einführung in die Verwendung von Storyboards zum Definieren der Benutzeroberfläche einer xamarin. Mac-app können Sie ein neues xamarin. Mac-Projekt starten. Wählen Sie **Mac**  >  -**App**  >  **Cocoa-App** aus, und klicken Sie auf **weiter** :

[![](quickstart-images/qs01.png "Adding a new Cocoa App")](quickstart-images/qs01.png#lightbox)

Verwenden Sie den **APP-Namen** , `MacStoryboard` und klicken Sie auf die Schaltfläche **weiter** :

[![](quickstart-images/qs02.png "Setting the App Name")](quickstart-images/qs02.png#lightbox)

Verwenden Sie den Standard **Projektnamen** und Projektmappennamen, und klicken Sie auf die Schaltfläche **Erstellen** **Solution Name**

[![](quickstart-images/qs03.png "The project and solution names")](quickstart-images/qs03.png#lightbox)

Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder von Xcode zu öffnen:

[![](quickstart-images/qs04.png "Editing the storyboard in Xcode")](quickstart-images/qs04.png#lightbox)

Wie Sie oben sehen können, definiert das Standard Storyboard sowohl die Menüleiste der App als auch das Hauptfenster mit dem Ansichts Controller und der Ansicht. Für unsere Beispiel-app erstellen wir eine Benutzeroberfläche, die über eine Haupt _Inhaltsansicht_ auf einer Seite und eine Inspektor- _Ansicht_ in der zweiten Seite verfügt.

Zu diesem Zweck müssen wir zunächst den Standard Ansichts Controller **und die Ansicht** aus dem Storyboard entfernen, indem wir ihn in Interface Builder auswählen und die ENTF-Taste drücken:

[![](quickstart-images/qs05.png "Removing the default view controller")](quickstart-images/qs05.png#lightbox)

Geben Sie als nächstes `split` in den **Filter** Bereich ein, wählen Sie den vertikalen geteilten Ansichts Controller aus, und ziehen Sie ihn auf den _Designoberfläche_:

[![](quickstart-images/qs06.png "Searching for the split view controller")](quickstart-images/qs06.png#lightbox)

Beachten Sie, dass der Controller automatisch zwei untergeordnete Ansichts Controller (und zugehörige Ansichten) enthält, die Links und rechts in der geteilten Ansicht bündig sind. Um die geteilte Ansicht an das übergeordnete Fenster zu binden, drücken Sie die STRG **-Taste,** klicken Sie auf den Fenster Controller (den blauen Kreis im Frame des Fenster Controllers), und ziehen Sie eine Linie auf den Controller für geteilte Ansicht. Wählen Sie im Popup **Fensterinhalt** aus:

[![](quickstart-images/qs07.png "Setting the windows content view")](quickstart-images/qs07.png#lightbox)

Dadurch werden die beiden Schnittstellen Elemente mithilfe von "*" verknüpft:

[![](quickstart-images/qs08.png "The Segue between the window and the content")](quickstart-images/qs08.png#lightbox)

Wir möchten eine Text Ansicht auf der linken Seite der geteilten Ansicht platzieren und lassen, dass der verfügbare Bereich automatisch ausgefüllt wird, wenn die Größe des Fensters oder der geteilten Ansicht geändert wird. Ziehen Sie eine Text Ansicht auf den oberen Ansichts Controller, der der geteilten Ansicht angefügt ist, und klicken Sie auf die **Pin** Auto Layout-Einschränkung (das zweite Symbol rechts unten im Designoberfläche).

[![](quickstart-images/qs09.png "Configuring the constraints")](quickstart-images/qs09.png#lightbox)

Von hier aus klicken wir am oberen Rand der Einschränkungs **Schaltfläche** auf alle vier Symbol Symbole um das umgebende Feld und klicken im unteren Bereich auf die Schaltfläche **4 Einschränkungen hinzufügen** , um die erforderlichen Einschränkungen hinzuzufügen.

Wenn wir zu Visual Studio für Mac zurückkehren und das Projekt ausführen, beachten Sie, dass die Text Ansicht automatisch angepasst wird, um die linke Seite der geteilten Ansicht zu füllen, wenn die Größe des Fensters oder der Aufteilung geändert wird:

[![](quickstart-images/qs10.png "An example of the app running")](quickstart-images/qs10.png#lightbox)

Da wir die Rechte Seite der geteilten Ansicht als Inspektor-Bereich verwenden, möchten wir, dass Sie eine geringere Größe aufweisen und den Vorgang reduzieren kann. Kehren Sie zu Xcode zurück, und bearbeiten Sie die Ansicht für die Rechte Seite, indem Sie Sie im Designoberfläche auswählen und auf den **Größen Inspektor**klicken. Geben Sie hier eine **Breite** von ein `250` :

[![](quickstart-images/qs11.png "Setting the width")](quickstart-images/qs11.png#lightbox)

Wählen Sie als nächstes das geteilte Element aus, das die Rechte Seite darstellt, legen Sie eine höhere **Priorität** fest, und klicken Sie auf das Kontrollkästchen **Benutzer kann** reduzieren:

[![](quickstart-images/qs12.png "Editing the holding priority")](quickstart-images/qs12.png#lightbox)

Wenn wir zu Visual Studio für Mac zurückkehren und das Projekt jetzt ausführen, beachten Sie, dass auf der rechten Seite die Größe kleiner ist und die Größe des Fensters angepasst wird:

[![](quickstart-images/qs13.png "An example of the app running")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue"></a>

## <a name="defining-a-presentation-segue"></a>Definieren eines Präsentations-segue

Das Layout der rechten Seite der geteilten Ansicht wird als Inspektor für die Eigenschaften des ausgewählten Texts fungieren. Wir ziehen einige Steuerelemente auf die untere Ansicht, um die Benutzeroberfläche des Inspektors zu gestalten. Für das letzte Steuerelement möchten wir ein popover anzeigen, das es dem Benutzer ermöglicht, vier vordefinierte Zeichenstile auszuwählen.

Wir fügen dem Inspektor eine Schaltfläche und einen Ansichts Controller zum Designoberfläche hinzu. Wir ändern die Größe des Ansichts Controllers in der Größe, die unser popover sein soll, und fügen vier Schaltflächen hinzu. Im nächsten Schritt **Steuern** Sie die Taste. Klicken Sie auf die Schaltfläche in der inspektoransicht, und ziehen Sie Sie auf den Ansichts Controller, der unser popover darstellt:

[![](quickstart-images/qs14.png "Dragging to create a new segue")](quickstart-images/qs14.png#lightbox)

Wählen Sie im Popup Menü **popover**aus: 

[![](quickstart-images/qs15.png "Selecting the segue type")](quickstart-images/qs15.png#lightbox)

Schließlich wählen wir den segue im Designoberfläche aus und legen den **bevorzugten Edge** auf **left**fest. Anschließend ziehen wir eine Linie aus der **Anker Ansicht** auf die Schaltfläche, an die das popover angefügt werden soll:

[![](quickstart-images/qs16.png "Dragging to create a new segue")](quickstart-images/qs16.png#lightbox)

Wenn Sie zu Visual Studio für Mac zurückkehren, führen Sie die APP aus, und klicken Sie auf die Schaltfläche **keine** im Inspektor. das Popup Fenster wird angezeigt:

[![](quickstart-images/qs17.png "An example of the segue running")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences"></a>

## <a name="creating-app-preferences"></a>Erstellen von App-Einstellungen

Die meisten macOS-Standard-Apps bieten ein _Einstellungs Dialogfeld_ , das es dem Benutzer ermöglicht, mehrere Optionen zu definieren, die verschiedene Aspekte der App steuern, z. b. Darstellung oder Benutzerkonten.

Wenn Sie ein Standard Dialogfeld für die Einstellung definieren möchten, ziehen Sie zuerst einen Registerkarten Ansichts Controller auf den Designoberfläche

[![](quickstart-images/qs18.png "Editing the storyboard in Xcode")](quickstart-images/qs18.png#lightbox)

Auch hier werden automatisch zwei untergeordnete Ansichts Controller angefügt. Zum Beispiel fügen wir jeder Ansicht, die darin zentriert wird, eine Bezeichnung hinzu:

[![](quickstart-images/qs19.png "Setting the constraints")](quickstart-images/qs19.png#lightbox)

Als nächstes möchten wir das Fenster "Einstellungen" anzeigen, wenn der Benutzer das Menü Element " **Einstellungen** " auswählt. Wählen Sie in der Menüleiste das Menü Element "Einstellungen" aus, und klicken Sie dann auf **die Tastenkombination** .

[![](quickstart-images/qs20.png "Dragging to create a segue")](quickstart-images/qs20.png#lightbox)

Wählen Sie im Popup Fenster **Modal** aus, um dieses Fenster als modales Dialog Feld anzuzeigen:

[![](quickstart-images/qs21.png "Selecting the segue type")](quickstart-images/qs21.png#lightbox)

Wenn unsere Änderungen gespeichert werden, kehren Sie zu Visual Studio für Mac zurück, führen Sie die APP aus, und wählen Sie das Menü Element "Einstellungen" aus **.** das Dialogfeld "neue Einstellungen" wird angezeigt:

[![](quickstart-images/qs22.png "An example of the segue running")](quickstart-images/qs22.png#lightbox)

Sie bemerken möglicherweise, dass dies nicht wie ein Standard Dialogfeld für die macOS-app-Einstellung aussieht. Um dieses Problem zu beheben, schließen Sie zwei Bilddateien in den Ordner der xamarin. Mac-app `Resources` im **Projektmappen-Explorer** ein, und kehren Sie zur Interface Builder von Xcode zurück.

Wählen Sie den Registerkarten Ansichts Controller aus, und wechseln Sie dessen **Stil** zu **Symbolleiste** 

[![](quickstart-images/qs23.png "Setting the tab bar style")](quickstart-images/qs23.png#lightbox)

Wählen Sie jede Registerkarte aus, und weisen Sie Ihr eine **Bezeichnung** zu, und wählen Sie eines der Bilder aus

[![](quickstart-images/qs24.png "Configuring each tab in Xcode")](quickstart-images/qs24.png#lightbox)

Wenn wir unsere Änderungen speichern, kehren Sie zu Visual Studio für Mac zurück, führen Sie die APP aus, und wählen Sie das Menü Element " **Preferences...** " aus. das Dialogfeld wird nun wie eine Standard-macOS-App angezeigt:

[![](quickstart-images/qs25.png "An example of the running preferences window")](quickstart-images/qs25.png#lightbox)

Weitere Informationen finden Sie in der Dokumentation [Arbeiten mit Bildern](~/mac/app-fundamentals/image.md), [Menüs](~/mac/user-interface/menu.md), [Fenstern](~/mac/user-interface/window.md) und [Dialog](~/mac/user-interface/dialog.md) Feldern.

## <a name="related-links"></a>Verwandte Links

- [Hello, Mac (Hallo Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [macOS-Eingaberichtlinien](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
