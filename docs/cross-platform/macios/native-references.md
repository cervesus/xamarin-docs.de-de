---
title: Native Verweise auf Ios-, Mac-und Bindungs Projekte
description: Native Verweise bieten Ihnen die Möglichkeit, ein natives Framework in ein xamarin. IOS-, xamarin. Mac-oder Bindungs Projekt einzubetten.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: de34dcdd194bd3777214d23fded7e5f42ec5141c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287540"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>Native Verweise in Ios-, Mac-und Bindungs Projekten

_Native Verweise bieten Ihnen die Möglichkeit, ein natives Framework in ein xamarin. IOS-oder xamarin. Mac-Projekt oder ein Bindungs Projekt einzubetten._

Seit IOS 8,0 ist es möglich, ein eingebettetes Framework für die gemeinsame Nutzung von Code zwischen App-Erweiterungen und der Haupt-app in Xcode zu erstellen. Mithilfe des systemeigenen Referenz Features können diese eingebetteten Frameworks (erstellt mit Xcode) in xamarin. IOS genutzt werden.
 
> [!IMPORTANT]
> Es ist nicht möglich, eingebettete Frameworks von einem beliebigen xamarin. IOS-oder xamarin. Mac-Projekt zu erstellen. Native Verweise ermöglichen nur die Nutzung vorhandener nativer Frameworks (Ziel-C).

<a name="Terminology" />

## <a name="terminology"></a>Terminologie

In ios 8 (und höher) können **eingebettete Frameworks** sowohl statisch verknüpfte als auch dynamisch verknüpfte Frameworks enthalten. Um diese ordnungsgemäß zu verteilen, müssen Sie Sie in "FAT"-Frameworks einschließen, die alle Ihre _Slices_ für jede Geräte Architektur enthalten, die Sie mit Ihrer APP unterstützen möchten.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Statische und Dynamische Frameworks

**Statische Frameworks** werden zum Zeitpunkt der Kompilierung verknüpft, bei dem **dynamische Frameworks** zur Laufzeit verknüpft werden, und Sie können ohne erneute Verknüpfung geändert werden. Wenn Sie ein Drittanbieter-Framework vor IOS 8 verwendet haben, haben Sie ein **statisches Framework** verwendet, das in Ihre APP kompiliert wurde. Weitere Informationen finden Sie in der Dokumentation zur [dynamischen Bibliotheks Programmierung](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) von Apple.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Eingebettete und System Frameworks

**Eingebettete Frameworks** sind in Ihrem App-Bündel enthalten und sind nur über die Sandbox für Ihre APP zugänglich. **System-Frameworks** werden auf Betriebs System Ebene gespeichert und sind für alle apps auf dem Gerät verfügbar. Derzeit ist nur Apple in der Lage, Frameworks auf Betriebs System Ebene zu erstellen.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Dünn im Vergleich zu FAT-Frameworks

**Thin Frameworks** enthalten nur den kompilierten Code für eine bestimmte Systemarchitektur, in der **FAT-Frameworks** Code für mehrere Architekturen enthalten. Jede architekturspezifische Codebasis, die in ein Framework kompiliert wurde, wird als _Slice_bezeichnet. Wenn wir z. b. ein Framework haben, das für die beiden IOS-simulatorarchitekturen (i386 und x86_64) kompiliert wurde, würde es zwei Slices enthalten.

Wenn Sie versucht haben, dieses Beispiel Framework mit Ihrer APP zu verteilen, würde es im Simulator ordnungsgemäß ausgeführt werden, aber auf dem Gerät fehlschlagen, da das Framework keine Code spezifischen Slices für ein IOS-Gerät enthält. Um sicherzustellen, dass ein Framework in allen Instanzen funktioniert, muss es auch gerätespezifische Slices enthalten, wie z. b. arm64, armv7 und armv7s.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Arbeiten mit eingebetteten Frameworks

Es gibt zwei Schritte, die ausgeführt werden müssen, um mit eingebetteten Frameworks in einer xamarin. IOS-oder xamarin. Mac-app zu arbeiten: Erstellen eines FAT-Frameworks und Einbetten des Frameworks.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Erstellen eines FAT-Frameworks

Wie bereits erwähnt, muss es ein FAT-Framework sein, das alle Systemarchitekturen für die Geräte enthält, auf denen Ihre APP ausgeführt wird, um ein eingebettetes Framework in Ihrer APP nutzen zu können.

Wenn sich das Framework und die verbrauchende App im gleichen Xcode-Projekt befinden, ist dies kein Problem, da Xcode sowohl das Framework als auch die APP mit denselben Buildeinstellungen erstellt. Da xamarin-apps keine eingebetteten Frameworks erstellen können, kann diese Technik nicht verwendet werden.

Um dieses Problem zu beheben, `lipo` kann das Befehlszeilen Tool verwendet werden, um zwei oder mehr Frameworks in einem FAT-Framework zusammenzuführen, das alle erforderlichen Slices enthält. Weitere Informationen zum Arbeiten mit dem `lipo` Befehl finden Sie in unserer Dokumentation zum Verknüpfen von [nativen Bibliotheken](~/ios/platform/native-interop.md) .

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Einbetten eines Frameworks

Der folgende Schritt ist erforderlich, um ein Framework in ein xamarin. IOS-oder xamarin. Mac-Projekt mithilfe nativer Verweise einzubetten:

1. Erstellen Sie ein neues, oder öffnen Sie ein vorhandenes xamarin. IOS-, xamarin. Mac-oder-Bindungs Projekt.
2. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzu** > fügen**nativer Verweis**hinzufügen aus: 

    [![](native-references-images/ref01.png "Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie nativen Verweis hinzufügen aus.")](native-references-images/ref01.png#lightbox)
3. Wählen Sie im Dialogfeld **Öffnen** den Namen des nativen Frameworks aus, das Sie einbetten möchten, und klicken Sie auf die Schaltfläche **Öffnen** : 

    [![](native-references-images/ref02.png "Wählen Sie den Namen des Einbindungs Frameworks aus, und klicken Sie auf die Schaltfläche öffnen.")](native-references-images/ref02.png#lightbox)
4. Das Framework wird der Struktur des Projekts hinzugefügt: 

    [![](native-references-images/ref03.png "Das Framework wird der Projektstruktur hinzugefügt.")](native-references-images/ref03.png#lightbox)

Wenn das Projekt kompiliert wird, wird das Native Framework in das Paket der APP eingebettet.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>App-Erweiterungen und eingebettete Frameworks

Intern kann xamarin. IOS dieses Feature nutzen, um eine Verknüpfung mit der Mono-Laufzeit als Framework zu erstellen (wenn das Bereitstellungs Ziel > = IOS 8,0 ist), wodurch die APP-Größe für apps mit Erweiterungen erheblich reduziert wird (da die Mono-Laufzeit nur einmal für die gesamte App Bundle, anstatt einmal für die Container-APP und einmal für jede Erweiterung).

Erweiterungen werden mit der Mono-Laufzeit als Framework verknüpft, da für alle Erweiterungen IOS 8,0 erforderlich ist.

Apps, die keine Erweiterungen und Apps für IOS als Ziel haben 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie ein natives Framework in eine xamarin. IOS-oder xamarin. Mac-Anwendung einbetten.

