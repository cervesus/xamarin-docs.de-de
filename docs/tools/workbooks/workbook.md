---
title: Interaktive Arbeitsmappen
description: Verwenden Sie Arbeitsmappen erstellen von live-Dokumenten mit C#-Code zum experimentieren, durcharbeiten, Training oder durchsuchen.
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: de88bbc9bc45b8a6326924d964bdd9385acb82aa
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="interactive-workbooks"></a>Interaktive Arbeitsmappen

_Verwenden Sie Arbeitsmappen erstellen von live-Dokumenten mit C#-Code zum experimentieren, durcharbeiten, Training oder durchsuchen._

Sie können Arbeitsmappen als eigenständige Anwendung, unabhängig von der IDE verwenden.

Zum Erstellen einer neuen Arbeitsmappe starten, führen Sie die Arbeitsmappen-app. Wenn Sie dies noch nicht installiert haben, besuchen Sie die [Installation](~/tools/workbooks/install.md#install) Seite. Sie werden aufgefordert, die zum Erstellen einer Arbeitsmappe in der Plattform Ihrer Wahl, der eine Agent-app, und Sie können das Dokument in Echtzeit zu visualisieren automatisch eine Verbindung herstellt.

Wenn die Arbeitsmappen app bereits ausgeführt wird, können Sie ein neues Dokument erstellen, indem Sie zu **Datei > neues**.

Arbeitsmappen können gespeichert und in der Anwendung zu einem späteren Zeitpunkt erneut geöffnet werden. Sie können dann Benutzer freigeben Ideen veranschaulichen, untersuchen die neuen APIs oder neue Konzepte werden folgende Themen behandelt.

## <a name="code-editing"></a>Bearbeiten von Code

Der Code, der Bearbeitungsfenster bietet codevervollständigung, Syntaxfarben, Inline-live-Diagnose und Unterstützung für mehrzeilige-Anweisung.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "Der Code Bearbeitungsfenster bietet codevervollständigung, Syntaxfarben, Inline-live-Diagnose und Unterstützung für mehrzeilige-Anweisung")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin-Arbeitsmappen werden gespeichert, einer `.workbook` -Datei, die eine CommonMark-Datei mit Metadaten oben ist (finden Sie unter [Arbeitsmappen Dateitypen](#workbooks-files-types) detaillierte Informationen auf wie die Arbeitsmappen gespeichert werden können).

### <a name="nuget-package-support"></a>NuGet-Paket-Unterstützung

Viele gängige NuGet-Pakete werden direkt in die Xamarin-Arbeitsmappen unterstützt. Sie können für Pakete suchen, indem Sie zu **Datei > Paket hinzufügen**. Hinzufügen eines Pakets wird automatisch importiert werden sollen `#r` Anweisungen verweisen auf Assemblys von Paket, und Sie können sie sofort verwenden.

Wenn Sie eine Arbeitsmappe mit Paketverweise speichern, werden diese Verweise sowie gespeichert. Wenn Sie die Arbeitsmappe für andere Personen freigeben, wird die referenzpakete automatisch heruntergeladen.

Es gibt einige bekannte Einschränkungen mit NuGet-Paket-Unterstützung in Arbeitsmappen:

  * Systemeigene Bibliotheken sind nur für iOS, und nur dann unterstützt, wenn mit der verwalteten Bibliothek verknüpft.
  * Pakete, die von abhängig sind `.targets` Dateien oder PowerShell-Skripts können wahrscheinlich nicht wie erwartet funktionieren.
  * Zum Entfernen oder Ändern einer paketabhängigkeit, bearbeiten Sie die Arbeitsmappe-Manifest mit einem Text-Editor. Richtige paketverwaltung ist auf dem Weg.

### <a name="xamarinforms-support"></a>Xamarin.Forms-Unterstützung

Wenn Sie das Xamarin.Forms-NuGet-Paket in der Arbeitsmappe verweisen, ändert sich die Arbeitsmappe app die Hauptansicht um Xamarin.Forms-basiert ist. Sie erreichen dies über `Xamarin.Forms.Application.Current.MainPage`.

Die Registerkarte "Ansicht Inspektor" hat auch spezielle Unterstützung für das Anzeigen der Hierarchie Xamarin.Forms anzeigen, können Sie die Layouts besser verstehen.

## <a name="rich-text-editing"></a>Rich-Text bearbeiten

Sie können den Text bearbeiten, um Ihren Code mit der rich-Text-Editor enthalten, wie unten gezeigt:

![](workbook-images/inspector-0.6.2-editing.gif "Bearbeiten Sie den Text aus, um den Code mit dem integrierten rich-Text-editor")

### <a name="markdown-authoring"></a>Erstellen von markdown

Arbeitsmappenautoren möglicherweise manchmal für eine direkte Bearbeitung von der CommonMark "Quelle", der die Arbeitsmappe mit ihren bevorzugten Editor einfacher.

Denken Sie daran, dass wenn Sie anschließend bearbeiten und speichern Sie die Arbeitsmappe innerhalb der Arbeitsmappen-Client, CommonMark Text formatiert werden kann.

Beachten Sie, dass aufgrund der Erweiterung CommonMark verwenden wir YAML Metadaten in Arbeitsmappendateien aktivieren `---` für diesen Zweck reserviert ist. Wenn Sie möchten [thematische Pausen](http://spec.commonmark.org/0.27/#thematic-break) in Ihrem Text sollten Sie verwenden `***` oder `___` stattdessen. Solche Pausen sollte vermieden werden, in Arbeitsmappen 1.2 und früher aufgrund eines Fehlers beim Speichern.

### <a name="improvements-in-workbooks-13"></a>Verbesserungen in Arbeitsmappen 1.3

Wir haben auch die Markdown-Syntax zum Angebot Block etwas, zur Verbesserung der Präsentation erweitert. Hinzufügen einer Emoji als erstes Zeichen in Ihr Angebot blockieren, können Sie die Farbe des Hintergrunds des Angebots beeinflussen:

- `> [!NOTE]
>"wird als Knoten mit einem blauen Hintergrund gerendert
- `> [!IMPORTANT]
>' wird als Warnung mit einem gelben Hintergrund gerendert
- `> [!WARNING]
>"wird als ein Problem mit einem roten Hintergrund dargestellt

Sie können auch mit Kopfzeilen im Dokument Arbeitsmappe verknüpfen. Wir generieren Anker für jeden Header mit der Premium-ID wird von den Headertext wie folgt verarbeitet:

1. Der Header ist untere-Schreibweise angegeben.
1. Entfernt alle Zeichen außer alphanumerische Zeichen und Bindestriche enthalten.
1. Alle Leerzeichen werden durch Bindestriche ersetzt.

Dies bedeutet, dass einen Header wie "Wichtige Header" Id ruft `important-header` und können mit verknüpft werden, indem Sie einen Link zum Einfügen `#important-header` in der Arbeitsmappe.

## <a name="document-structure"></a>Dokumentstruktur

### <a name="cell"></a>Zelle

Eine getrennte Einheit von Inhalten, die ausführbaren Code oder Markdown darstellt. Eine Zelle Code besteht aus bis zu vier untergeordneten Komponenten:

- Editor
  - Puffer
- Compilerdiagnose
- Konsolenausgabe
- Ausführungsergebnisse

### <a name="editor"></a>Editor

Die interaktive Textkomponente einer Zelle. Für Code Zellen ist dies der tatsächliche Code-Editor mit syntaxhervorhebung usw. an. Für Markdown Zellen ist dies eine Inhalts-Rich-Text-Editor mit einer kontextabhängig formatieren und Erstellen von Symbolleisten.

### <a name="buffer"></a>Puffer
Der tatsächliche Textinhalt eines Editors.

### <a name="compiler-diagnostics"></a>Compilerdiagnose

Alle Diagnosen erstellt beim Kompilieren von Code, gerendert, wenn explizite Ausführung angefordert wird. Unmittelbar unterhalb des Editors Zelle angezeigt.

### <a name="console-output"></a>Konsolenausgabe

Keine Ausgabe, die als Standardausgabe oder Standardfehler geschrieben wird, während der Ausführung der Zelle. Standardausgabe werden als schwarzer Text gerendert und Standardfehler werden in roter Schrift dargestellt.

### <a name="execution-results"></a>Ausführungsergebnisse

Umfassende und potenziell interaktive Darstellungen der Ergebnisse für eine Zelle werden nach der erfolgreichen Kompilierung gerendert werden, sofern durch die Ausführung tatsächlich ein Ergebnis erzeugt wird. Ausnahmen werden Ergebnisse in diesem Kontext berücksichtigt, da sie aufgrund der tatsächlich Ausführung der Kompilierung erstellt wurden.

## <a name="workbooks-files-types"></a>Arbeitsmappen-Dateitypen

### <a name="plain-files"></a>Einfache Dateien

Standardmäßig eine Arbeitsmappe speichert, als nur-Text `.workbook` Datei, die CommonMark-formatiertem Text enthält.

### <a name="packages"></a>Pakete

Ein Paket für die Arbeitsmappe ist ein Verzeichnis, das mit dem Namen der `.workbook` Erweiterung.
Auf Macs Finder und im Dialogfeld "Öffnen" (Xamarin-Arbeitsmappen und zuletzt verwendete Dateien im Menü wird dieses Verzeichnis erkannt werden, als handele es sich um eine Datei.

Das Verzeichnis darf eine `index.workbook` -Datei, die die tatsächlichen nur-Text-Arbeitsmappe ist, die in die Xamarin-Arbeitsmappen geladen werden. Das Verzeichnis kann auch von benötigten Ressourcen enthalten `index.workbook`, z. B. Bilder oder andere Dateien.

Wenn ein nur-Text `.workbook` geöffneten Datei, die Ressourcen aus der gleichen Verzeichnis verweist in Arbeitsmappen 0.99.3 oder später, wenn er gespeichert wird, erfolgt eine Konvertierung in einen `.workbook` Paket. Dies ist "true" bei Macintosh- und Windows.

> [!NOTE]
> Windows-Benutzer werden geöffnet. die `package.workbook\index.workbook` direkt Datei jedoch andernfalls das Paket wird weisen das gleiche Verhalten wie auf Mac

### <a name="archives"></a>Archive

Arbeitsmappe-Pakete, die Verzeichnisse, wird vorstellbar über das Internet vereinfacht die Verteilung schwer. Die Lösung besteht Arbeitsmappe Archive. Ein Archiv der Arbeitsmappe ist ein Zip-komprimierte Arbeitsmappe-Paket, mit dem Namen der `.workbook` Erweiterung.

Beim Speichern eines Pakets Arbeitsmappe Arbeitsmappen 1.1 ab, bietet das Dialogfeld "Speichern" die Option stattdessen als ein Archiv zu speichern. Arbeitsmappen 1.0 hatte keine integrierte Option zum Erstellen oder Speichern von Archiven.

In Arbeitsmappen 1.0, wenn ein Archiv Arbeitsmappe geöffnet wurde, er transparent in ein Paket für die Arbeitsmappe konvertiert und die Zip-Datei unterbrochen wurde. In Arbeitsmappen 1.1 bleibt die Zip-Datei. Wenn der Benutzer das Archiv speichert, wird es mit einer neuen Zipdatei ersetzt.

Sie können eine Arbeitsmappe Archiv manuell erstellen, indem Sie ein Paket für die Arbeitsmappe mit der rechten Maustaste und auswählen **komprimieren** auf Mac oder **senden an > komprimierten (gezippten) Ordner** unter Windows. Benennen Sie die Zip-Datei, damit eine `.workbook` Dateierweiterung. Dies funktioniert nur mit nicht plain Arbeitsmappendateien Arbeitsmappe-Paketen.

## <a name="related-links"></a>Verwandte Links

- [Willkommen beim Arbeitsmappen](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
