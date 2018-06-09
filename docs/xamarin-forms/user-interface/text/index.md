---
title: Text in Xamarin.Forms
description: Xamarin.Forms verfügt über drei Hauptansichten zum Arbeiten mit Text und in diesem Artikel wird erläutert, wie Sie verwenden, um eingeben und Anzeigen von Texten in Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c5bd157299c9388b561f316e65f2ba290bd15224
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244986"
---
# <a name="text-in-xamarinforms"></a>Text in Xamarin.Forms

_Verwenden Xamarin.Forms, um Text eingeben oder anzeigen._

Xamarin.Forms verfügt über drei Hauptansichten zum Arbeiten mit Text:

- **[Bezeichnung](#Label)**  &mdash; für einzelne oder mehrere Zeilen Text darstellen. Text mit mehreren Formatierungsoptionen können in der gleichen Zeile angezeigt werden.
- **[Eintrag](#Entry)**  &mdash; zum Eingeben von Text, der nur eine Linie ist. Eintrag ist einen Kennwort-Modus.
- **[Editor](#Editor)**  &mdash; zum Eingeben von Text, der mehr als eine Zeile einige Zeit dauern kann.

Textdarstellung kann geändert werden, mithilfe der integrierten oder benutzerdefinierten [Stile](#Styles) und einige Steuerelemente unterstützen benutzerdefinierte [Schriftarten](#Fonts).

<a name="Label" />

## <a name="labellabelmd"></a>[Bezeichnung](label.md)

Die `Label` Ansicht zum Anzeigen von Text verwendet wird. Es kann mehrere Textzeilen oder eine einzelne Textzeile anzeigen. `Label` mehrere Formatierungsoptionen in Inline verwendeten kann Text stellen. Die Bezeichnung Sicht kann umschließen oder Text abgeschnitten, wenn er nicht in einer Zeile passen.

![](images/label.png "Beispiel für Bezeichnung")

Finden Sie unter der [Bezeichnung](label.md) Artikel detailliertere Informationen.

Informationen zum Anpassen der Schriftart in eine Bezeichnung finden Sie unter [Schriftarten](fonts.md).

<a name="Entry" />

## <a name="entryentrymd"></a>[Eingabe](entry.md)

`Entry` wird verwendet, um einzeilige Texteingabe zu akzeptieren. `Entry` Angebote steuern, Farben, jedoch nicht möglich, Schriftarten, nicht angepasst haben. `Entry` besitzt einen Kennwortmodus, und Platzhaltertext können angezeigt werden, solange Text eingegeben wird.

![](images/entry.png "Beispiel für einen Indexeintrag")

Finden Sie unter der [Eintrag](entry.md) Artikel finden Sie weitere Informationen.

Beachten Sie, dass im Gegensatz zu `Label`, `Entry` sind keine Einstellungen für benutzerdefinierte Schriftart.

<a name="Editor" />

## <a name="editoreditormd"></a>[Editor](editor.md)

`Editor` wird verwendet, um für mehrzeilige Texteingabe zu akzeptieren. `Editor` kann eine benutzerdefinierte Hintergrundfarbe, aber die Textfarbe und Schriftart kann nicht geändert werden.

![](images/editor.png "Editor-Beispiel")

Finden Sie unter der [Editor](editor.md) Artikel finden Sie weitere Informationen.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[Schriftarten](fonts.md)

Die `Label` Steuerelement unterstützt verschiedene schriftarteinstellungen unter Verwendung der integrierten Schriftarten auf jeder Plattform oder benutzerdefinierte Schriftarten, die mit der app enthalten. Finden Sie unter der [Schriftarten](fonts.md) Artikel detailliertere Informationen.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Stile](styles.md)

Finden Sie unter [arbeiten mit Formatvorlagen](~/xamarin-forms/user-interface/styles/index.md) Informationen zum Einrichten der Schriftart, [Farbe](~/xamarin-forms/user-interface/colors.md), und andere Anzeigeeigenschaften, die für mehrere Steuerelemente gelten.



## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
