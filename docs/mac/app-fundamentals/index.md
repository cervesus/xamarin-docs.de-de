---
title: Grundlagen der Xamarin.Mac-Anwendung
description: Dieses Dokument enthält Links zu Leitfäden, die zum Verständnis bei der Entwicklung von Xamarin.Mac-Anwendungen erforderlich sind verschiedene Konzepte zu beschreiben.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: e085cf33615d216e1ce9963254050ef2b623ae40
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320803"
---
# <a name="xamarinmac-application-fundamentals"></a>Grundlagen der Xamarin.Mac-Anwendung

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Allgemeine Muster und Ausdrücke](~/mac/app-fundamentals/patterns.md)

In der Apple-APIs über C# -Code verfügbar gemacht wird aktiviert bestimmte Ausdrücke und Muster immer wieder. Wenn Sie keine Erfahrung mit Programmierung mit Xamarin.iOS verfügen, können dies vertraut sein. Dokumentation verweist oft diese Muster und Idiome wiederholt wird, auf daher müssen solide Kenntnisse über diese können Sie der Dokumentation sinnvoll sein, die Sie zu finden.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Grundlegendes zu Mac-APIs](~/mac/app-fundamentals/mac-apis.md)

Für einen Großteil Ihrer Zeit für die Entwicklung mit Xamarin.Mac können Sie denken, lesen und ohne viel Beachtung mit den zugrunde liegenden Objective-C-APIs in c# schreiben. Jedoch werden manchmal müssen, lesen die API-Dokumentation von Apple, eine Antwort von Stack Overflow zu einer Lösung für Ihr Problem zu übersetzen, oder vergleichen, um ein Beispiel für eine vorhandene.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Konsolen-apps](~/mac/app-fundamentals/console.md)

Sie können auch "monitorlose" Konsolen-apps, die native MacOS-APIs zugreifen erstellen mit Xamarin.Mac.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Arbeiten mit XIB-Dateien](~/mac/app-fundamentals/xib.md)

Dieser Artikel behandelt die Arbeit mit XIB-Dateien, die in Xcode Interface Builder erstellen und Verwalten von Benutzeroberflächen für eine Xamarin.Mac-Anwendung erstellt.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIB weniger Entwurf der Benutzeroberfläche](~/mac/app-fundamentals/xibless-ui.md)

Dieser Artikel behandelt die Benutzeroberfläche einer Xamarin.Mac-Anwendung direkt aus dem C#-Code ohne Verwendung von Interface Builder von Xcode mit Storyboard oder XIB-Dateien zu erstellen.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Arbeiten mit Bildern](~/mac/app-fundamentals/image.md)

In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer Xamarin.Mac-Anwendung behandelt. Es wird beschrieben, erstellen und Verwalten der Images, die zum Erstellen von Symbol und Verwendung von Images in C#-Code und Interface Builder von Xcode Ihrer Anwendung erforderlich sind.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Die Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md)

Dieser Artikel befasst sich mit der Codierung mit Schlüssel-Wert und Schlüssel-Wert beachtet werden, um die Datenbindung an Benutzeroberflächenelemente in Interface Builder von Xcode zu ermöglichen. Mit diesem Verfahren können Sie deutlich verringern die Menge der C#-Code, der für die Xamarin.Mac-Anwendung geschrieben werden muss. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Arbeiten mit Datenbanken](~/mac/app-fundamentals/databases.md)

Dieser Artikel befasst sich mit der Codierung mit Schlüssel-Wert und Schlüssel-Wert für die Datenbindung mit direktem Zugriff auf die SQLite-Datenbanken, um Benutzeroberflächenelemente in Interface Builder von Xcode zu beobachten. Hierin sind auch mit, dass die von SQLite.NET ORM bieten Zugriff auf SQLite-Daten.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Arbeiten mit kopieren und einfügen](~/mac/app-fundamentals/copy-paste.md)

Dieser Artikel behandelt die Verwendung der Zwischenablage kopieren und fügen in einer Xamarin.Mac-Anwendung. Es zeigt das Arbeiten mit standard-Datentypen, die mehrere apps und Unterstützung für benutzerdefinierte Daten in einer app bieten gemeinsam genutzt werden können.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Sandkasten einer Xamarin.Mac-app](~/mac/app-fundamentals/sandboxing.md)

Dieser Artikel behandelt die Sandbox einer Xamarin.Mac-Anwendung zur Freigabe zu den App Store. Hierin sind alle Elemente, die in der Sandbox eingehen: Containerverzeichnisse, Berechtigungen, Benutzer bestimmte Berechtigungen, die Trennung von Berechtigungen und Erzwingung von Kernel.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Wiedergabe von sound mit AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

In diesem Artikel zeigt, wie eine Hilfsklasse zum Steuern der Wiedergabe von sound mithilfe einer AVAudioPlayer verwendet wird.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Melden von Fehlern](~/mac/app-fundamentals/troubleshooting.md)

In einigen Fällen abstürzen wir alle bei der Arbeit an einem Projekt auf die Unfähigkeit, erhalten eine API für die gewünschte arbeiten oder in einen Fehler umgehen möchten. Unser Ziel bei Xamarin ist es für Sie erfolgreich in Ihre mobilen und desktop-Anwendungen schreiben, und wir haben einige Ressourcen für bereitgestellt.
