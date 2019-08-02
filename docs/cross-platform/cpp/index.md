---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Verwenden von CC++ /Bibliotheken mit xamarin
description: Visual Studio für Mac können zum Erstellen und integrieren von Platt Form übergreifendem CC++ /Code in Mobile Apps für Android und IOS mithilfe von xamarin C#und verwendet werden. In diesem Artikel wird erläutert, wie Sie ein C++ Projekt in einer xamarin-App einrichten und Debuggen.
author: mikeparker104
ms.author: miparker
ms.date: 12/17/2018
ms.openlocfilehash: a6c5e172fa9fe41e210f332d351adc307d0c7df3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648191"
---
# <a name="use-cc-libraries-with-xamarin"></a>Verwenden von CC++ /Bibliotheken mit xamarin

## <a name="overview"></a>Übersicht

Xamarin ermöglicht Entwicklern das Erstellen von plattformübergreifenden, nativen mobilen apps mit Visual Studio. Im allgemeinen C# werden Bindungen verwendet, um vorhandene Platt Form Komponenten für Entwickler verfügbar zu machen. Es gibt jedoch Zeiten, in denen xamarin-apps mit vorhandenen CodeBases arbeiten müssen. Manchmal haben Teams nicht die Zeit, das Budget oder die Ressourcen, um eine große, gut getestete und hochgradig optimierte Codebasis zu C#portieren.

[Visual C++ für die plattformübergreifende Mobile Entwicklung](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) ermöglicht das ErstellenC++ von C# C/und Code als Teil der gleichen Lösung und bietet zahlreiche Vorteile, darunter eine einheitliche debuggingdarstellung. Microsoft hat C/C++ und xamarin auf diese Weise verwendet, um apps wie [hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) und [pix Kamera](https://www.microsoft.com/microsoftpix)bereitzustellen.

In einigen Fällen gibt es jedoch einen Wunsch (oder eine Anforderung), vorhandene C/C++ Tools und Prozesse beizubehalten und den Bibliotheks Code von der Anwendung entkoppeln zu lassen, und die Bibliothek so zu behandeln, als wäre sie mit einer Drittanbieter Komponente vergleichbar. In diesen Fällen besteht die Herausforderung nicht nur darin, die relevanten Member C# verfügbar zu machen, sondern die Bibliothek als Abhängigkeit zu verwalten. Und natürlich können Sie so viel von diesem Prozess wie möglich automatisieren.  

In diesem Beitrag wird ein allgemeiner Ansatz für dieses Szenario erläutert und ein einfaches Beispiel erläutert.

## <a name="background"></a>Hintergrund

C/C++ wird als plattformübergreifende Sprache betrachtet, aber es muss sehr sorgfältig vorgegangen werden, um sicherzustellen, dass der Quellcode tatsächlich plattformübergreifend ist, wobeiC++ nur c/unterstützt von allen Ziel Compilern verwendet wird, die wenig oder keine bedingt enthaltene Plattform enthalten. compilerspezifischer Code.

Letztendlich muss der Code auf allen Zielplattformen kompiliert und erfolgreich ausgeführt werden. Daher ist dies die Gemeinsamkeit der Plattformen (und Compiler), die als Zielplattform fungieren. Probleme können weiterhin von geringfügigen Unterschieden zwischen den Compilern auftreten, sodass gründliche Tests (vorzugsweise automatisiert) auf den einzelnen Zielplattformen immer wichtiger werden.  

## <a name="high-level-approach"></a>Allgemeiner Ansatz

Die folgende Abbildung stellt den vierstufigen Ansatz dar, der zum Transformieren vonC++ C/Quellcode in eine plattformübergreifende xamarin-Bibliothek verwendet wird, die über nuget freigegeben und dann in einer xamarin. Forms-App verwendet wird.
 
![Allgemeiner Ansatz für die Verwendung von C/C++ mit xamarin](images/cpp-steps.jpg)

Die vier Phasen lauten:

1. Kompilieren des C/C++ Quellcode-Codes in plattformspezifische systemeigene Bibliotheken
2. Umwickeln der nativen Bibliotheken mit einer Visual Studio-Projekt Mappe.
3. Packen und pushen eines nuget-Pakets für den .net-Wrapper.
4. Verwenden des nuget-Pakets aus einer xamarin-app.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>Phase 1: Kompilieren des C/C++ Quellcode-Codes in plattformspezifische Native Bibliotheken

Das Ziel dieser Phase besteht darin, Native Bibliotheken zu erstellen, die vom C# Wrapper aufgerufen werden können. Dies kann je nach Situation relevant sein oder nicht. Die zahlreichen Tools und Prozesse, die für dieses gängige Szenario durchgeführt werden können, gehen über den Rahmen dieses Artikels hinaus. Wichtige Überlegungen: das Synchronisieren vonC++ C/Codebasis mit nativem Wrapper Code, ausreichenden Komponententests und Buildautomatisierung. 

Die Bibliotheken in der exemplarischen Vorgehensweise wurden mit Visual Studio Code mit einem begleitenden Shellskript erstellt. Eine erweiterte Version dieser exemplarischen Vorgehensweise finden Sie im [Mobile Cat GitHub-Repository](https://github.com/xamarin/mobcat/blob/dev/samples/cpp_with_xamarin/) , in dem dieser Teil des Beispiels ausführlicher erläutert wird. Die nativen Bibliotheken werden in diesem Fall als Drittanbieter-Abhängigkeit behandelt. diese Phase wird jedoch für den Kontext veranschaulicht.

Der Einfachheit halber ist die exemplarische Vorgehensweise nur eine Teilmenge der Architekturen. Für IOS wird das Dienstprogramm Lipo verwendet, um eine einzelne FAT-Binärdatei aus den einzelnen architekturspezifischen Binärdateien zu erstellen. Android verwendet dynamische Binärdateien mit einem. so verwendet Extension und IOS eine statische FAT-Binärdatei mit der Erweiterung. a. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>Phase 2: Umwickeln der nativen Bibliotheken mit einer Visual Studio-Projekt Mappe

Die nächste Phase besteht darin, die systemeigenen Bibliotheken so zu wrappen, dass Sie problemlos von .NET verwendet werden können. Dies erfolgt mit einer Visual Studio-Projekt Mappe mit vier Projekten. Ein frei gegebenes Projekt enthält den allgemeinen Code. Für Projekte, die auf die einzelnen xamarin. Android-, xamarin. IOS-und .NET Standard abzielen, kann die Bibliothek plattformunabhängig referenziert werden.

Der Wrapper verwendet den "[Köder und Switch"-Trick](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/), der von Paul Betts beschrieben wird. Dies ist nicht die einzige Möglichkeit, aber es vereinfacht den Verweis auf die Bibliothek, und es wird vermieden, dass plattformspezifische Implementierungen in der verarbeitenden Anwendung selbst explizit verwaltet werden müssen. Der Trick ist im Grunde sicherzustellen, dass die Ziele (.NET Standard, Android, IOS) denselben Namespace, denselben Assemblynamen und dieselbe Klassenstruktur verwenden. Da nuget immer eine plattformspezifische Bibliothek bevorzugt, wird die .NET Standard-Version niemals zur Laufzeit verwendet.

Der größte Teil der Arbeit in diesem Schritt konzentriert sich auf die Verwendung von P/aufrufen, um die nativen Bibliotheks Methoden aufzurufen und die Verweise auf die zugrunde liegenden Objekte zu verwalten. Das Ziel besteht darin, die Funktionalität der Bibliothek für den Consumer verfügbar zu machen und dabei jede beliebige Komplexität zu abstrahieren. Die xamarin. Forms-Entwickler müssen nicht über Kenntnisse in der inneren Funktionsweise der nicht verwalteten Bibliothek verfügen. Es sollte so aussehen, als ob Sie eine beliebige C# andere verwaltete Bibliothek verwenden.

Letztendlich handelt es sich bei der Ausgabe dieser Phase um einen Satz von .NET-Bibliotheken, eine pro Ziel, zusammen mit einem nuspec-Dokument, das die erforderlichen Informationen zum Erstellen des Pakets im nächsten Schritt enthält.

**Phase 3: Packen und pushen eines nuget-Pakets für den .net-Wrapper**

Die dritte Phase ist das Erstellen eines nuget-Pakets mithilfe der Build-Artefakte aus dem vorherigen Schritt. Das Ergebnis dieses Schritts ist ein nuget-Paket, das in einer xamarin-App verwendet werden kann. In der exemplarischen Vorgehensweise wird ein lokales Verzeichnis verwendet, das als nuget-Feed fungiert. In der Produktion sollte dieser Schritt ein Paket in einem öffentlichen oder privaten nuget-Feed veröffentlichen und vollständig automatisiert werden.

**Phase 4: Verwenden des nuget-Pakets aus einer xamarin. Forms-App**

Der letzte Schritt besteht darin, auf das nuget-Paket aus einer xamarin. Forms-APP zu verweisen und dieses zu verwenden. Hierfür müssen Sie den nuget-Feed in Visual Studio so konfigurieren, dass er den im vorherigen Schritt definierten Feed verwendet.

Nachdem der Feed konfiguriert wurde, muss auf das Paket von jedem Projekt in der plattformübergreifenden xamarin. Forms-App verwiesen werden. "Der Köder-und-Switch-Trick" stellt identische Schnittstellen bereit, sodass die native Bibliotheks Funktionalität mithilfe von Code aufgerufen werden kann, der an einer einzigen Stelle definiert ist.

Das Quellcoderepository enthält eine [Liste weiterer](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up) Informationen, die Artikel zum Einrichten eines privaten nuget-Feeds in Azure devops und zum Übertragen des Pakets per Push an diesen Feed enthält. Diese Art von Feed ist in einer Team Entwicklungsumgebung besser, wenn Sie ein wenig mehr Einrichtungszeit benötigen als ein lokales Verzeichnis.

## <a name="walk-through"></a>Exemplarische Vorgehensweise

Die angegebenen Schritte gelten speziell für **Visual Studio für Mac**, aber die Struktur funktioniert auch in **Visual Studio 2017** .

### <a name="prerequisites"></a>Vorraussetzungen

Um die folgenden Schritte durchführen zu können, benötigt der Entwickler Folgendes:

- [Nuget-Befehlszeile (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Visual Studio* *für Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> Zum Bereitstellen von apps auf einem iPhone ist ein aktives [**Apple-Entwicklerkonto**](https://developer.apple.com/) erforderlich.

## <a name="creating-the-native-libraries-stage-1"></a>Erstellen der nativen Bibliotheken (Phase 1)

Die native Bibliotheks Funktionalität basiert auf dem Beispiel aus [exemplarischen Vorgehensweise: Erstellen und Verwenden einer statischen Bibliothek (C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017).

In dieser exemplarischen Vorgehensweise wird die erste Phase überspringt, und die nativen Bibliotheken werden aufgebaut, da die Bibliothek in diesem Szenario als Drittanbieter-Abhängigkeit bereitgestellt wird. Die vorkompilierten systemeigenen Bibliotheken sind neben dem [Beispielcode](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin) enthalten oder können direkt [heruntergeladen](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) werden.

### <a name="working-with-the-native-library"></a>Arbeiten mit der nativen Bibliothek

Das ursprüngliche *MathFuncsLib* -Beispiel enthält eine einzelne Klasse `MyMathFuncs` mit dem Namen mit der folgenden Definition:

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

Eine zusätzliche Klasse definiert Wrapper Funktionen, die es einem .net-Consumer ermöglichen, die zugrunde liegende Native `MyMathFuncs` Klasse zu erstellen, zu löschen und mit ihr zu interagieren.

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

Dabei handelt es sich um Wrapper Funktionen, die auf der [xamarin](https://visualstudio.microsoft.com/xamarin/) -Seite verwendet werden.

## <a name="wrapping-the-native-library-stage-2"></a>Umwickeln der nativen Bibliothek (Phase 2)

Diese Phase erfordert die [vorkompilierten Bibliotheken](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) , die im [vorherigen Abschnitt](##creating-the-native-libraries-stage-1)beschrieben wurden.

### <a name="creating-the-visual-studio-solution"></a>Erstellen der Visual Studio-Projekt Mappe

1. Klicken Sie in **Visual Studio für Mac**auf **Neues Projekt** (auf der *Willkommensseite*) oder auf **neue** Projekt Mappe (im Menü *Datei* ).
2. Wählen Sie im Fenster **Neues Projekt** die Option frei gegebenes **Projekt** aus (aus *> Bibliothek für mehrere Plattformen*), und klicken Sie dann auf **weiter**.
3. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    - **Projekt Name:** MathFuncs.Shared  
    - **Projektmappenname:** Mathfuncs  
    - **Hotels** Standard Speicherort verwenden (oder Alternative auswählen)   
    - **Erstellen Sie ein Projekt im Projektmappenverzeichnis:** Diese Option auf aktiviert festlegen
4. Doppelklicken Sie in **Projektmappen-Explorer**auf das Projekt **mathfuncs. Shared** , und navigieren Sie zu **Main settings**.
5. Entfernen Sie **.** Wird vom **Standard Namespace** freigegeben, sodass nur **mathfuncs** festgelegt ist, und klicken Sie dann auf **OK**.
6. Öffnen Sie **MyClass.cs** (von der Vorlage erstellt), benennen Sie die Klasse und den Dateinamen in **mymathfunctaurapper** um, und ändern Sie den Namespace in **mathfuncs**.
7. **Klicken Sie** mit der Maustaste auf die **projektmappenmathfuncs**und dann im Menü **Hinzufügen** auf **Neues Projekt hinzufügen...** .
8. Wählen Sie im Fenster **Neues Projekt** **.NET Standard Bibliothek** aus (aus *> Bibliothek für mehrere Plattformen*), und klicken Sie dann auf **weiter**.
9. Wählen Sie **.NET Standard 2,0** aus, und klicken Sie auf **weiter**.
10. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    - **Projekt Name:** MathFuncs.Standard  
    - **Hotels** Denselben Speicherort wie das freigegebene Projekt verwenden   

11. Doppelklicken Sie in **Projektmappen-Explorer**auf das Projekt **mathfuncs. Standard** .
12. Navigieren Sie zu **Main settings**, und aktualisieren Sie dann den **Standard Namespace** auf **mathfuncs**.
13. Navigieren Sie zu den **Ausgabe** Einstellungen, und aktualisieren Sie den Assemblynamen in **mathfuncs**.
14. Navigieren Sie zu den Compilereinstellungen, ändern Sie die **Konfiguration** in **Release**, legen Sie **Debuginformationen** auf **Symbole** fest, und klicken Sie auf **OK**.
15. DELETE **Class1.cs/Getting** wurde aus dem Projekt gestartet (sofern eines von Ihnen als Teil der Vorlage enthalten ist).
16. **Klicken Sie** auf den Ordner "Projekt **Abhängigkeiten/Verweise** ", und klicken Sie dann auf **Verweise bearbeiten**.
17. Wählen Sie **mathfuncs. Shared** auf der Registerkarte **Projekte** aus, und klicken Sie dann auf **OK**.
18. Wiederholen Sie die Schritte 7-17 (ohne Schritt 9), indem Sie die folgenden Konfigurationen verwenden:

    | **PROJEKT NAME**  | **VORLAGEN NAME**   | **MENÜ "NEUES PROJEKT"**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | Klassenbibliothek       | Android-> Bibliothek      |
    | MathFuncs.iOS     | Bindungs Bibliothek     | IOS-> Bibliothek          |

19. Doppelklicken Sie in **Projektmappen-Explorer**auf das Projekt **mathfuncs. Android** , und navigieren Sie dann zu den Compilereinstellungen.

20. Wenn Sie die **Konfiguration** auf **Debuggen**festgelegt haben, bearbeiten Sie **Symbole** zum Einschließen von **Android**.

21. Ändern Sie die **Konfiguration** in **Release**, und bearbeiten Sie dann **Symbole definieren** , um auch **Android**zu enthalten.

22. Wiederholen Sie die Schritte 19-20 für **mathfuncs. IOS**, und bearbeiten Sie die **Symbole** , die **IOS** enthalten, anstelle von **Android** . in beiden Fällen.

23. Erstellen Sie die Projekt Mappe in der **Releasekonfiguration** (**Control + Command + B**), und überprüfen Sie, ob alle drei Ausgabeassemblys (Android, Ios, .NET Standard) (in den entsprechenden Project bin-Ordnern) denselben Namen haben wie **mathfuncs. dll**.

Zu diesem Zeitpunkt sollte die Lösung über drei Ziele verfügen, eine benötigten mindestargumenten für Android, IOS und .NET Standard und ein frei gegebenes Projekt, auf das von jedem der drei Ziele verwiesen wird. Diese sollten so konfiguriert werden, dass Sie dieselben Standard Namespace-und Ausgabeassemblys mit demselben Namen verwenden. Dies ist für den zuvor erwähnten Ansatz "Köder und Switch" erforderlich.

### <a name="adding-the-native-libraries"></a>Hinzufügen der nativen Bibliotheken

Der Prozess des Hinzufügens der nativen Bibliotheken zur Wrapper Lösung variiert geringfügig zwischen Android und IOS.

#### <a name="native-references-for-mathfuncsandroid"></a>Native Verweise für mathfuncs. Android

1. **Klicken** Sie auf das Projekt **mathfuncs. Android** , und klicken Sie dann im Menü **Hinzufügen** auf **neuer Ordner** , **um ihn zu**benennen.

2. **Klicken** Sie für jede **ABI** (Anwendungs Binärschnittstelle) auf den Ordner " **lisb** ", und wählen Sie dann im Menü **Hinzufügen** die Option **neuer Ordner** aus, und benennen Sie ihn nach der jeweiligen **ABI**. In diesem Fall gilt Folgendes:

    - arm64 v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > Eine ausführlichere Übersicht finden Sie im Thema zu [Architekturen und CPUs](https://developer.android.com/ndk/guides/arch) im [NDK-Entwicklerhandbuch](https://developer.android.com/ndk/guides/), insbesondere im Abschnitt über die Adressierung von System eigenem [Code in App-Paketen](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages).

3. Überprüfen Sie die Ordnerstruktur:  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. Fügen Sie die entsprechenden **. so** -Bibliotheken jedem der **ABI** -Ordner basierend auf der folgenden Zuordnung hinzu:

    **arm64-V8A:** lisb/Android/arm64

    **ARMEABI-v7a:** lisb/Android/Arm  

    **x86:** lisb/Android/x86

    **x86_64:** lisb/Android/x86_64

    > [!NOTE]
    > Um Dateien hinzuzufügen, **Klicken Sie** auf den Ordner, der die jeweilige **ABI**darstellt, und wählen Sie dann im Menü **Hinzufügen** die Option **Dateien hinzufügen...** aus. Wählen Sie die entsprechende Bibliothek aus (im Verzeichnis " **precompiledlisb** "), klicken Sie auf **Öffnen** , und klicken Sie dann auf **OK** , um die *Datei in das Verzeichnis zu kopieren*.

5. Wählen Sie für jede der **. so** -Dateien **Control + click** aus, und wählen Sie dann im Menü **Build Action** die Option **embeddednativelibrary** aus.

Der Ordner " **lisb** " sollte nun wie folgt aussehen:

```folders
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

#### <a name="native-references-for-mathfuncsios"></a>Native Verweise für mathfuncs. IOS

1. **Klicken Sie** mit der Maustaste auf das Projekt **mathfuncs. IOS** , und wählen Sie im Menü **Hinzufügen** die Option systemeigene **Verweis hinzufügen** aus. 
2. Wählen Sie die Bibliothek **libmathfuncs. a** aus (von lisb/IOS unter dem Verzeichnis **precompiledlisb** ), und klicken Sie dann auf **Öffnen** . 
3. **Klicken Sie** auf die Datei **libmathfuncs** (im Ordner **native Verweise** ), und klicken Sie dann im Menü auf die Option **Eigenschaften** .  
4. Konfigurieren Sie die systemeigenen **Verweis** Eigenschaften, sodass Sie im eigenschaftenpad aktiviert sind (ein Tick-Symbol wird angezeigt):

    - Laden erzwingen
    - RichtetC++
    - Intelligenter Link

    > [!NOTE]
    > Die Verwendung eines Bindungs Bibliotheksprojekt Typs zusammen mit einem [nativen Verweis](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) bettet die statische Bibliothek ein und ermöglicht die automatische Verknüpfung mit der xamarin. IOS-APP, die darauf verweist (auch wenn Sie über ein nuget-Paket eingeschlossen wird).

5. Öffnen Sie **ApiDefinition.cs**, löschen Sie den auf Vorlagen basierenden kommentierten Code `MathFuncs` (nur den Namespace), und führen Sie dann den gleichen Schritt für **structs.cs** aus. 

    > [!NOTE]
    > Ein Bindungs Bibliotheksprojekt erfordert diese Dateien (mit den Build-Aktionen " *objcbindingapidefinition* " und " *objcbindingcoresource* "), um zu erstellen. Wir schreiben jedoch den Code, um unsere native Bibliothek außerhalb dieser Dateien auf eine Weise aufzurufen, die von den Zielen von Android-und IOS-Bibliotheken mithilfe von Standard-P/-aufrufen gemeinsam genutzt werden kann.

### <a name="writing-the-managed-library-code"></a>Schreiben des Codes der verwalteten Bibliothek

Schreiben Sie nun den C# Code, um die native Bibliothek aufzurufen. Ziel ist es, jede zugrunde liegende Komplexität auszublenden. Der Consumer sollte keine Kenntnisse in den systeminternen Bibliotheks internen oder den Konzepten von P/aufrufen benötigen.  

#### <a name="creating-a-safehandle"></a>Erstellen eines SafeHandle

1. **Klicken Sie** mit der Maustaste auf das Projekt **mathfuncs. Shared** , und wählen Sie dann im Menü **Hinzufügen** die Option **Datei hinzufügen...** aus. 
2. Wählen Sie im Fenster **neue Datei** die Option **leere Klasse** aus, benennen Sie Sie **mymathfuncssafehandle** , und klicken Sie dann auf **neu** .
3. Implementieren Sie die **mymathfuncssafehandle** -Klasse:

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
    > Ein [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) ist die bevorzugte Methode, um mit nicht verwalteten Ressourcen in verwaltetem Code zu arbeiten. Dadurch wird eine Menge von Code Bausteinen entfernt, der sich auf die kritische Beendigung und den Lebenszyklus von Objekten bezieht. Der Besitzer dieses Handles kann ihn anschließend wie jede andere verwaltete Ressource behandeln und muss nicht das vollständige verwerfbare [Muster](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)implementieren. 

#### <a name="creating-the-internal-wrapper-class"></a>Erstellen der internen Wrapper Klasse

1. Öffnen Sie **MyMathFuncsWrapper.cs**, und ändern Sie es in eine interne statische Klasse.

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. Fügen Sie der-Klasse in der gleichen Datei die folgende Bedingungs Anweisung hinzu:

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > Dadurch wird der Konstante Wert **dllName** festgelegt, je nachdem, ob die Bibliothek für **Android** oder **IOS**erstellt wird. Dadurch werden die unterschiedlichen Benennungs Konventionen der einzelnen Plattformen, aber auch der in diesem Fall verwendete Bibliothekstyp behandelt. Android verwendet eine dynamische Bibliothek und erwartet daher einen Dateinamen einschließlich der Erweiterung. Für IOS ist " *__Internal*" erforderlich, da eine statische Bibliothek verwendet wird.

3. Fügen Sie einen Verweis auf " **System. Runtime. InteropServices** " am Anfang der Datei " **MyMathFuncsWrapper.cs** " hinzu.

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. Fügen Sie die Wrapper Methoden hinzu, um das Erstellen und Entfernen der **MyMathFuncs** -Klasse zu behandeln:

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > Wir übergeben den Konstanten **dllName** an das **DllImport** -Attribut zusammen mit dem **entryPoint** , der der .NET-Laufzeit explizit den Namen der Funktion angibt, die innerhalb dieser Bibliothek aufgerufen werden soll. Technisch gesehen müssen wir den **entryPoint** -Wert nicht angeben, wenn die Namen der verwalteten Methoden mit der nicht verwalteten Methode identisch sind. Wenn keine Angabe erfolgt, wird der Name der verwalteten Methode stattdessen als **entryPoint** verwendet. Es ist jedoch besser, explizit zu sein.  

5. Fügen Sie die Wrapper Methoden hinzu, damit wir mit der **MyMathFuncs** -Klasse arbeiten können, indem Sie den folgenden Code verwenden:

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
    > In diesem Beispiel verwenden wir einfache Typen für die Parameter. Da das Marshalling eine bitweise Kopie ist, ist in diesem Fall keine zusätzliche Arbeit erforderlich. Beachten Sie auch die Verwendung der **mymathfuncssafehandle** -Klasse anstelle der Standard- **IntPtr**. Der **IntPtr** wird automatisch dem **SafeHandle** im Rahmen des Marshallingprozesses zugeordnet.

6. Vergewissern Sie sich, dass die fertige **mymathfunctaurapper** -Klasse wie folgt aussieht:

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

#### <a name="completing-the-mymathfuncssafehandle-class"></a>Vervollständigen der mymathfuncssafehandle-Klasse

1. Öffnen Sie die **mymathfuncssafehandle** -Klasse, und navigieren Sie zum Platzhalter- **TODO** -Kommentar innerhalb der **ReleaseHandle** -Methode:

    ```csharp
    // TODO: Release the handle here
    ```

1. Ersetzen Sie die **TODO** -Zeile:

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>Schreiben der MyMathFuncs-Klasse

Nachdem der Wrapper nun fertig ist, erstellen Sie eine MyMathFuncs-Klasse, die den Verweis auf das nicht C++ verwaltete MyMathFuncs-Objekt verwaltet.  

1. **Klicken Sie** mit der Maustaste auf das Projekt **mathfuncs. Shared** , und wählen Sie dann im Menü **Hinzufügen** die Option **Datei hinzufügen...** aus. 
2. Wählen Sie im Fenster **neue Datei** die Option **leere Klasse** aus, benennen Sie Sie **MyMathFuncs** , und klicken Sie dann auf **neu** .
3. Fügen Sie der **MyMathFuncs** -Klasse die folgenden Member hinzu:

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. Implementieren Sie den Konstruktor für die-Klasse, sodass ein Handle für das systemeigene **MyMathFuncs** -Objekt erstellt und gespeichert wird, wenn die-Klasse instanziiert wird:

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. Implementieren Sie die iverwerfbare Schnittstelle mithilfe des folgenden Codes:

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

6. Implementieren Sie die **MyMathFuncs** -Methoden mithilfe der **mymathfunctaurapper** -Klasse, um die eigentliche Arbeit im Hintergrund auszuführen, indem Sie den-Zeiger übergeben, den wir in dem zugrunde liegenden nicht verwalteten Objekt gespeichert haben. Der Code sollte wie folgt lauten:

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

#### <a name="creating-the-nuspec"></a>Erstellen der nuspec-Datei

Um die Bibliothek über nuget Verpacken und verteilen zu können, benötigt die Lösung eine **nuspec** -Datei. Dadurch wird bestimmt, welche der resultierenden Assemblys für die einzelnen unterstützten Plattformen eingeschlossen werden.

1. **Klicken** Sie auf die Projekt Mappe " **mathfuncs**", und wählen Sie dann im Menü **Hinzufügen** die Option Projektmappenordner **Hinzufügen** **aus.**
2. **Klicken Sie** mit der Maustaste auf den Ordner **SolutionItems** , und wählen Sie dann im Menü **Hinzufügen** die Option **neue Datei...** aus.
3. Wählen Sie im Fenster **neue Datei** die Option **leere XML-Datei** aus, nennen Sie Sie **mathfuncs. nuspec** , und klicken Sie dann auf **neu**.
4. Aktualisieren Sie **mathfuncs. nuspec** mit den grundlegenden Paket Metadaten, die für den **nuget** -Consumer angezeigt werden sollen. Beispiel:

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
    > Weitere Details zu dem für dieses Manifest verwendeten Schema finden Sie im [nuspec-Referenz](https://docs.microsoft.com/nuget/reference/nuspec) Dokument.

5. Fügen Sie `<files>` ein-Element als untergeordnetes `<package>` Element des-Elements `<metadata>`(direkt unten) hinzu, um jede `<file>` Datei mit einem separaten Element zu identifizieren:

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > Wenn ein Paket in einem Projekt installiert wird und mehrere Assemblys mit demselben Namen angegeben sind, wählt nuget die Assembly aus, die für die jeweilige Plattform am spezifischsten ist.

6. Fügen Sie `<file>` die Elemente für **Android** -Assemblys hinzu:

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. Fügen Sie `<file>` die Elemente der **IOS** -Assemblys hinzu:

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. Fügen Sie `<file>` die Elemente für die **netstandard 2.0** -Assemblys hinzu:

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. Überprüfen Sie das **nuspec** -Manifest:

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
    > Diese Datei gibt die assemblyausgabepfade aus einem **Releasebuild** an. Stellen Sie daher sicher, dass Sie die Lösung mit dieser Konfiguration erstellen.

An diesem Punkt enthält die Lösung drei .NET-Assemblys und ein unterstützendes **nuspec** -Manifest.

## <a name="distributing-the-net-wrapper-with-nuget"></a>Verteilen des .NET-Wrappers mit nuget

Der nächste Schritt besteht darin, das nuget-Paket zu verpacken und zu verteilen, damit es problemlos von der APP genutzt und als Abhängigkeit verwaltet werden kann. Die Umbrüchen und der Verbrauch können alle innerhalb einer einzigen Lösung erfolgen, aber die Verteilung der Bibliothek über nuget unterstützt das entkoppeln und ermöglicht uns, diese Codebasen unabhängig zu verwalten.

### <a name="preparing-a-local-packages-directory"></a>Vorbereiten eines lokalen Paket Verzeichnisses

Die einfachste Form eines nuget-Feeds ist ein lokales Verzeichnis:

1. Navigieren Sie in **Finder**zu einem praktischen Verzeichnis. Beispielsweise **/Users**.
2. Wählen Sie im Menü **Datei** die Option **neuer Ordner** aus, und geben Sie einen aussagekräftigen Namen wie **local-nuget-Feed**an.

### <a name="creating-the-package"></a>Erstellen des Pakets

1. Legen Sie die Buildkonfiguration auf **Release**fest, und führen Sie einen Build mit dem **Befehl + B**aus.
2. Öffnen Sie das **Terminal** , und wechseln Sie in den Ordner, der die **nuspec** -Datei enthält.
3. Führen Sie im **Terminal**den Befehl **nuget Pack** aus, und geben Sie dabei die **nuspec** -Datei, die **Version** (z. b. 1.0.0) und **OutputDirectory** mit dem im [vorherigen Schritt](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed)erstellten Ordner an, **d. h. local-nuget-Feed**. Beispiel:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **Vergewissern** Sie sich, dass **mathfuncs. 1.0.0. nupkg** im Verzeichnis **local-nuget-Feed** erstellt wurde.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>Optionale Verwenden eines privaten nuget-Feeds mit Azure devops

Ein stabileres Verfahren wird in " [Get Started with nuget Packages in Azure devops](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package)" beschrieben, das zeigt, wie ein privater Feed erstellt und das im vorherigen Schritt erstellte Paket per Push in diesen Feed übertragen wird.

Es ist ideal, diesen Workflow vollständig zu automatisieren, z. b. mit [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts). Weitere Informationen finden Sie unter [Get Started with Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts).

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Verwenden des .NET-Wrappers aus einer xamarin. Forms-App

Um die exemplarische Vorgehensweise abzuschließen, erstellen Sie eine **xamarin. Forms** -APP, die das soeben im lokalen **nuget** -Feed veröffentlichte Paket verwendet.

### <a name="creating-the-xamarinforms-project"></a>Erstellen des **xamarin. Forms** -Projekts

1. Öffnen Sie eine neue Instanz von **Visual Studio für Mac**. Dies kann über das **Terminal**erfolgen:

    ```bash
    open -n -a "Visual Studio"
    ```

2. Klicken Sie in **Visual Studio für Mac**auf **Neues Projekt** (auf der *Willkommensseite*) oder auf **neue** Projekt Mappe (im Menü *Datei* ).
3. Wählen Sie im Fenster **Neues Projekt** die Option **leere Formular-App** aus (innerhalb der *Multiplattform-> app*), und klicken Sie dann auf **weiter**.
4. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **weiter**:

    - **App-Name:** Mathfuncsapp.
    - **Organisations-ID:** Verwenden Sie einen Reverse-Namespace, z _. b. com. {your_org}_ .
    - **Zielplattformen:** Verwenden Sie die Standardeinstellung (Android-und IOS-Ziele).
    - **Gemeinsam verwendeter Code:** Legen Sie diese Einstellung auf .NET Standard (eine "freigegebene Bibliothek"-Lösung ist möglich, jedoch über den Rahmen dieser exemplarischen Vorgehensweise hinaus).

5. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    - **Projekt Name:** Mathfuncsapp.
    - **Projektmappenname:** Mathfuncsapp.  
    - **Hotels** Verwenden Sie den Standard Speicherort (oder wählen Sie eine Alternative aus).

6. **Klicken** Sie in **Projektmappen-Explorer**auf das Ziel (**mathfuncsapp. Android** oder **mathfuncs. IOS**), und wählen Sie dann **als Startprojekt festlegen**aus.
7. Wählen Sie den bevorzugten **Geräte** -oder **simulatoremulator**/aus. 
8. Führen Sie die Projekt Mappe aus (**Command + Return**), um zu überprüfen, ob das **xamarin. Forms** -Projekt mit Vorlagen erstellt und ausgeführt wird. 

    > [!NOTE]
    > **IOS** (insbesondere der Simulator) ist tendenziell die schnellste Build/Bereitstellungs Zeit.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>Hinzufügen des lokalen nuget-Feeds zur nuget-Konfiguration

1. Wählen Sie in **Visual Studio**die Option **Einstellungen** (über das **Visual Studio** -Menü) aus.
2. Wählen Sie im **nuget** -Abschnitt **Quellen** aus, und klicken Sie dann auf **Hinzufügen**.
3. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Quelle hinzufügen**:

    - **Name:** Geben Sie einen aussagekräftigen Namen an, z. b. lokale Pakete.  
    - **Hotels** Geben Sie den **lokalen nuget-Feed-** Ordner an, der im [vorherigen Schritt](#preparing-a-local-packages-directory)erstellt wurde.

    > [!NOTE]
    > In diesem Fall ist es nicht erforderlich, einen **Benutzernamen** und ein **Kennwort**anzugeben. 

4. Klicken Sie auf **OK**.

### <a name="referencing-the-package"></a>Verweisen auf das Paket

Wiederholen Sie die folgenden Schritte für jedes Projekt (**mathfuncsapp**, **mathfuncsapp. Android**und **mathfuncsapp. IOS**).

1. **Klicken Sie** mit der Maustaste auf das Projekt, und wählen Sie im Menü **Hinzufügen** die Option **nuget-Pakete hinzufügen...** aus.
2. Suchen Sie nach **mathfuncs**. 
3. Vergewissern Sie sich, dass die **Version** des Pakets **1.0.0** lautet und die anderen Details erwartungsgemäß wie **Titel** und **Beschreibung**, d. h. *mathfuncs* und *Beispiel C++ Wrapper Bibliothek*, angezeigt werden. 
4. Wählen Sie das **mathfuncs** -Paket aus, und klicken Sie auf **Paket hinzufügen**.

### <a name="using-the-library-functions"></a>Verwenden der Bibliotheksfunktionen

Nun, mit einem Verweis auf das **mathfuncs** -Paket in jedem der Projekte, sind die Funktionen für den C# Code verfügbar.

1. Öffnen Sie **MainPage.XAML.cs** aus dem Common **xamarin. Forms**-Projekt **mathfuncsapp** (auf das von **mathfuncsapp. Android** und **mathfuncsapp. IOS**verwiesen wird).
2. Fügen Sie **using** -Anweisungen für " **System. Diagnostics** " und " **mathfuncs** " am Anfang der Datei hinzu:

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. Deklarieren Sie eine Instanz `MyMathFuncs` der-Klasse am Anfang `MainPage` der-Klasse:

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. Überschreiben `OnAppearing` Sie `OnDisappearing` die-Methode `ContentPage` und die-Methode der Basisklasse:

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

5. Aktualisieren Sie `OnAppearing` die-Methode, um `myMathFuncs` die zuvor deklarierte Variable zu initialisieren:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. Aktualisieren Sie `OnDisappearing` die-Methode, `Dispose` um die `myMathFuncs`-Methode aufzurufen:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. Implementieren Sie eine private Methode namens **testmathfuncs** wie folgt:

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

8. Zum Schluss wird `TestMathFuncs` am Ende `OnAppearing` der-Methode aufgerufen:

    ```csharp
    TestMathFuncs();
    ```

9. Führen Sie die APP auf jeder Zielplattform aus, und überprüfen Sie, ob die Ausgabe im **Anwendungs Ausgabe** -Pad wie folgt aussieht:

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Wenn beim Testen unter Android eine "*DllNotFoundException*" oder ein Buildfehler auf IOS auftritt, stellen Sie sicher, dass die CPU-Architektur des verwendeten Geräts/Emulators/Simulators mit der unterstützten Teilmenge kompatibel ist. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie eine xamarin. Forms-app erstellen, die Native Bibliotheken über einen allgemeinen, über ein nuget-Paket verteilten .net-Wrapper verwendet. Das in dieser exemplarischen Vorgehensweise bereitgestellte Beispiel ist absichtlich sehr einfach, um den Ansatz leichter zu veranschaulichen. Eine echte Anwendung muss sich mit Komplexitäten befassen, wie z. b. Ausnahmebehandlung, Rückrufe, Marshalling komplexer Typen und Verknüpfungen mit anderen Abhängigkeits Bibliotheken. Ein wichtiger Aspekt ist der Prozess, mit dem die Entwicklung des C++ Codes koordiniert und mit den Wrapper-und Client Anwendungen synchronisiert wird. Dieser Vorgang kann variieren, je nachdem, ob eine oder beide dieser Aspekte die Verantwortung eines einzelnen Teams haben. In jedem Fall ist Automation ein echter Vorteil. Im folgenden finden Sie einige Ressourcen, die weitere Informationen zu einigen der wichtigsten Konzepte sowie zu den relevanten Downloads bereitstellen. 

### <a name="downloads"></a>Downloads

- [Nuget-Befehlszeilen Tools (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>Beispiele

- [Hyperlapse-plattformübergreifende Mobile-Entwicklung mitC++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++ und xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles-Beispielport](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>Weiterführende Themen

[Artikel im Zusammenhang mit dem Inhalt dieses Beitrags](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
