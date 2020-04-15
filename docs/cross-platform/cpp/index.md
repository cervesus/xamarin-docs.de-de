---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Verwenden von C/C++-Bibliotheken mit Xamarin
description: Visual Studio für Mac kann zum Erstellen und Integrieren von plattformübergreifendem C/C++-Code in mobilen Apps für Android und iOS mithilfe von Xamarin und C# verwendet werden. In diesem Artikel wird erläutert, wie Sie ein C++-Projekt in einer Xamarin-App einrichten und debuggen.
author: mikeparker104
ms.author: miparker
ms.date: 11/07/2019
ms.openlocfilehash: 42a59570d727657b2f3c23bd9d1f37e1205717d0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73842818"
---
# <a name="use-cc-libraries-with-xamarin"></a>Verwenden von C/C++-Bibliotheken mit Xamarin

## <a name="overview"></a>Übersicht

Mit Xamarin können Entwickler plattformübergreifende native mobile Apps mit Visual Studio erstellen. Im Allgemeinen werden C#-Bindungen verwendet, um vorhandene Plattformkomponenten für Entwickler verfügbar zu machen. In einigen Fällen müssen Xamarin-Apps jedoch eine vorhandene Codebasis nutzen. Und nicht immer haben Teams die Zeit, das Budget oder die Ressourcen, um eine große, umfangreich getestete und umfassend optimierte Codebasis zu C# zu portieren.

Mit [Visual C++ für plattformübergreifende Mobile-Entwicklung](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) kann der C/C++- und C#-Code als Teil der gleichen Lösung erstellt werden. Dies bietet eine Vielzahl von Vorteilen, u.a. eine einheitliche Debuggingoberfläche. Microsoft hat C/C++ und Xamarin auf diese Weise verwendet, um Apps wie [Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) und die [Pix-Kamera](https://www.microsoft.com/microsoftpix) bereitzustellen.

In einigen Fällen ist es jedoch wünschenswert oder erforderlich, vorhandene C/C++-Tools und -Prozesse beizubehalten und den Bibliothekscode weiterhin von der Anwendung zu entkoppeln. Dabei wird die Bibliothek so behandelt, als handle es sich um eine Drittanbieterkomponente. In diesen Szenarien besteht die Herausforderung nicht nur darin, die relevanten Member für C# verfügbar zu machen, sondern auch darin, die Bibliothek als Abhängigkeit zu verwalten. Und selbstverständlich soll dieser Prozess soweit wie möglich automatisiert werden.  

In diesem Beitrag wird ein allgemeiner Ansatz für dieses Szenario erläutert und anhand eines einfachen Beispiels veranschaulicht.

## <a name="background"></a>Hintergrund

C/C++ wird als plattformübergreifende Sprache betrachtet. Es muss jedoch mit großer Sorgfalt sichergestellt werden, dass der Quellcode tatsächlich plattformübergreifend ist. Dabei müssen nur C/C++-Elemente verwendet werden, die von allen Zielcompilern unterstützt werden und wenig oder gar keinen über Bedingungen eingebundenen Plattformcode bzw. compilerspezifischen Code umfassen.

Letztendlich muss der Code auf allen Zielplattformen kompiliert und erfolgreich ausgeführt werden können. Entscheidend ist daher die Gemeinsamkeit aller Zielplattformen (und Zielcompiler). Dennoch können aufgrund von geringfügigen Unterschieden bei den Compilern Probleme auftreten. Ein entscheidender Faktor, weshalb umfangreiche Tests (vorzugsweise automatisiert) auf den einzelnen Zielplattformen immer wichtiger werden.  

## <a name="high-level-approach"></a>Die vier Phasen bei Verwendung von C/C++-Quellcode

Die folgende Abbildung zeigt den vierstufigen Ansatz, mit dem C/C++-Quellcode in eine plattformübergreifende Xamarin-Bibliothek umgewandelt wird, die über NuGet freigegeben und dann in einer Xamarin.Forms-App verwendet wird.

![Ansatz für die Verwendung von C/C++ mit Xamarin](images/cpp-steps.jpg)

Die vier Phasen sind:

1. Kompilieren des C/C++-Quellcodes in plattformspezifische native Bibliotheken.
2. Umschließen der nativen Bibliotheken mit einer Visual Studio-Projektmappe.
3. Verpacken und Übertragen eines NuGet-Pakets per Push für den .NET-Wrapper.
4. Verwenden des NuGet-Pakets aus einer Xamarin-App.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>Phase 1: Kompilieren des C/C++-Quellcodes in plattformspezifische native Bibliotheken

Das Ziel dieser Phase besteht darin, native Bibliotheken zu erstellen, die vom C#-Wrapper aufgerufen werden können. Dies kann je nach Situation relevant sein oder nicht. Leider ist es im Rahmen dieses Artikels nicht möglich, auf alle Tools und Prozesse einzugehen, die für dieses gängige Szenario verwendet werden können. Wichtige Überlegungen betreffen die Synchronisierung der C/C++-Codebasis mit nativem Wrappercode, ausreichende Unittests sowie die Buildautomatisierung. 

Die Bibliotheken in dieser exemplarischen Vorgehensweise wurden mit Visual Studio Code und einem begleitenden Shellskript erstellt. Diese exemplarische Vorgehensweise und das Beispiel sind im [Mobile CAT GitHub-Repository](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin) näher beschrieben. Die nativen Bibliotheken werden in diesem Fall als Drittanbieterabhängigkeit behandelt. Die Veranschaulichung dieser Phase dient lediglich zur Bereitstellung von Kontextinformationen.

Der Einfachheit halber wird in der exemplarischen Vorgehensweise nur eine Teilmenge der Architekturen als Ziel verwendet. Für iOS wird das Hilfsprogramm lipo verwendet, um eine einzige FAT-Binärdatei aus den einzelnen architekturspezifischen Binärdateien zu erstellen. Android verwendet dynamische Binärdateien mit der Erweiterung .so, iOS verwendet eine statische FAT-Binärdatei mit der Erweiterung .a. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>Phase 2: Umschließen der nativen Bibliotheken mit einer Visual Studio-Projektmappe

In der nächsten Phase werden die nativen Bibliotheken umschlossen, damit sie problemlos von .NET verwendet werden können. Zu diesem Zweck kommt eine Visual Studio-Projektmappe mit vier Projekten zum Einsatz. Der gemeinsame Code ist in einem freigegebenen Projekt enthalten. Durch verschiedene Projekte, in denen je Xamarin.Android, Xamarin.iOS und .NET Standard als Ziele definiert sind, kann plattformunabhängig auf die Bibliothek verwiesen werden.

Der Wrapper verwendet die von Paul Betts beschriebene „[Bait-and-Switch-Technik](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)“. Dies ist nicht die einzige Möglichkeit, durch diese Vorgehensweise kann jedoch problemlos auf die Bibliothek verwiesen werden. Außerdem ist keine explizite Verwaltung plattformspezifischer Implementierungen innerhalb der Anwendung selbst erforderlich. Durch diese Technik wird im Wesentlichen sichergestellt, dass die Ziele (.NET Standard, Android, iOS) denselben Namespace, denselben Assemblynamen und dieselbe Klassenstruktur verwenden. Da NuGet immer eine plattformspezifische Bibliothek bevorzugt, wird die .NET Standard-Version nie zur Laufzeit verwendet.

Bei den wesentlichen Aufgaben dieses Schritts geht es um die Verwendung von P/Invoke, um die Methoden der nativen Bibliothek aufzurufen, und um die Verwaltung der Verweise auf die zugrunde liegenden Objekte. Das Ziel besteht darin, die Funktionalität der Bibliothek für den Consumer verfügbar zu machen und dabei jegliche Komplexität zu abstrahieren. Xamarin.Forms-Entwickler müssen nicht mit den internen Vorgängen der nicht verwalteten Bibliothek vertraut sein. Entwickler sollten keinen Unterschied zur Verwendung einer verwalteten C#-Bibliothek bemerken.

Das Ergebnis dieser Phase ist eine Reihe von .NET-Bibliotheken (eine Bibliothek pro Ziel) sowie ein NUSPEC-Dokument, das die erforderlichen Informationen zum Erstellen des Pakets im nächsten Schritt enthält.

**Phase 3: Verpacken und Übertragen eines NuGet-Pakets per Push für den .NET-Wrapper**

In der dritten Phase wird mithilfe der Buildartefakte aus dem vorherigen Schritt ein NuGet-Paket erstellt. Das Ergebnis dieses Schritts ist ein NuGet-Paket, das von einer Xamarin-App verwendet werden kann. In der exemplarischen Vorgehensweise wird ein lokales Verzeichnis verwendet, das als NuGet-Feed fungiert. In einer Produktionsumgebung sollte mit diesem Schritt ein Paket in einem öffentlichen oder privaten NuGet-Feed veröffentlicht werden, und der Schritt sollte vollständig automatisiert erfolgen.

**Phase 4: Verwenden des NuGet-Pakets aus einer Xamarin.Forms-App**

Der letzte Schritt besteht darin, aus einer Xamarin.Forms-App auf das NuGet-Paket zu verweisen und dieses zu verwenden. Dazu muss der NuGet-Feed in Visual Studio so konfiguriert werden, dass er den im vorherigen Schritt definierten Feed verwendet.

Nachdem der Feed konfiguriert wurde, muss von jedem Projekt in der plattformübergreifenden Xamarin.Forms-App auf das Paket verwiesen werden. Da mit der „Bait-and-Switch-Technik“ identische Schnittstellen bereitgestellt werden, kann die Funktionalität der nativen Bibliothek mithilfe von Code aufgerufen werden, der an einem einzigen Ort definiert ist.

Das Quellcoderepository umfasst eine [Liste mit weiteren Informationen](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up), die u.a. Artikel zum Einrichten eines privaten NuGet-Feeds in Azure DevOps und zum Übertragen des Pakets per Push an diesen Feed enthält. Wenngleich der Zeitaufwand für die Einrichtung etwas höher ist als bei Verwendung eines lokalen Verzeichnisses, eignet sich diese Art von Feed besser für eine Teamentwicklungsumgebung.

## <a name="walk-through"></a>Exemplarische Vorgehensweise

Die aufgeführten Schritte gelten für **Visual Studio für Mac**, lassen sich jedoch auch auf **Visual Studio 2017** übertragen.

### <a name="prerequisites"></a>Voraussetzungen

Zur Ausführung der beschriebenen Schritte wird Folgendes benötigt:

- [NuGet-Befehlszeile (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Visual Studio* *für Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> Für die Bereitstellung von Apps für iPhones wird ein aktives [**Apple Developer-Konto**](https://developer.apple.com/) benötigt.

## <a name="creating-the-native-libraries-stage-1"></a>Erstellen von nativen Bibliotheken (Phase 1)

Die Funktionalität der nativen Bibliothek basiert auf dem Beispiel aus der [exemplarischen Vorgehensweise: Erstellen und Verwenden einer statischen Bibliothek (C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017).

In dieser exemplarischen Vorgehensweise wird die erste Phase – das Erstellen der nativen Bibliotheken – übersprungen, da die Bibliothek in diesem Szenario als Drittanbieterabhängigkeit definiert ist. Die vorkompilierten nativen Bibliotheken werden gemeinsam mit dem [Beispielcode](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin) hinzugefügt oder können direkt [heruntergeladen](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts) werden.

### <a name="working-with-the-native-library"></a>Arbeiten mit der nativen Bibliothek

Das ursprüngliche Beispiel *MathFuncsLib* umfasst eine einzelne Klasse `MyMathFuncs` mit der folgenden Definition:

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

Eine weitere Klasse definiert Wrapperfunktionen, mit denen ein .NET-Consumer die native `MyMathFuncs`-Klasse erstellen und löschen bzw. mit dieser Klasse interagieren kann.

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

Diese Wrapperfunktionen werden [Xamarin](https://visualstudio.microsoft.com/xamarin/)-seitig verwendet.

## <a name="wrapping-the-native-library-stage-2"></a>Umschließen der nativen Bibliothek (Phase 2)

Für diese Phase werden die [vorkompilierten Bibliotheken](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts) benötigt, die im [vorherigen Abschnitt](#creating-the-native-libraries-stage-1) beschrieben sind.

### <a name="creating-the-visual-studio-solution"></a>Erstellen der Visual Studio-Projektmappe

1. Klicken Sie in **Visual Studio für Mac** auf **Neues Projekt** (auf der *Willkommensseite*) oder auf **Neue Projektmappe** (im Menü *Datei*).
2. Wählen Sie im Fenster **Neues Projekt** die Option **Freigegebenes Projekt** (*Multi-Plattform > Bibliothek*) aus, und klicken Sie dann auf **Weiter**.
3. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    - **Projektname:** MathFuncs.Shared  
    - **Projektmappenname:** MathFuncs  
    - **Standort:** Verwenden Sie den Standardspeicherort (oder wählen Sie einen alternativen Speicherort aus)   
    - **Projektverzeichnis im Projektmappenverzeichnis erstellen:** Aktivieren Sie diese Option
4. Doppelklicken Sie im **Projektmappen-Explorer** auf das Projekt **MathFuncs.Shared**, und wechseln Sie zu **Haupteinstellungen**.
5. Entfernen Sie **.Shared** aus dem **Standardnamespace**, sodass dieser lediglich auf **MathFuncs** festgelegt ist. Klicken Sie dann auf **OK**.
6. Öffnen Sie **MyClass.cs** (von der Vorlage erstellt), benennen Sie sowohl die Klasse als auch den Dateinamen in **MyMathFuncsWrapper** um, und ändern Sie den Namespace in **MathFuncs**.
7. **Klicken Sie bei gedrückter STRG-Taste** auf die Projektmappe **MathFuncs**, und wählen Sie im Menü **Hinzufügen** die Option **Neues Projekt hinzufügen...** aus.
8. Wählen Sie im Fenster **Neues Projekt** die Option **.NET Standard-Bibliothek** (*Multi-Plattform > Bibliothek*) aus, und klicken Sie dann auf **Weiter**.
9. Wählen Sie **.NET Standard 2.0** aus, und klicken Sie auf **Weiter**.
10. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    - **Projektname:** MathFuncs.Standard  
    - **Standort:** Verwenden Sie denselben Speicherort wie beim freigegebenen Projekt.   

11. Doppelklicken Sie im **Projektmappen-Explorer** auf das Projekt **MathFuncs.Standard**.
12. Wechseln Sie zu **Haupteinstellungen**, und ändern Sie den **Standardnamespace** in **MathFuncs**.
13. Wechseln Sie zu den **Ausgabeoptionen**, und ändern Sie **Assemblyname** in **MathFuncs**.
14. Wechseln Sie zu den **Compilereinstellungen**, ändern Sie die **Konfiguration** in **Release**, legen Sie für **Debuginformationen** die Option **Nur Symbole** fest, und klicken Sie dann auf **OK**.
15. Löschen Sie **Class1.cs/Getting Started** aus dem Projekt (sofern diese Einstellung als Teil der Vorlage hinzugefügt wurde).
16. **Klicken Sie bei gedrückter STRG-Taste** auf den Projektordner **Abhängigkeiten/Verweise**, und wählen Sie **Verweise bearbeiten** aus.
17. Wählen Sie auf der Registerkarte **Projekte** den Eintrag **MathFuncs.Shared** aus, und klicken Sie auf **OK**.
18. Wiederholen Sie die Schritte 7-17 (ignorieren Sie dabei Schritt 9) mit folgenden Konfigurationen:

    | **PROJEKTNAME**  | **VORLAGENNAME**   | **MENÜ „NEUES PROJEKT“**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | Klassenbibliothek       | Android > Bibliothek      |
    | MathFuncs.iOS     | Bindungsbibliothek     | iOS > Bibliothek          |

19. Doppelklicken Sie im **Projektmappen-Explorer** auf das Projekt **MathFuncs.Android**, und wechseln Sie zu den **Compilereinstellungen**.

20. Wählen Sie für die **Konfiguration** die Option **Debug** aus, und fügen Sie **Android;** zu **Symbole definieren** hinzu.

21. Ändern Sie die Einstellung von **Konfiguration** in **Release**, und fügen Sie **Android;** zu **Symbole definieren** hinzu.

22. Wiederholen Sie die Schritte 19-20 für **MathFuncs.iOS**. Legen Sie für **Symbole definieren** dabei in beiden Fällen fest, dass **iOS;** anstelle von **Android;** hinzugefügt wird.

23. Erstellen Sie die Projektmappe in der Konfiguration **Release** (**STRG + BEFEHL + B**), und überprüfen Sie, ob alle drei Ausgabeassemblys (Android, iOS, .NET Standard) (in den jeweiligen bin-Ordnern des Projekts) dieselbe **MathFuncs.dll** verwenden.

Zu diesen Zeitpunkt sollte die Projektmappe über drei Ziele (Android, iOS und .NET Standard) sowie ein freigegebenes Projekt verfügen, auf das jedes dieser drei Ziele verweist. In der Konfiguration der Ziele sollten derselbe Standardnamespace und Ausgabeassemblys mit identischem Namen festgelegt sein. Dies ist für die zuvor erwähnte „Bait-and-Switch-Technik“ erforderlich.

### <a name="adding-the-native-libraries"></a>Hinzufügen nativer Bibliotheken

Bei den erforderlichen Schritten zum Hinzufügen von nativen Bibliotheken zum Wrapper müssen bei Android und iOS geringfügige Unterschiede berücksichtigt werden.

#### <a name="native-references-for-mathfuncsandroid"></a>Native Verweise für MathFuncs.Android

1. **Klicken Sie bei gedrückter STRG-Taste** auf das Projekt **MathFuncs.Android**, und wählen Sie im Menü **Hinzufügen** die Option **Neuer Ordner** aus. Legen Sie als Ordnernamen **libs** fest.

2. **Klicken Sie bei gedrückter STRG-Taste** für jede **ABI** (Application Binary Interface) auf den Ordner **libs**, wählen Sie im Menü **Hinzufügen** die Option **Neuer Ordner** aus, und geben Sie als Ordnernamen den Namen der jeweiligen **ABI** ein. In diesem Fall gilt Folgendes:

    - arm64 v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > Eine detailliertere Übersicht finden Sie im Thema [Architekturen und CPUs](https://developer.android.com/ndk/guides/arch) im [NDK-Entwicklerhandbuch](https://developer.android.com/ndk/guides/). Lesen Sie insbesondere den Abschnitt zu [nativem Code in App-Paketen](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages).

3. Überprüfen Sie die Ordnerstruktur:  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. Fügen Sie basierend auf der folgenden Zuordnung die entsprechenden **.so**-Bibliotheken zu jedem **ABI**-Ordner hinzu:

    **arm64-v8a:** libs/Android/arm64

    **armeabi-v7a:** libs/Android/arm  

    **x86:** libs/Android/x86

    **x86_64:** libs/Android/x86_64

    > [!NOTE]
    > Zum Hinzufügen von Dateien **klicken Sie bei gedrückter STRG-Taste** auf den Ordner der jeweiligen **ABI**, und wählen Sie im Menü **Hinzufügen** den Eintrag **Dateien hinzufügen...** aus. Wählen Sie die geeignete Bibliothek aus (im Verzeichnis **PrecompiledLibs**), klicken Sie auf **Öffnen**, und klicken Sie dann auf **OK** (behalten Sie die Standardoption *Kopieren der Datei in das Verzeichnis* bei).

5. **Klicken Sie bei gedrückter STRG-Taste** auf jede **.so**-Datei, und wählen Sie die Option **EmbeddedNativeLibrary** aus dem Menü **Buildvorgang** aus.

Der Ordner **libs** sollte nun wie folgt aussehen:

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

#### <a name="native-references-for-mathfuncsios"></a>Native Verweise für MathFuncs.iOS

1. **Klicken Sie bei gedrückter STRG-Taste** auf das Projekt **MathFuncs.iOS**, und wählen Sie im Menü **Hinzufügen** die Option **Nativen Verweis hinzufügen** aus. 
2. Wählen Sie die Bibliothek **libMathFuncs.a** aus (unter „libs/ios“ im Verzeichnis **PrecompiledLibs**), und klicken Sie auf **Öffnen** 
3. **Klicken Sie bei gedrückter STRG-Taste** auf die Datei **libMathFuncs** (im Ordner **Native Verweise**), und wählen Sie die Option **Eigenschaften** aus dem Menü aus  
4. Konfigurieren Sie die Eigenschaften von **Native Verweise**, indem Sie sie im **Eigenschaftenpad** aktivieren (gesetztes Häkchen):

    - Laden erzwingen
    - Ist C++
    - Intelligenter Link

    > [!NOTE]
    > Durch die Verwendung eines Bindungsbibliothek-Projekttyps mit einem [nativen Verweis](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) wird die statische Bibliothek eingebettet. Außerdem kann sie automatisch mit der Xamarin.iOS-App verknüpft werden, die auf die Bibliothek verweist (selbst wenn sie über ein NuGet-Paket eingeschlossen wird).

5. Öffnen Sie **ApiDefinition.cs**, löschen Sie den kommentierten Vorlagencode (behalten Sie lediglich den `MathFuncs`-Namespace bei), und führen Sie denselben Schritt für **Structs.cs** aus. 

    > [!NOTE]
    > Diese Dateien (mit den Buildvorgängen *ObjCBindingApiDefinition* und *ObjCBindingCoreSource*) werden zum Erstellen eines Bindungsbibliotheksprojekts benötigt. Wir schreiben den Code für den Aufruf unserer nativen Bibliothek jedoch außerhalb dieser Dateien und zwar so, dass er von den Android- und iOS-Zielen über einen Standardaufruf von P/Invoke gemeinsam verwendet werden kann.

### <a name="writing-the-managed-library-code"></a>Schreiben des verwalteten Bibliothekscodes

Schreiben Sie nun den C#-Code, um die native Bibliothek aufzurufen. Ziel dieses Schritts ist, die zugrunde liegende Komplexität zu verbergen. Der Consumer sollte sich nicht mit den internen Abläufen der nativen Bibliothek oder den Konzepten von P/Invoke auskennen müssen.  

#### <a name="creating-a-safehandle"></a>Erstellen eines SafeHandle

1. **Klicken Sie bei gedrückter STRG-Taste** auf das Projekt **MathFuncs.Shared**, und wählen Sie im Menü **Hinzufügen** die Option **Datei hinzufügen...** aus. 
2. Wählen Sie im Fenster **Neue Datei** die Option **Leere Klasse** aus, weisen Sie den Namen **MyMathFuncsSafeHandle** zu, und klicken Sie dann auf **Neu**
3. Implementieren Sie die Klasse **MyMathFuncsSafeHandle**:

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > Ein [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) ist die bevorzugte Methode, um mit nicht verwalteten Ressourcen in verwaltetem Code zu arbeiten. Auf diese Weise wird ein großer Teil der Codebausteine abstrahiert, die sich auf die kritische Finalisierung und den Objektlebenszyklus beziehen. Der Besitzer dieses Handles kann ihn anschließend wie jede andere verwaltete Ressource behandeln und muss nicht das vollständige [Dispose-Muster](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) implementieren. 

#### <a name="creating-the-internal-wrapper-class"></a>Erstellen der internen Wrapperklasse

1. Öffnen Sie **MyMathFuncsWrapper.cs**, und ändern Sie die Klasse in eine interne statische Klasse.

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. Fügen Sie in derselben Datei die folgende Bedingungsanweisung zur Klasse hinzu:

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > Dadurch wird der Wert der Konstante **DllName** abhängig davon festgelegt, ob die Bibliothek für **Android** oder für **iOS** erstellt wird. So wird den unterschiedlichen Namenskonventionen der Plattformen Rechnung getragen, aber auch dem in diesem Fall verwendeten Bibliothekstyp. Android verwendet eine dynamische Bibliothek und erwartet daher einen Dateinamen mit Erweiterung. Da wir eine statische Bibliothek verwenden, ist für iOS „ *__Internal*“ erforderlich.

3. Fügen Sie am Anfang der Datei **MyMathFuncsWrapper.cs** einen Verweis auf **System.Runtime.InteropServices** hinzu.

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. Fügen Sie die Wrappermethoden zum Erstellen und Löschen der Klasse **MyMathFuncs** hinzu:

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > Wir übergeben unsere Konstante **DllName** gemeinsam mit **EntryPoint** an das Attribut **DllImport**. „EntryPoint“ gibt den Namen der Funktion, die innerhalb dieser Bibliothek aufgerufen werden soll, explizit an die .NET-Laufzeit weiter. Technisch gesehen müssen wir den **EntryPoint**-Wert nicht angeben, wenn die Namen der verwalteten Methoden mit den Namen der nicht verwalteten Methoden identisch sind. Wenn kein Name angegeben wird, wird stattdessen der Name der verwalteten Methode als **EntryPoint** verwendet. Die explizite Angabe wird jedoch empfohlen.  

5. Fügen Sie die Wrappermethoden hinzu, damit wir unter Verwendung des folgenden Codes mit der Klasse **MyMathFuncs** arbeiten können:

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
    > Für die Parameter in diesem Beispiel werden einfache Typen verwendet. Da das Marshalling eine bitweise Kopie ist, ist in diesem Fall keine zusätzliche Arbeit erforderlich. Beachten Sie auch die Verwendung der Klasse **MyMathFuncsSafeHandle** anstelle der Standardklasse **IntPtr**. **IntPtr** wird dem **SafeHandle** automatisch als Teil des Marshallingvorgangs zugeordnet.

6. Überprüfen Sie, ob die fertig gestellte Klasse **MyMathFuncsWrapper** wie unten gezeigt aussieht:

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

#### <a name="completing-the-mymathfuncssafehandle-class"></a>Fertigstellen der Klasse „MyMathFuncsSafeHandle“

1. Öffnen Sie die Klasse **MyMathFuncsSafeHandle**, und wechseln Sie zum Platzhalterkommentar **TODO** innerhalb der Methode **ReleaseHandle**:

    ```csharp
    // TODO: Release the handle here
    ```

1. Ersetzen Sie die Zeile **TODO**:

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(handle);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>Erstellen der Klasse „MyMathFuncs“

Nachdem der Wrapper abgeschlossen wurde, erstellen Sie im nächsten Schritt eine Klasse „MyMathFuncs“, die den Verweis auf das nicht verwaltete C++-Objekt „MyMathFuncs“ verwaltet.  

1. **Klicken Sie bei gedrückter STRG-Taste** auf das Projekt **MathFuncs.Shared**, und wählen Sie im Menü **Hinzufügen** die Option **Datei hinzufügen...** aus. 
2. Wählen Sie im Fenster **Neue Datei** die Option **Leere Klasse** aus, weisen Sie den Namen **MyMathFuncs** zu, und klicken Sie dann auf **Neu**.
3. Fügen Sie der Klasse **MyMathFuncs** die folgenden Member hinzu:

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. Implementieren Sie den Konstruktor für die Klasse so, dass beim Instanziieren der Klasse ein Handle für das native **MyMathFuncs**-Objekt erstellt und gespeichert wird:

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. Implementieren Sie die **IDisposable**-Schnittstelle mithilfe des folgenden Codes:

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

6. Implementieren Sie die **MyMathFuncs**-Methoden mithilfe der **MyMathFuncsWrapper**-Klasse, um die zugrunde liegenden Vorgänge auszuführen. Dabei wird der Zeiger auf das zugrunde liegende nicht verwaltete Objekt übergeben, den wir gespeichert haben. Der Code sollte wie folgt aussehen:

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

#### <a name="creating-the-nuspec"></a>Erstellen der NUSPEC-Datei

Um die Bibliothek zu verpacken und über NuGet zu verteilen, wird eine **NUSPEC**-Datei benötigt. In dieser Datei ist festgelegt, welche der resultierenden Assemblys für die verschiedenen unterstützten Plattformen hinzugefügt werden.

1. **Klicken Sie bei gedrückter STRG-Taste** auf die Projektmappe **MathFuncs**, wählen Sie im Menü **Hinzufügen** die Option **Projektmappenordner hinzufügen** aus, und weisen Sie den Namen **SolutionItems** zu.
2. **Klicken Sie bei gedrückter STRG-Taste** auf den Ordner **SolutionItems**, und wählen Sie im Menü **Hinzufügen** die Option **Neue Datei...** aus.
3. Wählen Sie im Fenster **Neue Datei** die Option **Leere XML-Datei** aus, weisen Sie den Namen **MathFuncs.nuspec** zu, und klicken Sie dann auf **Neu**.
4. Aktualisieren Sie **MathFuncs.nuspec** mit den grundlegenden Paketmetadaten, die für den **NuGet**-Consumer angezeigt werden sollen. Zum Beispiel:

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
    > In der [NUSPEC-Referenz](https://docs.microsoft.com/nuget/reference/nuspec) finden Sie weitere Einzelheiten zum Schema, das für dieses Manifest verwendet wird.

5. Fügen Sie ein `<files>`-Element als untergeordnetes Element des `<package>`-Elements hinzu (direkt unterhalb von `<metadata>`), um jede Datei mit einem separaten `<file>`-Element zu identifizieren:

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > Wenn ein Paket in einem Projekt installiert wird und mehrere Assemblys mit demselben Namen angegeben sind, wählt NuGet effektiv die Assembly aus, die für die jeweilige Plattform am spezifischsten ist.

6. Fügen Sie die `<file>`-Elemente für die **Android**-Assemblys hinzu:

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. Fügen Sie die `<file>`-Elemente für die **iOS**-Assemblys hinzu:

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. Fügen Sie die `<file>`-Elemente für die **netstandard2.0**-Assemblys hinzu:

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. Überprüfen Sie das **NUSPEC**-Manifest:

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
    > Diese Datei gibt die Assemblyausgabepfade aus einem **Release**-Build an. Stellen Sie daher sicher, dass Sie die Projektmappe mit dieser Konfiguration erstellen.

An diesem Punkt enthält die Projektmappe drei .NET-Assemblys und ein unterstützendes **NUSPEC**-Manifest.

## <a name="distributing-the-net-wrapper-with-nuget"></a>Verteilen des .NET-Wrappers mit NuGet

Im nächsten Schritt wird das NuGet-Paket verpackt und verteilt, damit es problemlos von der App genutzt und als Abhängigkeit verwaltet werden kann. Die Umschließung und Nutzung könnte über eine einzige Projektmappe erfolgen, das Verteilen der Bibliothek über NuGet ist jedoch für die Entkopplung nützlich und ermöglicht es uns, jede Codebasis separat zu verwalten.

### <a name="preparing-a-local-packages-directory"></a>Vorbereiten eines lokalen Paketverzeichnisses

Die einfachste Form eines NuGet-Feeds ist ein lokales Verzeichnis:

1. Wechseln Sie im **Finder** zu einem beliebigen Verzeichnis. Beispiel: **/Benutzer**.
2. Wählen Sie im Menü **Datei** die Option **Neuer Ordner** aus, und geben Sie einen aussagekräftigen Namen wie z.B. **local-nuget-feed** ein.

### <a name="creating-the-package"></a>Erstellen des Pakets

1. Legen Sie für die **Buildkonfiguration** die Option **Release** fest, und führen Sie den Build über **BEFEHL + B** aus.
2. Öffnen Sie **Terminal**, und wechseln Sie in den Ordner, der die **NUSPEC**-Datei enthält.
3. Führen Sie in **Terminal** den Befehl **nuget pack** aus, und geben Sie dabei die **NUSPEC**-Datei, die **Version** (z.B. 1.0.0) sowie das **OutputDirectory** an (den im [vorherigen Schritt](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed) erstellten Ordner, **local-nuget-feed**). Zum Beispiel:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **Überprüfen Sie**, ob **MathFuncs.1.0.0.nupkg** im Verzeichnis **local-nuget-feed** erstellt wurde.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[OPTIONAL] Verwenden eines privaten NuGet-Feeds mit Azure DevOps

Ein zuverlässigeres Verfahren wird unter [Erste Schritte mit NuGet-Paketen in Azure DevOps](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package) beschrieben. In diesem Artikel erfahren Sie, wie Sie einen privaten Feed erstellen und das im vorherigen Schritt generierte Paket per Push an diesen Feed übertragen.

Idealerweise wird dieser Workflow vollständig automatisiert, beispielsweise mithilfe von [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts). Weitere Informationen finden Sie unter [Erste Schritte mit Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts).

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Verwenden des .NET-Wrappers aus einer Xamarin.Forms-App

Zum Abschluss der exemplarischen Vorgehensweise erstellen Sie eine **Xamarin.Forms**-App, die das soeben im lokalen **NuGet**-Feed veröffentlichte Paket verwendet.

### <a name="creating-the-xamarinforms-project"></a>Erstellen des **Xamarin.Forms**-Projekts

1. Öffnen Sie eine neue Instanz von **Visual Studio für Mac**. Dieser Schritt kann über **Terminal** ausgeführt werden:

    ```bash
    open -n -a "Visual Studio"
    ```

2. Klicken Sie in **Visual Studio für Mac** auf **Neues Projekt** (auf der *Willkommensseite*) oder auf **Neue Projektmappe** (im Menü *Datei*).
3. Wählen Sie im Fenster **Neues Projekt** die Option **Leere Forms-App** (*Multi-Plattform > App*) aus, und klicken Sie dann auf **Weiter**.
4. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Weiter**:

    - **App-Name:** MathFuncsApp.
    - **Organisations-ID:** Verwenden Sie einen Reverse-Namespace, z.B. _com.{Ihre_Org}_ .
    - **Zielplattformen:** Verwenden Sie die Standardeinstellung (sowohl Android- als auch iOS-Ziele).
    - **Freigegebener Code:** Legen Sie für diese Einstellung .NET Standard fest (eine Projektmappe mit freigegebener Bibliothek ist möglich, kann im Rahmen dieser exemplarischen Vorgehensweise jedoch nicht behandelt werden).

5. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    - **Projektname:** MathFuncsApp.
    - **Projektmappenname:** MathFuncsApp.  
    - **Standort:** Verwenden Sie den Standardspeicherort (oder wählen Sie einen alternativen Speicherort aus).

6. **Klicken Sie bei gedrückter STRG-Taste** im **Projektmappen-Explorer** auf das Ziel (**MathFuncsApp.Android** oder **MathFuncs.iOS**) für die ersten Tests, und wählen Sie dann **Als Startprojekt festlegen**.
7. Wählen Sie das bevorzugte **Gerät** oder den bevorzugten **Simulator**/**Emulator** aus. 
8. Führen Sie die Projektmappe aus (**BEFEHL + EINGABE**), um zu überprüfen, ob das **Xamarin.Forms**-Vorlagenprojekt ordnungsgemäß erstellt und ausgeführt wird. 

    > [!NOTE]
    > **iOS** (insbesondere der Simulator) weist tendenziell die kürzeste Zeit für das Erstellen/Bereitstellen auf.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>Hinzufügen des lokalen NuGet-Feeds zur NuGet-Konfiguration

1. Wählen Sie in **Visual Studio** die Option **Einstellungen** aus (im **Visual Studio**-Menü).
2. Wählen Sie im Abschnitt **NuGet** die Option **Quellen** aus, und klicken Sie auf **Hinzufügen**.
3. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Quelle hinzufügen**:

    - **Name:** Geben Sie einen aussagekräftigen Namen ein, z.B. „Local-Packages“.  
    - **Standort:** Geben Sie den im [vorherigen Schritt](#preparing-a-local-packages-directory) erstellten Ordner **local-nuget-feed** an.

    > [!NOTE]
    > In diesem Fall muss kein **Benutzername** und kein **Kennwort** angegeben werden. 

4. Klicken Sie auf **OK**.

### <a name="referencing-the-package"></a>Verweisen auf das Paket

Wiederholen Sie die folgenden Schritte für alle Projekte (**MathFuncsApp**, **MathFuncsApp.Android** und **MathFuncsApp.iOS**).

1. **Klicken Sie bei gedrückter STRG-Taste** auf das Projekt, und wählen Sie im Menü **Hinzufügen** die Option **NuGet-Pakete hinzufügen...** aus.
2. Suchen Sie nach **MathFuncs**. 
3. Überprüfen Sie, ob die **Version** des Pakets **1.0.0** ist und auch die übrigen Details wie erwartet angezeigt werden (z.B. **Titel** und **Beschreibung**, also *MathFuncs* und *C++-Beispielwrapperbibliothek*). 
4. Wählen Sie das Paket **MathFuncs** aus, und klicken Sie auf **Paket hinzufügen**.

### <a name="using-the-library-functions"></a>Verwenden der Bibliotheksfunktionen

Nachdem Sie einen Verweis auf das **MathFuncs**-Paket in jedem Projekt erstellt haben, können die Funktionen vom C#-Code verwendet werden.

1. Öffnen Sie **MainPage.xaml.cs** aus dem gemeinsamen **Xamarin.Forms**-Projekt **MathFuncsApp** (auf das sowohl von **MathFuncsApp.Android** als auch von **MathFuncsApp.iOS** verwiesen wird).
2. Fügen Sie am Anfang der Datei **using**-Anweisungen für **System.Diagnostics** und **MathFuncs** hinzu:

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. Deklarieren Sie oben in der `MainPage`-Klasse eine Instanz der `MyMathFuncs`-Klasse:

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. Setzen Sie die Methoden `OnAppearing` und `OnDisappearing` aus der `ContentPage`-Stammklasse außer Kraft:

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

5. Aktualisieren Sie die Methode `OnAppearing`, um die zuvor deklarierte Variable `myMathFuncs` zu initialisieren:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. Aktualisieren Sie die Methode `OnDisappearing`, um die Methode `Dispose` für `myMathFuncs` aufzurufen:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. Implementieren Sie wie nachfolgend beschrieben eine private Methode **TestMathFuncs**:

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

8. Rufen Sie am Ende der `OnAppearing`-Methode `TestMathFuncs` auf:

    ```csharp
    TestMathFuncs();
    ```

9. Führen Sie die App auf jeder Zielplattform aus, und überprüfen Sie, ob die Ausgabe im **Anwendungsausgabepad** wie folgt aussieht:

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Wenn beim Testen unter Android eine *DLLNotFoundException*-Ausnahme oder unter iOS ein Buildfehler auftritt, sollten Sie überprüfen, ob die CPU-Architektur des verwendeten Geräts/Emulators/Simulators mit der ausgewählten unterstützten Teilmenge kompatibel ist. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie eine Xamarin.Forms-App erstellen, die native Bibliotheken über einen gemeinsamen .NET-Wrapper verwendet, der über ein NuGet-Paket bereitgestellt wird. Das in dieser exemplarischen Vorgehensweise verwendete Beispiel ist absichtlich sehr einfach gehalten, um die Vorgehensweise besser veranschaulichen zu können. Bei einer echten Anwendung müssen komplexe Aspekte wie die Ausnahmebehandlung, Rückrufe oder das Marshallen komplexerer Typen sowie Verknüpfungen mit weiteren Abhängigkeitsbibliotheken berücksichtigt werden. Ein wichtiger Aspekt ist der Prozess, über den die Entwicklung des C++-Codes koordiniert und mit den Wrapper- und Clientanwendungen synchronisiert wird. Dieser Prozess kann variieren, je nachdem, ob ein einzelnes Team für einen oder beide dieser Aspekte verantwortlich ist. In jedem Fall bietet die Automatisierung entscheidende Vorteile. Im Folgenden sind eine Reihe von Ressourcen aufgeführt, in denen Sie weitere Informationen zu einigen der wichtigsten Konzepte sowie zu den relevanten Downloads finden. 

### <a name="downloads"></a>Downloads

- [NuGet-Befehlszeilentools (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>Beispiele

- [Hyperlapse – plattformübergreifende Mobile-Entwicklung mit C++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++ und Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles-Beispielport](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>Weiterführende Themen

[Verwandte Artikel für Inhalte dieses Beitrags](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)