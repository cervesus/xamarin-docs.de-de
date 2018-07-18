---
title: Grundlagen der Xamarin.Forms-Anwendung
description: Untersuchen die Grundlagen der Xamarin.Forms-Anwendungsentwicklung, einschließlich alle erforderliche Kerndienste Konzepte, bis zum Abschluss des Workflows, z. B. Barrierefreiheit und Lokalisierung.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995611"
---
# <a name="xamarinforms-application-fundamentals"></a>Grundlagen der Xamarin.Forms-Anwendung

## <a name="accessibilityaccessibilityindexmd"></a>[Barrierefreiheit](accessibility/index.md)

Tipps zugänglich-Funktionen (wie die Unterstützung von Sprachausgabe Tools) mit Xamarin.Forms zu integrieren.

## <a name="app-classapplication-classmd"></a>[App-Klasse](application-class.md)

Die `Application` Klasse ist der Ausgangspunkt für Xamarin.Forms – jede app muss eine Unterklasse implementieren `App` für die erste Seite festgelegt. Außerdem wird die `Properties` Sammlung für die Speicherung von einfachen Daten. Sie können in c# oder XAML definiert werden.

## <a name="app-lifecycleapp-lifecyclemd"></a>[App-Lebenszyklus](app-lifecycle.md)

Die `Application` Klasse `OnStart`, `OnSleep`, und `OnResume` Methoden sowie modalen Navigation-Ereignissen, können Sie die Ereignisse des Anwendungslebenszyklus mit benutzerdefiniertem Code zu behandeln.

## <a name="behaviorsbehaviorsindexmd"></a>[Verhalten](behaviors/index.md)

Steuerelemente der Benutzeroberfläche können problemlos ohne Unterklassen mithilfe von Verhalten zum Hinzufügen von Funktionen erweitert werden.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Benutzerdefinierte Renderer](custom-renderer/index.md)

Benutzerdefinierte rendert können Entwickler, die "die standardmäßige Umsetzung von Xamarin.Forms-Steuerelemente zur Anpassung von deren Darstellung und Verhalten auf jeder Plattform (mithilfe systemeigener SDKs auf, falls gewünscht) override".

## <a name="data-bindingdata-bindingindexmd"></a>[Datenbindung](data-binding/index.md)

Datenbindung verknüpft die Eigenschaften von zwei Objekten, sodass Änderungen an einer Eigenschaft in der anderen Eigenschaft automatisch wiedergegeben werden. Die Datenbindung ist ein wesentlicher Bestandteil der Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) Anwendungsarchitektur.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Abhängigkeitsdienst](dependency-service/index.md)

Die `DependencyService` stellt einen einfachen Locator bereit, sodass Sie an Schnittstellen im freigegebenen Code Code und Bereitstellen von plattformspezifischen Implementierungen, die automatisch aufgelöst werden kann, vereinfacht das verweisen plattformspezifische Funktionalität in Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Effekte](effects/index.md)

Können durch Effekte native Steuerelemente auf den einzelnen Plattformen angepasst werden, und für kleine Formatierungsänderungen in der Regel verwendet werden.

## <a name="filesfilesmd"></a>[Dateien](files.md)

Dateiverarbeitung mit Xamarin.Forms kann mithilfe von Code in einer .NET Standard-Bibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden.

## <a name="gesturesgesturesindexmd"></a>[Gesten](gestures/index.md)

Die Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) Klasse unterstützt Tap, verkleinern und Schwenken Gesten auf Benutzeroberflächen-Steuerelementen.

## <a name="localizationlocalizationindexmd"></a>[Lokalisierung](localization/index.md)

Integrierte .NET Framework Lokalisierung kann zum Erstellen von plattformübergreifenden mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden.

## <a name="local-databasesdatabasesmd"></a>[Lokale Datenbanken](databases.md)

Xamarin.Forms unterstützt die Datenbank-gestützten Anwendungen, die die SQLite-Datenbank-Engine, die es ermöglicht, laden und Speichern von Objekten in freigegebenem Code verwenden.

## <a name="messaging-centermessaging-centermd"></a>[Center-Messaging](messaging-center.md)

Xamarin.Forms `MessagingCenter` ermöglicht das anzeigen, Modelle und andere Komponenten für die Kommunikation mit ohne nichts über den anderen neben einem einfachen Nachrichtenvertrag kennen zu müssen.

## <a name="navigationnavigationindexmd"></a>[Navigation](navigation/index.md)

Xamarin.Forms stellt eine Reihe von der anderen seitennavigationen, abhängig von der `Page` geben verwendet wird.

## <a name="templatestemplatesindexmd"></a>[Vorlagen](templates/index.md)

Steuerelementvorlagen bieten die Möglichkeit, einfache Weise Designs und überarbeitet Anwendungsseiten zur Laufzeit, während Data-Vorlagen die Möglichkeit, definieren die Darstellung von Daten für unterstützte Steuerelemente bereitstellen.

## <a name="triggerstriggersmd"></a>[Trigger](triggers.md)

Aktualisieren Sie Steuerelemente durch Reaktion auf eigenschaftenänderungen und Ereignisse in XAML.


## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
