---
title: Xamarin.Mac-Registrierungsstelle
description: Dieses Dokument beschreibt den Zweck von der Registrierungsstelle Xamarin.Mac und die dynamische, static und partielle statische (Hybrid) Nutzung Konfigurationen.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 21e1a2c6ae5a9ad8b6acf520851ea92a340da887
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032497"
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac-Registrierungsstelle

_Dieses Dokument beschreibt den Zweck der Xamarin.Mac-Registrierungsstelle und seiner verschiedene Konfigurationen._

## <a name="overview"></a>Übersicht

Xamarin.Mac schließt die Lücke zwischen der verwalteten Welt (.NET) und Cocoa Laufzeit ermöglicht verwaltete Klassen zum Aufrufen von nicht verwalteter Objective-C-Klassen und aufgerufen werden, wenn Ereignisse auftreten. Der erforderlichen Aufwand zum Durchführen dieser "magischen" wird von der Registrierungsstelle verarbeitet und wird im Allgemeinen verborgen.

Es gibt Leistungseinbußen bei der dieser Erfassung, insbesondere beim Start der Anwendung Zeit, und was "hinter den Kulissen" geht ein wenig verstehen kann manchmal hilfreich sein.

## <a name="configurations"></a>Konfigurationen

Die Registrierungsstelle Auftrag beim Start kann grundsätzlich in zwei Kategorien unterteilt werden:

- Überprüfen Sie alle verwalteten Klasse für die Ableitung von NSObject und Sammeln Sie eine Liste von Elementen, die für die Objective-C-Laufzeit verfügbar gemacht werden.
- Registrieren Sie diese Informationen, mit der Objective-C-Laufzeit.

Im Laufe der Zeit wurden drei Konfigurationen von anderen Registrierungsstelle erstellt, um verschiedene Fälle abdecken. Jedes anderen Build hat, und führen die Zeit folgen:

- **Dynamische Registrierungsstelle** – verwenden Sie während des Starts .NET Reflection zum Überprüfen von jedem geladenen Typ bestimmen die Liste der relevanten Elemente aus, und die native-Runtime zu informieren. Diese Option wird keine Zeit mit dem Build hinzugefügt, aber es ist sehr aufwändig zu berechnen sind beim Start (bis zu mehrere Sekunden).
- **Statische Registrierungsstelle** – während des Buildvorgangs, berechnen Sie den Satz von Elementen, die registriert werden und Objective-C-Code für die Registrierung zu generieren. Dieser Code wird während des Startvorgangs schnell registrieren alle Elemente aufgerufen. Fügt eine signifikante Pause zu erstellen, kann jedoch eine viel Zeit vom Start der Anwendung Ausschneiden hinzu.
- **"Partielle" statische** – ein neuer "Hybrid"-Ansatz der Großteil der Vorteile bringt. Da die Exporte aus **"xamarin.Mac.dll"** sind konstant, Speichern einer vorausberechnete-Bibliothek in ihre Registrierung verarbeiten und zu verknüpfen, in. Mithilfe von Reflektion benutzerbibliotheken behandeln, aber als benutzerbibliotheken exportieren deutlich weniger Typen an, die die Bindungen für die Plattform dies oft recht schnell ist. Eine neglectable Auswirkungen erstellen, und reduziert die große Mehrzahl der dynamischen "Kosten".

Heute teilweise statisch ist die Standardeinstellung für die Debugkonfiguration und statische ist die Standardeinstellung für Releasekonfigurationen.

Es gibt einige Szenarien:

- Laden nach dem Start mit Klassen, die von NSObject-Plug-Ins
- Dynamisch erstellte Klasseninstanzen, die von NSObject

in denen die Registrierungsstelle ist nicht erkennbar, dass er eine Art beim Start zu registrieren muss. Die `ObjCRuntime.Runtime.RegisterAssembly` Methode wird bereitgestellt, um der Registrierungsstelle zu informieren, dass zusätzliche Typen zu berücksichtigen sind.
