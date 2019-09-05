---
title: Xamarin. Mac-Registrierungsstelle
description: In diesem Dokument wird der Zweck der xamarin. Mac-Registrierungsstelle und der dynamischen, statischen und partiell statischen (Hybriden) Verwendungs Konfigurationen beschrieben.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: d44f445b0c3bcc6fd498372f6cdf3e20be39d5b5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290099"
---
# <a name="xamarinmac-registrar"></a>Xamarin. Mac-Registrierungsstelle

_In diesem Dokument wird der Zweck der xamarin. Mac-Registrierungsstelle und der unterschiedlichen Verwendungs Konfigurationen beschrieben._

## <a name="overview"></a>Übersicht

Xamarin. Mac verbindet die Lücke zwischen der verwalteten (.net) Welt und der Cocoa-Laufzeit, sodass verwaltete Klassen nicht verwaltete Ziel-C-Klassen aufrufen und bei Auftreten von Ereignissen wieder aufgerufen werden können. Die für diese "Magic" erforderliche Arbeit wird von der Registrierungsstelle behandelt und ist im Allgemeinen aus der Ansicht ausgeblendet.

Diese Registrierung wirkt sich auf die Leistung aus, insbesondere auf die Startzeit der Anwendung, und das Verständnis der Aktivitäten, die sich im Hintergrund befinden, kann manchmal hilfreich sein.

## <a name="configurations"></a>Konfigurationen

Im Grunde kann der Auftrag der Registrierungsstelle beim Start in zwei Kategorien unterteilt werden:

- Scannen Sie jede verwaltete Klasse nach den von NSObject abgeleiteten Klassen, und erfassen Sie eine Liste von Elementen, die für die Ziel-C-Laufzeit verfügbar gemacht werden sollen.
- Registrieren Sie diese Informationen bei der Ziel-C-Laufzeit.

Im Laufe der Zeit wurden drei verschiedene Registrierungs Konfigurationen erstellt, um verschiedene Anwendungsfälle abzudecken. Jede hat verschiedene Auswirkungen auf die Build-und Lauf Zeit:

- **Dynamische Registrierungs** Stelle – verwenden Sie während des Starts die .NET-Reflektion, um jeden geladenen Typ zu scannen, die Liste der relevanten Elemente zu bestimmen und die native Laufzeit zu informieren. Mit dieser Option wird dem Build keine Zeit hinzugefügt, aber es ist sehr aufwendig, während des Starts zu berechnen (bis zu mehrere Sekunden).
- **Statische Registrierungs** Stelle – während des Buildvorgangs berechnen Sie den Satz der zu registrierenden Elemente und generieren den Ziel-C-Code für die Registrierung. Dieser Code wird während des Starts aufgerufen, um schnell alle Elemente zu registrieren. Fügt eine bedeutende Pause zum Erstellen hinzu, kann jedoch einen beträchtlichen Zeitraum vom Anwendungsstart Ausschneiden.
- **"Partiell" statisch** – ein neuerer "Hybrid"-Ansatz, der den größten Teil der Vorteile von beidem bietet. Da die Exporte aus **xamarin. Mac. dll** konstant sind, speichern Sie eine Voraus berechnete Bibliothek, um Ihre Registrierung zu behandeln und diese in zu verknüpfen. Verwenden Sie Reflektion zur Handhabung von Benutzer Bibliotheken, aber da Benutzer Bibliotheken weitaus weniger Typen exportieren, die diese Platt Form Bindungen häufig verwenden, ist dies häufig sehr schnell. Eine vernachlässigbare Buildzeit-Auswirkung und reduziert einen Großteil der "Kosten" von Dynamic.

Heute ist partielle static der Standardwert für die Debugkonfiguration, und static ist die Standardeinstellung für Releasekonfigurationen.

Es gibt einige Szenarios:

- Plug-ins nach dem Start mit von NSObject abgeleiteten Klassen
- Dynamisch erstellte Klassen Instanzen, die von NSObject abgeleitet werden

die Registrierungsstelle kann nicht erkennen, dass Sie beim Start einen Typ registrieren muss. Die `ObjCRuntime.Runtime.RegisterAssembly` -Methode wird bereitgestellt, um der Registrierungsstelle mitzuteilen, dass Sie über zusätzliche zu berücksichtigende Typen verfügt
