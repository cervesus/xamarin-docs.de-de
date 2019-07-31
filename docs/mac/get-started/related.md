---
title: 'Xamarin.Mac: Verwandte Dokumentationen'
description: 'Dieses Dokument enthält Links zu Dokumentationen, die für Xamarin.Mac-Entwickler relevant sind: Xamarin.iOS-Dokumentation, Mac Developer Center von Apple sowie verschiedene Leitfäden, in denen das Erstellen von Benutzeroberflächen mit Xamarin.Mac beschrieben wird.'
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 12/02/2016
ms.openlocfilehash: 2e4c2ebea7783fe2400298b03a54e951d89960fe
ms.sourcegitcommit: 9a2a21974d35353c3765eb683ef2fd7161c1d94a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68329933"
---
# <a name="xamarinmac-related-documentation"></a>Xamarin.Mac: Verwandte Dokumentationen

Zusätzlich zum Mac-Bereich auf [Microsoft-Dokumentation](~/mac/get-started/index.md) gibt es drei Stellen, an denen Sie Dokumentationen finden, die Ihnen helfen können, wenn Sie Fragen zu Xamarin.Mac haben:

- [**Die Xamarin.iOS-Dokumentation**](~/ios/get-started/index.md): Für viele APIs (hauptsächlich außerhalb von AppKit und UIKit) bestehen nur geringfügige Unterschiede zwischen den iOS- und den macOS-Versionen. Wenn eine iOS-API z.B. den Namen `UIFoo` hat, können Sie häufig eine ähnliche API mit dem Namen `NSFoo` unter macOS finden. Diese Beispiele sind prinzipiell schon in C#.

- **Das [Mac Developer Center](https://developer.apple.com/devcenter/mac/) von Apple**: Häufig können Beispiele der aufzurufenden APIs in Objective-C unkompliziert in C# konvertiert werden. Weitere Informationen dazu finden Sie unter [Understanding Mac APIs (Grundlegendes zu Mac-APIs)](~/mac/app-fundamentals/mac-apis.md).

- [**Stack Overflow**](https://stackoverflow.com/): eine praktische Ressource für einfache, einmalige Fragen wie die folgende: [Wie kann ich alle Knoten in NSOutlineView automatisch erweitern?](https://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Diese Beispiele sind oft in Objective-C geschrieben und müssen in C# konvertiert werden. Es gibt jedoch auch einige Antworten in C#.

## <a name="user-interface"></a>Benutzeroberfläche

Ein Entwickler, der mit C# und .NET in einer Xamarin.Mac-Anwendung arbeitet, hat Zugang zu denselben Benutzeroberflächen-Steuerelementen wie ein Entwickler, der mit *Objective-C* und *Xcode* arbeitet. Da Xamarin.Mac direkt in Xcode integriert ist, kann der Entwickler _Interface Builder_ von Xcode verwenden, um die Benutzeroberfläche einer App zu erstellen und zu verwalten (oder um diese optional in C#-Code zu erstellen).

In folgenden Leitfäden erhalten Sie ausführliche Informationen zum Arbeiten mit macOS-Elementen in einer Xamarin.Mac-Anwendung:

- [Windows](~/mac/user-interface/window.md)
- [Dialogfelder](~/mac/user-interface/dialog.md)
- [Benachrichtigungen](~/mac/user-interface/alert.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Symbolleisten](~/mac/user-interface/toolbar.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
