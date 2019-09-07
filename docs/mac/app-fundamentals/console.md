---
title: Verwenden von xamarin. Mac-Bindungen für Konsolen-apps
description: Erstellen Sie mit xamarin. Mac eine Start lose Konsolen-APP, um auf native macOS-APIs zuzugreifen.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: a38543d575f655a3b1b70ff94eece7fef1bf2d40
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769895"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Xamarin. Mac-Bindungen in Konsolen-apps

Es gibt einige Szenarien, in denen Sie einige der systemeigenen Apple-APIs C# in verwenden möchten, um eine &ndash; Anwendung ohne Agent zu erstellen, für die &ndash; keine C#Benutzeroberfläche mit vorhanden ist.

Die Projektvorlagen für Macintosh-Anwendungen enthalten einen- `NSApplication.Init()` Rückruf `NSApplication.Main(args)`, gefolgt von einem-Befehl, der in der Regel wie folgt aussieht:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

Der- `Main(args)` Befehl bereitetdiexamarin.Mac-Laufzeitvor,der-BefehlstartetdieCocoa-AnwendungsHauptschleife,diedieAnwendungdaraufvorbereitet,Tastatur-undMausereignissezuempfangenunddasHauptfenster`Init` der Anwendung anzuzeigen.   Der-Versuch von versucht auch, Cocoa-Ressourcen zu finden, ein topebenenfenster vorzubereiten und zu erwarten, dass das Programm Teil eines Anwendungspakets ist (in einem Verzeichnis `.app` verteilte Programme mit der Erweiterung und einem sehr spezifischen Layout). `Main`

Anwendungen ohne Start benötigen keine Benutzeroberfläche und müssen nicht als Teil eines Anwendungspakets ausgeführt werden.

## <a name="creating-the-console-app"></a>Erstellen der Konsolen-App

Es ist daher besser, mit einem regulären .net-Konsolen Projekttyp zu beginnen.

Sie müssen einige Dinge tun:

- Erstellen Sie ein leeres Projekt.
- Verweisen Sie auf die xamarin. Mac. dll-Bibliothek.
- Bringen Sie die nicht verwaltete Abhängigkeit in das Projekt.

Diese Schritte werden im folgenden ausführlicher erläutert:

### <a name="create-an-empty-console-project"></a>Erstellen eines leeren Konsolen Projekts

Erstellen Sie ein neues .net-Konsolen Projekt, und stellen Sie sicher, dass es .net und nicht .net Core ist, da xamarin. Mac. dll nicht unter der .net Core-Laufzeit ausgeführt wird, sondern nur mit der Mono-Laufzeit ausgeführt wird.

### <a name="reference-the-xamarinmac-library"></a>Verweisen auf die xamarin. Mac-Bibliothek

Zum Kompilieren des Codes sollten Sie aus diesem Verzeichnis auf die `Xamarin.Mac.dll` Assembly verweisen:`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Wechseln Sie zu diesem Zweck zu den Projekt verweisen, wählen Sie die Registerkarte **.NET-Assembly** aus, und klicken Sie auf die Schaltfläche **Durchsuchen** , um die Datei im Dateisystem zu suchen.  Navigieren Sie zum Pfad oben, und wählen Sie dann die Datei **xamarin. Mac. dll** aus diesem Verzeichnis aus.

Dadurch erhalten Sie zur Kompilierzeit Zugriff auf die Cocoa-APIs.   An diesem Punkt können Sie am Anfang `using AppKit` der Datei hinzufügen und die `NSApplication.Init()` -Methode aufzurufen.   Es gibt nur noch einen Schritt, bevor Sie die Anwendung ausführen können.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Bringen Sie die nicht verwaltete Unterstützungs Bibliothek in Ihr Projekt.

Bevor die Anwendung ausgeführt wird, müssen Sie die `Xamarin.Mac` Unterstützungs Bibliothek in Ihr Projekt einbringen.   Fügen Sie zu diesem Zweck dem Projekt eine neue Datei hinzu (Wählen Sie unter "Projektoptionen" die Option **Hinzufügen**und dann **vorhandene Datei hinzufügen**) aus, und navigieren Sie zu diesem Verzeichnis:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

Wählen Sie die Datei **libxammac. dylib**aus.   Ihnen wird die Auswahl von kopieren, verknüpfen oder verschieben geboten.   Ich persönlich mag das verknüpfen, aber das Kopieren funktioniert ebenfalls.    Wählen Sie dann die Datei aus, und legen Sie im Eigenschaften Bereich (Klicken Sie auf **Ansicht > Pads > Eigenschaften** , wenn das eigenschaftenpad nicht angezeigt wird) den Abschnitt **Build** hinzu, und legen Sie die Einstellung in **Ausgabeverzeichnis kopieren** auf **kopieren, wenn neuer**fest.

Sie können jetzt Ihre xamarin. Mac-Anwendung ausführen.

Das Ergebnis im bin-Verzeichnis sieht wie folgt aus:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Um diese APP auszuführen, benötigen Sie alle Dateien im selben Verzeichnis.

## <a name="building-a-standalone-application-for-distribution"></a>Aufbauen einer eigenständigen Anwendung für die Verteilung

Möglicherweise möchten Sie eine einzelne ausführbare Datei an Ihre Benutzer verteilen.  Zu diesem Zweck können Sie das `mkbundle` Tool verwenden, um die verschiedenen Dateien in eine eigenständige ausführbare Datei umzuwandeln.

Stellen Sie zunächst sicher, dass Ihre Anwendung kompiliert und ausgeführt wird.   Wenn Sie mit den Ergebnissen zufrieden sind, können Sie den folgenden Befehl über die Befehlszeile ausführen:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

Im obigen Befehlszeilen Aufruf wird die-Option `-o` verwendet, um die generierte Ausgabe anzugeben. in diesem Fall haben wir den Wert weitergegeben. `/tmp/consoleapp`   Dabei handelt es sich um eine eigenständige Anwendung, die Sie verteilen können und keine externen Abhängigkeiten von Mono oder xamarin. Mac aufweist. es handelt sich hierbei um eine vollständig eigenständige ausführbare Datei.

Die Befehlszeile hat manuell die zu verwendende Datei " **Machine. config** " und eine Konfigurationsdatei für die systemweite Bibliotheks Zuordnung angegeben.   Sie sind nicht für alle Anwendungen erforderlich, aber es ist praktisch, Sie zu bündeln, da Sie verwendet werden, wenn Sie weitere Funktionen von .NET verwenden.

## <a name="project-less-builds"></a>Projekt lose Builds

Sie benötigen kein vollständiges Projekt, um eine eigenständige xamarin. Mac-Anwendung zu erstellen. Sie können auch einfache UNIX-Makefiles verwenden, um die Aufgabe zu erledigen.   Im folgenden Beispiel wird gezeigt, wie Sie ein Makefile für eine einfache Befehlszeilen Anwendung einrichten können:

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

Der obige `Makefile` Abschnitt bietet drei Ziele:

- `make`erstellt das Programm.
- `make run`Das Programm wird im aktuellen Verzeichnis erstellt und ausgeführt.
- `make bundle`erstellt eine eigenständige ausführbare Datei.
