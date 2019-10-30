---
title: Xamarin.Forms-Datenbindung
description: Bei der Datenbindung werden die Eigenschaften von zwei Objekten verknüpft. Dadurch werden Änderungen an einer Eigenschaft automatisch in der anderen widergespiegelt. Die Datenbindung ist ein integraler Teil der „Model View ViewModel“-Anwendungsarchitektur (MVVM).
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/27/2019
ms.openlocfilehash: fa92409b33717e528c3cfb83a24148c698836594
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697147"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms-Datenbindung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

_Bei der Datenbindung werden die Eigenschaften von zwei Objekten verknüpft. Dadurch werden Änderungen an einer Eigenschaft automatisch in der anderen widergespiegelt. Die Datenbindung ist ein integraler Teil der „Model View ViewModel“-Anwendungsarchitektur (MVVM)._

## <a name="the-data-linking-problem"></a>Das Problem: Verknüpfung von Daten

Eine Xamarin.Forms-Anwendung besteht aus mindestens einer Seite, die wiederum mehrere Benutzeroberflächenobjekte, sogenannte *Ansichten*, enthält. Eine der Hauptaufgaben des Programms ist es, diese Ansichten zu synchronisieren und die unterschiedlichen Werte und Auswahlvorgänge nachzuverfolgen, für die diese stehen. Häufig stehen die Ansichten für Werte einer zugrunde liegenden Datenquelle, und der Benutzer passt die Ansichten an, um die Daten zu verändern. Wenn die Ansicht verändert wird, müssen die zugrunde liegenden Daten diese Änderung widerzuspiegeln. Ebenso muss die Ansicht Änderungen der Daten widerzuspiegeln.

Dazu muss das Programm über Änderungen der Ansichten und der zugrunde liegenden Daten informiert werden. Dies kann durch das definieren von Ereignissen erreicht werden, die angeben, wenn eine Änderung vorgenommen wurde. Dann kann ein Ereignishandler installiert werden, der über die Änderungen informiert wird. Er reagiert, indem er Daten von einem Objekt zu einem anderen überträgt. Wenn es jedoch viele Ansichten gibt, muss es dementsprechend auch viele Ereignishandler geben. Dazu ist viel Code erforderlich.

## <a name="the-data-binding-solution"></a>Die Lösung: Datenbindung

Die Datenbindung automatisiert diesen Prozess und macht Ereignishandler überflüssig. (Ereignisse sind jedoch immer noch erforderlich, da sie von der Datenbindungsinfrastruktur verwendet werden.) Datenbindungen können entweder in Code oder in XAML implementiert werden. Sie werden häufiger in XAML verwendet, wo sie die Größe der CodeBehind-Datei deutlich verringern. Durch das Ersetzen prozeduralen Codes in Ereignishandlern durch deklarativen Code oder Markup wird die Anwendung vereinfacht und eindeutiger.

Eines der beiden Objekte, das an einer Datenbindung beteiligt ist, ist immer ein von `View` abgeleitetes Element und ist Teil der visuellen Oberfläche einer Seite. Das andere Objekt ist eines der folgenden:

- Ein weiteres `View`-Derivat, meist auf der gleichen Seite
- Ein Objekt in einer Codedatei

In Beispielprogrammen wie dem [**DataBindingDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)-Beispiel werden zu Demonstrationszwecken und der Einfachheit halber oft zwei `View`-Derivate verwendet. Die gleichen Prinzipien können jedoch auch auf Datenbindungen zwischen einem `View`-Objekt und einem anderen Objekt angewendet werden. Wenn eine Anwendung mit einer MVVM-Architektur (Model-View-ViewModel) erstellt wird, wird die Klasse mit den zugrunde liegenden Daten oft als ViewModel bezeichnet.

In den folgenden Artikeln wird ausführlicher auf Datenbindungen eingegangen:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Grundlegende Bindungen](basic-bindings.md)

Erfahren Sie, was ein Datenbindungsziel und eine Datenbindungsquelle von einander unterscheidet, und lernen Sie einfache Datenbindungen in Code und XAML kennen.

## <a name="binding-modebinding-modemd"></a>[Bindungsmodus](binding-mode.md)

Erfahren Sie, wie der Bindungsmodus des Datenfluss zwischen zwei Objekten bestimmen kann.

## <a name="string-formattingstring-formattingmd"></a>[Formatierung von Zeichenfolgen](string-formatting.md)

Verwenden Sie eine Datenbindung, um Objekte als Zeichenfolgen zu formatieren und anzuzeigen.

## <a name="binding-pathbinding-pathmd"></a>[Bindungspfad](binding-path.md)

Beschäftigen Sie sich ausführlicher mit der `Path`-Eigenschaft der Datenbindung, und erfahren Sie, wie Sie auf untergeordnete Eigenschaften und Collectionmembers zugreifen.

## <a name="binding-value-convertersconvertersmd"></a>[Binden von Wertkonvertern](converters.md)

Verwenden Sie Bindungswertkonverter, um Werte in der Datenbindung anzupassen.

## <a name="relative-bindingsrelative-bindingsmd"></a>[Relative Bindungen](relative-bindings.md)

Verwenden Sie relative Bindungen, um die Bindungsquelle relativ zur Position des Bindungsziels festzulegen.

## <a name="binding-fallbacksbinding-fallbacksmd"></a>[Bindungsfallbacks](binding-fallbacks.md)

Gestalten Sie Datenbindungen widerstandfähiger, indem Sie Fallbackwerte definieren, die verwendet werden, wenn die Bindung fehlschlägt.

## <a name="the-command-interfacecommandingmd"></a>[Die Befehlsschnittstelle](commanding.md)

Implementieren Sie die `Command`-Eigenschaft mit Datenbindungen.

## <a name="compiled-bindingscompiled-bindingsmd"></a>[Kompilierte Bindungen](compiled-bindings.md)

Verwenden Sie kompilierte Datenbindungen, um die Leistung von Datenbindungen zu verbessern.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Kapitel zu Datenbindung aus dem Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
