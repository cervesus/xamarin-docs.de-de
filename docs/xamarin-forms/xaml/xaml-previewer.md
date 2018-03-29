---
title: Verwendung von XAML-Vorschau für Xamarin.Forms
description: Wir sehen Sie uns die Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe!
ms.topic: article
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 02/24/2017
ms.openlocfilehash: a7a9c4fed92cb4ed8c8c12e97129bc8379037acb
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="xaml-previewer-for-xamarinforms"></a>Verwendung von XAML-Vorschau für Xamarin.Forms

_Wir sehen Sie uns die Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe!_

## <a name="requirements"></a>Anforderungen

Projekte erfordern das aktuellste Xamarin.Forms-NuGet-Paket für die Verwendung von XAML-Vorschau zu arbeiten. Anzeigen einer Vorschau von Android-apps erfordert [JDK 1.8 X64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Es sind weitere Informationen in den [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Erste Schritte

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Verwenden der **Ansicht > Weitere Fenster > Xamarin.Forms-Vorschau** Menü in Visual Studio zum Öffnen des Vorschaufensters. Verwenden der **Fenster > Neue vertikale Registerkartengruppe** -Menü, um die Seite-an-Seite zu positionieren.

[![ListView-Steuerelement-Vorschau in Visual Studio](xaml-previewer-images/xamlp-list-vs-sml.png "Forms-Vorschau in Visual Studio")](xaml-previewer-images/xamlp-list-vs.png#lightbox "Forms-Vorschau in Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die **Vorschau** Schaltfläche im Editor angezeigt werden kann, durch einen XAML-Datei mit der rechten Maustaste und Auswählen von **Öffnen mit > Verwendung von XAML-Viewer**. Im Vorschaufenster klicken Sie dann angezeigt oder ausgeblendet werden kann durch Drücken der **Vorschau** Schaltfläche in der oberen rechten Ecke des jedes Dokumentfenster XAML:

[![ListView-Steuerelement-Vorschau in Visual Studio für Mac](xaml-previewer-images/xamlp-list-sml.png "Forms-Vorschau in Visual Studio für Mac")](xaml-previewer-images/xamlp-list.png#lightbox "Forms-Vorschau in Visual Studio für Mac")

-----

## <a name="xaml-preview-options"></a>Verwendung von XAML-Vorschau-Optionen

Die Optionen am oberen Rand des Vorschaufensters sind:

* **Phone** – in einem Bildschirm Phone Größe Rendern
* **Tablet** – in einem Tablet-Größe Bildschirm rendern (Beachten Sie, dass es Zoomsteuerelemente auf der unteren rechten Ecke des Bereichs)
* **Android** – die Android-Version des Bildschirms anzeigen
* **iOS** – die iOS-Version des Bildschirms anzeigen
* Portait (Symbol) – verwendet Hochformat für die Vorschau
* Querformat (Symbol) – verwendet Querformat für die Vorschau

## <a name="adding-design-time-data"></a>Hinzufügen von Daten zur Entwurfszeit

Einige Layouts möglicherweise schwer zu visualisieren ohne Daten an Steuerelemente der Benutzeroberfläche gebunden. Um die Vorschau nützlicher zu gestalten, weisen Sie einige statische Daten die Steuerelemente von hartcodierten einen Bindungskontext (entweder im Code-Behind oder mit XAML).

Finden Sie in der James [Blogbeitrag zum Hinzufügen von Entwurfszeitdaten](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) zum Binden an eine statische ViewModel in XAML finden Sie unter.

## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie die folgenden Probleme und die [Xamarin-Foren](https://forums.xamarin.com/categories/xamarin-forms), wenn Probleme auftreten.

### <a name="xaml-preview-isnt-showing"></a>Verwendung von XAML-Vorschau nicht angezeigt.

Überprüfen Sie folgende Punkte:

* Projekt sollte (kompilierte) erstellt werden, bevor Sie versuchen, die XAML-Dateien in der Vorschau anzeigen.
* Der Designer-Agent muss Einrichtung erstmalig Sie eine XAML-Datei in der Vorschau anzeigen - eine Statusanzeige wird angezeigt, in der Vorschau, zusammen mit der statusmeldungen, bis das bereit ist.
* Versuchen Sie es schließen und erneutes Öffnen der XAML-Datei.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>Ungültiges XAML: Das Android-Projekt erstellt, bevor der Vorschau kann erstellt werden muss

Die Verwendung von XAML-Vorschau erfordert, dass das Projekt erstellt werden, vor dem Rendern einer Seite.
Wenn die nachstehende Fehlermeldung am oberen Rand im Vorschaufenster angezeigt wird, erstellen Sie die Anwendung neu, und wiederholen Sie den Vorgang.

![Fehlermeldung: Projekt muss zuerst erstellt](xaml-previewer-images/error-not-built-sml.png "Fehlermeldung: das Projekt neu erstellen")
