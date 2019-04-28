---
title: Benennen von Parametern mit Javadoc
description: In diesem Artikel wird erläutert, wie beim Parameternamen in einem Projekt für die Java-Bindung Wiederherstellen von Javadoc aus dem Java-Projekt generiert wird.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/20/2017
ms.openlocfilehash: e394377043953a297afed36a3ce0747a3e6d1512
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60955870"
---
# <a name="naming-parameters-with-javadoc"></a>Benennen von Parametern mit Javadoc

_In diesem Artikel wird erläutert, wie beim Parameternamen in einem Projekt für die Java-Bindung Wiederherstellen von Javadoc aus dem Java-Projekt generiert wird._


## <a name="overview"></a>Übersicht

Einige Metadaten über die gebundenen-API geht verloren, wenn Sie eine vorhandene Java-Bibliothek zu binden. Insbesondere die Namen der Parameter für Methoden. Parameternamen werden `p0`, `p1`usw. Grund hierfür ist das Java `.class` Dateien behalten keine die Parameternamen, die in der Java-Quellcode verwendet wurden. 

Ein Xamarin.Android-Java-bindungsprojekt kann die Parameternamen bereitstellen, wenn sie Zugriff auf den Javadoc-HTML-Code aus der ursprünglichen Bibliothek hat. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrieren von Javadoc HTML in einer Java Projekt wird gebunden

Die Integration von den Javadoc-HTML-Code in ein Binden von Java-Projekt ist ein manueller Prozess, der die folgenden Schritte aus: 

1.  Die Javadoc für die Bibliothek herunterladen
2.  Bearbeiten der `.csproj` -Datei und fügen eine `<JavaDocPaths>` Eigenschaft:
3.  Bereinigen und das Projekt neu erstellen

Sobald dies geschehen ist, sollte die Namen der ursprünglichen Java-Parameter vorhanden ist, in den APIs, die von einem Projekt für die Java-Bindung gebunden sein. 


> [!NOTE]
> Es gibt eine große Menge von Varianz in der JavaDoc-Ausgabe. Die. JAR-Bindung-toolkette unterstützt nicht jede einzelne möglich Permutation und daher einige Parameter möglicherweise nicht ordnungsgemäß benannt.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie Javadoc in einem Projekt für die Java-Bindung verwenden, um die Bedeutung von Parameternamen für das gebundene-APIs bieten verwendet wird. 

