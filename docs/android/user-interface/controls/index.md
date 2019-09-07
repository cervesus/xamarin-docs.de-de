---
title: Android-Steuerelemente (Widgets)
description: Bausteine zum Erstellen von xamarin. Android-Benutzeroberflächen
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: e3f6524f03612ee39c537f482b1db916ecf08a23
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759102"
---
# <a name="xamarinandroid-controls-widgets"></a>Xamarin. Android-Steuerelemente (Widgets)

Xamarin. Android macht alle von Android bereitgestellten nativen Benutzeroberflächen-Steuerelemente (Widgets) verfügbar. Diese Steuerelemente können problemlos xamarin. Android-Apps mithilfe des Android Designer oder Programm gesteuert über XML-Layoutdateien hinzugefügt werden. Unabhängig davon, welche Methode Sie auswählen, macht xamarin. Android alle Eigenschaften und Methoden der Benutzeroberflächen Objekte in C#verfügbar. In den folgenden Abschnitten werden die gängigsten Android-Benutzeroberflächen-Steuerelemente vorgestellt und erläutert, wie Sie in xamarin. Android-Apps integriert werden.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Aktionsleiste](~/android/user-interface/controls/action-bar.md) 

`ActionBar`ist eine Symbolleiste, auf der der Aktivitäts Titel, Navigations Schnittstellen und andere interaktive Elemente angezeigt werden. In der Regel wird die Aktionsleiste am oberen Rand des Fensters der Aktivität angezeigt.

![Beispiel für eine Aktionsleiste](images/action-bar.png)

## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Automatische Vervollständigung](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView`ein bearbeitbares Text Ansichts Element, das die Vervollständigungs Vorschläge automatisch anzeigt, während der Benutzer die Eingabe durchläuft. Die Liste der Vorschläge wird in einem Dropdown Menü angezeigt, in dem der Benutzer ein Element auswählen kann, um den Inhalt des Bearbeitungs Felds durch zu ersetzen.

![Beispiel für Auto Vervollständigen](images/auto-complete.png)

## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Schaltflächen](~/android/user-interface/controls/buttons/index.md)

Schaltflächen sind UI-Elemente, auf die der Benutzer tippt, um eine Aktion auszuführen.

![Beispiel Schaltflächen](images/buttons.png)

## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Kalender](~/android/user-interface/controls/calendar.md)

Die `Calendar` -Klasse wird verwendet, um eine bestimmte Instanz zeitlich (einen Millisekundenwert, der von der Epoche abweicht) auf Werte wie z. b. Jahr, Monat, Stunde, Tag des Monats und das Datum der nächsten Woche zu umrechnen.
`Calendar`unterstützt eine Vielzahl von Interaktions Optionen mit Kalenderdaten, einschließlich der Möglichkeit, Ereignisse, Teilnehmer und Erinnerungen zu lesen und zu schreiben. Wenn Sie den Kalender Anbieter in Ihrer Anwendung verwenden, werden Daten, die Sie über die API hinzufügen, in der integrierten Kalender-App angezeigt, die mit Android ausgestattet ist.

![Beispiel Kalender](images/calendar.png)

## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView`ist eine Benutzeroberflächen Komponente, die Text-und Bildinhalte in Sichten darstellt, die Karten ähneln. `CardView`wird als `FrameLayout` Widget mit abgerundeten Ecken und einem Schatten implementiert. In der Regel `CardView` wird ein-Element verwendet, um ein einzelnes Zeilen `ListView` Element `GridView` in einer-oder-Ansichts Gruppe darzustellen.

![Beispiel Kartenansicht](images/cardview.png)

## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Text bearbeiten](~/android/user-interface/controls/edit-text.md)

`EditText`ist ein Benutzeroberflächen Element, das zum eingeben und Ändern von Text verwendet wird.

![Beispiel für Bearbeitungs Text](images/edit-text.png)

## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Katalog](~/android/user-interface/controls/gallery.md)

`Gallery`ist ein layoutwidget, das verwendet wird, um Elemente in einer horizontalen Bildlauf-Liste anzuzeigen. die aktuelle Auswahl wird in der Mitte der Ansicht positioniert.

![Beispiel Katalog](images/gallery.png)

## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)

Die *Navigationsleiste* stellt Navigations Steuerelemente auf Geräten bereit, die keine Hardware Schaltflächen für **Home**, **Back**und **Menü**enthalten.

![Beispiel-Navigationsleiste](images/navigation-bar.png)

## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Auswahl](~/android/user-interface/controls/pickers/index.md)

*Picker* sind Elemente der Benutzeroberfläche, die es dem Benutzer ermöglichen, ein Datum oder eine Uhrzeit mithilfe von Dialogfeldern auszuwählen, die von Android bereitgestellt werden.

![Beispiel Auswahl](images/picker.png)

## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Popupmenü](~/android/user-interface/controls/popup-menu.md)

`PopupMenu`wird zum Anzeigen von Popup Menüs verwendet, die an eine bestimmte Ansicht angefügt sind.

![Beispielpopup-Menü](images/popup-menu.png)

## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[Ratingleiste](~/android/user-interface/controls/ratingbar.md)

Ein `RatingBar` ist ein Benutzeroberflächen Element, das eine Bewertung in Sternen anzeigt.

![Beispiel für eine ratingleiste](ratingbar-images/01-ratingbar.png)

## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Drehfeld](~/android/user-interface/controls/spinner.md)

`Spinner`ist ein Benutzeroberflächen Element, das eine schnelle Möglichkeit bietet, einen Wert aus einer Menge auszuwählen. Es ist ein simmilar einer Dropdown Liste. 

![Beispiel Spinner](images/spinner.png)

## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Schalter](~/android/user-interface/controls/switch.md)

`Switch`ist ein Benutzeroberflächen Element, das es einem Benutzer ermöglicht, zwischen zwei Zuständen umzuschalten, z. b. ein-oder ausschalten. Der `Switch` Standardwert ist off.

![Beispiel Wechsel](images/switch.png)

## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView`bei handelt es sich um eine Ansicht, in der Hardware beschleunigtes 2D-Rendering verwendet wird, um ein Video oder einen OpenGL-Inhaltsstream anzuzeigen.

![Beispiel für eine Textur Ansicht](images/texture-view.png)

## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

Das `Toolbar` Widget (eingeführt in Android 5,0 Lollipop) kann sich als Generalisierung der Aktionsleisten Schnittstelle &ndash; vorstellen, die die Aktionsleiste ersetzen soll. Der `Toolbar` kann an beliebiger Stelle in einem App-Layout verwendet werden, und er ist viel anpassbarer als eine Aktionsleiste.

![Beispiel Symbolleiste](images/toolbar.png)

## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

Der `ViewPager` ist ein Layoutmanager, mit dem der Benutzer nach links und rechts durch Datenseiten blättern kann.

![Beispiel für "viewpager"](images/viewpager.png)

## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView`ist ein Benutzeroberflächen Element, mit dem Sie ein eigenes Fenster zum Anzeigen von Webseiten erstellen können (oder sogar einen kompletten Browser entwickeln).

![Beispiel für Webansicht](images/web-view.png)
