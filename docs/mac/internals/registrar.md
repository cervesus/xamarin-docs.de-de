---
title: Xamarin.Mac Registrierungsstelle
description: Dieses Dokument beschreibt den Zweck der Xamarin.Mac-Registrierungsstelle und die statische dynamischer, statischer und teilweise (Hybrid) Nutzung Konfigurationen.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: b6e971e608c8b9228523222cebc4d6dac9395def
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792418"
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac Registrierungsstelle

_Dieses Dokument beschreibt den Zweck des Xamarin.Mac-Registrierungsstelle und seiner Verwendung der verschiedenen Konfigurationen._

## <a name="overview"></a>Überblick

Xamarin.Mac schließt die Lücke zwischen verwalteten weltweit (.NET) und des Kakao-Runtime verwaltete Klassen zum Aufrufen von nicht verwalteten Objective-C-Klassen und aufgerufen werden, wenn Ereignisse auftreten. Der Arbeitsaufwand zum Durchführen dieser "magische" müssen erfolgt durch die Registrierungsstelle und wird im Allgemeinen aus der Ansicht ausgeblendet.

Leistungseinbußen bei der diese Registrierung, insbesondere beim Anwendungsstart Betriebszeit, vorhanden sind und Grundlegendes zu einem gewissen Grad was "hinter den Kulissen" kann in einigen Fällen hilfreich sein.

## <a name="configurations"></a>Konfigurationen

Grundsätzlich kann die Registrierungsstelle Auftrag beim Start in zwei Kategorien unterteilt werden:

- Scannen Sie alle verwalteten Klasse für die Ableitung von NSObject und Sammeln Sie eine Liste von Elementen, die die Objective-C-Laufzeit zur Verfügung gestellt werden.
- Registrieren Sie diese Informationen, mit der Objective-C-Laufzeit.

Im Laufe der Zeit wurden drei andere Registrierungsstelle Konfigurationen erstellt, um verschiedene Anwendungsfälle abzudecken. Jeder anderen Build hat, und führen Zeit folgen im Klaren sein:

- **Dynamische Registrierungsstelle** – während des Starts mithilfe von Reflektion .NET jedem geladenen Überprüfungstyp, bestimmen die Liste der relevanten Elemente und die native Common Language Runtime zu informieren. Diese Option des Builds NULL Zeit hinzugefügt, aber es ist sehr aufwendig, um beim Start (bis zu mehrere Sekunden) zu berechnen.
- **Statische Registrierungsstelle** – während des Buildvorgangs des Satzes von Elementen registriert werden, und generieren Sie Objective-C-Code für die Registrierung zu berechnen. Dieser Code wird aufgerufen, während des Startvorgangs, um alle Elemente schnell zu registrieren. Fügt eine erhebliche Pause Build, kann jedoch sehr viel Zeit vom Start der Anwendung ausgeschnitten werden.
- **"Partiell" statischen** – eine neuere "Hybrider" Ansatz der Großteil der Vorteile beider geschaltet wird. Da die Exporte aus **Xamarin.Mac.dll** sind Konstanten, speichern Sie eine vorausberechnete Bibliothek aus, die Registrierung zu behandeln und zu verknüpfen, die in. Mithilfe der Reflektion benutzerbibliotheken behandeln, jedoch deutlich weniger Typen, dass die Bindungen für die Plattform dies oft vielmehr schnelle als benutzerbibliotheken zu exportieren. Eine neglectable erstellen zeitlichen Auswirkungen und einen Großteil der "Cost" dynamischen reduziert.

Partielle statische ist die Standardeinstellung für die Debugkonfiguration und statische ist die Standardeinstellung für Releasekonfigurationen.

Es gibt einige Szenarios:

- Nach dem Start mit Klassen ableiten von NSObject geladen-Plug-Ins
- Ableiten von NSObject Klasseninstanzen dynamisch erstellt.

wobei die Registrierungsstelle ist nicht erkennbar, dass er einen Typ beim Start zu registrieren muss. Die `ObjCRuntime.Runtime.RegisterAssembly` Methode wird bereitgestellt, um der Registrierungsstelle zu informieren, dass zusätzliche Typen zu berücksichtigen ist.
