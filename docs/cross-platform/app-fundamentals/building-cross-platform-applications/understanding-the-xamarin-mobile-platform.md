---
title: "Teil 1 – Grundlegendes zum der Xamarin-Mobile-Plattform"
ms.topic: article
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0c215cc03904d312d8548eddf15614fabe229b25
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Teil 1 – Grundlegendes zum der Xamarin-Mobile-Plattform

Die Xamarin-Plattform umfasst eine Reihe von Elementen, die Sie zum Entwickeln von Anwendungen für iOS und Android zulassen:

-   **C#-Sprache** – können Sie mit einer vertrauten Syntax und anspruchsvolle Funktionen wie Generika, LINQ und Task Parallel Library.
-   **Mono-/ .NET Framework** – bietet eine plattformübergreifende Implementierung der umfangreiche Funktionen in Microsoft .NET Framework.
-   **Compilerfehler** – abhängig von der Plattform erzeugt eine systemeigene app (z. b. iOS) oder einer integrierten .NET-Anwendung und die Common Language Runtime (z. b. Android). Der Compiler führt auch viele Optimierungen für mobile Bereitstellung z. B. zum beseitigen nicht verwendeten Code zu verknüpfen.
-   **IDE-Tools** – Visual Studio auf Macintosh- und Windows können Sie erstellen, erstellen und Bereitstellen von Xamarin-Projekten.


Da die zugrunde liegenden Sprache c# mit dem .NET Framework ist, können darüber hinaus Projekte strukturiert werden zum Freigeben von Code, der auch für Windows Phone bereitgestellt werden können.

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>Hinter den Kulissen

Xamarin lässt Sie apps in c# schreiben und denselben Code auf mehreren Plattformen freigeben, wird die tatsächliche Implementierung auf allen Systemen sehr unterschiedliche.

 <a name="Compilation" />


## <a name="compilation"></a>Kompilierung

Die C#-Quellcode macht eine Methode in eine systemeigene app sehr unterschiedlich auf jeder Plattform:

-   **iOS** – C#-ahead-des-Time (AOT) in ARM Assemblysprache kompiliert wird. .NET Framework ist enthalten, mit nicht verwendeten Klassen, die während der Verknüpfung zum Reduzieren der Anwendungsgröße entfernt wird. Apple lässt keine codegenerierung für Laufzeit bei iOS kann, damit einige Sprachfunktionen nicht verfügbar sind (siehe [Xamarin.iOS Einschränkungen](~/ios/internals/limitations.md) ).
-   **Android** – c# ist in IL kompiliert und verpackt, wobei MonoVM + JIT'ing. Nicht verwendete Klassen Framework werden während der Verknüpfung entfernt. Die Anwendung ausgeführt wird Seite-an-Seite mit Java/Grafiken (Android Common Language Runtime) dar und interagiert mit den systemeigenen Typen über JNI (finden Sie unter [Xamarin.Android Einschränkungen](~/android/internals/limitations.md) ).
-   **Windows** – c# auf IL kompiliert wird und von der integrierten Laufzeit ausgeführt und erfordert keine Xamarin-Tools. Entwerfen von Windows-Anwendungen vereinfacht die folgenden Xamarin Anleitung vom Code auf IOS- und Android erneut zu verwenden.
  Beachten Sie, dass die universelle Windows-Plattform auch eine **.NET Native** Option 'Xamarin.iOS AOT Kompilierung entsprechend verhält.


Der Linker-Dokumentation für [Xamarin.iOS](~/ios/deploy-test/linker.md) und [Xamarin.Android](~/android/deploy-test/linker.md) enthält weitere Informationen zu diesem Teil des Kompilierungsprozesses.

Common Language Runtime "Kompilierung" – Generieren von Code dynamisch mit `System.Reflection.Emit` – sollte vermieden werden.

Apple Kernel wird verhindert, dass dynamische codegenerierung auf iOS-Geräten, daher Ausgeben von Code auf dynamische funktionieren nicht in Xamarin.iOS. Ebenso können die Dynamic Language Runtime-Funktionen nicht mit Xamarin-Tools verwendet werden.

Einige Reflektionsfunktionen funktionieren (z. b. MonoTouch.Dialog wird es für die Reflektions-API), nicht nur bei der codegenerierung.

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>Platform SDK-Zugriff

Xamarin macht die Funktionen, die vom SDK plattformspezifischen problemlos mit vertrauten C#-Syntax bereitgestellt:

-   **iOS** – Xamarin.iOS stellt Apple CocoaTouch SDK-Frameworks als Namespaces, die Sie in c# verweisen können. Beispielsweise kann UIKit-Framework, die alle Steuerelemente der Benutzeroberfläche enthält mit einem einfachen enthalten sein `using UIKit;` Anweisung.
-   **Android** – Xamarin.Android stellt Google Android SDK als Namespaces, damit Sie einen beliebigen Teil der unterstützten SDK mit einer Hand haben-Anweisung, wie z. B. `using Android.Views;` auf die Steuerelemente der Benutzeroberfläche zugreifen.
-   **Windows** – Windows-apps mithilfe von Visual Studio unter Windows erstellt. Projekttypen enthalten Windows Forms, WPF, WinRT und die universelle Windows-Plattform (UWP).


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>Nahtlose Integration für Entwickler

Der Vorteil von Xamarin ist, dass trotz der Unterschiede hinter den Kulissen Xamarin.iOS und Xamarin.Android (gekoppelt mit Microsofts Windows SDKs) bieten eine einheitliche Erfahrung für C#-Code schreiben, der für alle drei Plattformen nicht erneut verwendet werden kann.

Geschäftslogik, die Datenbankverwendung, Zugriff auf das Netzwerk und andere häufig verwendete Funktionen können einmal geschrieben und wieder auf jeder Plattform, die gleichzeitig eine Grundlage für plattformspezifische Benutzeroberflächen, die suchen, und führen Sie als systemeigene Anwendungen verwendet werden.

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>Verfügbarkeit von Integrated Development Environment, (IDE)

Xamarin-Entwicklung in Visual Studio auf dem Mac oder Windows möglich. Die IDE gewählten der Plattformen bestimmt wird, die Sie abzielen möchten.

Da Windows-apps können nur unter Windows zur Erstellung für iOS, entwickelt werden Android, _und_ fordert Windows von Visual Studio für Windows. Es ist jedoch möglich, freigeben, Projekte und Dateien zwischen Windows- und Mac-Computern, damit iOS und Android-apps auf einem Mac erstellt werden können und die freigegebener Code später zu einem Windows-Projekt hinzugefügt werden kann.

Die Anforderungen an die Entwicklungsumgebung für jede Plattform werden ausführlicher in den [Anforderung](~/cross-platform/get-started/requirements.md) Handbuch.


<a name="iOS" />

### <a name="ios"></a>iOS

Entwickeln von Anwendungen für iOS erfordert einen Mac-Computer, MacOS ausgeführt. Sie können auch Visual Studio verwenden, schreiben und Bereitstellen von iOS-Anwendungen mit Xamarin in Visual Studio. Ein Mac ist jedoch weiterhin für den Build und lizenzzwecke erforderlich.

Apple Xcode IDE muss installiert sein, um den Compiler und den Simulator zum Testen angeben. Sie können testen, auf Ihren eigenen Geräten [kostenlos](~/ios/get-started/installation/device-provisioning/free-provisioning.md), sondern zum Erstellen von Anwendungen für die Verteilung (z. b. die App-Store), müssen Sie Apple Developer Program (99 US-Dollar pro Jahr) verknüpfen. Jedes Mal, die Sie übermitteln oder aktualisieren eine Anwendung, muss es überprüft und genehmigt werden von Apple bevor es zum download zur Verfügung steht.

Schreiben von Code mit der Visual Studio-IDE und Bildschirmlayouts als Programm eingebaut oder mit Xamarin iOS-Designer in beiden IDEs bearbeitet werden können.

Finden Sie in der [Xamarin.iOS Installationshandbuch](~/ios/get-started/installation/index.md) detaillierte Anweisungen zum einrichten.

<a name="Android" />

### <a name="android"></a>Android

Android Anwendungsentwicklung erfordert die Java und Android SDKs installiert werden. Diese enthalten den Compiler, Emulator und andere Tools, die für die Erstellung, Bereitstellung und Tests erforderlich. Java, Google Android-SDK und die Xamarin Tools können alle installiert und ausgeführt werden in den folgenden Konfigurationen:

-  Mac OS X El Capitan und höher (10.11 und höher) mit Visual Studio für Mac
-  Windows 7 und höher mit Visual Studio 2015 oder 2017


Xamarin bietet einen unified Installer löst, der das System mit der erforderlichen Softwarekomponenten Java, Android und Xamarin-Tools (z. B. einen visuellen Designer für Bildschirmlayouts) konfigurieren. Finden Sie in der [Xamarin.Android Installationshandbuch](~/android/get-started/installation/index.md) ausführliche Anweisungen.

Sie können Anwendungen entwickeln und Testen auf einem echten Gerät ohne keinerlei Lizenzrechte von Google, jedoch um Ihre Anwendung über einen Speicher zu verteilen (z. B. Google Play, Amazon oder Barnes &amp; Noble) eine Gebühr für die Registrierung ist möglicherweise auf den Operator. Google Play wird Ihre Anwendung sofort, veröffentlichen, während die andere Niederlassungen einen Genehmigungsprozess Apple ähnelt aufweisen.

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

Windows-apps (WinForms, WPF oder universelle Windows-Plattform) werden mit Visual Studio erstellt. Sie verwenden nicht direkt Xamarin. Allerdings kann C#-Code für Windows-, IOS- und Android freigegeben werden.
Besuchen Sie Microsofts [Dev Center](https://developer.microsoft.com/) Informationen zu den Tools für Windows-Entwicklung erforderlich sind.

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>Erstellen der Benutzeroberfläche (UI)

Ein wesentlicher Vorteil der Verwendung von Xamarin ist, dass die Benutzeroberfläche der Anwendung verwendet die systemeigene Steuerelementen für jede Plattform, die beim Erstellen von apps, die nicht von einer Anwendung, die in Objective-C oder Java geschrieben sind (für IOS- und Android bzw.).

Beim Erstellen von Bildschirmen in Ihrer app können Sie das Layout der Steuerelemente im Code oder vollständige Bildschirme, die mithilfe der Entwurfstools verfügbar für jede Plattform zu erstellen.

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>Erstellen Sie Steuerelemente programmgesteuert

Jede Plattform ermöglicht es Benutzer Steuerelemente der Benutzeroberfläche eines Bildschirms mithilfe von Code hinzugefügt werden. Dies kann sehr zeitaufwändig sein, wie es den fertigen Entwurf Darstellung schwierig sein kann, wenn Hardcodieren Pixel Koordinaten für die Positionen und Größen.

Programmgesteuertes Erstellen von Steuerelementen muss Vorteile jedoch insbesondere auf iOS zum Erstellen von Sichten, die die Größe oder über die iPhone und iPad Bildschirmgrößen unterschiedlich dargestellt.

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>Visueller Designer

Jede Plattform gibt es eine andere Methode für das visuelle Layout Bildschirme:

-   **iOS** – die Xamarin iOS Designer erleichtert das zum Erstellen von Sichten, die mithilfe von Drag & Drop-Funktionalität und die Eigenschaft Felder. Zusammenfassend diese Sichten bilden ein Storyboard und zugegriffen werden kann, der **. Storyboard** -Datei, die in Ihrem Projekt enthalten ist.
-   **Android** – Xamarin bietet einen Android Drag-and-Drop-UI-Designer für Visual Studio. Android Bildschirmlayouts gespeichert sind, als **. AXML** Dateien, wenn Sie Xamarin-Tools verwenden.
-   **Windows** – Microsoft stellt einen Drag-and-Drop-UI-Designer in Visual Studio und Blend. Der Bildschirmlayouts werden als gespeichert. Verwendung von XAML-Dateien.


Diese Screenshots zeigen die visual Bildschirm-Designer verfügbar, auf jeder Plattform:

 [ ![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "Diese Screenshots zeigen die visual Bildschirm-Designer verfügbar auf jeder Plattform")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

In allen Fällen können die Elemente, die Sie visuell erstellen im Code verwiesen werden.

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>Überlegungen zur Benutzeroberfläche

Ein wichtiger Vorteil der Verwendung von Xamarin zum Erstellen von plattformübergreifenden Anwendungen ist, dass sie systemeigenen UI-Toolkits an eine vertraute Oberfläche für dem Benutzer nutzen können. Die Benutzeroberfläche wird außerdem so schnell wie jede andere systemeigene Anwendung ausgeführt.

Einige UI Metaphern auf mehreren Plattformen funktionieren (z. B. für alle drei Plattformen verwenden ein ähnliches Durchführen eines Bildlaufs Strukturelement-Steuerelement) in der Reihenfolge für die Anwendung "rechten festlegt" die Benutzeroberfläche sollte profitieren aber Clientplattform-spezifische Benutzer bei der entsprechenden Elemente der Benutzeroberfläche. Clientplattform-spezifische UI Metaphern zählen:

-   **iOS** – hierarchische Navigation mit weichen Schaltfläche "zurück", Registerkarten am unteren Rand des Bildschirms.
-   **Android** – Hardware/Systemsoftware Schaltfläche, die im Aktionsmenü, die Registerkarten am oberen Bildschirmrand zurück.
-   **Windows** – Windows-apps können auf Desktops, Tablet-PCs (z. B. Microsoft Surface-) und -Smartphones ausgeführt. Windows 10-Geräte möglicherweise die Hardware-Schaltfläche und live-Kacheln, z. B. sichern.


Es wird empfohlen, die relevant für die Plattformen Richtlinien zum Entwerfen Ihrer Zielgruppe zu lesen:

-   **iOS** – [Apple Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** – [Google Richtlinien zur Benutzeroberfläche](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** – [User Experience Entwurfsrichtlinien für Windows](https://developer.microsoft.com/en-us/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>Bibliothek- und Wiederverwendung von Code

Die Xamarin-Plattform ermöglicht erneuten Verwendung eines vorhandenen C#-Code für alle Plattformen sowie die Integration von Bibliotheken, die für jede Plattform systemintern geschrieben.

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>C#-Quellcode und Bibliotheken

Da Xamarin Produkte c# und .NET Framework verwenden, können viele des vorhandenen Quellcodes (beide offen Quell- und interne Projekte) in Xamarin.iOS und Xamarin.Android Projekte erneut verwendet werden. Oft die Quelle einfach zu einer Xamarin-Projektmappe hinzugefügt werden kann, und kann sofort verwendet werden kann. Wenn eine nicht unterstützte .NET Framework-Funktion verwendet wurde, möglicherweise einiger kleiner Anpassungen erforderlich.

Beispiele für C#-Quellcode, die in Xamarin.iOS oder Xamarin.Android verwendet werden können: SQLite-NET "," NewtonSoft.JSON "und" SharpZipLib.

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Bindungen für Objective-C + Bindung-Projekte

Xamarin bietet ein Tool namens *Btouch* , erleichtert das Bindungen erstellen, mit denen Objective-C-Bibliotheken in Xamarin.iOS-Projekten verwendet werden können. Finden Sie in der [Objective-C-Typen binden Dokumentation](~/cross-platform/macios/binding/binding-types-reference.md) ausführliche Informationen wie dies funktioniert.

Beispiele für Objective-C-Bibliotheken, die in Xamarin.iOS verwendet werden können: RedLaser Barcode scannen, Google Analytics und PayPal-Integration. Open-Source-Xamarin.iOS-Bindungen stehen [Github](https://github.com/mono/monotouch-bindings).

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>JAR-Bindungen + Bindung-Projekte

Xamarin unterstützt die Verwendung von vorhandener Java-Bibliotheken in Xamarin.Android. Finden Sie in der [binden eine Bibliothek für Java-Dokumentation](~/android/platform/binding-java-library/index.md) Weitere Informationen zur Verwendung von ein. Die JAR-Datei aus Xamarin.Android.

Open-Source-Xamarin.Android-Bindungen stehen unter [Github](https://github.com/mono/monodroid-bindings).

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>C über PInvoke

"Platform Invoke"-Technologie (P/Invoke) ermöglicht es verwaltetem Code (c#), rufen Methoden in systemeigene Bibliotheken als auch Unterstützung für systemeigene Bibliotheken aufrufen, wieder in verwaltetem Code.

Z. B. die [SQLite-NET](https://github.com/praeclarum/sqlite-net) Library verwendeten Anweisungen wie folgt:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

Bindet an die native Implementierung der Programmiersprache C SQLite in iOS und Android.
Mit einer vorhandenen C# API vertraute Entwickler können eine Reihe von C#-Klassen zum Zuordnen der systemeigenen API und nutzen vorhandene Plattformcode erstellen. Es stellt Dokumentation für [systemeigene Bibliotheken verknüpfen](~/ios/platform/native-interop.md) in Xamarin.iOS, ähnliche Prinzipien, die auf Xamarin.Android angewendet.

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>C++ über CppSharp

Miguel erläutert CXXI (jetzt [CppSharp](https://github.com/mono/CppSharp)) über seine [Blog](http://tirania.org/blog/archive/2011/Dec-19.html). Eine Alternative zur Bindung an eine C++-Bibliothek, die direkt ist einen C#-Wrapper erstellen und Binden an, die über P/Invoke.

