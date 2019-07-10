---
title: Interaktive Arbeitsmappen
description: Dieses Dokument beschreibt, wie Xamarin Workbooks zum Erstellen von live-Dokumenten mit C# Code experimentieren, Lehren, Schulungen und untersuchen.
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: a900d427ad6ac2a0e211ef4f00d2f014b13e5d1c
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674419"
---
# <a name="interactive-workbooks"></a>Interaktive Arbeitsmappen

Sie können Arbeitsmappen als eigenständige Anwendung, getrennt von der IDE verwenden.

Führen Sie zum Erstellen einer neuen Arbeitsmappe starten die Workbooks-app ein. Wenn Sie dies noch nicht installiert haben, besuchen Sie die [Installation](~/tools/workbooks/install.md#install) Seite. Sie werden aufgefordert, die zum Erstellen einer Arbeitsmappe in die Plattform Ihrer Wahl, automatisch eine Agent-app, sodass Sie das Dokument in Echtzeit visualisieren eine Verbindung mit hergestellt werden.

Wenn die Workbooks-app bereits ausgeführt wird, können Sie ein neues Dokument erstellen, indem Sie zu **Datei > Neu**.

Arbeitsmappen gespeichert, und es später noch Mal in der Anwendung geöffnet werden können. Sie können dann diese mit anderen gemeinsam veranschaulichen Ideen, Informationen zu neuen APIs oder neue Konzepte zu erfahren.

## <a name="code-editing"></a>Bearbeiten von Code

Der Code-Bearbeitungsfenster bietet codevervollständigung, farbige syntaxmarkierung, Inline-live-Diagnose und Unterstützung für mehrzeilige Anweisung.

[![](workbook-images/inspector-0.6.0-repl-small.png "Der Code-Bearbeitungsfenster bietet codevervollständigung, farbige syntaxmarkierung, Inline-live-Diagnose und Unterstützung für mehrzeilige Anweisung")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin Workbooks werden gespeichert, eine `.workbook` -Datei, die eine CommonMark-Datei mit Metadaten oben ist (finden Sie unter [Arbeitsmappen Dateitypen](#workbooks-files-types) Weitere Einzelheiten, wie die Arbeitsmappen gespeichert werden können).

### <a name="nuget-package-support"></a>Unterstützung für NuGet-Paket

Viele gängige NuGet-Pakete werden direkt in Xamarin Workbooks unterstützt. Sie können nach Paketen suchen, indem Sie zu **Datei > Add Package**. Hinzufügen eines Pakets automatisch integrieren `#r` Anweisungen verweisen auf die paketassemblys, sodass Sie sie sofort verwenden.

Wenn Sie eine Arbeitsmappe mit paketverweisen speichern, werden ebenfalls diese Verweise gespeichert. Wenn Sie die Arbeitsmappe für andere Personen freigeben, wird der referenzierten Pakete automatisch heruntergeladen.

Es gibt einige bekannten Einschränkungen mit NuGet-Paket-Unterstützung in Arbeitsmappen:

- Native Bibliotheken sind, wird nur unter iOS unterstützt, und nur, wenn mit der verwalteten Bibliothek verknüpft.
- Pakete, mit denen abhängig `.targets` Dateien oder PowerShell-Skripts werden wahrscheinlich nicht funktionieren wie erwartet funktioniert.
- Zum Entfernen oder eine paketabhängigkeit ändern, müssen bearbeiten Sie der Arbeitsmappe Manifest mit einem Text-Editor. Richtige paketverwaltung ist auf dem Weg.

### <a name="xamarinforms-support"></a>Xamarin.Forms-Support

Wenn Sie das Xamarin.Forms-NuGet-Paket in der Arbeitsmappe verweisen, ändert sich die Arbeitsmappe-app die Hauptansicht, um die Xamarin.Forms-basiert ist. Sie erreichen ihn über `Xamarin.Forms.Application.Current.MainPage`.

Die Registerkarte "Ansicht Inspector" hat auch spezielle Unterstützung für die Hierarchie von Inhaltsansichten Xamarin.Forms können Sie besser verstehen Ihre Layouts angezeigt.

## <a name="rich-text-editing"></a>Bearbeiten von Rich-Text

Sie können den Text bearbeiten, um im Code enthalten, rich-Text-Editor verwenden, wie unten gezeigt:

![](workbook-images/inspector-0.6.2-editing.gif "Bearbeiten Sie den Text aus, um den Code, der mit dem integrierten rich-Text-editor")

### <a name="markdown-authoring"></a>Markdown-Dokumenterstellung

Autoren von Arbeitsmappen manchmal finden es möglicherweise einfacher, den CommonMark "Source" der Arbeitsmappe mit ihren bevorzugten Editor direkt zu bearbeiten.

Denken Sie daran, dass wenn Sie dann bearbeiten und speichern Sie die Arbeitsmappe innerhalb der Arbeitsmappen-Client, der CommonMark Text neu formatiert werden kann.

Beachten Sie, dass aufgrund der Erweiterung CommonMark wir verwenden, um die YAML-Metadaten in der Arbeitsmappendateien aktivieren `---` für diesen Zweck reserviert ist. Wenn Sie erstellen möchten [thematischen Seitenumbrüche](http://spec.commonmark.org/0.27/#thematic-break) in Ihren Texten, verwenden Sie `***` oder `___` stattdessen. Solche Unterbrechungen sollte vermieden werden, in Arbeitsmappen 1.2 und früher aufgrund eines Fehlers beim Speichern.

### <a name="improvements-in-workbooks-13"></a>Verbesserungen in Arbeitsmappen 1.3

Wir haben auch die markdownsyntax für die Quote von Block leicht, zur Verbesserung der Präsentation erweitert. Ein Emoji als das erste Zeichen in Ihrem Angebot Block hinzufügen, können Sie die Hintergrundfarbe des Angebots beeinflussen:

- `> [!NOTE]`
    > wird als eine Notiz mit einem blauen Hintergrund gerendert werden.
- `> [!IMPORTANT]`
    > wird als eine Warnung mit einem gelben Hintergrund gerendert werden.
- `> [!WARNING]`
    > wird als ein Problem mit einem roten Hintergrund gerendert werden.

Sie können auch mit Header in der Arbeitsmappe Dokument verknüpfen. Wir generieren Anker für die einzelnen Header, mit der Anker-ID wird der Text der Überschrift, wie folgt verarbeitet:

1. Der Header ist Kleinbuchstaben umgewandelt.
1. Alle Zeichen außer Zahlen und Bindestriche enthalten, werden entfernt.
1. Alle Leerzeichen werden durch Bindestriche ersetzt.

Dies bedeutet, dass einen Header wie "Wichtige Header" Id ruft `important-header` und können mit verknüpft werden, indem Sie einen Link zum Einfügen `#important-header` in der Arbeitsmappe.

## <a name="document-structure"></a>Dokumentstruktur

### <a name="cell"></a>Zelle

Eine getrennte Einheit von Inhalt, der ausführbare Code oder in Markdown darstellt. Eine codezelle besteht aus bis zu vier untergeordneten Komponenten:

- Editor
  - Puffer
- Compilerdiagnose
- Konsolenausgabe
- Ergebnisse der Ausführung

### <a name="editor"></a>Editor

Die interaktive Komponente einer Zelle. Codezellen ist dies die tatsächliche Code-Editor mit syntaxhervorhebung usw. Markdown-Zellen ist dies eine Inhalts-Rich-Text-Editor mit dem eine kontextabhängige formatieren und erstellen die Symbolleiste.

### <a name="buffer"></a>Puffer

Der eigentliche Text-Inhalt eines Editors.

### <a name="compiler-diagnostics"></a>Compilerdiagnose

Alle Diagnosen erstellt beim Kompilieren von Code, der gerendert, wenn explizite Ausführung angefordert wird. Unmittelbar unterhalb des Editors Zelle angezeigt.

### <a name="console-output"></a>Konsolenausgabe

Keine Ausgabe, die während der Ausführung der Zelle Standardausgabe oder Standardfehler geschrieben wird. Standardausgabe werden als schwarzer Text gerendert, und Standardfehler in rotem Text gerendert wird.

### <a name="execution-results"></a>Ergebnisse der Ausführung

Umfassende und potenziell interaktive Darstellungen der Ergebnisse für eine Zelle werden bei erfolgreicher Kompilierung gerendert werden, vorausgesetzt ein Ergebnis durch die Ausführung tatsächlich erstellt wird. Ausnahmen werden Ergebnisse in diesem Kontext berücksichtigt, da sie als Ergebnis der Ausführung tatsächlich der Kompilierung erstellt werden.

## <a name="workbooks-files-types"></a>Arbeitsmappen-Dateitypen

### <a name="plain-files"></a>Einfache Dateien

Standardmäßig eine Arbeitsmappe speichert, als nur-Text `.workbook` Datei, die CommonMark-formatiertem Text enthält.

### <a name="packages"></a>Pakete

Ein Paket für die Arbeitsmappe ist ein Verzeichnis, das mit dem Namen der `.workbook` Erweiterung.
Mac Finder und im Dialogfeld "Öffnen" (Xamarin Workbooks und zuletzt verwendete Dateien im Menü wird, dieses Verzeichnis erkannt, als handele es sich um eine Datei.

Das Verzeichnis muss enthalten eine `index.workbook` -Datei, die die tatsächlichen nur-Text-Arbeitsmappe ist, die in Xamarin Workbooks geladen werden. Das Verzeichnis kann auch über die erforderlichen Ressourcen enthalten `index.workbook`, einschließlich Bilder oder andere Dateien.

Wenn eine nur-Text `.workbook` Datei, die Ressourcen aus der gleichen Verzeichnis verweist, wird in Arbeitsmappen 0.99.3 geöffnet oder später, wenn es gespeichert wird, erfolgt eine Konvertierung in einen `.workbook` Paket. Dies gilt unter Mac und Windows.

> [!NOTE]
> Windows-Benutzer öffnet die `package.workbook\index.workbook` Datei direkt, aber andernfalls das Paket wird dasselbe Verhalten haben wie auf Mac.

### <a name="archives"></a>Archive

Arbeitsmappe-Pakete, werden die Verzeichnisse, können die einfache Verteilung über das Internet schwierig sein. Die Lösung ist die Archive der Arbeitsmappe. Ein Archiv der Arbeitsmappe ist ein Zip-komprimierte Arbeitsmappe-Paket, mit dem Namen und die `.workbook` Erweiterung.

Arbeitsmappen 1.1, das Sie beim Speichern eines Pakets für die Arbeitsmappe ab, bietet das Dialogfeld "Speichern" die Auswahl der stattdessen als Archiv speichern. Arbeitsmappen 1.0 hatte keine integrierte Möglichkeit des Erstellens oder Speichern von Archiven.

In Arbeitsmappen 1.0, wenn ein Archiv für die Arbeitsmappe geöffnet wurde, es transparent in ein Paket für die Arbeitsmappe konvertiert war und die Zip-Datei unterbrochen wurde. In Version 1.1 mit Arbeitsmappen bleibt die Zip-Datei. Wenn der Benutzer das Archiv speichert, wird er durch eine neue Zipdatei ersetzt.

Sie können ein Archiv der Arbeitsmappe manuell erstellen, indem Sie mit der rechten Maustaste in ein Paket für die Arbeitsmappe und auswählen **komprimieren** auf Mac oder **senden an > komprimierten (gezippten) Ordner** auf Windows. Klicken Sie dann benennen Sie die Zip-Datei, damit eine `.workbook` Dateierweiterung. Dies funktioniert nur mit nicht einfache Arbeitsmappendateien Arbeitsmappe-Paketen.

## <a name="related-links"></a>Verwandte Links

- [Willkommen bei Arbeitsmappen](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
