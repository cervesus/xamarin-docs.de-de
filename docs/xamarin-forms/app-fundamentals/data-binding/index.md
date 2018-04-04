---
title: Datenbindung
description: Binden von Daten ist das Verfahren der Eigenschaften von zwei Objekten verknüpfen, sodass Änderungen in einer Eigenschaft in der anderen Eigenschaft automatisch wiedergegeben werden. Binden von Daten ist ein wesentlicher Bestandteil der Anwendungsarchitektur Model-View-ViewModel (MVVM).
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: ee8481696b0ef85aec949c6def7767e57eb99e17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="data-binding"></a>Datenbindung

_Binden von Daten ist das Verfahren der Eigenschaften von zwei Objekten verknüpfen, sodass Änderungen in einer Eigenschaft in der anderen Eigenschaft automatisch wiedergegeben werden. Binden von Daten ist ein wesentlicher Bestandteil der Anwendungsarchitektur Model-View-ViewModel (MVVM)._

## <a name="the-data-linking-problem"></a>Das Problem verknüpfen

Eine Xamarin.Forms-Anwendung besteht aus einer oder mehreren Seiten, von denen jedes in der Regel mehrere Objekte der Benutzeroberfläche aufgerufen enthält *Ansichten*. Eine der wichtigsten Aufgaben des Programms ist, diese Sichten synchronisiert zu halten, und klicken Sie zum Nachverfolgen verschiedener Werte oder die Auswahl, die sie darstellen. Die Ansichten darstellen häufig Werte, aus einer zugrunde liegenden Datenquelle und der Benutzer bearbeitet diese Ansichten zum Ändern dieser Daten. Wenn die Sicht ändert, müssen die zugrunde liegenden Daten widerzuspiegeln dieser Änderung und auf ähnliche Weise, wenn die zugrunde liegenden Daten ändert, diese Änderung muss wiedergegeben werden in der Ansicht.

Um diesen Auftrag erfolgreich verarbeiten zu können, muss die Anwendung von Änderungen in diesen Ansichten oder die zugrunde liegenden Daten darüber informiert werden. Die allgemeine Lösung besteht darin, Ereignisse zu definieren, die signalisieren, wenn eine Änderung vorgenommen wird. Dann kann ein Ereignishandler installiert werden, die benachrichtigt wird, diese Änderungen werden. Er reagiert, durch die Übertragung von Daten von einem Objekt in eine andere. Wenn Sie jedoch es gibt viele Ansichten, es muss auch viele Ereignishandler vorhanden sein, und viel Code ruft beteiligt.

## <a name="the-data-binding-solution"></a>Die Bindung Data-Lösung

Die Datenbindung dieser Auftrag automatisiert und rendert die Ereignishandler nicht erforderlich. (Die Ereignisse sind weiterhin erforderlich, jedoch, da die Infrastruktur für die Datenbindung verwendet.) Datenbindungen können entweder im Code oder in XAML implementiert werden, aber sie sind sehr viel häufiger in XAML, in dem sie sind zum Reduzieren der Größe der Code-Behind-Datei hilfreich. Durch Ersetzen von prozeduralem Code im Ereignishandler mit deklarativen Code oder die Markuperweiterung, die Anwendung vereinfacht und es wird erläutert.

Einer der beiden Objekte, die eine Datenbindung beteiligt ist fast immer ein Element, das von abgeleitet ist `View` und bildet einen Teil der Benutzeroberfläche einer Seite. Das andere Objekt ist entweder:

- Eine andere `View` Ableitung, in der Regel auf derselben Seite.
- Ein Objekt in einer Codedatei an.

In der Demoprogramme, wie die in der [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) (Beispiel), datenbindungen zwischen zwei `View` ableitungen werden häufig zum Zweck der Übersichtlichkeit und Einfachheit angezeigt. Allerdings können dieselben Prinzipien angewendet werden, Daten Bindungen zwischen einer `View` und andere Objekte. Wenn eine Anwendung die Verwendung der Architektur der Model-View-ViewModel (MVVM) erstellt wird, wird die Klasse mit dem zugrunde liegenden Daten häufig ein ViewModel bezeichnet.

Datenbindungen sind in den folgenden Artikeln untersucht:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Grundlegende Bindungen](basic-bindings.md)

Erläutert den Unterschied zwischen dem Datenbindung Ziel und Quelle und im einfachen datenbindungen in Code und XAML.

## <a name="binding-modebinding-modemd"></a>[Bindungsmodus](binding-mode.md)

Ermitteln Sie, wie die Bindungsmodus den Datenfluss zwischen den beiden Objekten steuern kann.

## <a name="string-formattingstring-formattingmd"></a>[Formatierung von Zeichenfolgen](string-formatting.md)

Verwenden Sie eine Datenbindung zum Formatieren und Anzeigen von Objekten als Zeichenfolgen.

## <a name="binding-pathbinding-pathmd"></a>[Bindungspfad](binding-path.md)

Detaillierte Informationen in der `Path` Eigenschaft die Datenbindung an die untergeordneten Eigenschaften und Elemente der Auflistung zuzugreifen.

## <a name="binding-value-convertersconvertersmd"></a>[Binden von Wertkonvertern](converters.md)

Verwenden Sie Wertkonverter Bindung, um Werte innerhalb der Datenbindung zu ändern.

## <a name="the-command-interfacecommandingmd"></a>[Die Befehlsschnittstelle](commanding.md)

Implementieren der `Command` Eigenschaft mit datenbindungen.



## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
