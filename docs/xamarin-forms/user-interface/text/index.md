---
title: Text in Xamarin.Forms
description: Xamarin.Forms verfügt über drei primäre Ansichten zum Arbeiten mit Text, und in diesem Artikel wird erläutert, wie Sie mit, dass sie eingeben und Anzeigen von Text in Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
ms.openlocfilehash: 60dd54ff8ed06cbeea3a3e202e7058ea7747ea3d
ms.sourcegitcommit: 06a52ac36031d0d303ac7fc8163a59c178799c80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50911566"
---
# <a name="text-in-xamarinforms"></a>Text in Xamarin.Forms

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

Die `Label` -Steuerelement unterstützt andere Einstellungen, die unter Verwendung der integrierten Schriftarten auf jeder Plattform oder benutzerdefinierte Schriftarten, die mit Ihrer app enthalten. Finden Sie unter den [Schriftarten](fonts.md) Artikel Weitere Informationen.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Stile](styles.md)

Finden Sie unter [arbeiten mit Stilen](~/xamarin-forms/user-interface/styles/index.md) Informationen zum Einrichten der Schriftart [Farbe](~/xamarin-forms/user-interface/colors.md), und andere anzeigen-Eigenschaften, die für mehrere Steuerelemente gelten.

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
