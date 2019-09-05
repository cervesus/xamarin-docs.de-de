---
title: Grundlagen der xamarin. Mac-Anwendung
description: Dieses Dokument ist mit Anleitungen verknüpft, in denen verschiedene Konzepte beschrieben werden, die für die Entwicklung von xamarin. Mac-Anwendungen erforderlich sind.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 12/17/2015
ms.openlocfilehash: 73ec847b697c2d588d0c217bcbf12d4f0b6aa817
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291344"
---
# <a name="xamarinmac-application-fundamentals"></a>Grundlagen der xamarin. Mac-Anwendung

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Allgemeine Muster und Idiome](~/mac/app-fundamentals/patterns.md)

In den Apple-APIs, C#die über verfügbar gemacht werden, werden bestimmte Idiome und Muster wieder immer wieder hochskalieren. Wenn Sie Erfahrungen mit der Programmierung mit xamarin. IOS haben, sind diese möglicherweise vertraut. Die Dokumentation bezieht sich häufig auf diese Muster und Idiome, sodass ein solides Verständnis der Dokumente Ihnen hilft, die von Ihnen festzulegende Dokumentation zu verstehen.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Informationen zu Mac-APIs](~/mac/app-fundamentals/mac-apis.md)

Für einen Großteil ihrer Zeit mit xamarin. Mac können Sie sich mit den zugrunde liegenden Ziel-C- C# APIs in Gedanken machen, lesen und schreiben. Manchmal müssen Sie jedoch die API-Dokumentation von Apple lesen, eine Antwort von Stack Overflow auf eine Lösung für Ihr Problem übersetzen oder eine vorhandene Stichprobe vergleichen.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Konsolen-Apps](~/mac/app-fundamentals/console.md)

Sie können auch "Start lose" Konsolen-Apps erstellen, die mit xamarin. Mac auf native macOS-APIs zugreifen.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Arbeiten mit XIb-Dateien](~/mac/app-fundamentals/xib.md)

In diesem Artikel wird das Arbeiten mit XIb-Dateien erläutert, die in Xcode-Interface Builder erstellt wurden, um Benutzeroberflächen für eine xamarin. Mac-Anwendung zu erstellen und zu verwalten.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[Storyboard/. XIb-Design ohne Benutzeroberfläche](~/mac/app-fundamentals/xibless-ui.md)

In diesem Artikel wird beschrieben, wie Sie die Benutzeroberfläche einer xamarin. C# Mac-Anwendung direkt aus dem Code erstellen, ohne die Interface Builder von Xcode mit. Storyboard-oder XIb-Dateien zu verwenden.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Arbeiten mit Bildern](~/mac/app-fundamentals/image.md)

In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer xamarin. Mac-Anwendung behandelt. Es behandelt das Erstellen und Verwalten der Images, die zum Erstellen des Anwendungs Symbols und Verwenden von Bildern C# sowohl im Code als auch in der Interface Builder von Xcode benötigt werden.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Datenbindung und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md)

In diesem Artikel wird die Verwendung von Schlüssel-Wert-Codierung und Schlüssel-Wert-Beobachtung erläutert, um die Datenbindung an Benutzeroberflächen Elemente in der Interface Builder von Xcode zuzulassen. Mit dieser Technik verringern Sie die Menge an C# Code, der für Ihre xamarin. Mac-Anwendung geschrieben werden muss, erheblich. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Arbeiten mit Datenbanken](~/mac/app-fundamentals/databases.md)

In diesem Artikel wird die Verwendung von Schlüssel-Wert-Codierung und Schlüssel-Wert-Beobachtung erläutert, um die Datenbindung mit direktem Zugriff auf SQLite-Datenbanken auf Benutzeroberflächen Elemente in der Interface Builder von Xcode zuzulassen. Außerdem wird die Verwendung des sqlite.net ORM zum Bereitstellen des Zugriffs auf SQLite-Daten behandelt.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Arbeiten mit Kopieren und Einfügen](~/mac/app-fundamentals/copy-paste.md)

In diesem Artikel wird das Arbeiten mit dem Zwischenablage erläutert, um das Kopieren und Einfügen in eine xamarin. Mac-Anwendung zu ermöglichen. Es wird gezeigt, wie Sie mit Standard Datentypen arbeiten, die von mehreren apps gemeinsam genutzt werden können, und wie Sie benutzerdefinierte Daten in einer Give-App unterstützen.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Sandboxing einer xamarin. Mac-app](~/mac/app-fundamentals/sandboxing.md)

In diesem Artikel wird das Sandkasten einer xamarin. Mac-Anwendung für die Veröffentlichung im App Store behandelt. Sie deckt alle Elemente ab, die in Sandboxing fließen: Container Verzeichnisse, Berechtigungen, Benutzer seitig festgelegte Berechtigungen, Berechtigungs Trennung und Kernel Erzwingung.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Wiedergabe von Sound mit AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

In diesem Artikel wird gezeigt, wie Sie eine Hilfsklasse verwenden, um die Wiedergabe von Sound mithilfe von AVAudioPlayer zu steuern.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Melden von Fehlern](~/mac/app-fundamentals/troubleshooting.md)

Manchmal bleiben wir bei der Arbeit an einem Projekt auf dem Weg, entweder wenn Sie nicht in der Lage sind, eine API so zu arbeiten, wie wir möchten, oder wenn Sie versuchen, einen Fehler zu umgehen. Unser Ziel von xamarin besteht darin, dass Sie Ihre mobilen und Desktop Anwendungen erfolgreich schreiben können. Wir haben einige Ressourcen bereitgestellt, die Ihnen helfen.
