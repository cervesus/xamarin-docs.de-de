---
ms.openlocfilehash: 14d4f5de500c8e8ced6cbeb67019f9152ed63df3
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118916"
---
# <a name="contributing"></a>Beitragen

Vielen Dank für Ihr Interesse und Ihre Bereitschaft, an der Xamarin-Dokumentation mitzuwirken!

Auf dieser Seite wird der grundlegende Prozess zum Aktualisieren von Inhalten in der [Xamarin-Dokumentation](https://docs.microsoft.com/xamarin) behandelt.

- [Lizenzvereinbarung für Mitwirkende](LICENSE)

## <a name="process-for-contributing"></a>Mitwirkungsprozess

### <a name="small-changes--edits"></a>Kleine Änderungen

Sie können auf jeder Seite auf **Bearbeiten** klicken und die GitHub-Website verwenden, um Korrekturen und kleine Änderungen vorzunehmen, oder Sie gehen folgendermaßen vor:

1. Forken Sie das Repository `MicrosoftDocs/xamarin-docs`.

2. Erstellen Sie einen `branch` für Ihre Änderungen.

3. Schreiben Sie Ihren Inhalt. Halten Sie sich dabei an die [Vorlage](contributing-guidelines/template.md) und an den [Styleguide](contributing-guidelines/voice-tone.md).

4. Übermitteln Sie einen Pull Request (PR) von Ihrem Branch an `MicrosoftDocs/xamarin-docs/live`.

5. Nehmen Sie, wie mit dem Team besprochen, alle erforderlichen Aktualisierungen in Ihrem Branch über den PR vor.

6. Nachdem das Feedback angewendet und die Änderung als in Ordnung eingestuft wurde, führen die Verwalter Ihren PR ein. Die Änderungen werden kurz darauf auf docs.microsoft.com angezeigt.


> [!NOTE]
> Wenn sich Ihr PR auf ein vorhandenes Problem bezieht, fügen Sie der Commit-Nachricht oder PR-Beschreibung das Schlüsselwort `Fixes #Issue_Number` hinzu, damit das Problem automatisch geschlossen werden kann, wenn der PR eingebunden wird. Weitere Informationen finden Sie unter [Closing issues via commit messages](https://help.github.com/articles/closing-issues-via-commit-messages/) (Schließen von Tickets mithilfe von Commit-Nachrichten).


### <a name="big-changes-or-new-content"></a>Umfangreiche Änderungen oder neue Inhalte

Für umfangreichere Beiträge und neue Inhalte [öffnen Sie ein Issue](https://github.com/MicrosoftDocs/xamarin-docs/issues), in dem Sie sowohl den Artikel beschreiben, den Sie schreiben möchten, als auch beschreiben, wie er mit vorhandenen Inhalten zusammenhängt. Der Inhalt im Ordner „docs“ ist in Abschnitte unterteilt, die nach Produktbereichen (z.B. Android und iOS) strukturiert sind. Versuchen Sie, den richtigen Ordner für Ihren neuen Inhalt zu ermitteln. 

**Sie erhalten über das Issue Feedback zu Ihrem Vorschlag. Beginnen Sie erst danach mit dem Schreiben.**

Wenn es sich um ein neues Thema handelt, können Sie diese [Vorlagendatei](../contributing-guidelines/template.md) als Ausgangspunkt verwenden. Sie enthält die Richtlinien zum Schreiben und erläutert auch die Metadaten, die für jeden Artikel erforderlich sind (z. B. Informationen zum Autor).

Wenn Sie über Bilder und andere statische Ressourcen verfügen, fügen Sie diese zu dem Unterordner **\<mypage>-images** hinzu. Wenn Sie einen neuen Ordner für Inhalte erstellen, fügen Sie diesem neuen Ordner einen Ordner für Bilder hinzu.

#### <a name="example-structure"></a>Beispielstruktur

```
docs
    /android
        mypage.md
        /mypage-images
            some-image.png
```

Achten Sie darauf, die richtige Markdownsyntax einzuhalten. Weitere Informationen finden Sie im [Styleguide](../contributing-guidelines/template.md).

Die tatsächlichen Schritte zum Einreichen sind identisch mit denen für kleine Änderungen ([oben](#process-for-contributing)).

Das Xamarin-Team überprüft Ihren PR und teilt Ihnen (über Feedback zum PR) mit, ob die Änderung in Ordnung ist oder ob für die Genehmigung noch weitere Aktualisierungen/Änderungen erforderlich sind.

Nachdem das Feedback angewendet und die Änderung als in Ordnung eingestuft wurde, führen die Verwalter Ihren PR ein.

Alle Commits werden in einem bestimmten Rhythmus per Push aus dem Masterbranch an die Livewebsite übertragen. Dann können Sie Ihren Beitrag auf https://docs.microsoft.com/xamarin/ sehen.

## <a name="dos-and-donts"></a>Verhaltensregeln

Es folgt eine kurze Liste mit Anleitungsregeln, die Sie beachten sollten, wenn Sie an der .NET-Dokumentation mitwirken.

- **NEIN**: Überraschen Sie uns nicht mit umfangreichen Pull Requests. Eröffnen Sie stattdessen ein Ticket, und starten Sie eine Diskussion, damit wir uns auf eine Richtung einigen können, bevor Sie sehr viel Zeit investieren.
- **JA** Lesen Sie den [Styleguide](contributing-guidelines/template.md) und die Richtlinien bezüglich [Sprache und Schreibstil](contributing-guidelines/voice-tone.md).
- **JA** Verwenden Sie die [Vorlagendatei](contributing-guidelines/template.md) als Ausgangspunkt für Ihre Arbeit.
- **JA** Erstellen Sie einen separaten Branch in Ihrer Verzweigung, bevor Sie an den Artikeln arbeiten.
- **JA** Folgen Sie dem [GitHub-Flow-Workflow](https://guides.github.com/introduction/flow/).
- **JA** Schreiben Sie häufig Blogs und Tweets (oder was auch immer) über Ihre Beiträge!

> [!NOTE]
> Sie stellen möglicherweise fest, dass sich derzeit einige der Themen nicht nach allen hier und im [Styleguide](contributing-guidelines/template.md) definierten Richtlinien richten. Wir arbeiten daran, auf der gesamten Website Konsistenz zu erzielen. 


