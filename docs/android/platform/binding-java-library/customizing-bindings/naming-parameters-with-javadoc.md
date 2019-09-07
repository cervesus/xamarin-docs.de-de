---
title: Benennen von Parametern mit Javadoc
description: In diesem Artikel wird erläutert, wie Parameternamen in einem Java-Bindungs Projekt mithilfe von Javadoc wieder hergestellt werden, die aus dem Java-Projekt generiert wurden.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/20/2017
ms.openlocfilehash: fa1fb0656384455322a2d0a3562fc0ee3ca52397
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757600"
---
# <a name="naming-parameters-with-javadoc"></a>Benennen von Parametern mit Javadoc

_In diesem Artikel wird erläutert, wie Parameternamen in einem Java-Bindungs Projekt mithilfe von Javadoc wieder hergestellt werden, die aus dem Java-Projekt generiert wurden._

## <a name="overview"></a>Übersicht

Wenn eine vorhandene Java-Bibliothek gebunden wird, gehen einige Metadaten über die gebundene API verloren. Dies sind insbesondere die Namen von Parametern für Methoden. Parameter Namen werden als `p0`, `p1`usw. angezeigt. Dies liegt daran, dass `.class` die Java-Dateien die Parameternamen, die im Java-Quellcode verwendet wurden, nicht beibehalten. 

Ein xamarin. Android-Java-Bindungs Projekt kann die Parameternamen bereitstellen, wenn es auf den Javadoc-HTML-Code aus der ursprünglichen Bibliothek zugreifen kann. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrieren von Javadoc-HTML in ein Java-Bindungs Projekt

Das Integrieren des Javadoc-HTML in ein Java-Bindungs Projekt ist ein manueller Prozess, der aus den folgenden Schritten besteht: 

1. Herunterladen von Javadoc für die Bibliothek
2. Bearbeiten Sie `.csproj` die Datei, und `<JavaDocPaths>` fügen Sie eine Eigenschaft hinzu:
3. Bereinigen und erneutes Erstellen des Projekts

Nachdem dies geschehen ist, sollten die ursprünglichen Java-Parameternamen in den APIs vorhanden sein, die von einem Java-Bindungs Projekt gebunden werden. 

> [!NOTE]
> Die Javadoc-Ausgabe ist sehr viel Varianz. Die. Die jar-Bindungs Toolkette unterstützt nicht jede einzelne mögliche permutations. Folglich werden einige Parameter möglicherweise nicht ordnungsgemäß benannt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Javadoc in einem Java-Bindungs Projekt verwendet wird, um die Bedeutung von Parameternamen für gebundene APIs anzugeben. 
