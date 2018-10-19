---
title: Android-Steuerelemente (Widgets)
description: Bausteine zum Erstellen von Benutzeroberflächen für Xamarin.Android
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 842fb1df2c9cc1aaf1a106687179a3730c2503bd
ms.sourcegitcommit: 7e4070bc104d612b6754ea35dd5a49c5c3d45f4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "32436712"
---
# <a name="android-controls-widgets"></a>Android-Steuerelemente (Widgets)

Xamarin.Android stellt den gesamten native Steuerelemente der Benutzeroberfläche (Widgets) von Android bereitgestellt. Diese Steuerelemente können problemlos für Xamarin.Android-apps, die mit dem Android-Designer oder programmgesteuert über XML-Layout-Dateien hinzugefügt werden. Unabhängig von der Methode, die Sie auswählen, macht alle Methoden in c# und Benutzeroberflächeneigenschaften Objekt Xamarin.Android verfügbar. Die folgenden Abschnitte führen die am häufigsten verwendeten Android Steuerelemente der Benutzeroberfläche und wird erläutert, wie sie in Xamarin.Android-apps zu integrieren.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Aktionsleiste](~/android/user-interface/controls/action-bar.md) 

`ActionBar` ist eine Symbolleiste, die den Titel der Aktivität, Navigation, Schnittstellen und andere interaktive Elemente werden angezeigt. Wird Sie in der Regel die Aktionsleiste am oberen Rand einer Aktivität im Fenster angezeigt.

![Beispiel-ActionBar](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Automatische Vervollständigung](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` ist ein bearbeitbarer Text anzeigen-Element, das vervollständigungsvorschläge automatisch zeigt während der Eingabe des Benutzers an. Die Liste der Vorschläge wird in einem Dropdown-Menü angezeigt, in dem der Benutzer ein Element hinzu, ersetzen Sie den Inhalt des Bearbeitungsfelds mit auswählen kann.

![Beispiel für die automatische Vervollständigung](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Schaltflächen](~/android/user-interface/controls/buttons/index.md)

Schaltflächen sind Benutzeroberflächenelemente, die der Benutzer tippt, um eine Aktion auszuführen.

![Beispiel-Schaltflächen](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Kalender](~/android/user-interface/controls/calendar.md)

Die `Calendar` Klasse wird verwendet, für die Konvertierung in einer bestimmten Instanz (einen Millisekundenwert, der von der Epoche versetzt wird) auf Werte wie z. B. Jahr, Monat, Stunde, Tag des Monats, und das Datum der nächsten Woche Zeit.
`Calendar` unterstützt eine Fülle von Optionen für die Interaktion mit Kalenderdaten, einschließlich der Möglichkeit zum Lesen und Schreiben von Ereignissen, Teilnehmer und Erinnerungen an. Mithilfe des Kalender-Anbieters in Ihrer Anwendung zu verwenden, werden die Daten, die Sie über die API hinzufügen in die integrierte Kalender-app angezeigt, die mit Android steht.

![Beispiel-Kalender](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` ist eine UI-Komponente, die Text- und Image-Inhalt in Sichten darstellt, die Karten ähneln. `CardView` wird als implementiert eine `FrameLayout` Widget mit abgerundeten Ecken und einen Schatten. In der Regel eine `CardView` wird verwendet, um eine einzelne Zeilenelement im zu präsentieren einer `ListView` oder `GridView` Gruppe anzeigen.

![Beispiel für die Kartenansicht](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Bearbeiten von Text](~/android/user-interface/controls/edit-text.md)

`EditText` ist ein UI-Element, das zum eingeben und Ändern des Texts verwendet wird.

![Bearbeiten der Beispieltext](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Katalog](~/android/user-interface/controls/gallery.md)

`Gallery` ist ein Layout-Widget, das zum Anzeigen von Elementen in einer Liste mit horizontalem Bildlauf verwendet wird. Sie haben die aktuelle Auswahl in der Mitte der Ansicht positioniert.

![Beispiel-Katalog](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)

Die *Navigationsleiste* stellt Steuerelemente für die Seitennavigation auf Geräten, die keine Hardwaretasten für enthalten **Startseite**, **wieder**, und **Menü**.

![Beispiel-Navigationsleiste](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Auswahl](~/android/user-interface/controls/pickers/index.md)

*Datumsauswahl* sind Elemente der Benutzeroberfläche, mit denen den Benutzer ein Datum oder eine Uhrzeit auswählen, durch die Verwendung von Dialogen an, die von Android bereitgestellt werden können.

![Beispiel-Auswahl](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Popupmenü](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` Dient zum Anzeigen von Popupmenüs, die eine bestimmte Ansicht zugeordnet sind.

![Beispiel-Popupmenü](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

Ein `RatingBar` ist das Benutzeroberflächenelement, das eine Bewertung in Sterne anzeigt.

![Beispiel für eine RatingBar](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Drehfeld](~/android/user-interface/controls/spinner.md)

`Spinner` ist ein UI-Element, das eine schnelle Möglichkeit, wählen Sie einen Wert aus einem Satz bereitstellt. Es handelt sich um Simmilar auf eine Dropdown-Liste. 

![Beispiel für Spinner](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Schalter](~/android/user-interface/controls/switch.md)

`Switch` ist das Benutzeroberflächenelement, mit dem Benutzer zum Umschalten zwischen zwei Zuständen, z. B. auf oder aus. Die `Switch` Standardwert ist OFF.

![Beispiel-Schalter](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` ist eine Ansicht, die hardwarebeschleunigtes 2D-Rendering verwendet, um ein Video oder einen OpenGL-Inhaltsdatenstrom anzuzeigende zu ermöglichen.

![Beispiel-Texturansicht](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

Die `Toolbar` Widget (eingeführt in Android 5.0 Lollipop) kann als eine Generalisierung der Aktion Leiste-Schnittstelle betrachtet werden &ndash; es dient zum Ersetzen der Aktionsleiste. Die `Toolbar` kann überall in einem app-Layout verwendet werden, und es ist viel mehr als eine Aktionsleiste angepasst.

![Beispiel-Symbolleiste](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

Die `ViewPager` ist ein Layout-Manager, der dem Benutzer ermöglicht, links und rechts durchlaufen, um Daten zu kippen.

![Beispiel-ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` ist ein UI-Element mit dem Sie Ihre eigenen Fenster für die Anzeige von Webseiten erstellen (oder sogar einen vollständigen Browser entwickeln).

![Beispiel-Web-Ansicht](images/web-view.png)

