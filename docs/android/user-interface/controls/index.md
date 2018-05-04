---
title: Android-Steuerelemente (Widgets)
description: Bausteine zum Erstellen von Benutzeroberflächen Xamarin.Android
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 28418c3b3fedcd24963008eb3b59ffa782d791f1
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="android-controls-widgets"></a>Android-Steuerelemente (Widgets)

Xamarin.Android macht alle systemeigenen Steuerelemente der Benutzeroberfläche (Widgets) von Android bereitgestellt. Diese Steuerelemente können leicht auf Xamarin.Android-apps, die mit dem Android-Designer oder programmgesteuert über XML-Layout-Dateien hinzugefügt werden. Unabhängig davon, welche Methode Sie wählen, Xamarin.Android aller Methoden in C# geschrieben und Benutzeroberflächeneigenschaften Objekt verfügbar macht. In den folgenden Abschnitten stellen die am häufigsten verwendeten Android Benutzeroberflächen-Steuerelemente vor und wird erläutert, wie sie in Xamarin.Android apps integrieren.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Aktionsleiste](~/android/user-interface/controls/action-bar.md) 

`ActionBar` ist eine Symbolleiste, die den aktivitätstitel, Navigation Schnittstellen und andere interaktive Elemente werden angezeigt. In der Regel wird die Aktionsleiste am oberen Rand einer Aktivität im Fenster angezeigt.

![Beispiel ActionBar](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Automatische Vervollständigung](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` ist ein bearbeitbares Textfeld Ansicht-Element, in der Abschluss Vorschläge automatisch angezeigt, während der Benutzer eingibt. Die Liste mit Vorschlägen wird in ein Dropdown-Menü angezeigt, in dem der Benutzer ein Element hinzu, ersetzen Sie den Inhalt im Bearbeitungsfeld mit auswählen kann.

![Beispiel für die automatische Vervollständigung](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Schaltflächen](~/android/user-interface/controls/buttons/index.md)

Schaltflächen sind Benutzeroberflächenelemente, die der Benutzer tippt, um eine Aktion auszuführen.

![Beispiel-Schaltflächen](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Kalender](~/android/user-interface/controls/calendar.md)

Die `Calendar` Klasse dient zum Konvertieren in einer bestimmten Instanz von Werten, z. B. Jahr, Monat, Stunde, Tag des Monats, und das Datum der nächsten Woche Zeit (einen Millisekundenwert, der von der Epoche versetzt wird).
`Calendar` unterstützt eine Vielzahl von Optionen der Interaktion mit Kalenderdaten, einschließlich der Möglichkeit zum Lesen und Schreiben von Ereignissen, Teilnehmer und Erinnerungen an. Mithilfe des Calendar-Anbieters in Ihrer Anwendung werden Daten, die Sie über die API hinzufügen in der integrierten Kalender-app angezeigt, die mit Android geliefert wird.

![Beispiel-Kalender](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` ist eine UI-Komponente, die Text- und Image-Inhalt in den Ansichten bereitgestellt werden, die Karten ähneln. `CardView` wird als implementiert eine `FrameLayout` Widget mit abgerundeten Ecken und Schatten. In der Regel eine `CardView` dient zum Darstellen einer einzeiligen Element in einem `ListView` oder `GridView` Gruppe "Ansicht".

![Beispiel für die Kartenansicht](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Bearbeiten von Text](~/android/user-interface/controls/edit-text.md)

`EditText` ist ein Benutzeroberflächenelement, das für die Eingabe, und Ändern von Text verwendet wird.

![Bearbeiten der Beispieltext](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Katalog](~/android/user-interface/controls/gallery.md)

`Gallery` ist ein Widget eines Layouts, das zum Anzeigen von Elementen in einer horizontal bildlauffähigen Liste verwendet wird. die aktuelle Auswahl positioniert in der Mitte der Ansicht.

![Beispiel-Katalog](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)

Die *Navigationsleiste* stellt Steuerelemente für die Seitennavigation auf Geräten, die kein Hardwaretasten für **Home**, **wieder**, und **Menü**.

![Beispiel-Navigationsleiste](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Auswahl](~/android/user-interface/controls/pickers/index.md)

*Bildlaufbereich* sind Elemente der Benutzeroberfläche, mit denen den Benutzer ein Datum oder eine Uhrzeit auswählen, mithilfe der Dialogfelder, die von Android bereitgestellt werden können.

![Beispiel-Auswahl](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Popupmenü](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` Dient zum Anzeigen von Popupmenüs, die eine bestimmte Sicht zugeordnet sind.

![Beispiel-Popupmenü](images/popup-menu.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Drehfeld](~/android/user-interface/controls/spinner.md)

`Spinner` ist ein Element der Benutzeroberfläche, die eine schnelle Möglichkeit, wählen Sie einen Wert aus einem Satz bereitstellt. Es handelt sich um Simmilar auf eine Dropdown-Liste. 

![Beispiel "Spinner"](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Schalter](~/android/user-interface/controls/switch.md)

`Switch` ist ein Benutzeroberflächenelement, mit dem Benutzer Umschalten zwischen zwei Zuständen, z. B. unter oder deaktiviert. Die `Switch` Standardwert ist OFF.

![Beispiel-Switch](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` ist eine Sicht, die Hardware accelerated 2D Rendering verwendet wird, so aktivieren Sie ein Video oder OpenGL-inhaltsdatenstroms angezeigt werden.

![Beispiel-Texturansicht](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

Die `Toolbar` Widget (eingeführt in Android 5.0 Lollipop) kann als eine Generalisierung der Aktion Leiste Schnittstelle betrachtet werden &ndash; die Aktionsleiste ersetzen soll. Die `Toolbar` überall in einem app-Layout verwendet werden kann, und es viel mehr als eine Aktionsleiste anpassbar ist.

![Beispiel-Symbolleiste](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

Die `ViewPager` ist ein Layout-Manager, die dem Benutzer ermöglicht, links und rechts durch Datenseiten kippen.

![Beispiel ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` ist ein Benutzeroberflächenelement, mit dem Sie eigene Fenster für die Anzeige von Webseiten zu erstellen (oder selbst entwickeln einen vollständige Browser).

![Beispiel-Webansicht](images/web-view.png)

