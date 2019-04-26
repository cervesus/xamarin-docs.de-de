---
title: Native Verweise iOS-, Mac und Bindungen-Projekte
description: Native Verweise bietet Ihnen die Möglichkeit, ein natives Framework in einem Xamarin.iOS, Xamarin.Mac oder bindungsprojekt einzubetten.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3a497d0bb4674014b8063cb1fbc91eec6e7ae5ea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61201544"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>Native Verweise in iOS, Mac und Bindungen-Projekte

_Native Verweise Ihnen die Möglichkeit zum Einbetten von eines nativen Frameworks in einem Xamarin.iOS oder Xamarin.Mac-Projekt oder Bindung._

Da iOS 8.0 oder höher war es möglich, ein eingebettetes Framework zum Teilen von Code zwischen app-Erweiterungen und die Haupt-app in Xcode zu erstellen. Mit der nativen Verweis-Funktion werden diese eingebettete Frameworks (erstellt mit Xcode) in Xamarin.iOS genutzt.
 
> [!IMPORTANT]
> Es nicht möglich, eingebettete Frameworks von jeder Art von Xamarin.iOS oder Xamarin.Mac-Projekte zu erstellen sein wird, können Native Verweise nur für den Verbrauch von vorhandenen systemeigenen (Objective-C)-Frameworks.

<a name="Terminology" />

## <a name="terminology"></a>Terminologie

In iOS 8 (oder höher) **eingebettete Frameworks** kann sowohl die statisch verknüpfte und dynamisch verknüpfte Frameworks eingebettet sein kann. Um sie ordnungsgemäß zu verteilen, müssen Sie sie in "Fat"-Frameworks, die alle enthalten, deren _Slices_ für jedes Gerätearchitektur, die Sie mit Ihrer app unterstützen möchten.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Statische und Dynamische Frameworks

**Statische Frameworks** zur Kompilierzeit verknüpft sind, in denen **dynamische Frameworks** , die zur Laufzeit verknüpft sind und deshalb kann geändert werden, ohne neu verknüpfen. Wenn Sie ein beliebiges Framework 3rd-Party vor iOS 8 verwendet haben, wurden Sie verwenden eine **statische Framework** , die in Ihrer app kompiliert wurde. Finden Sie unter Apple [dynamischen Bibliothek Programmierung](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) Dokumentation.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Embedded Visual Studio. SystemFrameworks

**Eingebettete Frameworks** in Ihrem apps-Paket enthalten sind, und sind nur für Ihre spezifische app über ihren Sandkasten zugänglich. **SystemFrameworks** auf Betriebssystemebene gespeichert werden, und für alle apps auf dem Gerät verfügbar sind. Derzeit kann nur mit Apple zum Betriebssystem-Level-Frameworks zu erstellen.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Schlanke im Vergleich zu FAT-Frameworks

**Schlanke Frameworks** enthalten nur den kompilierten Code für eine bestimmte Systemarchitektur, in denen **Fat Frameworks** enthält Code für mehrere Architekturen. Jede architekturspezifische Codebase, die in einem Framework kompiliert wird als bezeichnet ein _Slice_. Also z. B. enthalten bei einem Framework, die für die zwei iOS-simulatorarchitekturen ("i386" und "X86_64") kompiliert wurde, zwei Segmente.

Wenn Sie versucht haben, um dieses Beispiel-Framework mit Ihrer app zu verteilen, würde Sie ordnungsgemäß im Simulator ausgeführt, aber auf dem Gerät fehl, da das Framework keine Code-spezifische Slices für ein iOS-Gerät enthält. Um sicherzustellen, dass ein Framework in allen Instanzen funktioniert, müssten sie auch gerätespezifische Slices z. B. arm64, armv7 und armv7s enthalten.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Arbeiten mit eingebettete Frameworks

Es gibt zwei Schritte, die abgeschlossen werden müssen, um eingebettete Frameworks in einer Xamarin.iOS oder Xamarin.Mac-app arbeiten: Erstellen ein Fat-Framework, und das Framework einbetten.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Erstellen ein Fat-Framework

Wie bereits erwähnt, um ein eingebettetes Framework in Ihrer app verwenden kann muss es ein Fat-Framework, die alle von der Systemarchitekturen Slices für die Geräte enthält, die auf Ihre app ausgeführt wird.

Wenn das Framework und der Nutzung der Anwendung in der gleichen Xcode-Projekt sind, ist dies kein Problem, wie Xcode sowohl für das Framework als auch für die App, die mit den gleichen Buildeinstellungen erstellt wird. Da Xamarin-apps nicht eingebettete Frameworks erstellen können, kann diese Technik verwendet werden.

Um dieses Problem zu beheben, die `lipo` Befehlszeilentool kann verwendet werden, zum Zusammenführen von zwei oder mehr Frameworks in einem Fat-Framework, die alle erforderlichen Segmente enthält. Weitere Informationen zum Arbeiten mit der `lipo` Befehl, finden Sie unter unserem [Native Bibliotheken verknüpfen](~/ios/platform/native-interop.md) Dokumentation.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Einbetten eines Frameworks

Im folgenden Schritt sind erforderlich, ein Framework in einem Xamarin.iOS oder Xamarin.Mac-Projekt, die mithilfe von Native Verweise einbetten:

1. Erstellen Sie ein neues oder öffnen Sie ein vorhandenes Xamarin.iOS, Xamarin.Mac oder Bindung-Projekt.
2. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **hinzufügen nativer Verweis**: 

    [![](native-references-images/ref01.png "Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie die systemeigenen Verweis hinzufügen")](native-references-images/ref01.png#lightbox)
3. Von der **öffnen** Dialogfeld Feld, wählen Sie den Namen des nativen Frameworks, die Sie einbetten möchten und klicken Sie auf die **öffnen** Schaltfläche: 

    [![](native-references-images/ref02.png "Wählen Sie den Namen des nativen Frameworks einbetten, und klicken Sie auf die Schaltfläche \"Öffnen\"")](native-references-images/ref02.png#lightbox)
4. Das Framework wird das Projekt in der Struktur hinzugefügt werden: 

    [![](native-references-images/ref03.png "Das Framework wird die Struktur von Projekten hinzugefügt werden")](native-references-images/ref03.png#lightbox)

Wenn das Projekt kompiliert wird, wird in der App Bundle natives Framework eingebettet.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>App-Erweiterungen und eingebettete Frameworks

Intern Xamarin.iOS möglicherweise nutzen Sie dieses Feature zur Verknüpfung mit dem Mono-Laufzeit als Framework (wenn das Bereitstellungsziel ist > = iOS 8.0 oder höher), wodurch die app-Größe erheblich für apps mit Erweiterungen, (da die Mono-Laufzeit für nur einmal eingeschlossen wird die gesamte app-Bündel, nicht einmal für die Container-app und einmal für jede Erweiterung).

Erweiterungen werden als ein Framework, das mit dem Mono-Laufzeit verknüpfen, da alle Erweiterungen auf iOS 8.0 oder höher erforderlich.

Apps, die Erweiterungen und apps für iOS besitzen 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel hat es sich um einen detaillierten Einblick in die Einbettung von eines nativen Frameworks in einer Xamarin.iOS oder Xamarin.Mac-Anwendung geführt.

