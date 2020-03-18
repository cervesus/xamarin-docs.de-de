---
title: Anpassen von Bindungen
description: Sie können eine Xamarin.Android-Bindung anpassen, indem Sie die Metadaten bearbeiten, mit denen der Bindungsprozess gesteuert wird. Diese manuellen Änderungen sind häufig erforderlich, um Buildfehler zu beheben und die resultierende API so zu strukturieren, dass sie mit C#/.NET konsistent ist. Diese Leitfäden erläutern die Struktur dieser Metadaten, das Ändern der Metadaten und die Verwendung von JavaDoc, um die Namen von Methodenparametern wiederherzustellen.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 04f3720d8684129476c955819390e91330a7800a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020654"
---
# <a name="customizing-bindings"></a>Anpassen von Bindungen

_Sie können eine Xamarin.Android-Bindung anpassen, indem Sie die Metadaten bearbeiten, mit denen der Bindungsprozess gesteuert wird. Diese manuellen Änderungen sind häufig erforderlich, um Buildfehler zu beheben und die resultierende API so zu strukturieren, dass sie mit C#/.NET konsistent ist. Diese Leitfäden erläutern die Struktur dieser Metadaten, das Ändern der Metadaten und die Verwendung von JavaDoc, um die Namen von Methodenparametern wiederherzustellen._

## <a name="overview"></a>Übersicht

Xamarin.Android automatisiert einen Großteil des Bindungsvorgangs. In einigen Fällen ist jedoch eine manuelle Änderung erforderlich, um die folgenden Probleme zu beheben:

- Beheben von Buildfehlern, die durch fehlende oder verborgene Typen, doppelte Namen, Probleme mit der Sichtbarkeit von Klassen und andere Situationen verursacht werden, die nicht mithilfe von Xamarin.Android-Tools behoben werden können. 

- Ändern der Zuordnung, die Xamarin.Android verwendet, um die Android-API an verschiedene Typen in C# zu binden (z. B. bevorzugen es viele Entwickler, die Java-Konstanten `int` den C#-Konstanten `enum` zuzuordnen)

- Entfernen nicht verwendeter Typen, die nicht gebunden werden müssen 

- Hinzufügen von Typen, die keine Entsprechung in der zugrunde liegenden Java-API haben 

Sie können einige oder alle dieser Änderungen vornehmen, indem Sie die Metadaten ändern, mit denen der Bindungsprozess gesteuert wird.

## <a name="guides"></a>Führungslinien

Die folgenden Leitfäden beschreiben die Metadaten, die den Bindungsprozess steuern, und erläutern, wie diese Metadaten geändert werden, um diese Probleme zu beheben:

- Der Leitfaden zu [Java-Bindungsmetadaten](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bietet eine Übersicht über die Metadaten, die in einer Java-Bindung enthalten sind.
    Darin werden die verschiedenen manuellen Schritte beschrieben, die manchmal zum Erstellen einer Java-Bindungsbibliothek erforderlich sind. Außerdem wird erläutert, wie Sie eine von einer Bindung verfügbar gemachte API so strukturieren, dass diese den .NET-Entwurfsrichtlinien entspricht.

- Im Leitfaden zum [Benennen von Parametern mit JavaDoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) wird erläutert, wie Parameternamen in einem Java-Bindungsprojekt mithilfe von JavaDoc wieder hergestellt werden, die vom gebundenen Java-Projekt erstellt wurden.
