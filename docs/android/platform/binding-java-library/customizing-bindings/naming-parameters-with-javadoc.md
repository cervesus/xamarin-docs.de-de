---
title: Benennen von Parametern mit Javadoc
description: In diesem Artikel wird erläutert, wie Parameternamen in einem Java-Bindungsprojekt mithilfe von JavaDoc wieder hergestellt werden, die vom Java-Projekt generiert wurden.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: 060c4759d39bc3b8c424ce46dc615644540fe9c2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027669"
---
# <a name="naming-parameters-with-javadoc"></a>Benennen von Parametern mit Javadoc

_In diesem Artikel wird erläutert, wie Parameternamen in einem Java-Bindungsprojekt mithilfe von JavaDoc wieder hergestellt werden, die vom Java-Projekt generiert wurden._

## <a name="overview"></a>Übersicht

Wenn eine vorhandene Java-Bibliothek gebunden wird, gehen einige Metadaten zur gebundenen API verloren. Dies gilt besonders für die Namen von Parametern zu Methoden. Parameternamen werden als `p0`, `p1` usw. angezeigt. Dies liegt daran, dass die `.class`-Dateien in Java die Parameternamen nicht beibehalten, die im Java-Quellcode verwendet wurden. 

Eine Java-Bindungsprojekt in Xamarin.Android kann die Parameternamen bereitstellen, wenn es Zugriff auf die Javadoc-HTML über die ursprüngliche Bibliothek hat. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrieren der Javadoc-HTML in ein Java-Bindungsprojekt

Das Integrieren der Javadoc-HTML in ein Java-Bindungsprojekt ist ein manueller Vorgang, der aus folgenden Schritten besteht: 

1. Herunterladen der Javadoc-Datei für die Bibliothek
2. Bearbeiten der Datei `.csproj` und Hinzufügen einer `<JavaDocPaths>`-Eigenschaft
3. Bereinigen und Neuerstellung des Projekts

Sobald diese Schritte abgeschlossen sind, sollten die ursprünglichen Java-Parameternamen in den durch das Java-Bindungsprojekt gebundenen APIs vorhanden sein. 

> [!NOTE]
> Die JavaDoc-Ausgabe kann sehr unterschiedlich ausfallen. Die JAR-Bindungstoolkette unterstützt nicht jede mögliche Permutation und deshalb auch einige Parameter nicht, die nicht ordnungsgemäß benannt wurden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie JavaDoc in einem Java-Bindungsprojekt verwendet wird, um sinnvolle Parameternamen für gebundene APIs bereitzustellen. 
