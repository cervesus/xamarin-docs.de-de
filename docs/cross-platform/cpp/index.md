---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Verwenden von C/C++-Bibliotheken mit Xamarin
description: Visual Studio für Mac kann zum Erstellen und Integrieren von plattformübergreifenden C/C++-Code in mobilen apps für Android und iOS mit Xamarin und C#. In diesem Artikel erläutert das Einrichten und das Debuggen ein C++-Projekt in einer Xamarin-app.
author: mikeparker104
ms.author: miparker
ms.date: 12/17/2018
ms.openlocfilehash: a235a24d544e938d4bf29e6569564aface2f6972
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275578"
---
# <a name="use-cc-libraries-with-xamarin"></a>Verwenden von C/C++-Bibliotheken mit Xamarin

## <a name="overview"></a>Übersicht

Xamarin ermöglicht Entwicklern das Erstellen von plattformübergreifenden nativen mobilen apps mit Visual Studio. Im allgemeinen C# Bindungen werden verwendet, um vorhandene Plattformkomponenten für Entwickler verfügbar zu machen. Es gibt jedoch noch Mal, wenn Xamarin-apps benötigen zum Arbeiten mit vorhandenen Codebasen. Manchmal Teams ist einfach nicht genug der Zeit, Budgets oder Ressourcen an Port einen großen, gut getestete und hochgradig optimierte Codebasis zu C#.

[Visual C++ für plattformübergreifende mobile Entwicklung](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) aktiviert die C/C++- und C# Code als Teil der gleichen Projektmappe, und bietet viele Vorteile, die eine einheitliche Debugleistung einschließlich erstellt werden sollen. Auf diese Weise zum Bereitstellen von apps wie z. B. wurde die Microsoft C/C++- und Xamarin verwendet [Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) und [PIX-Kamera](https://www.microsoft.com/microsoftpix).

In einigen Fällen ist gibt es jedoch ein Wunsch (oder Anforderung) zu vorhandenen C-/C++-Tools und Prozesse, die an Ort und zu den Bibliothekscode, der von der Anwendung, die die Bibliothek behandeln, als wäre er einer Drittanbieter-Komponente ähnlich entkoppelt. In diesen Fällen die Herausforderung ist nicht nur Verfügbarmachen der relevanten Member für C# jedoch Verwalten der Bibliothek als Abhängigkeit. Und natürlich als Großteil dieses Prozesses wie möglich automatisieren.  

Dieser Beitrag wird beschrieben, einen allgemeinen Ansatz für dieses Szenario und stellt exemplarisch ein einfaches Beispiel.

## <a name="background"></a>Hintergrund

C/C++-gilt eine plattformübergreifende-Sprache, aber mit großer Sorgfalt erforderlich, um sicherzustellen, dass der Quellcode in der Tat plattformübergreifende Verwendung von alle Ziel-Compiler unterstützt werden und enthält, die nur wenig oder keine bedingt enthaltenen Plattform nur C/C++ ist oder Compiler-spezifischen Code.

Letztendlich muss der Code kompilieren und auf allen Zielplattformen, die daher auf die Gemeinsamkeit abgezielt wird Plattform-(und Compiler) läuft dies, erfolgreich ausgeführt. Probleme können durch geringfügige Unterschiede zwischen Compiler immer noch auftreten, und so gründlichen Tests (vorzugsweise automatisiert) für jedes Ziel Plattform wird immer wichtiger.  

## <a name="high-level-approach"></a>Allgemeine Ansatz

In der folgenden Abbildung stellt dar, den vier Phasen Ansatz zum Transformieren von C/C++-Quellcode in einer plattformübergreifenden Xamarin-Bibliothek, die über NuGet freigegeben, und klicken Sie dann in einer Xamarin.Forms-app genutzt wird.
 

![Allgemeine Ansatz für die Verwendung von C/C++-mit Xamarin](images/cpp-steps.jpg)

Die 4 Phasen sind:

1.  Kompilieren den C-/C++-Quellcode in plattformspezifische, systemeigene Bibliotheken.
2.  Umschließen die nativen Bibliotheken mit Visual Studio-Projektmappe.
3.  Packen und die Übertragung von NuGet-Paket für den Wrapper für .NET.
4.  Nutzen das NuGet-Paket aus einer Xamarin-app.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>Phase 1: Kompilieren den C-/C++-Quellcode in plattformspezifische, systemeigene Bibliotheken

Das Ziel dieser Phase ist die Erstellung native Bibliotheken, die aufgerufen werden können, indem die C# Wrapper. Dies kann oder möglicherweise nicht relevant, je nach Situation. Viele Tools und Prozesse, mit die, die bei diesem häufigen Szenario sollten wiederhergestellt werden können, sind über den Rahmen dieses Artikels hinaus. Wichtige Überlegungen werden beibehalten, die die C/C++-Codebasis synchron mit dem nativen Wrappercode, genug Komponententests aus, und Buildautomatisierung. 

Die Bibliotheken in der exemplarischen Vorgehensweise erstellt wurden, verwenden Visual Studio Code mit einer begleitenden Shell-Skript. Eine erweiterte Version des in dieser exemplarischen Vorgehensweise finden Sie in der [Mobile CAT-GitHub-Repository](https://github.com/xamarin/mobcat/blob/dev/samples/cppwithxamarin/README.md) , die diesen Teil des Beispiels detaillierter erläutert. Die nativen Bibliotheken werden in diesem Fall als eine Drittanbieter Abhängigkeit behandelt wird, jedoch für den Kontext dieser Phase dargestellt wird.


Der Einfachheit halber ist die exemplarische Vorgehensweise nur eine Teilmenge der Architekturen ausgerichtet. Für iOS verwendet das Hilfsprogramm Lipo verwendet, um eine einzelne Fat von den einzelnen architekturspezifischen Binärdateien binäre erstellen. Android verwendet dynamische Binärdateien mit der Erweiterung so ein, und iOS wird eine statische Fat binary verwenden, mit der Erweiterung a. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>Phase 2: Umschließen die nativen Bibliotheken mit Visual Studio-Projektmappe

Die nächste Phase werden die nativen Bibliotheken zu wrappen, sodass sie problemlos in .NET verwendet werden. Dies erfolgt mit Visual Studio-Projektmappe mit vier Projekten. Ein freigegebenes Projekt enthält die gemeinsamen Code. Projekte für jede Xamarin.Android, Xamarin.iOS und .NET Standard können die Bibliothek in eine plattformunabhängige Weise verwiesen werden.

Der Wrapper verwendet[der Trick lockvogel](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/), "von Paul Betts beschrieben. Dies ist nicht die einzige Möglichkeit, aber es erleichtert Ihnen die Bibliothek verweisenden, und es verhindert, dass plattformspezifische Implementierungen in der verarbeitenden Anwendung selbst explizit verwaltet werden müssen. Der Trick dabei im Grunde sicherstellen, dass die Ziele (.NET Standard und Android, iOS), die denselben Namespace, Assemblyname und Struktur der Klasse freigeben. Da NuGet immer eine plattformspezifischen Bibliothek bevorzugt wird, wird die Version von .NET Standard nie zur Laufzeit verwendet.

Die meisten Aufgaben in diesem Schritt konzentriert sich auf P/Invoke verwenden, um die native bibliotheksmethoden aufzurufen, und verwalten die Verweise auf die zugrunde liegenden Objekte. Das Ziel ist, die Bibliotheksfunktionen an den Consumer verfügbar zu machen, bei der Abstraktion von beliebiger Komplexität. Die Xamarin.Forms-Entwickler müssen nicht über ausreichende Kenntnisse für die interne Funktionsweise der nicht verwaltete Bibliothek verfügen. Sollte Ihnen alles wie jede andere verwaltete Verwendung C# Bibliothek.

Die Ausgabe dieser Phase ist letztlich eine Reihe von Bibliotheken für .NET, eine pro-Ziel, zusammen mit einer Nuspec-Dokument mit den Informationen erforderlich, um das Paket im nächsten Schritt erstellen.

**Phase 3: Packen und die Übertragung eines NuGet-Pakets für den Wrapper für .NET**

Die dritte Phase wird ein NuGet-Paket mit dem Build-Artefakte aus dem vorherigen Schritt erstellt. Das Ergebnis aus diesem Schritt wird ein NuGet-Paket, das aus einer Xamarin-app genutzt werden kann. Die exemplarische Vorgehensweise verwendet ein lokales Verzeichnis als NuGet-feed dienen. In der Produktion dieser Schritt sollte Veröffentlichen eines Pakets auf einen öffentlichen oder privaten NuGet-feed und vollständig automatisiert werden soll.

**Schritt 4: Nutzen das NuGet-Paket aus einer Xamarin.Forms-app**

Der letzte Schritt ist zum Verweisen auf und verwenden Sie das NuGet-Paket aus einer Xamarin.Forms-app. Dies erfordert, dass das Konfigurieren des NuGet-feed in Visual Studio, um den Feed, der im vorherigen Schritt definierten verwenden.

Sobald der Feed konfiguriert ist, muss das Paket aus dem Projekt in der plattformübergreifenden Xamarin.Forms-app verwiesen werden. "Der Bait-and-Switch-Trick" bietet identische Schnittstellen an, sodass mithilfe von Code in einem zentralen Ort definiert die Funktionalität der nativen Bibliothek aufgerufen werden kann.

Der Quellcode-Repository enthält eine [Liste weitere](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up) , enthält der Artikel zum Einrichten eines privaten NuGet-feed auf Azure DevOps und wie Sie das Paket auf diesem Feed mithilfe von Push übertragen. Erforderlich ist ein wenig Setup länger als ein lokales Verzeichnis aus, ist dieser Feeds in einer teamentwicklungsumgebung besser.

## <a name="walk-through"></a>Exemplarische Vorgehensweise

Die Schritte gelten für **Visual Studio für Mac**, aber die Struktur funktioniert in **Visual Studio 2017** ebenfalls.

### <a name="prerequisites"></a>Vorraussetzungen

Um folgen zu können, muss der Entwickler:

-   [NuGet-Befehlszeile (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

-   [*Visual Studio* *für Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> Ein aktives [ **Apple-Entwicklerkonto** ](https://developer.apple.com/) ist erforderlich, um apps auf einem iPhone bereitstellen.

## <a name="creating-the-native-libraries-stage-1"></a>Erstellen die nativen Bibliotheken (Stufe 1)

Die Funktionalität der nativen Bibliothek basiert auf dem Beispiel aus [Exemplarische Vorgehensweise: Erstellen und Verwenden einer statischen Bibliothek (C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017).

In dieser exemplarischen Vorgehensweise überspringt die erste Phase erstellen die nativen Bibliotheken, da die Bibliothek als Drittanbieter-Abhängigkeit in diesem Szenario bereitgestellt wird. Die vorkompilierten nativen Bibliotheken befinden sich zusammen mit den [Beispielcode](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin) oder [heruntergeladen](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) direkt.

### <a name="working-with-the-native-library"></a>Arbeiten mit der nativen Bibliothek

Die ursprüngliche *MathFuncsLib* Beispiel enthält eine einzelne Klasse, die Namen "MyMathFuncs" mit folgender Definition: 

```cpp
namespace MathFuncs
{
    class MyMathFuncs
    {
    public:
        double Add(double a, double b);
        double Subtract(double a, double b);
        double Multiply(double a, double b);
        double Divide(double a, double b);
    };
}
```

Eine zusätzliche Klasse definiert die Wrapperfunktionen, mit die einen Consumer .NET erstellen, löschen und die zugrunde liegende systemeigene "MyMathFuncs"-Klasse interagieren können.

```cpp
#include "MyMathFuncs.h"
using namespace MathFuncs;

extern "C" {
    MyMathFuncs* CreateMyMathFuncsClass();
    void DisposeMyMathFuncsClass(MyMathFuncs* ptr);
    double MyMathFuncsAdd(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsSubtract(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsMultiply(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsDivide(MyMathFuncs *ptr, double a, double b);
}
```

Sie werden diese Wrapperfunktionen, die verwendet werden die [Xamarin](https://visualstudio.microsoft.com/xamarin/) Seite.

## <a name="wrapping-the-native-library-stage-2"></a>Umschließen die native Bibliothek (Stufe 2)

Diese Phase erfordert die [vorkompilierte Bibliotheken](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) beschrieben, die der [vorherigen Abschnitt](https://docs.microsoft.com/xamarin/cross-platform/cpp/index).

### <a name="creating-the-visual-studio-solution"></a>Erstellen von Visual Studio-Projektmappe

1. In **Visual Studio für Mac**, klicken Sie auf **neues Projekt** (aus der *Seite "Willkommen"*) oder **neue Projektmappe** (aus der *Datei* Menü).
2. Von der **neues Projekt** Fenster wählen **freigegebenes Projekt** (aus *Multi-Plattform > Bibliothek*), und klicken Sie dann auf **Weiter**.
3. Aktualisieren Sie die folgenden Felder aus, und klicken Sie auf **erstellen**:

    - **Projektname:** MathFuncs.Shared  
    - **Projektmappenname:** MathFuncs  
    - **Speicherort:** Verwenden Sie den standardmäßigen Speicherort (oder wählen Sie eine Alternative)   
    - **Erstellen Sie ein Projekt im Projektmappenverzeichnis ein:** Legen Sie diese überprüft
4. Von **Projektmappen-Explorer**, doppelklicken Sie auf die **MathFuncs.Shared** Projekt, und navigieren Sie zu **Main Einstellungen**.
5. Entfernen Sie **. Freigegebene** aus der **Default Namespace** festgelegt wird **MathFuncs** , klicken Sie dann auf **OK**.
6. Open **MyClass.cs** (erstellt von der Vorlage), benennen Sie die Klassen und den Dateinamen, **MyMathFuncsWrapper** , und ändern Sie den Namespace **MathFuncs**.
7. **Strg + Klick** auf die Projektmappe **MathFuncs**, wählen Sie dann **neues Projekt hinzufügen...**  aus der **hinzufügen** Menü.
8. Aus der **neues Projekt** Fenster wählen **.NET Standardbibliothek** (aus *Multi-Plattform > Bibliothek*), und klicken Sie dann auf **Weiter**.
9. Wählen Sie **.NET Standard 2.0** , und klicken Sie dann auf **Weiter**.
10. Aktualisieren Sie die folgenden Felder aus, und klicken Sie auf **erstellen**:

    - **Projektname:** MathFuncs.Standard  
    - **Speicherort:** Verwenden Sie den gleichen Speicherort wie das freigegebene Projekt   

11. Von **Projektmappen-Explorer**, doppelklicken Sie auf die **MathFuncs.Standard** Projekt.
12. Navigieren Sie zu **Main Einstellungen**, aktualisieren Sie dann **Default Namespace** zu **MathFuncs**.
13. Navigieren Sie zu der **Ausgabe** Aktualisieren der Einstellungen, klicken Sie dann **Assemblyname** zu **MathFuncs**.
14. Navigieren Sie zu der **Compiler** -Einstellungen, die **Konfiguration** zu **Version**wird durch das Festlegen **Debuginformationen** zu  **Nur Symbole** klicken Sie dann auf **OK**.
15. Löschen Sie **Class1.cs/Getting Schritte** aus dem Projekt (falls eine davon als Teil der Vorlage enthalten war).
16. **Strg + Klick** auf das Projekt **Abhängigkeiten/References** Ordner, wählen Sie dann **Verweise bearbeiten**.
17. Wählen Sie **MathFuncs.Shared** aus der **Projekte** Registerkarte, und klicken Sie dann **OK**.
18. Wiederholen Sie die Schritte 7-17 (wird ignoriert. Schritt 9) mithilfe der folgenden Konfigurationen:

    | **PROJEKTNAME**  | **VORLAGENNAME**   | **MENÜ "NEUES PROJEKT"**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | Klassenbibliothek       | Android > Bibliothek      |
    | MathFuncs.iOS     | Bindungsbibliothek     | iOS > Bibliothek          |

19. Von **Projektmappen-Explorer**, doppelklicken Sie auf die **MathFuncs.Android** Projekt, und navigieren Sie zu der **Compiler** Einstellungen.

20. Mit der **Konfiguration** festgelegt **Debuggen**, bearbeiten **Definieren von Symbolen** sollen **Android;**.

21. Ändern der **Konfiguration** zu **Version**, bearbeiten Sie dann **Definieren von Symbolen** auch **Android;**.

22. Wiederholen Sie die Schritte für die 19 – 20 **MathFuncs.iOS**, Bearbeitung **Definieren von Symbolen** sollen **iOS;** anstelle von **Android;** in beiden Fällen.

23. Erstellen Sie die Projektmappe **Version** Configuration (**STRG + -Befehl + B**) und sicherzustellen, dass alle drei Ausgabeassemblys (Android, iOS, .NET Standard) (in die Bin-Ordner des jeweiligen Projekt) die denselben Namen **MathFuncs.dll**.

In dieser Phase sollte die Lösung haben drei Ziele, die eine bestätigungsphase für Android, iOS und .NET Standard und ein freigegebenes Projekt, das von jedem der drei Ziele verwiesen wird. Diese sollten konfiguriert werden, um den gleichen Namespace und die Ausgabe Standardassemblys mit dem gleichen Namen zu verwenden. Dies ist erforderlich, für den zuvor erwähnten "lockvogel"-Ansatz.

### <a name="adding-the-native-libraries"></a>Die nativen Bibliotheken hinzufügen

Beim Hinzufügen der nativen Bibliotheken der Wrapper-Projektmappe variiert leicht zwischen Android und iOS.

#### <a name="native-references-for-mathfuncsandroid"></a>Native Verweise für MathFuncs.Android

1. **STRG + Klicken Sie auf** auf die **MathFuncs.Android** Projekt, und wählen Sie dann **neuer Ordner** aus der **hinzufügen** nennen dieses Menü **Libs**.

2. Für jede **ABI** (Application Binary Interface), **STRG + Klicken Sie auf** auf die **Libs** Ordner, wählen Sie dann **neuer Ordner** aus der  **Hinzufügen** im Menü, und nennen Sie es anschließend entsprechenden **ABI**. In diesem Fall gilt Folgendes:

    - arm64 v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > Eine detailliertere Übersicht finden Sie unter den [Architekturen und CPUs](https://developer.android.com/ndk/guides/arch) Thema aus der [NDK-Entwicklerhandbuch](https://developer.android.com/ndk/guides/), insbesondere den Abschnitt für die Adressierung [nativen Code in app-Pakete](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages) .

3. Überprüfen Sie die Ordnerstruktur:  

    ```
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. Hinzufügen der entsprechenden **so** Bibliotheken auf jedes der **ABI** Ordner anhand der folgenden Zuordnung:

    **arm64-v8a:** Bibliotheken/Android/arm64

    **Armeabi-v7a:** Bibliotheken/Android/Arm  

    **x86:** libs/Android/x86

    **x86_64:** libs/Android/x86_64

    > [!NOTE]
    > Beim Hinzufügen von Dateien, **Strg + Klick** für den Ordner, der die entsprechenden darstellt **ABI**, wählen Sie dann **Dateien hinzufügen...**  aus der **hinzufügen** Menü. Wählen Sie die entsprechende Bibliothek (aus der **PrecompiledLibs** Verzeichnis) klicken Sie dann auf **öffnen** , und klicken Sie dann auf **OK** verlassen die Standardoption zur *Kopie der Datei in das Verzeichnis*.

5. Für jede der **so** Dateien **STRG + Klicken Sie auf** wählen Sie dann die **EmbeddedNativeLibrary** option die **Buildvorgang** im Menü.

Jetzt die **Libs** Ordner sollte wie folgt aussehen:

```bash
- lib
    - arm64-v8a
        - libMathFuncs.so
    - armeabi-v7a
        - libMathFuncs.so
    - x86 
        - libMathFuncs.so
    - x86_64 
        - libMathFuncs.so
```

#### <a name="native-references-for-mathfuncsios"></a>Native Verweise für MathFuncs.iOS

1. **Strg + Klick** auf die **MathFuncs.iOS** Projekt, und wählen Sie dann **nativen Verweis hinzufügen** aus der **hinzufügen** im Menü. 
2. Wählen Sie die **libMathFuncs.a** Bibliothek (Bibliotheken/IOS unter der **PrecompiledLibs** Verzeichnis) klicken Sie dann auf **öffnen** 
3. **STRG + Klicken Sie auf** auf die **LibMathFuncs** Datei (innerhalb der **Native Verweise** Ordner, wählen Sie dann die **Eigenschaften** Option im Menü  
4. Konfigurieren der **nativen Verweis** Eigenschaften, damit sie aktiviert sind (ein Tick-Symbol angezeigt) in der **Eigenschaften** Pad:
        
    - Laden erzwingen
    - Is C++
    - Intelligenten Link 

    > [!NOTE]
    > Mit einer Bindung-Bibliothek-Projekttyp zusammen mit einem [nativen Verweis](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) bettet die statische Bibliothek und aktiviert ihn automatisch mit der Xamarin.iOS-app verknüpft werden, die darauf verweist (auch wenn es über NuGet-Paket enthalten ist). 

5. Open **ApiDefinition.cs**, löschen, die auf Vorlagen basierenden kommentierten Code (sondern nur die **MathFuncs** Namespace), führen Sie dann auf den gleichen Schritt für **Structs.cs** 

    > [!NOTE]
    > Ein Binding-Steuerelementbibliothek-Projekt benötigt diese Dateien (mit der *ObjCBindingApiDefinition* und *ObjCBindingCoreSource* Buildvorgänge) zum Erstellen. Schreiben wir jedoch den Code, um unsere native Bibliothek, außerhalb dieser Dateien auf eine Weise aufzurufen, die zwischen Android und iOS-Bibliothek-Ziele, die mit standardmäßigen P/Invoke freigegeben werden kann.

### <a name="writing-the-managed-library-code"></a>Das Schreiben des Codes der verwalteten Bibliothek

Schreiben Sie jetzt die C# Code aufrufen, die native Bibliothek. Ziel ist es, alle zugrunde liegenden Komplexität zu verbergen. Der Consumer sollte Kenntnisse zum Innenleben der nativen Bibliothek oder P/Invoke-Konzepte nicht erforderlich.  

#### <a name="creating-a-safehandle"></a>Erstellen ein SafeHandle

1. **Strg + Klick** auf die **MathFuncs.Shared** Projekt, und wählen Sie dann **-Datei hinzufügen...**  aus der **hinzufügen** Menü. 
2. Wählen Sie **leere Klasse** aus der **neue Datei** Fenster, nennen Sie sie **MyMathFuncsSafeHandle** , und klicken Sie dann auf **neu**
3. Implementieren der **MyMathFuncsSafeHandle** Klasse:

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => this.handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > Ein [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) ist die bevorzugte Methode zum Arbeiten mit nicht verwalteten Ressourcen in verwaltetem Code. Dies abstrahiert eine Menge Standardcode, die im Zusammenhang mit der kritische Finalisierung und Objekt-Lebenszyklus. Der Besitzer dieses Handle kann anschließend behandelt wie jede andere Ressource verwaltet und muss nicht die vollständige Implementierung [Dispose-Muster](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose). 

#### <a name="creating-the-internal-wrapper-class"></a>Interne Wrapperklasse erstellen

1. Open **MyMathFuncsWrapper.cs**, ändern ihn in eine interne, statische Klasse

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. Fügen Sie in der gleichen Datei die folgende Auswertung der Anweisung zur Klasse hinzu:

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > Hiermit wird die **DllName** Grundlage, dass konstanter Wert gibt an, ob die Bibliothek für erstellt wird **Android** oder **iOS**. So wird beheben die verschiedenen Benennungskonventionen, die von jeder entsprechenden Plattform, sondern auch den Typ der Bibliothek, die verwendet wird, in diesem Fall verwendet. Android verwendet eine dynamische Bibliothek und daher erwartet, dass ein Dateiname einschließlich Erweiterung. Für iOS "*__Internal*" ist erforderlich, da wir eine statische Bibliothek verwenden.

3. Hinzufügen eines Verweises auf **System.Runtime.InteropServices** am oberen Rand der **MyMathFuncsWrapper.cs** Datei

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. Fügen Sie die Wrappermethoden zum Behandeln von die Erstellung und Löschung von der **"MyMathFuncs"** Klasse:

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > In unserer Konstante übergeben werden **DllName** auf die **DllImport** Attribut zusammen mit den **EntryPoint** wodurch explizit angewiesen wird, der .NET Runtime den Namen der aufzurufenden Funktion innerhalb dieser Bibliothek. Technisch gesehen, wir müssen nicht zu den **EntryPoint** -Wert, wenn die Namen für die verwaltete Methode identisch mit dem nicht verwalteten wurden. Wenn eine nicht angegeben wird, würde der Name der verwalteten Methode verwendet werden, als die **EntryPoint** stattdessen. Allerdings ist es besser, die explizit sein.  

5. Fügen Sie die Wrappermethoden, um uns ermöglichen, arbeiten mit der **"MyMathFuncs"** Klasse mit dem folgenden Code:

    ```csharp
    [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
    internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
    internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
    internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
    internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
    ```

    > [!NOTE]
    > Für die Parameter in diesem Beispiel verwenden wir einfache Typen. Da marshalling eine bitweise Kopie ist erfordert in diesem Fall es ohne zusätzliche Arbeit Ihrerseits. Beachten Sie auch die Verwendung der **MyMathFuncsSafeHandle** Klasse anstelle des standardmäßigen **IntPtr**. Die **IntPtr** automatisch zugeordnet ist die **SafeHandle** als Teil des Prozesses marshalling.

6. Überprüfen Sie, ob die fertige **MyMathFuncsWrapper** -Klasse angezeigt wird, wie unten:

    ```csharp
    using System.Runtime.InteropServices;

    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
            #if Android
                const string DllName = "libMathFuncs.so";
            #else
                const string DllName = "__Internal";
            #endif

            [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
            internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

            [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
            internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);

            [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
            internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
            internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
            internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
            internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
        }
    }
    ```

#### <a name="completing-the-mymathfuncssafehandle-class"></a>Abschließen der MyMathFuncsSafeHandle-Klasse
1. Öffnen der **MyMathFuncsSafeHandle** Klasse, navigieren Sie zu den Platzhalter **TODO** kommentieren Sie in der **ReleaseHandle** Methode:
    ```csharp
    // TODO: Release the handle here
    ```
2. Ersetzen Sie die **TODO** Zeile:

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>Schreiben Sie die MyMathFuncs-Klasse

Nun, da der Wrapper abgeschlossen ist, erstellen Sie eine "MyMathFuncs"-Klasse, die den Verweis auf das nicht verwaltete C++ "MyMathFuncs"-Objekt verwaltet werden sollen.  

1. **Strg + Klick** auf die **MathFuncs.Shared** Projekt, und wählen Sie dann **-Datei hinzufügen...**  aus der **hinzufügen** Menü. 
2. Wählen Sie **leere Klasse** aus der **neue Datei** Fenster, nennen Sie sie **"MyMathFuncs"** , und klicken Sie dann auf **neu**
3. Fügen Sie die folgenden Elemente der **"MyMathFuncs"** Klasse:

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. Den Konstruktor für die Klasse zu implementieren, damit erstellt und ein Handle für das systemeigene speichert **"MyMathFuncs"** Objekt, wenn die Klasse instanziiert wird:

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. Implementieren der **"IDisposable"** Schnittstelle mit dem folgenden Code:

    ```csharp
    public class MyMathFuncs : IDisposable
    {
        ...

        protected virtual void Dispose(bool disposing)
        {
            if (handle != null && !handle.IsInvalid)
                handle.Dispose();
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        // ...
    }
    ```

6. Implementieren der **"MyMathFuncs"** Methoden mithilfe der **MyMathFuncsWrapper** Klasse, um die eigentliche Arbeit im Hintergrund auszuführen, übergeben wir haben gespeicherten Zeigers auf den zugrunde liegenden, nicht verwaltete Objekt. Der Code sollte wie folgt lauten:

    ```csharp
    public double Add(double a, double b)
    {
        return MyMathFuncsWrapper.Add(handle, a, b);
    }

    public double Subtract(double a, double b)
    {
        return MyMathFuncsWrapper.Subtract(handle, a, b);
    }

    public double Multiply(double a, double b)
    {
        return MyMathFuncsWrapper.Multiply(handle, a, b);
    }

    public double Divide(double a, double b)
    {
        return MyMathFuncsWrapper.Divide(handle, a, b);
    }
    ```

#### <a name="creating-the-nuspec"></a>Erstellen der NuSpec-Datei

Um die Bibliothek über NuGet verteilt, die Lösung muss eine **Nuspec** Datei. Dies wird identifiziert, die von der resultierenden Assemblys für jede unterstützte Plattform eingeschlossen werden.

1.  **Strg + Klick** auf die Projektmappe **MathFuncs**, wählen Sie dann **Projektmappenordner hinzufügen** aus der **hinzufügen** nennen dieses Menü **SolutionItems**.
2.  **Strg + Klick** auf die **SolutionItems** Ordner, wählen Sie dann **neue Datei...**  aus der **hinzufügen** Menü.
3.  Wählen Sie **leere XML-Datei** aus der **neue Datei** Fenster, nennen Sie sie **MathFuncs.nuspec** , und klicken Sie dann auf **neu**.
4.  Update **MathFuncs.nuspec** mit den grundlegenden Paketmetadaten, angezeigt werden, die **NuGet** Consumer. Zum Beispiel:


    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>
            <id>MathFuncs</id>
            <version>$version$</version>
            <authors>Microsoft Mobile Customer Advisory Team</authors>
            <description>Sample C++ Wrapper Library</description>
            <requireLicenseAcceptance>false</requireLicenseAcceptance>
            <copyright>Copyright 2018</copyright>
        </metadata>
    </package>
    ```

    > [!NOTE]
    >  Finden Sie unter den [Nuspec-Referenz](https://docs.microsoft.com/nuget/reference/nuspec) Dokument Weitere Details zu das Schema für dieses Manifest verwendet.

5. Hinzufügen einer `<files>` Element als untergeordnetes Element der `<package>` Element (direkt unterhalb `<metadata>`), das jeder Datei mit einem separaten `<file>` Element:

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->        

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > Wenn ein Paket in einem Projekt installiert ist und es sind mehrere Assemblys mit demselben Namen angegeben, wählt NuGet effektiv die Assembly, die am häufigsten für die angegebene Plattform spezifisch ist.

6. Hinzufügen der `<file>` Elemente für die **Android** Assemblys:

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. Hinzufügen der `<file>` Elemente für die **iOS** Assemblys:

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. Hinzufügen der `<file>` Elemente für die **netstandard2.0** Assemblys:

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. Überprüfen Sie die **Nuspec** manifest:

    ```xml
    <?xml version="1.0"?>
    <package>
    <metadata>
        <id>MathFuncs</id>
        <version>$version$</version>
        <authors>Microsoft Mobile Customer Advisory Team</authors>
        <description>Sample C++ Wrapper Library</description>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <copyright>Copyright 2018</copyright>
    </metadata>
    <files>
    
        <!-- Android -->
        <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
        <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
        
        <!-- iOS -->
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
        
        <!-- netstandard2.0 -->
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />

    </files>
    </package>
    ```

    > [!NOTE]
    > Diese Datei gibt die Verweisassemblypfade für die Ausgabe aus einem **Version** "erstellen", daher werden Sie sicher, dass die Projektmappe, die mit dieser Konfiguration zu erstellen.

An diesem Punkt die Projektmappe enthält, 3 .NET-Assemblys und ein unterstützender **Nuspec** manifest.

## <a name="distributing-the-net-wrapper-with-nuget"></a>Verteilen von .NET Wrappers mit NuGet

Der nächste Schritt besteht Verpacken und verteilen das NuGet-Paket aus, damit sie problemlos von der app verbraucht und als Abhängigkeit verwaltet werden kann. Die Einbindung und Nutzung können alle erfolgen innerhalb einer einzigen Lösung, aber Verteilen der Bibliothek über NuGet unterstützt die Internetsuche Entkopplung und ermöglicht es uns, das Verwalten dieser Codebasen unabhängig voneinander.

### <a name="preparing-a-local-packages-directory"></a>Vorbereiten eines Verzeichnisses für die lokale Pakete

Die einfachste Form eines NuGet-feed handelt es sich um ein lokales Verzeichnis:

1.  In **Finder**, navigieren Sie zu einem geeigneten Verzeichnis. Z. B. **/Users**.
2.  Wählen Sie **neuer Ordner** aus der **Datei** Menü, z. B. einen aussagekräftigen Namen anzugeben **lokale-Nuget-Feed**.

### <a name="creating-the-package"></a>Erstellen des Pakets

1.  Legen Sie die **Buildkonfiguration** zu **Version**, und führen Sie einen Build mit **Befehl + B**.
2.  Open **Terminal** und wechseln Sie zum Ordner mit den **Nuspec** Datei.
3.  In **Terminal**, führen Sie die **Nuget Pack** -Befehl unter Angabe der **NuSpec-Datei** -Datei, die **Version** (z. B. 1.0.0), und die  **OutputDirectory** mit dem Ordner erstellt haben, der [zuvor](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed), d. h. **lokale-Nuget-Feed**. Zum Beispiel:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **Vergewissern Sie sich** , **MathFuncs.1.0.0.nupkg** wurde der **lokale-Nuget-Feed** Verzeichnis.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[OPTIONAL] Mit einer privaten NuGet-feed mit Azure DevOps

Eine stabilere Technik wird in beschrieben [beginnen Sie mit NuGet-Pakete in Azure DevOps](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package), erfahren, wie Sie einen privaten Feed erstellen und pushen das Paket (im vorherigen Schritt generiert) auf diesem Feed.

Dies ist ideal, damit dieser Workflow vollständig automatisiert werden, z. B. mit [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts). Weitere Informationen finden Sie unter [erste Schritte mit Azure-Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts).

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Verwenden des .NET-Wrappers von einer Xamarin.Forms-app
Zum Abschließen dieser exemplarischen Vorgehensweise erstellen Sie eine **Xamarin.Forms** -app für das Paket nur veröffentlicht werden, für die lokale **NuGet** feed.

### <a name="creating-the-xamarinforms-project"></a>Erstellen der **Xamarin.Forms** Projekt

1. Öffnen Sie eine neue Instanz der **Visual Studio für Mac**. Dies kann erfolgen über **Terminal**:

    ```bash
    open -n -a "Visual Studio"
    ```

2. In **Visual Studio für Mac**, klicken Sie auf **neues Projekt** (aus der *Seite "Willkommen"*) oder **neue Projektmappe** (aus der *Datei* Menü).
3. Von der **neues Projekt** Fenster wählen **leere Forms-App** (aus *Multi-Plattform > App*), und klicken Sie dann auf **Weiter**.
4. Aktualisieren Sie die folgenden Felder aus, und klicken Sie auf **Weiter**:

    - **App-Name:** MathFuncsApp.
    - **Organisations-ID:** Verwenden Sie einen reverse-Namespace, z. B. _com {Your_org}_.
    - **Zielplattformen:** Verwenden Sie die Standardeinstellung (Android und iOS-Ziele).
    - **Freigegebener Code:** Legen Sie diese auf .NET Standard (eine "Freigegebene Bibliothek"-Lösung ist möglich, allerdings würde den Rahmen dieser exemplarischen Vorgehensweise).

5. Aktualisieren Sie die folgenden Felder aus, und klicken Sie auf **erstellen**:

    - **Projektname:** MathFuncsApp.
    - **Projektmappenname:** MathFuncsApp.  
    - **Speicherort:** Verwenden Sie den standardmäßigen Speicherort (oder wählen Sie eine Alternative).

6. In **Projektmappen-Explorer**, **Strg + Klick** auf dem Ziel (**MathFuncsApp.Android** oder **MathFuncs.iOS**) für die anfänglichen Tests, klicken Sie dann Wählen Sie **als Startprojekt festlegen**.
7. Wählen Sie die bevorzugte **Gerät** oder **Simulator**/**Emulator**. 
8. Führen Sie die Projektmappe (**Befehl + RETURN**) zu überprüfen, ob die auf Vorlagen basierenden **Xamarin.Forms** Projekt erstellt und ausführt, kein Problem. 

    > [!NOTE]
    > **iOS** (insbesondere den Simulator) tendenziell Zeit nach dem dockerlauf Build/Bereitstellung.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>Hinzufügen des lokale NuGet-feed an den NuGet-Konfiguration

1. In **Visual Studio**, wählen Sie **Voreinstellungen** (aus der **Visual Studio** Menü).
2. Wählen Sie **Quellen** unter der **NuGet** Abschnitt, und klicken Sie dann **hinzufügen**.
3. Aktualisieren Sie die folgenden Felder aus, und klicken Sie auf **Quelle hinzufügen**:

    - **Name:** Geben Sie einen aussagekräftigen Namen, z. B. Lokale Pakete.  
    - **Speicherort:** Geben Sie die **lokale-Nuget-Feed** Ordner erstellt die [zuvor](#preparing-a-local-packages-directory).

    > [!NOTE]
    > In diesem Fall besteht keine Notwendigkeit zum Angeben einer **Benutzername** und **Kennwort**. 

4. Klicken Sie auf **OK**.

### <a name="referencing-the-package"></a>Verweisen auf das Paket

Wiederholen Sie die folgenden Schritte für jedes Projekt (**MathFuncsApp**, **MathFuncsApp.Android**, und **MathFuncsApp.iOS**).

1. **Strg + Klick** auf das Projekt, und wählen Sie dann **NuGet-Pakete hinzufügen...**  aus der **hinzufügen** Menü.
2. Suchen Sie nach **MathFuncs**. 
3. Überprüfen Sie die **Version** des Pakets wird **1.0.0** und die andere Details angezeigt werden, wie z. B. erwartet die **Titel** und **Beschreibung**, d. h. , *MathFuncs* und *Beispiel für C++-Wrapperbibliothek*. 
4. Wählen Sie die **MathFuncs** -Paket, und klicken Sie dann **Paket hinzufügen**.

### <a name="using-the-library-functions"></a>Verwenden die Bibliotheksfunktionen

Jetzt mit einem Verweis auf die **MathFuncs** -Paket in jedes der Projekte, die Funktionen sind verfügbar, um die C# Code.

1.  Öffnen **"MainPage.Xaml.cs"** innerhalb der **MathFuncsApp** allgemeine **Xamarin.Forms**Projekt (auf die verwiesen wird durch die beiden **MathFuncsApp.Android**und **MathFuncsApp.iOS**).
2.  Hinzufügen **mit** -Anweisungen für **System.Diagnostics** und **MathFuncs** am Anfang der Datei:

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. Deklarieren einer Instanz der `MyMathFuncs` Klasse am oberen Rand der `MainPage` Klasse:

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. Überschreiben der `OnAppearing` und `OnDisappearing` Methoden aus der `ContentPage` Basisklasse:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
    }
    ```

5. Update der `OnAppearing` Methode zum Initialisieren der `myMathFuncs` zuvor deklarierte Variable:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. Update der `OnDisappearing` aufzurufende Methode der `Dispose` Methode `myMathFuncs`:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. Implementieren Sie eine private Methode namens **TestMathFuncs** wie folgt:

    ```csharp
    private void TestMathFuncs()
    {
        var numberA = 1;
        var numberB = 2;

        // Test Add function
        var addResult = myMathFuncs.Add(numberA, numberB);

        // Test Subtract function
        var subtractResult = myMathFuncs.Subtract(numberA, numberB);

        // Test Multiply function
        var multiplyResult = myMathFuncs.Multiply(numberA, numberB);

        // Test Divide function
        var divideResult = myMathFuncs.Divide(numberA, numberB);

        // Output results
        Debug.WriteLine($"{numberA} + {numberB} = {addResult}");
        Debug.WriteLine($"{numberA} - {numberB} = {subtractResult}");
        Debug.WriteLine($"{numberA} * {numberB} = {multiplyResult}");
        Debug.WriteLine($"{numberA} / {numberB} = {divideResult}");
    }
    ```

8. Rufen Sie zum Schluss `TestMathFuncs` am Ende der `OnAppearing` Methode:

    ```csharp
    TestMathFuncs();
    ```

9. Führen Sie die app für jede Zielplattform, und überprüfen Sie die Ausgabe in die **Anwendungsausgabe** Pad sieht wie folgt:

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Wenn beim Auftreten einer "*DLLNotFoundException*" beim Testen unter Android, oder ein Buildfehler auf iOS-, sollten Sie unbedingt, dass die CPU-Architektur des/Emulator/gerätesimulators Sie verwenden mit der Teilmenge kompatibel ist, die wir ausgewählt haben unterstützt. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie eine Xamarin.Forms-app zu erstellen, die native Bibliotheken durch einen allgemeinen .NET-Wrapper, die über ein NuGet-Paket verteilt verwendet wird. Das Beispiel, das in dieser exemplarischen Vorgehensweise ist bewusst sehr einfach gehalten, um den Ansatz einfacher zu veranschaulichen. Eine reale Anwendung müssen für den Umgang mit Komplexität wie z. B. die Behandlung von Ausnahmen, Rückrufe, komplexere Typen marshallen und Verknüpfen mit anderen Bibliotheken abhängig. Eine wichtige Überlegung ist der Prozess, bei dem die Entwicklung von der C++-Code koordiniert und mit den Wrapper und Clientanwendungen synchronisiert wird. Dieser Prozess kann variieren, je nachdem, ob von diese Probleme eine oder beide ein einzelnes Team dafür verantwortlich sind. In beiden Fällen ist die Automatisierung ein echter Vorteil. Im folgenden finden einige Ressourcen bereitgestellt, um einige der wichtigsten Konzepte zusammen mit den relevanten Downloads Weitere Informationen. 

### <a name="downloads"></a>Downloads

- [NuGet-Befehlszeilentools (CLI)-tools](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>Beispiele

- [Hyperlapse plattformübergreifende mobile Entwicklung mit C++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft PIX-(C++- und Xamarin)](https://blog.xamarin.com/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles Beispiel Port](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)

### <a name="further-reading"></a>Weiterführende Themen

[Artikel in Bezug auf den Inhalt dieses Beitrags](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
