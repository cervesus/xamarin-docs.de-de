---
title: Text inXamarin.Forms
description: Xamarin.Formsverfügt über drei primäre Ansichten zum Arbeiten mit Text. in diesem Artikel wird erläutert, wie Sie diese zum eingeben und Anzeigen von Text in- Xamarin.Forms Anwendungen verwenden.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b44a0b3e3542638874ee366a86967d73c0d53652
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573793"
---
# <a name="text-in-xamarinforms"></a>Text inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Mithilfe Xamarin.Forms von können Sie Text eingeben oder anzeigen._

Xamarin.Formsverfügt über drei primäre Ansichten zum Arbeiten mit Text:

- **[Bezeichnung](#label)** &mdash; zum Darstellen von Text in einem oder mehreren Zeilen. Kann Text mit mehreren Formatierungsoptionen in derselben Zeile anzeigen.
- **[Eintrag](#entry)** &mdash; zum Eingeben von Text, der nur eine Zeile ist. Der Eintrag hat einen Kenn Wort Modus.
- **[Editor](#editor)** &mdash; zum Eingeben von Text, der mehr als eine Zeile annehmen kann.

Die Text Darstellung kann mithilfe integrierter oder benutzerdefinierter [Stile](#styles) geändert werden, und einige Steuerelemente unterstützen benutzerdefinierte [Schriftarten](#fonts).

## <a name="label"></a>[Bezeichnung](label.md)

Die `Label` Ansicht wird zum Anzeigen von Text verwendet. Es können mehrere Textzeilen oder eine einzelne Textzeile angezeigt werden. `Label`kann Text mit mehreren Formatierungsoptionen darstellen, die in Inline verwendet werden. Die Beschriftungs Ansicht kann Text wrappen oder abschneiden, wenn er nicht in eine Zeile passt.

![Beispiel für eine Bezeichnung](images/label.png)

Ausführlichere Informationen finden Sie im Artikel zur [Bezeichnung](label.md) .

Informationen zum Anpassen der Schriftart, die in einer Bezeichnung verwendet wird, finden Sie unter [Schriftarten](fonts.md).

## <a name="entry"></a>[Eingabe](entry.md)

`Entry`wird verwendet, um einzeilige Texteingaben zu akzeptieren. `Entry`bietet Kontrolle über Farben und Schriftarten. `Entry`hat einen Kenn Wort Modus und kann Platzhalter Text anzeigen, bis Text eingegeben wird.

![Beispiel für den Eintrag](images/entry.png)

Weitere Informationen finden Sie im Artikel zum [Eintrag](entry.md) .

Beachten Sie, dass im Gegensatz zu `Label` `Entry` keine benutzerdefinierten Schriftart Einstellungen aufweisen können.

## <a name="editor"></a>[Editor](editor.md)

`Editor`wird verwendet, um mehrzeilige Texteingaben zu akzeptieren. `Editor`bietet Kontrolle über Farben und Schriftarten.

![Editor-Beispiel](images/editor.png)

Weitere Informationen finden Sie im [Editor](editor.md) -Artikel.

## <a name="fonts"></a>[Fonts](fonts.md)

Viele Steuerelemente unterstützen unterschiedliche Schriftart Einstellungen mithilfe der integrierten Schriftarten auf jeder Plattform oder benutzerdefinierter Schriftarten, die in Ihrer APP enthalten sind. Ausführlichere Informationen finden Sie im Artikel zu [Schriftarten](fonts.md) .

## <a name="styles"></a>[Stile](styles.md)

Informationen zum Einrichten von Schriftart, [Farbe](~/xamarin-forms/user-interface/colors.md)und anderen Anzeigeeigenschaften, die über mehrere Steuerelemente gelten, finden Sie unter [Arbeiten mit Stilen](~/xamarin-forms/user-interface/styles/index.md) .

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
