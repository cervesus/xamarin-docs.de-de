---
title: Text in Xamarin.Forms
description: Xamarin.Forms verfügt über drei primäre Ansichten zum Arbeiten mit Text, und in diesem Artikel wird erläutert, wie Sie mit, dass sie eingeben und Anzeigen von Text in Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
ms.openlocfilehash: 3e30e3d4ea3bf0ec58aff842ffee68885a47d44a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656190"
---
# <a name="text-in-xamarinforms"></a>Text in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Verwenden Xamarin.Forms, um Text eingeben oder anzeigen._

Xamarin.Forms verfügt über drei primäre Ansichten zum Arbeiten mit Text:

- **[Bezeichnung](#Label)**  &mdash; für einzelne oder mehrere Zeilen Text darstellen. Text mit mehreren Formatierungsoptionen können in der gleichen Zeile angezeigt werden.
- **[Eintrag](#Entry)**  &mdash; zum Eingeben des Texts, der nur eine Zeile ist. Eintrag verfügt über eine Kennwort-Modus.
- **[Editor](#Editor)**  &mdash; zum Eingeben des Texts, die mehr als eine Zeile sein könnten.

Textdarstellung kann geändert werden, mithilfe von integrierten oder benutzerdefinierten [Stile](#Styles) und einige Steuerelemente unterstützen benutzerdefinierte [Schriftarten](#Fonts).

<a name="Label" />

## <a name="labellabelmd"></a>[Bezeichnung](label.md)

Die `Label` Ansicht zum Anzeigen von Text verwendet wird. Sie können mehrere Zeilen Text oder eine einzelne Textzeile anzeigen. `Label` kann Text mit mehreren Formatierungsoptionen in Inline verwendet darstellen. Die Bezeichnung kann umschließen oder Abschneiden von Text, wenn es nicht in einer Zeile passen.

![](images/label.png "Beispiel für Bezeichnung")

Finden Sie unter den [Bezeichnung](label.md) Artikel Weitere Informationen.

Informationen zum Anpassen der Schriftart in eine Bezeichnung verwendet, finden Sie unter [Schriftarten](fonts.md).

<a name="Entry" />

## <a name="entryentrymd"></a>[Eingabe](entry.md)

`Entry` Dient zum einzeiligen Texteingaben akzeptieren. `Entry` Angebote Steuerung Farben und Schriftarten. `Entry` verfügt über eine Kennwortmodus und Platzhaltertext können angezeigt werden, solange Sie Text eingegeben wurde.

![](images/entry.png "Beispiel für einen Indexeintrag")

Finden Sie unter den [Eintrag](entry.md) Weitere Informationen.

Beachten Sie, dass im Gegensatz zu `Label`, `Entry` sind keine Einstellungen für die benutzerdefinierte Schriftart.

<a name="Editor" />

## <a name="editoreditormd"></a>[Editor](editor.md)

`Editor` wird verwendet, um mehrzeilige Texteingabe zu akzeptieren. `Editor` Angebote Steuerung Farben und Schriftarten.

![](images/editor.png "Beispiel für den Editor")

Finden Sie unter den [Editor](editor.md) Weitere Informationen.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[Schriftarten](fonts.md)

Viele Steuerelemente unterstützen unterschiedliche Schriftart Einstellungen mithilfe der integrierten Schriftarten auf jeder Plattform oder benutzerdefinierter Schriftarten, die in Ihrer APP enthalten sind. Finden Sie unter den [Schriftarten](fonts.md) Artikel Weitere Informationen.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Stile](styles.md)

Finden Sie unter [arbeiten mit Stilen](~/xamarin-forms/user-interface/styles/index.md) Informationen zum Einrichten der Schriftart [Farbe](~/xamarin-forms/user-interface/colors.md), und andere anzeigen-Eigenschaften, die für mehrere Steuerelemente gelten.

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
