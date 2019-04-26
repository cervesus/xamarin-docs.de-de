---
title: Verwenden von Xamarin.Mac Bindungen für Konsolen-Apps
description: Erstellen Sie eine monitorlose Konsolen-app, die mit Xamarin.Mac auf native MacOS-APIs zugreifen.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378275"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Xamarin.Mac-Bindungen in der Konsolen-apps

Es gibt einige Szenarien, in denen Sie einige der nativen Apple-APIs in c# verwenden, zum Erstellen einer Anwendung monitorlosen möchten &ndash; , die nicht über eine Benutzeroberfläche verfügt &ndash; mithilfe von c#.

Die Projektvorlagen für Macintosh-Anwendungen enthalten einen Aufruf von `NSApplication.Init()` gefolgt von einem Aufruf von `NSApplication.Main(args)`, in der Regel sieht wie folgt:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

Der Aufruf von `Init` bereitet die Xamarin.Mac-Laufzeit den Aufruf von `Main(args)` startet die Cocoa-Anwendung-main-Schleife, die die Anwendung zum Empfangen von Ereignissen von Tastatur- und Mausereignisse, und zeigen im Hauptfenster der Anwendung vorbereitet.   Der Aufruf von `Main` versucht auch, Cocoa-Ressourcen zu suchen, bereiten Sie ein Fenster der obersten und erwartet, dass das Programm ein anwendungsbündel angehören (Programme, die in einem Verzeichnis mit verteilt die `.app` -Erweiterung und ein sehr spezifische Layout).

Monitorlose Anwendungen benötigen einen Benutzer keine Schnittstelle und müssen nicht als Teil einer Anwendungspaket ausführen.

## <a name="creating-the-console-app"></a>Erstellen die Konsolen-app

Daher ist es besser, die mit einem regulären .NET Console-Projekt zu starten.

Sie müssen einige Punkte beachten:

- Erstellen Sie ein leeres Projekt.
- Verweisen auf die Bibliothek "xamarin.Mac.dll".
- Schalten Sie den nicht verwaltete Abhängigkeit zu Ihrem Projekt ein.

Diese Schritte werden im folgenden ausführlicher erläutert:

### <a name="create-an-empty-console-project"></a>Erstellen Sie ein leeres Projekt für die Konsole

Erstellen Sie ein neues .NET-Konsolenprojekt zu, stellen Sie sicher, dass sie .NET und nicht .NET Core, als "xamarin.Mac.dll" wird nicht unter .NET Core-Runtime ausgeführt, er wird nur mit die Mono-Laufzeit ausgeführt.

### <a name="reference-the-xamarinmac-library"></a>Verweisen auf die Xamarin.Mac-Bibliothek

Um Ihren Code zu kompilieren, sollten Sie auf die `Xamarin.Mac.dll` Assembly aus diesem Verzeichnis: `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Wechseln Sie zu diesem Zweck den Projektverweisen die **.NET Framework-Assembly** Registerkarte, und klicken Sie auf die **Durchsuchen** Schaltfläche, um die Datei im Dateisystem zu suchen.  Navigieren Sie zu den oben angegebenen Pfad, und wählen Sie dann die **"xamarin.Mac.dll"** aus diesem Verzeichnis.

Dadurch erhalten Sie Zugriff auf die Cocoa-APIs zum Zeitpunkt der Kompilierung.   Sie können an diesem Punkt hinzufügen `using AppKit` am Anfang Ihrer Datei, und rufen die `NSApplication.Init()` Methode.   Es gibt nur ein weiterer Schritt vor dem Ausführen der Anwendung.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Schalten Sie die für nicht verwaltete Unterstützungsbibliothek in Ihr Projekt

Bevor Ihre Anwendung ausgeführt wird, müssen Sie bringen die `Xamarin.Mac` Support-Bibliothek in Ihr Projekt.   Zu diesem Zweck fügen Sie eine neue Datei zu Ihrem Projekt (Wählen Sie in den Projektoptionen **hinzufügen**, und klicken Sie dann **vorhandene Datei hinzufügen**), und navigieren Sie zu diesem Verzeichnis:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

Hier wählen Sie die Datei **libxammac.dylib**.   Sie werden eine Auswahl von kopieren, verknüpfen oder verschieben angeboten.   Mir persönlich gefällt verknüpfen, aber kopieren funktioniert ebenfalls.    Müssen Sie die Datei auszuwählen und im Bereich "Eigenschaft" (Wählen Sie **Ansicht > Pads > Eigenschaften** Wenn das Pad "Eigenschaft" nicht angezeigt wird), wechseln Sie zu der **erstellen** aus, und legen Sie die **in Ausgabeverzeichnis kopieren Directory** auf **kopieren, wenn neuer**.

Sie können jetzt Ihre Xamarin.Mac-Anwendung ausführen.

Das Ergebnis in Ihrem Verzeichnis "bin" wird wie folgt aussehen:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Um diese app auszuführen, benötigen Sie alle diese Dateien im selben Verzeichnis.

## <a name="building-a-standalone-application-for-distribution"></a>Erstellen eine eigenständige Anwendung für die Verteilung

Sie möchten eine einzige ausführbare Datei für Ihre Benutzer zu verteilen.  Zu diesem Zweck können Sie die `mkbundle` Tool, um die verschiedenen Dateien in eine eigenständige ausführbare Datei zu aktivieren.

Stellen Sie zunächst sicher, dass die Anwendung kompiliert und ausgeführt wird.   Sobald Sie mit den Ergebnissen zufrieden sind, können Sie den folgenden Befehl über die Befehlszeile ausführen:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

In der obigen Befehlszeile-Aufruf, die Option `-o` wird verwendet, um die generierte Ausgabe anzugeben, in diesem Fall wir übergeben `/tmp/consoleapp`.   Dies ist jetzt eine eigenständige Anwendung, die Sie verteilen können, und weist keine externen Abhängigkeiten auf Mono oder Xamarin.Mac, es ist eine vollständig eigenständige ausführbare Datei.

Die Befehlszeile, die manuell angegebene die **"Machine.config"** zu verwendende Datei an, und eine Konfigurationsdatei für eine systemweite-Bibliothek-Zuordnung.   Sie sind nicht für alle Anwendungen erforderlich sind, aber es ist sinnvoll, sie bündeln, wie sie verwendet werden, wenn Sie weitere Funktionen von .NET verwenden

## <a name="project-less-builds"></a>Ohne Projekte erstellt.

Ein vollständiges-Projekt zum Erstellen einer eigenständigen Xamarin.Mac-Anwendung ist nicht erforderlich, Sie können auch einfache Unix Makefiles für die Arbeit verwenden.   Das folgende Beispiel zeigt, wie Sie ein Makefile für eine einfache befehlszeilenanwendung einrichten können:

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)


bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

Die oben genannten `Makefile` bietet drei Ziele:

- `make` wird das Programm erstellen
- `make run` erstellt und führen Sie das Programm im aktuellen Verzeichnis
- `make bundle` eine eigenständige ausführbare Datei wird erstellt werden.
