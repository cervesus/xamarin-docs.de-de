---
title: Interaktive Arbeitsmappen
description: In diesem Dokument wird beschrieben, wie Sie mit Xamarin Workbooks Live Dokumente C# erstellen, die Code zum Experimentieren, unterrichten, trainieren oder untersuchen enthalten.
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 7b3c356460d9427821843dc084b3f306c026ffa0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293085"
---
# <a name="interactive-workbooks"></a>Interaktive Arbeitsmappen

Sie können Arbeitsmappen als eigenständige Anwendung verwenden, getrennt von Ihrer IDE.

Um mit dem Erstellen einer neuen Arbeitsmappe zu beginnen, führen Sie die Arbeitsmappe App aus. Wenn Sie dies nicht bereits installiert haben, besuchen Sie die Seite [Installation](~/tools/workbooks/install.md#install) . Sie werden aufgefordert, eine Arbeitsmappe auf der Plattform Ihrer Wahl zu erstellen. Dadurch wird automatisch eine Verbindung mit einer Agent-App hergestellt, sodass Sie Ihr Dokument in Echtzeit visualisieren können.

Wenn die Arbeitsmappen-App bereits ausgeführt wird, können Sie ein neues Dokument erstellen, indem Sie zu **Datei > neu**navigieren.

Arbeitsmappen können gespeichert und später in der Anwendung geöffnet werden. Anschließend können Sie Sie für andere Personen freigeben, um Ideen zu veranschaulichen, neue APIs kennenzulernen oder neue Konzepte zu vermitteln.

## <a name="code-editing"></a>Code Bearbeitung

Das Fenster Code Bearbeitung bietet Codevervollständigung, Syntax Farbgebung, Inline-Live Diagnose und mehrzeilige Anweisungs Unterstützung.

[![](workbook-images/inspector-0.6.0-repl-small.png "Das Fenster Code Bearbeitung bietet Codevervollständigung, Syntax Farben, Inline-Live Diagnose und mehrzeilige Anweisungs Unterstützung.")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin Workbooks werden in einer `.workbook` Datei gespeichert, bei der es sich um eine commonmark-Datei mit einigen Metadaten im oberen Bereich handelt (Weitere Informationen zum Speichern von Arbeitsmappen finden Sie unter [arbeitsmappendateitypen](#workbooks-files-types) ).

### <a name="nuget-package-support"></a>Unterstützung für nuget-Pakete

Viele beliebte nuget-Pakete werden direkt in Xamarin Workbooks unterstützt. Sie können nach Paketen suchen, indem Sie zu **Datei > Paket hinzufügen**navigieren. Das Hinzufügen eines Pakets führt automatisch `#r` Anweisungen aus, die auf paketassemblys verweisen, sodass Sie Sie sofort verwenden können.

Wenn Sie eine Arbeitsmappe mit Paket verweisen speichern, werden diese Verweise ebenfalls gespeichert. Wenn Sie die Arbeitsmappe für eine andere Person freigeben, werden die Pakete, auf die verwiesen wird, automatisch heruntergeladen.

Es gibt einige bekannte Einschränkungen bei der nuget-Paket Unterstützung in-Arbeitsmappen:

- Native Bibliotheken werden nur unter IOS unterstützt, und zwar nur, wenn Sie mit der verwalteten Bibliothek verknüpft sind.
- Pakete, die von `.targets` Dateien oder PowerShell-Skripts abhängen, funktionieren wahrscheinlich nicht erwartungsgemäß.
- Um eine Paketabhängigkeit zu entfernen oder zu ändern, bearbeiten Sie das Manifest der Arbeitsmappe mit einem Text-Editor. Die richtige Paketverwaltung ist auf dem Weg.

### <a name="xamarinforms-support"></a>Xamarin. Forms-Unterstützung

Wenn Sie in der-Arbeitsmappe auf das xamarin. Forms-nuget-Paket verweisen, wird die Hauptansicht der arbeitsmappenapp in xamarin. Forms-basiert geändert. Sie können über `Xamarin.Forms.Application.Current.MainPage`auf das zugreifen.

Die Registerkarte "View Inspector" bietet auch eine besondere Unterstützung für das Anzeigen der xamarin. Forms-Ansichts Hierarchie, damit Sie Ihre Layouts besser verstehen

## <a name="rich-text-editing"></a>Rich-Text-Bearbeitung

Sie können den Text um den Code umschließen, indem Sie den Rich-Text-Editor verwenden, wie unten dargestellt:

![](workbook-images/inspector-0.6.2-editing.gif "Bearbeiten Sie den Text um den Code, indem Sie den integrierten Rich-Text-Editor verwenden.")

### <a name="markdown-authoring"></a>Markdown-Erstellung

Arbeitsmappenautoren finden es manchmal einfacher, die commonmark-Quelle der Arbeitsmappe mit Ihrem bevorzugten Editor direkt zu bearbeiten.

Beachten Sie, dass der commonmark-Text ggf. neu formatiert werden kann, wenn Sie die Arbeitsmappe im Arbeitsmappen-Client bearbeiten und speichern.

Beachten Sie, dass wir aufgrund der commonmark-Erweiterung, die wir zum Aktivieren von YAML-Metadaten `---` in Arbeitsmappendateien verwenden, für diesen Zweck reserviert ist. Wenn Sie [Themen Umbrüche](http://spec.commonmark.org/0.27/#thematic-break) im Text erstellen möchten, sollten Sie stattdessen oder `***` `___` verwenden. Solche Unterbrechungen sollten in den Arbeitsmappen 1,2 und früher aufgrund eines Fehlers beim Speichern vermieden werden.

### <a name="improvements-in-workbooks-13"></a>Verbesserungen in Arbeitsmappen 1,3

Wir haben auch die Syntax für markdownblockanführungs Zeichen leicht erweitert, um die Darstellung zu verbessern. Durch das Hinzufügen eines Emoji als erstes Zeichen im Block Anführungszeichen können Sie die Hintergrundfarbe des Angebots beeinflussen:

- `> [!NOTE]`
    > wird als Hinweis mit einem blauen Hintergrund gerbt.
- `> [!IMPORTANT]`
    > wird als Warnung mit einem gelben Hintergrund dargestellt.
- `> [!WARNING]`
    > wird als ein Problem mit einem roten Hintergrund dargestellt.

Sie können auch eine Verknüpfung mit Headern im arbeitsmappendokument herstellen. Wir generieren Anker für jeden Header, wobei die Anker-ID der Header Text ist, der wie folgt verarbeitet wird:

1. Der Header ist klein geschrieben.
1. Alle Zeichen außer alphanumerische Zeichen und Bindestriche werden entfernt.
1. Alle Leerzeichen werden durch Bindestriche ersetzt.

Dies bedeutet, dass ein Header wie "wichtiger Header" eine ID von `important-header` erhält und durch Einfügen eines Links zu `#important-header` in der Arbeitsmappe verknüpft werden kann.

## <a name="document-structure"></a>Dokumentstruktur

### <a name="cell"></a>Kern

Eine diskrete Inhalts Einheit, die entweder ausführbaren Code oder markdown darstellt. Eine codezelle besteht aus bis zu vier unter Komponenten:

- Editor
  - Puffer
- Compilerdiagnose
- Konsolenausgabe
- Ausführungs Ergebnisse

### <a name="editor"></a>Editor

Die interaktive Textkomponente einer Zelle. Bei Code Zellen ist dies der tatsächliche Code-Editor mit Syntax Hervorhebung usw. Bei markdown-Zellen handelt es sich hierbei um einen Rich-Text-Inhalts-Editor mit einer Kontextsensiblen Symbolleiste für Formatierung und Erstellung.

### <a name="buffer"></a>Puffer

Der tatsächliche Text Inhalt eines Editors.

### <a name="compiler-diagnostics"></a>Compilerdiagnose

Alle Diagnosen, die beim Kompilieren von Code erzeugt wurden, werden nur gerendert, wenn eine explizite Ausführung angefordert Wird direkt unter dem Zellen-Editor angezeigt.

### <a name="console-output"></a>Konsolenausgabe

Jede Ausgabe, die bei der Ausführung der Zelle in Standard-out-oder Standard-Fehler geschrieben wurde. Standard out wird in schwarzem Text gerendert, und Standardfehler werden in rotem Text gerendert.

### <a name="execution-results"></a>Ausführungs Ergebnisse

Umfassende und potenziell interaktive Darstellungen von Ergebnissen für eine Zelle werden bei erfolgreicher Kompilierung gerendert, wenn ein Ergebnis tatsächlich durch die Ausführung erzeugt wird. Ausnahmen werden in diesem Kontext als Ergebnisse betrachtet, da Sie als Ergebnis der eigentlichen Ausführung der Kompilierung erstellt werden.

## <a name="workbooks-files-types"></a>Arbeitsmappen-Dateitypen

### <a name="plain-files"></a>Einfache Dateien

Standardmäßig wird eine Arbeitsmappe als nur-Text `.workbook` -Datei gespeichert, die commonmark-formatierten Text enthält.

### <a name="packages"></a>Pakete

Ein arbeitsmappenpaket ist ein Verzeichnis, das mit der `.workbook` Erweiterung benannt wird.
Auf dem Macintosh-Finder und im Menü "Xamarin Workbooks öffnen" und "zuletzt verwendete Dateien" wird dieses Verzeichnis so erkannt, als wäre es eine Datei.

Das Verzeichnis muss eine `index.workbook` -Datei enthalten, bei der es sich um die tatsächliche nur-Text-Arbeitsmappe handelt, die in Xamarin Workbooks geladen wird. Das Verzeichnis kann auch Ressourcen enthalten, die `index.workbook`für erforderlich sind, einschließlich Bilder oder andere Dateien.

Wenn eine nur- `.workbook` Text-Datei, die auf Ressourcen aus demselben Verzeichnis verweist, in Arbeitsmappen geöffnet wird 0.99.3 oder später, wenn Sie gespeichert wird, wird `.workbook` Sie in ein Paket konvertiert. Dies gilt sowohl für Mac als auch für Windows.

> [!NOTE]
> Windows-Benutzer öffnen die `package.workbook\index.workbook` Datei direkt, aber andernfalls verhält sich das Paket genauso wie auf dem Mac.

### <a name="archives"></a>Aufbewahrt

Arbeitsmappenpakete, die Verzeichnisse sind, können problemlos über das Internet verteilt werden. Die Lösung ist arbeitsmappenarchive. Ein arbeitsmappenarchiv ist ein ZIP-komprimiertes arbeitsmappenpaket mit `.workbook` dem Namen mit der Erweiterung.

Beim Speichern eines arbeitsmappenpakets in Arbeitsmappen 1,1 bietet das Dialogfeld Speichern stattdessen die Möglichkeit, stattdessen als Archiv zu speichern. Arbeitsmappen 1,0 verfügten über keine integrierte Methode zum Erstellen oder Speichern von Archiven.

Beim Öffnen eines arbeitsmappenarchivs in Arbeitsmappen 1,0 wurde es transparent in ein arbeitsmappenpaket konvertiert, und die ZIP-Datei ging verloren. In den-Arbeitsmappen 1,1 bleibt die ZIP-Datei erhalten. Wenn der Benutzer das Archiv speichert, wird es durch eine neue ZIP-Datei ersetzt.

Sie können ein arbeitsmappenarchiv manuell erstellen, indem Sie mit der rechten Maustaste auf ein arbeitsmappenpaket klicken und unter Mac **komprimieren** auswählen oder **an > komprimierten (gezippten) Ordner** unter Windows senden. Benennen Sie dann die ZIP-Datei mit `.workbook` einer Dateierweiterung um. Dies funktioniert nur bei arbeitsmappenpaketen, nicht bei reinen Arbeitsmappendateien.

## <a name="related-links"></a>Verwandte Links

- [Willkommen bei Arbeitsmappen](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
