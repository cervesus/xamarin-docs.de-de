---
title: Benennen von Parametern mit Javadoc
description: "Dieser Artikel beschreibt die Vorgehensweise beim Wiederherstellen von Parameternamen in einem Projekt für die Java-Bindung mithilfe von Javadoc aus dem Java-Projekt generiert."
ms.topic: article
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 84dfe88e912241eb0024143bca568ae75e5bfa28
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="naming-parameters-with-javadoc"></a>Benennen von Parametern mit Javadoc

_Dieser Artikel beschreibt die Vorgehensweise beim Wiederherstellen von Parameternamen in einem Projekt für die Java-Bindung mithilfe von Javadoc aus dem Java-Projekt generiert._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Einige Metadaten über die gebundenen-API geht verloren, wenn Sie eine vorhandene Java-Bibliothek zu binden. Insbesondere die Namen der Parameter für Methoden. Parameternamen hostnamensadresse `p0`, `p1`usw. Grund hierfür ist das Java `.class` Dateien die Parameternamen, die in der Java-Quellcode verwendet wurden nicht beibehalten. 

Eine Bindung Xamarin.Android Java-Projekt kann die Parameternamen bereitstellen, wenn sie Zugriff auf den Javadoc-HTML-Code aus der ursprünglichen Bibliothek hat. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrieren von Javadoc HTML in einer Java Project binden

Integrieren von Javadoc HTML in einem Binden von Java-Projekt ist ein manueller Prozess, der die folgenden Schritte aus: 

1.  Die Javadoc für die Bibliothek herunterladen
2.  Bearbeiten der `.csproj` und fügen eine `<JavaDocPaths>` Eigenschaft:
3.  Bereinigen und das Projekt neu erstellen

Sobald dies geschehen ist, sollte die Namen der ursprünglichen Java-Parameter in den APIs, die von einem Projekt für die Java-Bindung gebunden vorhanden sein. 


> [!NOTE]
> **Hinweis:** sehr viel systemverarbeitungszeit in die Varianz bei der die JavaDoc-Ausgabe vorhanden ist. Die. Jeder einzelnen möglichen Permutation JAR Bindung toolkette nicht unterstützt und daher einige Parameter möglicherweise nicht ordnungsgemäß benannt.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt wie verwenden Javadoc in einem Projekt für die Java-Bindung Bedeutung Parameternamen für das gebundene-APIs bereitstellen. 

