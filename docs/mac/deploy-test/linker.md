---
title: Xamarin.Mac-Linkeroptionen
description: In diesem Dokument wird das Verknüpfen in Xamarin.Mac beschrieben. Verknüpfen ist ein leistungsstarkes Optimierungstool, das die Größe Ihrer Anwendung verringert, indem nicht verwendeter Code entfernt wird.
ms.prod: xamarin
ms.assetid: F03176C3-F8D4-4DE8-870C-7F27D8CE525A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 73f652be32c72ef51170f44c28ce1590e6a0e92b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106844"
---
# <a name="xamarinmac-linker-options"></a>Xamarin.Mac-Linkeroptionen

_Verknüpfen ist ein leistungsstarkes Optimierungstool, das die Größe Ihrer Anwendung verringert, indem nicht verwendeter Code entfernt wird._

## <a name="overview"></a>Übersicht

Die verfügbaren Linkeroptionen können auf Grundlage des [Zielframeworks](~/mac/platform/target-framework.md), das Ihr Projekt verwendet, eingeschränkt sein. Dies ist darauf zurückzuführen, dass für die Verknüpfung die Erstellung eines Objektgraphs von jedem von Ihrer Anwendung verwendeten Typ erforderlich ist. Für „Full“ (vollständig) (oder „Unsupported“ – Nicht unterstützt) ist dies aufgrund von „System.Configuration“ nicht möglich.

Es gibt vier Möglichkeiten:

- **Keine** – Deaktiviert alle Verknüpfungen. Standard bei der Debugkonfiguration in Modern und allen Konfiguration in Full (vollständig)
- **SDK**: Verknüpft alle SDK-Assemblys (Benutzerassemblys ausgenommen). Standard in der Releasekonfiguration in Modern. Für „Full“ (vollständig) nicht verfügbar.
- **Full** (vollständig): Verknüpfen aller Assemblys. Dies erfordert, dass Benutzercode für den Linker sicher ist. Weitere Informationen finden Sie in den [Anmerkungen](~/ios/deploy-test/linker.md). Für „Full“ (vollständig) nicht verfügbar.
- **Plattform**: Verknüpft nur „Xamarin.Mac.dll“. Details finden Sie weiter unten.

## <a name="platform-linking"></a>Plattformverknüpfung

Das Verknüpfen von Anwendungen mithilfe des [Zielframeworks](~/mac/platform/target-framework.md) „Full“ ist in der Regel nicht sicher. In einigen Fällen ist jedoch eine eingeschränkte Form des Verknüpfens erforderlich.

Anwendungen, die z.B. an den macOS App Store übermittelt werden, dürfen auf keine verwalteten APIs (wie etwa QTKit) verweisen, von denen Xamarin.Mac Bindungen enthält. Auch wenn eine Anwendung diese Bindungen nicht aufruft, ist der Aufruf jedoch im SDK vorhanden und wird abgelehnt.

Die Plattformverknüpfung geht davon aus, dass die Anwendung und BCL nicht sicher für den Linker sind und entfernt nicht verwendeten Code aus „Xamarin.Mac.dll“. 

Alle Anwendungen, die sich nicht auf Xamarin.Mac.dll-Typen beziehen, erfahren eine geringe Leistungsverbesserung beim Start durch die Entfernung unnötiger Typen.

Die Plattformverknüpfung eignet sich in der Regel nur für Anwendung, die das Full-Zielframework verwenden, da die Modern-Anwendung die leistungsstärkere SDK-Option verwenden kann.

## <a name="setting-the-linker-configuration"></a>Festlegen der Linkerkonfiguration

Um zur Linkerkonfiguration für ein Xamarin.Mac-Projekt zu wechseln, führen Sie folgende Schritte aus:

1. Öffnen Sie das Xamarin.Mac-Projekt in Visual Studio für Mac.
2. Doppelklicken Sie auf die Projektdatei im **Projektmappen-Explorer**, um die **Projektoptionen** zu öffnen.
3. Wählen Sie auf der Registerkarte **Mac-Build** den Typ des **Linkerverhaltens**, der am besten zu Ihrer Anwendung passt:

  ![Auswählen des zu verwendenden Linkerverhaltens](linker-images/link-behavior.png "Choose which linker behavior to use")

4. Die Plattformverknüpfung für Full-Zielframeworks wird erst in einem späteren Update zur IDE hinzugefügt. Fügen Sie bis dahin stattdessen den **zusätzlichen mmp-Argumenten** `--linkplatform` hinzu.
5. Klicken Sie auf **OK**, um die Änderungen zu speichern.


## <a name="related-links"></a>Verwandte Links

- [Verknüpfung unter iOS](~/ios/deploy-test/linker.md)
