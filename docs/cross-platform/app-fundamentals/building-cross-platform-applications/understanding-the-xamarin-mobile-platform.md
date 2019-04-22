---
title: Teil 1 – Grundlegendes zu den mobilen Xamarin-Plattform
description: Dieses Dokument beschreibt die Xamarin-Plattform auf einer hohen Ebene Betrachten des Kompilierungsprozesses, Plattform SDK Zugriff, Codefreigabe, Erstellung von Netzwerkschnittstellen, visuelle Designer und mehr.
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f5008d4986baa0575030e077b66b69ec0a4fad00
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854417"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Teil 1 – Grundlegendes zu den mobilen Xamarin-Plattform

Die Xamarin-Plattform besteht aus einer Anzahl von Elementen, die Sie zum Entwickeln von Anwendungen für iOS und Android zu ermöglichen:

-   **C#Sprache** – können Sie eine vertraute Syntax und anspruchsvolle Features wie Generika, LINQ und die Task Parallel Library verwenden.
-   **Mono .NET Framework** – bietet eine plattformübergreifende Implementierung der umfangreichen Funktionen in Microsoft .NET Framework.
-   **Compilerfehler** – abhängig von der Plattform, erzeugt eine systemeigene app (z. b. iOS) oder eine integrierte Anwendung für .NET und die Common Language Runtime (z. b. Android). Der Compiler führt auch zahlreiche Optimierungen für die mobile-Bereitstellung, z. B. zum Verknüpfen von entfernt nicht verwendeten Code.
-   **IDE-Tools** : die Visual Studio für Mac und Windows können Sie erstellen, erstellen und Bereitstellen von Xamarin-Projekten.

Darüber hinaus, da die zugrunde liegende Sprache ist C# mit .NET Framework-Projekte können zum Teilen von Code, der auch für Windows Phone bereitgestellt werden können strukturiert werden.

## <a name="under-the-hood"></a>Im Hintergrund

Xamarin können Sie zum Schreiben von apps in C#, und teilen Sie den gleichen Code auf mehreren Plattformen, die tatsächliche Implementierung auf jedem System ist sehr unterschiedlich.

## <a name="compilation"></a>Kompilierung

Die C# Quelle gerichteten seinen Weg in einer nativen app auf den einzelnen Plattformen sehr unterschiedlich:

-   **iOS** – C# ahead-of-Time (AOT) in der ARM-Assemblysprache kompiliert wird. .NET Framework ist enthalten, wobei nicht verwendete Klassen, die dann entfernt werden, während der Verknüpfung, um die Größe der Anwendung zu reduzieren. Apple lässt keine codegenerierung für Laufzeit unter iOS, damit einige Sprachfunktionen nicht verfügbar sind (siehe [Xamarin.iOS Einschränkungen](~/ios/internals/limitations.md) ).
-   **Android** – C# ist in der IL kompiliert und mit MonoVM und JIT'ing verpackt. Nicht verwendete Klassen im Framework werden während des Verknüpfens entfernt. Die Anwendung ausgeführt wird Seite-an-Seite mit Java / (Android-Laufzeit) und interagiert mit der systemeigenen Typen über JNI (finden Sie unter [Xamarin.Android Einschränkungen](~/android/internals/limitations.md) ).
-   **Windows** – C# ist in der IL kompiliert und von der integrierten Runtime ausgeführt und erfordert keine Xamarin-Tools. Entwerfen von Windows-Anwendungen erleichtert folgende Xamarin Leitfaden erneut den Code für iOS und Android zu verwenden.
  Beachten Sie, dass die universelle Windows-Plattform auch eine **.NET Native** Option aus, die Xamarin.iOS AOT-Kompilierung auf ähnliche Weise verhält.


Der Linker Dokumentation [Xamarin.iOS](~/ios/deploy-test/linker.md) und [Xamarin.Android](~/android/deploy-test/linker.md) enthält weitere Informationen zu diesem Teil des Kompilierungsprozesses.

Common Language Runtime "Compilation" – Generieren von Code dynamisch mit `System.Reflection.Emit` – sollte vermieden werden.

Apple Kernel verhindert, dass dynamische codegenerierung auf iOS-Geräten, die aus diesem Grund Ausgeben von Code auf dynamische funktioniert nicht in Xamarin.iOS. Ebenso können die Dynamic Language Runtime-Funktionen mit Xamarin-Tools verwendet werden.

Einige Reflektionsfunktionen funktionieren (z. b. MonoTouch.Dialog wird es für die Reflektions-API), aber nicht die codegenerierung.

## <a name="platform-sdk-access"></a>Plattform-SDK-Zugriff

Xamarin stellt die Funktionen bereitgestellt, die durch das plattformspezifische SDK leicht zugänglich mit bekannten C# Syntax:

-   **iOS** – Xamarin.iOS macht Apple CocoaTouch SDK Frameworks wie Namespaces, die Sie in verweisen können C#. Zum Beispiel das UIKit-Framework, das alle Steuerelemente der Benutzeroberfläche enthält, kann eingefügt werden mit einem einfachen `using UIKit;` Anweisung.
-   **Android** – Xamarin.Android macht Googles Android SDK als Namespaces verfügbar, sodass Sie einen beliebigen Teil der unterstützten SDK mit einem verweisen können.-Anweisung, wie z. B. `using Android.Views;` auf die Steuerelemente der Benutzeroberfläche zugreifen.
-   **Windows** – Windows-apps werden mit Visual Studio unter Windows erstellt. Projekttypen enthalten Windows Forms, WPF, WinRT und die universelle Windows-Plattform (UWP).

## <a name="seamless-integration-for-developers"></a>Nahtlose Integration für Entwickler

Xamarin Installierungszeit trotz der Unterschiede im Hintergrund eine nahtlose benutzererfahrung für das Schreiben von Xamarin.iOS und Xamarin.Android (gekoppelt mit von Microsoft Windows SDKs) bieten C# Code, der auf allen drei Plattformen nicht erneut verwendet werden kann.

Geschäftslogik, Datenbankverwendung, Zugriff auf das Netzwerk und andere häufig verwendete Funktionen können einmal geschrieben und auf jeder Plattform, die gleichzeitig eine Grundlage für plattformspezifische Benutzeroberflächen, die aussehen, und führen Sie als native Anwendungen wiederverwendet werden.

## <a name="integrated-development-environment-ide-availability"></a>Verfügbarkeit von Integrated Development Environment (IDE)

Xamarin-Entwicklung in Visual Studio unter Mac oder Windows möglich. Die IDE, die Sie auswählen, durch die Plattform bestimmt wird, die Sie abzielen möchten.

Da Windows-apps nur können, können Sie auf Windows entwickelt werden, für die Erstellung für iOS, Android, _und_ Windows erfordert Visual Studio für Windows. So ist es jedoch möglich, Freigeben von Projekten und Dateien zwischen Windows und Mac-Computer damit IOS- und Android-apps auf einem Mac erstellt werden können und freigegebener Code später zu einem Windows-Projekt hinzugefügt werden kann.

Die-entwicklungsanforderungen für jede Plattform werden ausführlicher in den [Anforderung](~/cross-platform/get-started/requirements.md) Guide.

### <a name="ios"></a>iOS

Entwickeln von iOS-Anwendungen erfordert eine Mac-Computer mit MacOS. Sie können auch Visual Studio verwenden, schreiben und Bereitstellen von iOS-Anwendungen mit Xamarin in Visual Studio. Ein Mac ist jedoch weiterhin für Build- und lizenzzwecke erforderlich.

Xcode-IDE von Apple muss installiert sein, um den Compiler und Simulator zum Testen zu ermöglichen. Sie können auf Ihren eigenen Geräten testen [kostenlos](~/ios/get-started/installation/device-provisioning/free-provisioning.md), aber zum Erstellen von Anwendungen für die Verteilung (z. b. die App-Store), müssen Sie Apple Developer Program (99 US-Dollar pro Jahr) verknüpfen. Jedes Mal, die Sie übermitteln, oder aktualisieren eine Anwendung, muss geprüft und werden von Apple genehmigt werden, bevor sie für Kunden zum Herunterladen verfügbar gemacht wird.

Code wurde geschrieben, mit Visual Studio-IDE und Bildschirmlayouts programmgesteuert erstellt oder mit Xamarin iOS-Designer in beiden IDEs bearbeitet werden können.

Finden Sie in der [Leitfaden für Xamarin.iOS-Installation](~/ios/get-started/installation/index.md) ausführliche Anweisungen zur Einrichtung.

### <a name="android"></a>Android

Entwicklung von Android-Anwendung erfordert die Java- und Android SDKs installiert werden. Diese bieten Compiler, Emulator und anderen Tools, die für die Erstellung, Bereitstellung und Tests erforderlich sind. Java, Googles Android SDK und die Xamarin Tools können alle installiert und ausgeführt werden unter Windows und MacOS. Die folgenden Konfigurationen werden empfohlen:

- Windows 10 mit Visual Studio-2019
- MacOS Mojave (10.11 und höher) mit Visual Studio-2019 für Mac

Xamarin bietet einen einheitlichen Installer, der Ihr System mit den erforderlichen Java, Android und Xamarin-Tools (einschließlich einen visuellen Designer für Bildschirmlayouts) konfigurieren. Finden Sie in der [Leitfaden für Xamarin.Android-Installation](~/android/get-started/installation/index.md) ausführliche Anweisungen.

Sie können Anwendungen entwickeln und Testen auf einem echten Gerät ohne schriftlichen Lizenzverträgen von Google, jedoch verteilen Sie Ihre Anwendung über einen Speicher (wie Google Play, Amazon oder Barnes &amp; Noble) eine Registrierungsgebühr in möglicherweise an den Operator zu zahlenden. Google Play Veröffentlichen Ihrer app sofort, während die anderen speichern, die einen Genehmigungsprozess, der ähnlich wie von Apple haben.

### <a name="windows"></a>Windows

Windows-apps (Windows Forms, WPF oder UWP) werden mit Visual Studio erstellt. Diese ZS keine Xamarin direkt verwenden. Allerdings C# Codes kann übergreifend für Windows, iOS und Android.
Besuchen Sie die Microsoft [Dev Center](https://developer.microsoft.com/) Informationen zu den Tools für die Windows-Entwicklung.

## <a name="creating-the-user-interface-ui"></a>Erstellen der Benutzeroberfläche (UI)

Ein wichtiger Vorteil der Verwendung von Xamarin ist, dass die Benutzeroberfläche der Anwendung verwendet die systemeigenen Steuerelemente auf jeder Plattform, erstellen apps, die von einer Anwendung, die in Objective-C oder Java geschrieben sind (für iOS und Android bzw.).

Beim Erstellen von Bildschirmen in Ihrer app können Sie das Layout der Steuerelemente im Code oder vollständige Bildschirme, die mithilfe der Entwurfstools verfügbar für jede Plattform erstellen.

### <a name="create-controls-programmatically"></a>Programmgesteuertes Erstellen von Steuerelementen

Jede Plattform ermöglicht dem Benutzer Steuerelemente der Benutzeroberfläche eines Bildschirms mithilfe von Code hinzugefügt werden. Dies kann sehr zeitaufwändig sein, wie es Darstellung der fertigen Entwurf schwierig sein kann, wenn Pixel fest zu programmieren koordiniert für die Größen und Positionen.

Programmgesteuertes Erstellen von Steuerelementen muss Vorteile allerdings vor allem für iOS zum Erstellen von Ansichten, die die Größe oder unterschiedlich dargestellt werden, für das iPhone und iPad Bildschirmgrößen.

### <a name="visual-designer"></a>Visuelle Designer

Jede Plattform gibt es eine andere Methode für das visuelle Layout von Bildschirmen:

- **iOS** – Xamarin iOS-Designer erleichtert das Erstellen von Ansichten mithilfe von Drag & Drop-Funktionalität und die Eigenschaft Feldern. Zusammen diese Ansichten besteht aus einem Storyboard und zugegriffen werden kann, der **. Storyboard** -Datei, die in Ihrem Projekt enthalten ist.
- **Android** : Xamarin bietet einen Android-Drag & Drop-Benutzeroberflächen-Designer für Visual Studio. Android-Bildschirmlayouts werden gespeichert, als **. AXML** Dateien, wenn Sie Xamarin-Tools verwenden.
- **Windows** – Microsoft bietet einen Drag & Drop-Benutzeroberflächen-Designer in Visual Studio und Blend. Die Bildschirmlayouts werden als gespeichert. XAML-Dateien.

Diese Screenshots zeigen die Bildschirm-Designer zur Verfügung, auf jeder Plattform:

 [ ![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "Diese Screenshots zeigen die Bildschirm-Designer zur Verfügung, auf jeder Plattform")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

In allen Fällen können die Elemente, die Sie visuell zu erstellen, in Ihrem Code verwiesen werden.

### <a name="user-interface-considerations"></a>Überlegungen zur Benutzeroberfläche

Ein wichtiger Vorteil der Verwendung von Xamarin zum Erstellen von plattformübergreifenden Anwendungen ist, dass sie nativen UI-Toolkits, die eine vertraute Schnittstelle für dem Benutzer vorhanden nutzen können. Die Benutzeroberfläche wird außerdem so schnell wie jede andere systemeigene Anwendung ausgeführt werden.

Einige UI Metaphern auf mehreren Plattformen funktionieren (z. B. für alle drei Plattformen verwenden ein ähnliches Bildlauf-List-Steuerelement) in der Reihenfolge für die Anwendung auf die "richtige fühlen" die Benutzeroberfläche sollten profitieren aber plattformspezifische Benutzer bei Bedarf Benutzeroberflächenelemente. Beispiele für plattformspezifische Benutzeroberfläche Metaphern sind:

- **iOS** – hierarchische Navigation mit soft Schaltfläche "zurück", Registerkarten am unteren Rand des Bildschirms.
- **Android** – Hardware-/System-Software-back-Schaltfläche, die im Aktionsmenü, die Registerkarten am oberen Bildschirmrand.
- **Windows** – Windows-apps können auf Desktops, Tablets (z. B. Microsoft Surface-) und -Smartphones ausgeführt werden. Windows 10-Geräte möglicherweise die Hardware-Schaltfläche und live-Kacheln, z. B. zu sichern.

Es wird empfohlen, die Entwurfsrichtlinien für die Plattformen zu lesen, die Sie verwenden möchten:

- **iOS** – [Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
- **Android** – [Google Richtlinien zur Benutzeroberfläche](https://developer.android.com/guide/practices/ui_guidelines/index.html)
- **Windows** – [User Experience-Entwurfsrichtlinien für Windows](https://developer.microsoft.com/windows/design)

## <a name="library-and-code-re-use"></a>-Bibliothek und Wiederverwendungsrate für Code

Die Xamarin-Plattform ermöglicht die erneute Verwendung von vorhandenen C# -Codes gemeinsam auf allen Plattformen sowie die Integration von Bibliotheken, die für jede Plattform systemintern geschrieben.

### <a name="c-source-and-libraries"></a>C#Quell- und Bibliotheken

Da Xamarin-Produkte verwenden C# und .NET Framework, viele der vorhandenen Quellcode (sowohl open Source, interne Projekte) können in Xamarin.iOS oder Xamarin.Android-Projekten wiederverwendet werden. Häufig die Quelle einfach zu einer Xamarin-Projektmappe hinzugefügt werden kann, und es sofort funktioniert. Wenn eine nicht unterstützte .NET Framework-Funktion verwendet wurde, möglicherweise einige Anpassungen erforderlich.

Beispiele für C# Datenquelle, die in Xamarin.iOS oder Xamarin.Android verwendet werden kann umfassen: SQLite-NET "," NewtonSoft.JSON "und" SharpZipLib ".

### <a name="objective-c-bindings--binding-projects"></a>Objective-C-Bindungen und -Bindungsprojekte

Xamarin bietet ein Tool namens *Btouch* unterstützt Bindungen erstellen, mit denen Objective-C-Bibliotheken in Xamarin.iOS-Projekte verwendet werden können. Finden Sie in der [Binden von Objective-C-Typen Dokumentation](~/cross-platform/macios/binding/binding-types-reference.md) Einzelheiten, wie dies vonstatten geht.

Beispiele von Objective-C-Bibliotheken, die in Xamarin.iOS verwendet werden können: RedLaser-Barcode scannen, Google Analytics und PayPal-Integration. Open-Source-Xamarin.iOS-Bindungen stehen auf [Github](https://github.com/mono/monotouch-bindings).

### <a name="jar-bindings--binding-projects"></a>JAR-Bindungen und -Bindungsprojekte

Xamarin unterstützt die Verwendung von vorhandener Java-Bibliotheken in Xamarin.Android. Finden Sie in der [binden eine Java-Bibliothek-Dokumentation](~/android/platform/binding-java-library/index.md) Weitere Informationen zur Verwendung ein. JAR-Datei, die von Xamarin.Android.

Open-Source-Xamarin.Android-Bindungen stehen auf [Github](https://github.com/mono/monodroid-bindings).

### <a name="c-via-pinvoke"></a>C über PInvoke

"Platform Invoke"-Technologie (P/Invoke) ermöglicht es verwaltetem Code (C#) rufen Sie Methoden in native Bibliotheken als auch für native Bibliotheken Rückruf in verwaltetem Code zu unterstützen.

Z. B. die [SQLite-NET-](https://github.com/praeclarum/sqlite-net) Bibliothek werden Anweisungen wie folgt verwendet:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

Bindet an die native Implementierung der Programmiersprache C SQLite in iOS und Android.
Entwickler, mit einer vorhandenen C-API vertraut sind, können eine Reihe von erstellen C# Klassen, die systemeigene API zuordnen und den vorhandenen Plattformcode nutzen. Es ist die Dokumentation für [native Bibliotheken verknüpfen](~/ios/platform/native-interop.md) in Xamarin.iOS, ähnliche Prinzipien gebunden, die in Xamarin.Android anwenden.

### <a name="c-via-cppsharp"></a>C++ über CppSharp

Miguel erläutert CXXI (jetzt [CppSharp](https://github.com/mono/CppSharp)) über seinen [Blog](https://tirania.org/blog/archive/2011/Dec-19.html). Eine Alternative Bindung direkt in einer C++-Bibliothek ist einen C#-Wrapper erstellen und Binden an, die über P/Invoke.
