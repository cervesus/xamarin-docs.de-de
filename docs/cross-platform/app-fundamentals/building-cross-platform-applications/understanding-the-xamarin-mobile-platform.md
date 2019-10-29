---
title: Teil 1 – Grundlegendes zur xamarin Mobile-Plattform
description: In diesem Dokument wird die xamarin-Plattform auf hoher Ebene beschrieben. dabei werden der Kompilierungsprozess, der Zugriff auf das Plattform-SDK, die Freigabe von Code, die Erstellung von Benutzeroberflächen, visuelle Designer und mehr erläutert.
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: e10e9f5330de3226fb0f08051ab135ea58900fe7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016867"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Teil 1 – Grundlegendes zur xamarin Mobile-Plattform

Die xamarin-Plattform besteht aus einer Reihe von Elementen, mit denen Sie Anwendungen für IOS und Android entwickeln können:

- Language – ermöglicht Ihnen die Verwendung einer vertrauten Syntax und ausgereiften Features wie Generika, LINQ und der parallelen Aufgaben Bibliothek. **C#**
- **Mono .NET Framework** – bietet eine plattformübergreifende Implementierung der umfassenden Features von Microsoft .NET Framework.
- **Compiler** – abhängig von der Plattform erzeugt eine native app (z. b. IOS) oder eine integrierte .NET-Anwendung und-Laufzeit (z. b. Android). Der Compiler führt außerdem viele Optimierungen für die Mobile Bereitstellung aus, z. b. das Verknüpfen von nicht verwendeter Code.
- **IDE-Tools** – Visual Studio unter Mac und Windows ermöglicht Ihnen das Erstellen, erstellen und Bereitstellen von xamarin-Projekten.

Da die zugrunde liegende Sprache mit .NET Framework C# ist, können Projekte außerdem so strukturiert werden, dass Sie Code freigeben können, der auch für Windows Phone bereitgestellt werden kann.

## <a name="under-the-hood"></a>Im Hintergrund

Obwohl Sie mit xamarin apps in C#schreiben und denselben Code auf mehreren Plattformen freigeben können, ist die tatsächliche Implementierung auf den einzelnen Systemen sehr unterschiedlich.

## <a name="compilation"></a>Kompilierung

Die C# Quelle wird auf jeder Plattform auf sehr unterschiedliche Weise in eine native app integriert:

- **IOS** – C# wird in der Arm-Assemblysprache (Ahead-of-Time, AOT) kompiliert. .NET Framework ist enthalten, wobei nicht verwendete Klassen während der Verknüpfung entfernt werden, um die Anwendungs Größe zu verringern. Apple lässt keine Lauf Zeit Codegenerierung unter IOS zu, sodass einige sprach Features nicht verfügbar sind (siehe [Einschränkungen für xamarin. IOS](~/ios/internals/limitations.md) ).
- **Android** – C# wird in IL kompiliert und mit monovm und jitteten gepackt. Nicht verwendete Klassen im Framework werden während des Verknüpfens entfernt. Die Anwendung wird parallel mit Java/Art ausgeführt (Android-Laufzeit) und interagiert mit den systemeigenen Typen über jni (siehe [Einschränkungen für xamarin. Android](~/android/internals/limitations.md) ).
- **Windows** – C# wird in IL kompiliert und von der integrierten Laufzeit ausgeführt und erfordert keine xamarin-Tools. Das Entwerfen von Windows-Anwendungen nach der Anleitung von xamarin vereinfacht die erneute Verwendung des Codes unter IOS und Android.
  Beachten Sie, dass die universelle Windows-Plattform auch über eine **.net Native** -Option verfügt, die sich ähnlich verhält wie die AOT-Kompilierung von xamarin. IOS.

In der linkerdokumentation für [xamarin. IOS](~/ios/deploy-test/linker.md) und [xamarin. Android](~/android/deploy-test/linker.md) finden Sie weitere Informationen zu diesem Teil des Kompilierungsprozesses.

Runtime-Kompilierung – das dynamische Erstellen von Code mit `System.Reflection.Emit` – sollte vermieden werden.

Der Kernel von Apple verhindert die Generierung dynamischer Code auf IOS-Geräten, sodass die Ausgabe von Code im laufenden Betrieb nicht in xamarin. IOS funktioniert. Ebenso können die Funktionen der Dynamic Language Runtime nicht mit xamarin-Tools verwendet werden.

Einige Reflektionsobjekte funktionieren (z. b. MonoTouch. Dialog verwendet es für die reflektionsapi), nur die Codegenerierung.

## <a name="platform-sdk-access"></a>Platform SDK-Zugriff

Xamarin ermöglicht den einfachen Zugriff auf die Features, die vom plattformspezifischen SDK C# bereitgestellt werden, mit vertrauter Syntax:

- **IOS** – xamarin. IOS macht die cocoatouch SDK-Frameworks von C#Apple als Namespaces verfügbar, auf die Sie verweisen können. Beispielsweise kann das UIKit-Framework, das alle Steuerelemente der Benutzeroberfläche enthält, in eine einfache `using UIKit;`-Anweisung eingeschlossen werden.
- **Android** – xamarin. Android stellt Google-Android SDK als Namespaces zur Verfügung, sodass Sie mit einer using-Anweisung, z. b. `using Android.Views;`, auf die Benutzeroberflächen-Steuerelemente zugreifen können, auf einen beliebigen Teil des unterstützten SDK verweisen können.
- **Windows** – Windows-apps werden mithilfe von Visual Studio unter Windows erstellt. Zu den Projekttypen zählen Windows Forms, WPF, WinRT und die universelle Windows-Plattform (UWP).

## <a name="seamless-integration-for-developers"></a>Nahtlose Integration für Entwickler

Die Schönheit von xamarin besteht darin, dass xamarin. IOS und xamarin. Android (gekoppelt mit den Windows sdgs von Microsoft) eine nahtlose Oberflächen zum Schreiben C# von Code bieten, der für alle drei Plattformen wieder verwendet werden kann.

Geschäftslogik, Datenbanknutzung, Netzwerk Zugriff und andere gängige Funktionen können einmalig geschrieben und auf jeder Plattform wieder verwendet werden. Dadurch wird eine Grundlage für plattformspezifische Benutzeroberflächen bereitgestellt, die als native Anwendungen aussehen und durchgeführt werden.

## <a name="integrated-development-environment-ide-availability"></a>Verfügbarkeit der integrierten Entwicklungsumgebung (IDE)

Die xamarin-Entwicklung kann in Visual Studio entweder unter Mac oder Windows ausgeführt werden. Die von Ihnen gewählte IDE wird von den Plattformen bestimmt, auf die Sie abzielen möchten.

Windows-Apps können nur unter Windows entwickelt werden, um für IOS, Android _und_ Windows erstellt zu werden, erfordert Visual Studio für Windows. Es ist jedoch möglich, Projekte und Dateien zwischen Windows-und Macintosh-Computern gemeinsam zu nutzen, sodass IOS-und Android-Apps auf einem Mac erstellt werden können und der freigegebene Code später einem Windows-Projekt hinzugefügt werden kann.

Die Entwicklungsanforderungen für die einzelnen Plattformen werden im [Anforderungs](~/cross-platform/get-started/requirements.md) Handbuch ausführlicher erläutert.

### <a name="ios"></a>iOS

Für die Entwicklung von IOS-Anwendungen ist ein Macintosh-Computer mit macOS erforderlich. Sie können auch Visual Studio verwenden, um IOS-Anwendungen mit xamarin in Visual Studio zu schreiben und bereitzustellen. Ein Mac ist aber immer noch für Build-und Lizenzierungs Zwecke erforderlich.

Die Xcode-IDE von Apple muss installiert sein, um den Compiler und Simulator für das Testen bereitzustellen. Sie können [kostenlos](~/ios/get-started/installation/device-provisioning/free-provisioning.md)auf Ihren eigenen Geräten testen, aber zum Erstellen von Anwendungen für die Verteilung (z. b. App Store) Sie müssen dem Entwicklerprogramm von Apple ($99 USD pro Jahr) beitreten. Jedes Mal, wenn Sie eine Anwendung übermitteln oder aktualisieren, muss Sie von Apple überprüft und genehmigt werden, bevor Sie für den Download zur Verfügung gestellt wird.

Der Code wird mit der Visual Studio-IDE geschrieben, und Bildschirmlayouts können Programm gesteuert erstellt oder mit dem IOS-Designer von xamarin in beiden IDE bearbeitet werden.

Ausführliche Anweisungen zum Einrichten finden Sie im [xamarin. IOS-Installationshandbuch](~/ios/get-started/installation/index.md) .

### <a name="android"></a>Android

Die Entwicklung von Android-Anwendungen erfordert, dass die Java-und Android-sdche installiert werden. Diese stellen den Compiler, den Emulator und andere Tools bereit, die zum Erstellen, bereitstellen und Testen erforderlich sind. Java, das Google-Android SDK und die xamarin-Tools können alle unter Windows und macOS installiert und ausgeführt werden. Die folgenden Konfigurationen werden empfohlen:

- Windows 10 mit Visual Studio 2019
- macOS-mujave (10.11 +) mit Visual Studio 2019 für Mac

Xamarin bietet ein einheitliches Installationsprogramm, das Ihr System mit den erforderlichen Java-, Android-und xamarin-Tools (einschließlich eines visuellen Designers für Bildschirmlayouts) konfiguriert. Ausführliche Anweisungen finden Sie im [xamarin. Android-Installationshandbuch](~/android/get-started/installation/index.md) .

Sie können Anwendungen auf einem echten Gerät ohne jegliche Lizenz von Google erstellen und testen. um Ihre Anwendung jedoch über einen Store zu verteilen (z. b. Google Play, Amazon oder Barnes &amp; Noble), kann eine Registrierungsgebühr an den Operator gezahlt werden. Google Play wird Ihre APP umgehend veröffentlichen, während die anderen Geschäfte einen Genehmigungsprozess ähnlich dem von Apple haben.

### <a name="windows"></a>Windows

Windows-Apps (WinForms, WPF oder UWP) werden mit Visual Studio erstellt. Xamarin wird nicht direkt verwendet. C# Code kann jedoch für Windows, IOS und Android freigegeben werden.
Besuchen Sie das [dev Center](https://developer.microsoft.com/) von Microsoft, um mehr über die für die Windows-Entwicklung erforderlichen Tools zu erfahren.

## <a name="creating-the-user-interface-ui"></a>Erstellen der Benutzeroberfläche (UI)

Ein wichtiger Vorteil der Verwendung von xamarin besteht darin, dass die Benutzeroberfläche der Anwendung Native Steuerelemente auf jeder Plattform verwendet und Apps erstellt, die nicht von einer Anwendung unterschieden werden können, die in Ziel-C oder Java (für IOS und Android) geschrieben ist.

Beim Erstellen von Bildschirmen in der App können Sie entweder die Steuerelemente im Code anordnen oder mithilfe der Entwurfs Tools, die für jede Plattform verfügbar sind, komplette Bildschirme erstellen.

### <a name="create-controls-programmatically"></a>Programm gesteuertes Erstellen von Steuerelementen

Jede Plattform ermöglicht das Hinzufügen von Benutzeroberflächen-Steuerelementen zu einem Bildschirm mithilfe von Code. Dies kann sehr zeitaufwändig sein, da es schwierig sein kann, den fertigen Entwurf zu visualisieren, wenn Pixelkoordinaten für Positionen und Größen fest codiert werden.

Das programmgesteuerte Erstellen von Steuerelementen hat jedoch Vorteile, insbesondere bei IOS, um Ansichten zu erstellen, die sich auf der Bildschirm Größe des iPhone und iPads anders ändern oder verändern.

### <a name="visual-designer"></a>Visueller Designer

Jede Plattform verfügt über eine andere Methode zum visuellen Anordnen von Bildschirmen:

- **IOS** – der IOS-Designer von xamarin ermöglicht das Entwickeln von Ansichten mithilfe von Drag & amp; Drop-Funktionen und Eigenschaften Feldern. Zusammen bilden diese Sichten ein Storyboard, auf das in zugegriffen werden kann **. Die storyboarddatei** , die im Projekt enthalten ist.
- **Android** – xamarin bietet einen Android-Benutzeroberflächen-Designer für Visual Studio mit Drag & Drop. Android-Bildschirmlayouts werden unter gespeichert **. Axml** -Dateien bei Verwendung von xamarin-Tools.
- **Windows** – Microsoft stellt einen Drag & Drop-UI-Designer in Visual Studio und Blend bereit. Die Bildschirmlayouts werden als gespeichert. XAML-Dateien.

Diese Screenshots zeigen die visuellen Bildschirm-Designer, die auf jeder Plattform verfügbar sind:

 [![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "These screenshots show the visual screen designers available on each platform")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

In allen Fällen kann auf die Elemente, die Sie visuell erstellen, in Ihrem Code verwiesen werden.

### <a name="user-interface-considerations"></a>Überlegungen zu Benutzeroberflächen

Ein wichtiger Vorteil der Verwendung von xamarin zum Erstellen plattformübergreifender Anwendungen ist, dass Sie die Vorteile der nativen UI-Toolkits nutzen können, um dem Benutzer eine vertraute Benutzeroberfläche zur Verfügung zu stellen. Die Benutzeroberfläche führt auch so schnell wie jede andere native Anwendung aus.

Einige UI-Metaphern funktionieren auf mehreren Plattformen (z. b. für alle drei Plattformen wird ein ähnliches Scroll-List-Steuerelement verwendet), aber damit Ihre Anwendung das richtige Verhalten hat, sollte die Benutzeroberfläche bei Bedarf plattformspezifische Benutzeroberflächen Elemente nutzen. Beispiele für plattformspezifische UI-Metaphern sind:

- **IOS** – hierarchische Navigation mit weicher zurück-Schaltfläche, Registerkarten am unteren Bildschirmrand.
- **Android** – Hardware/System-Software zurück-Schaltfläche, Aktionsmenü, Registerkarten am oberen Bildschirmrand.
- **Windows** – Windows-Apps können auf Desktops, Tablets (z. b. Microsoft Surface) und Smartphones ausgeführt werden. Windows 10-Geräte verfügen möglicherweise über Hardware-zurück-Schaltflächen und Live-Kacheln.

Es wird empfohlen, dass Sie die Entwurfs Richtlinien lesen, die für die Zielplattformen relevant sind:

- **IOS** – [Richtlinien für die Human Schnittstelle von Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
- **Android** – [Richtlinien für die Benutzeroberfläche von Google](https://developer.android.com/guide/practices/ui_guidelines/index.html)
- **Windows** – [Benutzeroberflächen-Entwurfs Richtlinien für Windows](https://developer.microsoft.com/windows/design)

## <a name="library-and-code-re-use"></a>Wiederverwendung von Bibliothek und Code

Die xamarin-Plattform ermöglicht die erneute Verwendung von C# vorhandenem Code auf allen Plattformen sowie die Integration von Bibliotheken, die nativ für jede Plattform geschrieben wurden.

### <a name="c-source-and-libraries"></a>C#Quelle und Bibliotheken

Da xamarin-Produkte C# und .NET Framework verwenden, können viele vorhandene Quellcodes (Open Source-und interne Projekte) in xamarin. IOS-oder xamarin. Android-Projekten wieder verwendet werden. Häufig kann die Quelle einfach zu einer xamarin-Projekt Mappe hinzugefügt werden, und Sie wird sofort funktionieren. Wenn eine nicht unterstützte .NET Framework-Funktion verwendet wird, sind möglicherweise einige Anpassungen erforderlich.

Beispiele für C# die Quelle, die in xamarin. IOS oder xamarin. Android verwendet werden kann, sind: SQLite-net, newtonsoft. JSON und SharpZipLib.

### <a name="objective-c-bindings--binding-projects"></a>Ziel-C-Bindungen und Bindungs Projekte

Xamarin stellt ein Tool namens *bberührungs* bereit, mit dem Bindungen erstellt werden können, die das Verwenden von Ziel-C-Bibliotheken in xamarin. IOS-Projekten ermöglichen. Ausführliche Informationen zur Vorgehensweise finden Sie in der [Dokumentation zum Bindungs Ziel-C-Typ](~/cross-platform/macios/binding/binding-types-reference.md) .

Beispiele für Ziel-C-Bibliotheken, die in xamarin. IOS verwendet werden können, sind: RedLaser Barcode Scan, Google Analytics und PayPal-Integration. Open-Source-xamarin. IOS-Bindungen sind auf [GitHub](https://github.com/mono/monotouch-bindings)verfügbar.

### <a name="jar-bindings--binding-projects"></a>JAR-Bindungen und Bindungs Projekte

Xamarin unterstützt die Verwendung vorhandener Java-Bibliotheken in xamarin. Android. Ausführliche Informationen zur Verwendung eines-Objekts finden Sie in der [Dokumentation zur Bindung einer Java-Bibliothek](~/android/platform/binding-java-library/index.md) . JAR-Datei aus xamarin. Android.

Open-Source-xamarin. Android-Bindungen sind auf [GitHub](https://github.com/mono/monodroid-bindings)verfügbar.

### <a name="c-via-pinvoke"></a>C über PInvoke

Die Technologie "Platt Form Aufruf" (P/Aufruf) ermöglicht verwaltetenC#Code () das Aufrufen von Methoden in systemeigenen Bibliotheken sowie die Unterstützung von systemeigenen Bibliotheken zum Aufrufen von verwaltetem Code.

Beispielsweise verwendet die [SQLite-net-](https://github.com/praeclarum/sqlite-net) Bibliothek Anweisungen wie die folgende:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

Dies bindet an die native C-Sprache SQLite-Implementierung in IOS und Android.
Entwickler, die mit einer vorhandenen C-API vertraut sind, C# können eine Reihe von Klassen erstellen, um Sie der systemeigenen API zuzuordnen und den vorhandenen Platt Form Code zu verwenden. Es gibt eine Dokumentation zum Verknüpfen von systemeigenen [Bibliotheken](~/ios/platform/native-interop.md) in xamarin. IOS, ähnliche Prinzipien gelten für xamarin. Android.

### <a name="c-via-cppsharp"></a>C++über cppsharp

Miguel erläutert CXXI (jetzt [cppsharp](https://github.com/mono/CppSharp)genannt) in seinem [Blog](https://tirania.org/blog/archive/2011/Dec-19.html). Eine Alternative zur direkten Bindung an C++ eine Bibliothek besteht darin, einen C-Wrapper zu erstellen und über P/Aufrufen an diese zu binden.
