---
title: Anpassen von Bindungen
description: Sie können eine xamarin. Android-Bindung anpassen, indem Sie die Metadaten bearbeiten, mit denen der Bindungsprozess gesteuert wird. Diese manuellen Änderungen sind häufig erforderlich, um Buildfehler zu beheben und die resultierende API so zu strukturieren, dass Sie C#mit/.net. konsistent ist. Diese Leitfäden erläutern die Struktur dieser Metadaten, das Ändern der Metadaten und die Verwendung von Javadoc, um die Namen von Methoden Parametern wiederherzustellen.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: e29432504f3b8554c387d277004d3cc779aade95
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524639"
---
# <a name="customizing-bindings"></a>Anpassen von Bindungen

_Sie können eine xamarin. Android-Bindung anpassen, indem Sie die Metadaten bearbeiten, mit denen der Bindungsprozess gesteuert wird. Diese manuellen Änderungen sind häufig erforderlich, um Buildfehler zu beheben und die resultierende API so zu strukturieren, dass Sie C#mit/.net. konsistent ist. Diese Leitfäden erläutern die Struktur dieser Metadaten, das Ändern der Metadaten und die Verwendung von Javadoc, um die Namen von Methoden Parametern wiederherzustellen._


## <a name="overview"></a>Übersicht
 
Xamarin. Android automatisiert einen Großteil des Bindungs Vorgangs. in einigen Fällen ist jedoch eine manuelle Änderung erforderlich, um die folgenden Probleme zu beheben:

- Auflösen von Buildfehlern, die durch fehlende Typen, verborgene Typen, doppelte Namen, Probleme mit der Sichtbarkeit von Klassen und andere Situationen verursacht werden, die nicht durch die xamarin. Android-Tools aufgelöst werden können. 

- Ändern der Zuordnung, die xamarin. Android verwendet, um die Android-API an verschiedene C# Typen in zu binden (z. b. werden `int` viele Entwickler C# `enum` die Zuordnung von Java-Konstanten zu konstanten bevorzugen).

- Entfernen nicht verwendeter Typen, die nicht gebunden werden müssen. 

- Hinzufügen von Typen, die keine Entsprechung in der zugrunde liegenden Java-API haben. 

Sie können einige oder alle dieser Änderungen vornehmen, indem Sie die Metadaten ändern, mit denen der Bindungsprozess gesteuert wird.


## <a name="guides"></a>Führungslinien

Die folgenden Leitfäden beschreiben die Metadaten, die den Bindungsprozess steuern, und erläutern, wie diese Metadaten geändert werden, um diese Probleme zu beheben:

- [Metadaten der Java-Bindungen](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bieten eine Übersicht über die Metadaten, die in eine Java-Bindung integriert werden.
    Darin werden die verschiedenen manuellen Schritte beschrieben, die manchmal zum Durchführen einer Java-Bindungs Bibliothek erforderlich sind, und es wird erläutert, wie Sie eine API, die von einer Bindung verfügbar gemacht wird, so strukturieren, dass Sie den .net-Entwurfs Richtlinien

- In den [Benennungs Parametern mit Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) wird erläutert, wie Parameternamen in einem Java-Bindungs Projekt mithilfe von Javadoc wieder hergestellt werden, das aus dem gebundenen Java-Projekt erstellt wurde


 

