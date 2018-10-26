---
title: Anpassen von Bindungen
description: Sie können eine Xamarin.Android-Bindung anpassen, indem bearbeiten Sie die Metadaten, die den Bindungsprozess steuert. Diese manuellen Änderungen sind häufig erforderlich, für das Auflösen von Buildfehlern und erlaubt deren Strukturierung der resultierenden-API, damit es mit konsistenter ist C#/.NET. Diese Anleitungen erläutern die Struktur dieser Metadaten, wie Sie die Metadaten zu ändern und wie Sie JavaDoc zu verwenden, um die Namen der Parameter der Methode wiederherzustellen.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: 44bff372225ee1bf555eb3eeb34da918830980b4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102385"
---
# <a name="customizing-bindings"></a>Anpassen von Bindungen

_Sie können eine Xamarin.Android-Bindung anpassen, indem bearbeiten Sie die Metadaten, die den Bindungsprozess steuert. Diese manuellen Änderungen sind häufig erforderlich, für das Auflösen von Buildfehlern und erlaubt deren Strukturierung der resultierenden-API, damit es mit konsistenter ist C#/.NET. Diese Anleitungen erläutern die Struktur dieser Metadaten, wie Sie die Metadaten zu ändern und wie Sie JavaDoc zu verwenden, um die Namen der Parameter der Methode wiederherzustellen._


## <a name="overview"></a>Übersicht
 
Xamarin.Android automatisiert einen großen Teil des Bindungsvorgangs; Allerdings ist in einigen Fällen manuelle Änderungen erforderlich, um die folgenden Aspekte berücksichtigen:

-   Auflösen von build-Fehler durch fehlende Typen, verborgenen Typen, doppelte Namen, Klasse Sichtbarkeit Probleme und andere Situationen, in denen nicht aufgelöst werden können, verursacht die Xamarin.Android-Tools. 

-   Ändern die Zuordnung, die von Xamarin.Android verwendet wird, um die Android-API auf verschiedene Arten in binden C# (z. B. viele Entwickler bevorzugen Java zuordnen `int` Konstanten C# `enum` Konstanten).

-   Entfernen nicht verwendete Typen, die nicht gebunden werden müssen. 

-   Hinzufügen von Typen, die keine Entsprechung in der zugrunde liegenden Java-API zu erhalten. 

Sie können einige oder alle diese Änderungen vornehmen, durch Ändern der Metadaten, die den Bindungsprozess steuert.


## <a name="guides"></a>Führungslinien

Die folgenden Anleitungen beschreiben die Metadaten, die den Bindungsprozess steuert und wird erläutert, wie diese Metadaten, um diese Probleme zu ändern:

-   [Metadaten für Java-Bindungen](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bietet eine Übersicht über die Metadaten, die in einer Java-Bindung wechselt.
    Es beschreibt die verschiedenen manuelle Schritte, die manchmal zum Abschließen einer Java bindungsbibliothek erforderlich sind, und es wird erläutert, wie eine API verfügbar gemacht werden, durch die Bindung an genauer .NET Entwurfsrichtlinien halten zu strukturieren.

-   [Benennen von Parametern mit Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) wird erläutert, wie Parameternamen in einem Projekt für die Java-Bindung, die mithilfe von Javadoc-erstellt aus dem gebundenen Java-Projekt wiederherstellen.


 

