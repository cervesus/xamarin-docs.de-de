---
title: Xamarin für Objective-C-Entwickler
description: Wenn Sie ein Objective-C-Entwickler sind, sind Sie auf dem besten Weg, Ihre Fähigkeiten und den vorhandenen Objective-C-Code auf der Xamarin-Plattform zu nutzen und gleichzeitig die Vorteile von C# für die Wiederverwendung von Code auszuschöpfen. Dieser Abschnitt dient als Einstiegspunkt für Xamarin.iOS und zeigt eine Fülle an Informationen über die Verwendung von vorhandenem Objective-C-Code aus C#.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e29762fb258f7d796878c85bfe6f7aaa93207c5e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin für Objective-C-Entwickler

_Wenn Sie ein Objective-C-Entwickler sind, sind Sie auf dem besten Weg, Ihre Fähigkeiten und den vorhandenen Objective-C-Code auf der Xamarin-Plattform zu nutzen und gleichzeitig die Vorteile von C# für die Wiederverwendung von Code auszuschöpfen. Dieser Abschnitt dient als Einstiegspunkt für Xamarin.iOS und zeigt eine Fülle an Informationen über die Verwendung von vorhandenem Objective-C-Code aus C#._

Xamarin bietet einen Pfad für Entwickler, die iOS ansteuern, um ihren Code für die Benutzeroberfläche zu der plattformagnostischen Programmiersprache C# zu verschieben. So kann er überall verwendet werden, wo C# verfügbar ist, einschließlich Android über Xamarin.Android und den verschiedenen Arten von Windows. Nur weil Sie C# mit Xamarin verwenden, bedeutet dies nicht, dass Sie nicht die vorhandenen Fähigkeiten und den Objective-C-Code nutzen können. Wenn Sie Objective-C kennen, macht Sie dies in der Tat zu einem besseren Xamarin.iOS-Entwickler, da Xamarin alle bekannten nativen iOS- und OS X-Plattform-APIs verfügbar macht, wie UIKit, Core Animation, Core Foundation und Core Graphics, um nur einige zu nennen. Darüber hinaus erhalten Sie die Leistung der C#-Sprache, einschließlich Features wie LINQ und Generika sowie umfangreiche Bibliotheken der .NET-Basisklasse, die Sie in Ihren nativen Anwendungen verwenden können.

Außerdem erlaubt es Xamarin, vorhandene Objective-C-Ressourcen mithilfe einer Technologie zu nutzen, die als Bindung bezeichnet wird. Sie erstellen einfach eine statische Bibliothek in Objective-C und machen ihn wie im folgenden Diagramm gezeigt über die Bindung für C# verfügbar:

 [![](images/01-bindings.png "Eine statische Bibliothek in Objective-C, die über eine Bindung für C# verfügbar gemacht wird")](images/01-bindings.png#lightbox)

Dies beschränkt sich nicht nur auf Nicht-Benutzeroberflächencode. Bindungen können auch Benutzeroberflächencode, der in Objective-C entwickelt wurde, verfügbar machen.

## <a name="transitioning-from-objective-c"></a>Übergang von Objective-C

Auf unserer Dokumentationsseite finden Sie viele Informationen, die Ihnen den Übergang zu Xamarin erleichtern sollen und zeigen, wie Sie mit Ihrem vorhandenen Wissen C#-Code integrieren. Zu Beginn ein paar Highlights:

-   [C# Primer for Objective-C Developers (Einführung in C# für Objective-C-Entwickler:)](primer.md) Eine kurze Einführung für Objective-C-Entwickler, die zu Xamarin und zur C#-Sprache wechseln möchten. 
-   [Walkthrough: Binding an Objective-C Library (Exemplarische Vorgehensweise: Bindung einer Objective-C-Bibliothek:)](~/ios/platform/binding-objective-c/walkthrough.md) Eine ausführliche Anleitung zur Wiederverwendung von existierendem Objective-C-Code in einer Anwendung von Xamarin.iOS. 


## <a name="binding-objective-c"></a>Binden von Objective-C

Sobald Sie den Vergleich von C# mit Objective-C verstanden und sich durch die oben genannte exemplarische Vorgehensweise für die Bindung gearbeitet haben, können Sie den Übergang zur Xamarin-Plattform ausführen. Weitere Informationen über die Xamarin.iOS-Bindungstechnologien einschließlich eines umfassenden Bindungsverweises finden Sie im Abschnitt [Binding Objective-C (Bindung von Objective-C)](~/ios/platform/binding-objective-c/index.md).

## <a name="cross-platform-development"></a>Plattformübergreifende Entwicklung

Nachdem Sie zu Xamarin.iOS gewechselt sind, sollten Sie die plattformübergreifende Anleitung ausprobieren, die auch Fallstudien von Referenzanwendungen enthalten, die wir entwickelt haben. Zudem finden Sie bewährte Methoden für das Erstellen von wiederverwendbarem plattformübergreifendem Code im Abschnitt [Building Cross Platform Applications section (Erstellen von plattformübergreifenden Anwendungen)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
