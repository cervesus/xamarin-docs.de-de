---
title: Systemeigene Verweise
description: "Systemeigene Verweise bietet Ihnen die Möglichkeit, ein natives Framework in einem Projekt von Xamarin.iOS oder Xamarin.Mac oder Bindung einzubetten."
ms.topic: article
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 5a33993bdef16191b66127dcc68c57661636c0f8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="native-references"></a>Systemeigene Verweise

_Systemeigene Verweise bietet Ihnen die Möglichkeit, ein natives Framework in einem Projekt von Xamarin.iOS oder Xamarin.Mac oder Bindung einzubetten._


Seit iOS 8.0 oder höher wurde es möglich, ein eingebettetes Framework zum Freigeben von Code für app-Erweiterungen und die Haupt-app in Xcode zu erstellen. Mithilfe der systemeigenen Verweis-Funktion werden in Xamarin.iOS diese eingebetteten Frameworks (erstellt mit Xcode) genutzt.
 
> [!IMPORTANT]
> **Hinweis:** wird es nicht möglich, die eingebettete Frameworks aus jeder Art von Xamarin.iOS oder Xamarin.Mac Projekte erstellen, systemeigene Verweise nur für den Verbrauch von vorhandenen systemeigenen (Objective-C)-Frameworks zulassen.




<a name="Terminology" />

## <a name="terminology"></a>Terminologie

In iOS 8 (und höher) **eingebettete Frameworks** können sowohl der statisch verknüpften und dynamisch verknüpfte Frameworks eingebettet werden. Um sie ordnungsgemäß zu verteilen, müssen Sie sie in "Fat" Frameworks, die alle enthalten, deren _Slices_ für jede Gerätearchitektur, die, die mit Ihrer app unterstützt werden soll.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Statische im Vergleich zu Dynamische Frameworks

**Statische Frameworks** zur Kompilierzeit verknüpft sind, in denen **dynamische Frameworks** verknüpft sind, zur Laufzeit und Verlaufsebene kann geändert werden, ohne neu verknüpfen. Wenn Sie alle 3rd Party-Framework, bevor Sie iOS 8 verwendet haben, wurden Sie verwenden eine **statische Framework** , die in Ihrer app kompiliert wurde. Finden Sie in der Apple- [dynamische Bibliothek Programmierung](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) Dokumentation.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Eingebettete im Vergleich zu SystemFrameworks

**Eingebettete Frameworks** in Ihrer apps-Paket enthalten sind und nur für Ihre bestimmte app über seine Sandkasten zugegriffen werden. **SystemFrameworks** auf Betriebssystemebene gespeichert werden und für alle apps auf dem Gerät verfügbar sind. Gegenwärtig kann nur Apple die Fähigkeit zum Erstellen von Betriebssystem-Level-Frameworks.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Schlanke im Vergleich zu FAT-Frameworks

**Frameworks für die schlanke** nur den kompilierten Code für eine bestimmte Systemarchitektur enthalten, in denen **Fat Frameworks** Code für mehrere Architekturen enthalten. Jede architekturspezifischer Codebase in einem Framework kompiliert wird als bezeichnet eine _Slice_. Also z. B. enthalten Wenn es ein Framework, der für die zwei iOS-Simulator Architekturen (i386 und X86_64) kompiliert wurde enthielt, zwei Segmente.

Sie haben versucht, dieses Beispiel Framework mit Ihrer app zu verteilen, würde ordnungsgemäß im Simulator ausgeführt, aber auf dem Gerät fehlschlagen, weil durch das Framework keine codespezifische Slices für ein iOS-Gerät enthält. Um sicherzustellen, dass ein Framework in allen Fällen funktioniert, müssten sie auch gerätespezifischen Slices z. B. arm64, armv7 und armv7s enthalten.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Arbeiten mit eingebetteten Frameworks

Es gibt zwei Schritte, die abgeschlossen sein müssen, Informationen zum Arbeiten mit eingebetteten Frameworks in eine app Xamarin.iOS oder Xamarin.Mac: erstellen ein Fat-Framework und das Framework einbetten.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Erstellen ein Fat-Framework

Wie bereits erwähnt, um ein eingebettetes Framework in Ihrer app verwenden können muss sie ein Fat-Framework, die alle von der Systemarchitekturen Slices für die Geräte enthält, die auf Ihre app ausgeführt wird.

Wenn das Framework und die verbrauchende Anwendung in der gleichen Xcode-Projekt befinden, ist dies kein Problem, wie Xcode das Framework und die App mit den gleichen Buildeinstellungen erstellt wird. Da Xamarin-apps eingebettete Frameworks erstellt werden können, kann diese Technik nicht verwendet werden.

Zur Behebung dieses Problems die `lipo` -Befehlszeilentool zum Zusammenführen von zwei oder mehr Frameworks in einem Fat-Framework, das alle erforderlichen Segmente enthält verwendet werden kann. Weitere Informationen zum Arbeiten mit der `lipo` Befehl, finden Sie unter unsere [systemeigene Bibliotheken verknüpfen](~/ios/platform/native-interop.md) Dokumentation.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Einbetten von einem Framework

Führen Sie die Schritte sind erforderlich, um ein Framework in einem Xamarin.iOS oder Xamarin.Mac mithilfe systemeigener Verweise einzubetten:

1. Erstellen Sie ein neues oder öffnen Sie ein vorhandenes Xamarin.iOS, Xamarin.Mac oder Bindung Projekt.
2. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **systemeigenen Verweis hinzufügen**: 

    [![](native-references-images/ref01.png "Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie die systemeigenen Verweis hinzufügen")](native-references-images/ref01.png#lightbox)
3. Aus der **öffnen** Dialogfeld Feld, wählen Sie den Namen der systemeigenen Rahmen, die Sie einbetten möchten und klicken Sie auf die **öffnen** Schaltfläche: 

    [![](native-references-images/ref02.png "Wählen Sie den Namen von systemeigenen Framework einbetten, und klicken Sie auf die Schaltfläche "Öffnen"")](native-references-images/ref02.png#lightbox)
4. Das Framework wird das Projekt Struktur hinzugefügt werden: 

    [![](native-references-images/ref03.png "Das Framework wird die Struktur von Projekten hinzugefügt werden")](native-references-images/ref03.png#lightbox)

Wenn das Projekt kompiliert wird, wird systemeigene Rahmen in der App-Bündel eingebettet werden.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>App-Erweiterungen und eingebettete Frameworks

Intern Xamarin.iOS möglicherweise nutzen dieses Features zur Verknüpfung mit der Mono / Runtime als Framework (wenn das Bereitstellungsziel ist > = iOS 8.0 oder höher), wodurch die app Größe erheblich für apps mit Erweiterungen, (da die Mono / Runtime für nur einmal enthalten sein wird die gesamte app-Bündel, anstatt einmal für den Container-app und einmal für jede Erweiterung).

Erweiterungen werden als ein Framework mit der Mono / Runtime verknüpfen, da für alle Erweiterungen iOS 8.0 oder höher erforderlich.

Apps, die Erweiterungen und apps, iOS Ziel haben 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über ein systemeigenes Framework in einer Anwendung Xamarin.iOS oder Xamarin.Mac einbetten übernommen.

