---
title: Xamarin.Forms-Datenbindung
description: Datenbindung ist das Verfahren der Verknüpfung von Eigenschaften von zwei Objekten, damit Änderungen an einer Eigenschaft in der anderen Eigenschaft automatisch übernommen werden. Datenbindung ist ein wesentlicher Bestandteil der Architektur des Model-View-ViewModel (MVVM)-Anwendung.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 60bf0bdc5f1a4472dfd12424c3cc0375d3f34c24
ms.sourcegitcommit: 11c1df7594064e4e141470e092e55cc50c138068
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "35240352"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms-Datenbindung

_Datenbindung ist das Verfahren der Verknüpfung von Eigenschaften von zwei Objekten, damit Änderungen an einer Eigenschaft in der anderen Eigenschaft automatisch übernommen werden. Datenbindung ist ein wesentlicher Bestandteil der Architektur des Model-View-ViewModel (MVVM)-Anwendung._

## <a name="the-data-linking-problem"></a>Das Problem verknüpfen

Eine Xamarin.Forms-Anwendung besteht aus einer oder mehreren Seiten, von denen jedes in der Regel mehrere UI-Objekte mit dem Namen enthält *Ansichten*. Eine der wichtigsten Aufgaben des Programms ist, diese Sichten synchronisiert zu halten, und klicken Sie zum Nachverfolgen der verschiedenen Werte oder Auswahlen, die sie darstellen. Die Ansichten stellen häufig Werte aus einer zugrunde liegenden Datenquelle, und der Benutzer bearbeitet diese Ansichten, um die Daten zu ändern. Wenn die Sicht ändert, müssen die zugrunde liegenden Daten widerzuspiegeln dieser Änderung und entsprechend, wenn sich die zugrunde liegenden Daten ändert, diese Änderung muss wiedergegeben werden in der Ansicht.

Um diesen Auftrag erfolgreich verarbeiten zu können, muss die Anwendung von Änderungen in diesen Ansichten oder die zugrunde liegenden Daten benachrichtigt werden. Die gängige Lösung ist, Ereignisse zu definieren, die signalisieren, wenn eine Änderung vorgenommen wird. Dann kann ein Ereignishandler installiert werden, die diese Änderungen benachrichtigt wird werden. Es reagiert, indem Sie die Übertragung von Daten von einem Objekt in einen anderen. Wenn Sie jedoch es gibt viele Ansichten, es muss auch viele Ereignishandler vorhanden sein, und eine Menge Code ruft beteiligt.

## <a name="the-data-binding-solution"></a>Der Data-Lösung für Datenbindung

Die Datenbindung dieses Auftrags automatisiert und rendert die Ereignishandler nicht erforderlich. (Die Ereignisse sind weiterhin erforderlich, jedoch, da die Infrastruktur für die Datenbindung verwendet.) Datenbindungen können entweder im Code oder in XAML implementiert werden, aber sie sind sehr viel häufiger in XAML, in dem sie zum Verringern der Größe des Code-Behind-Datei Hilfe. Prozeduralen Code im Ereignishandler durch deklarativen Code oder Markup ersetzen, wird die Anwendung vereinfacht und erläutert.

Eines der zwei Objekte, die eine Datenbindung beteiligt ist fast immer ein Element, das von abgeleitet ist `View` und bildet einen Teil der visuellen Oberfläche einer Seite. Das andere Objekt ist entweder:

- Eine andere `View` Ableitung, in der Regel auf derselben Seite.
- Ein Objekt in einer Codedatei.

In der Demoprogramme, z. B. in der [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Beispiels datenbindungen, die zwischen zwei `View` ableitungen werden häufig zum Zweck der Klarheit und Einfachheit angezeigt. Allerdings können dieselben Prinzipien angewendet werden, auf datenbindungen, die zwischen einem `View` und anderen Objekten. Wenn eine Anwendung mithilfe der Model-View-ViewModel (MVVM)-Architektur erstellt wird, wird die Klasse mit dem zugrunde liegenden Daten häufig ein "ViewModel" bezeichnet.

Datenbindungen werden in den folgenden Artikeln besprochen:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Grundlegende Bindungen](basic-bindings.md)

Informationen Sie zum Unterschied zwischen dem Datenbindungsziel und die Quelle, und sehen Sie einfache datenbindungen in Code und XAML.

## <a name="binding-modebinding-modemd"></a>[Bindungsmodus](binding-mode.md)

Ermitteln Sie, wie der Bindungsmodus den Datenfluss zwischen den beiden Objekten steuern kann.

## <a name="string-formattingstring-formattingmd"></a>[Formatierung von Zeichenfolgen](string-formatting.md)

Verwenden Sie eine Datenbindung zum Formatieren und Anzeigen von Objekten als Zeichenfolgen.

## <a name="binding-pathbinding-pathmd"></a>[Bindungspfad](binding-path.md)

Dringen Sie tiefer in die `Path` Eigenschaft die Datenbindung an die untergeordneten Eigenschaften und Member der Auflistung zuzugreifen.

## <a name="binding-value-convertersconvertersmd"></a>[Binden von Wertkonvertern](converters.md)

Verwenden Sie zum Ändern der Werte in der Datenbindung Binden von Wertkonvertern.

## <a name="binding-fallbacksbinding-fallbacksmd"></a>[Binden Fallbacks](binding-fallbacks.md)

Stellen Sie datenbindungen, die robuster, durch die Definition von fallback-Werte verwenden, wenn die Bindung kann nicht ausgeführt werden.

## <a name="the-command-interfacecommandingmd"></a>[Die Befehlsschnittstelle](commanding.md)

Implementieren der `Command` Eigenschaft mit datenbindungen.

## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
