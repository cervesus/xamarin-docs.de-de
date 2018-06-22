---
title: Grundlagen der Xamarin.Forms-Anwendung
description: Untersuchen die Grundlagen der Xamarin.Forms die Anwendungsentwicklung, einschließlich aller der erforderlichen Kernkonzepte über Schliff z. B. Eingabehilfen und Lokalisierung.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: d75cac7a21b2c74a6627845cdda8e4c04e72bddc
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309774"
---
# <a name="xamarinforms-application-fundamentals"></a>Grundlagen der Xamarin.Forms-Anwendung

## <a name="accessibilityaccessibilityindexmd"></a>[Barrierefreiheit](accessibility/index.md)

Tipps zugänglich Funktionen (z. B. Unterstützungstools Lesemodus) mit Xamarin.Forms integriert.

## <a name="app-classapplication-classmd"></a>[App-Klasse](application-class.md)

Die `Application` Klasse stellt den Startpunkt für Xamarin.Forms – jede app muss eine Unterklasse implementieren `App` für die erste Seite festgelegt. Sie bietet außerdem die `Properties` Auflistung für die Speicherung von einfachen. Sie können in c# oder in XAML definiert werden.

## <a name="app-lifecycleapp-lifecyclemd"></a>[App-Lebenszyklus](app-lifecycle.md)

Die `Application` Klasse `OnStart`, `OnSleep`, und `OnResume` Methoden sowie modale Navigationsereignisse, ermöglichen es Ihnen die Anwendung Lebenszyklusereignisse mit benutzerdefiniertem Code zu behandeln.

## <a name="behaviorsbehaviorsindexmd"></a>[Verhalten](behaviors/index.md)

Benutzeroberflächen-Steuerelemente können durch Verwenden von Verhaltensweisen zum Hinzufügen von Funktionalität einfach ohne Erstellung von Unterklassen von erweitert werden.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Benutzerdefinierte Renderer](custom-renderer/index.md)

Benutzerdefinierte rendert können Entwickler, die "das Standardrendering von Xamarin.Forms Steuerelemente zur Anpassung von Darstellung und das Verhalten auf jeder Plattform (mithilfe von systemeigenen SDKs bei Bedarf)-override".

## <a name="data-bindingdata-bindingindexmd"></a>[Datenbindung](data-binding/index.md)

Datenbindung verknüpft die Eigenschaften von zwei Objekten, dass Änderungen an einer Eigenschaft in der anderen Eigenschaft automatisch wiedergegeben werden. Binden von Daten ist ein wesentlicher Bestandteil der Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) Anwendungsarchitektur.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Abhängigkeitsdienst](dependency-service/index.md)

Die `DependencyService` stellt einen einfachen Locator bereit, sodass Sie zu Schnittstellen im freigegebenen Code Code und, plattformspezifische Implementierungen, die automatisch aufgelöst werden kann bereitstellen, vereinfacht die Clientplattform-spezifische Funktionen in Xamarin.Forms zu verweisen.

## <a name="effectseffectsindexmd"></a>[Effekte](effects/index.md)

Effekte ermöglichen die native Steuerelemente für jede Plattform angepasst werden, und für kleine Styling Änderungen in der Regel verwendet werden.

## <a name="filesfilesmd"></a>[Dateien](files.md)

Dateibearbeitung mit Xamarin.Forms kann mithilfe von Code in eine .NET Standardbibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden.

## <a name="gesturesgesturesindexmd"></a>[Gesten](gestures/index.md)

Der Xamarin.Forms [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/) Klasse tippen, verkleinern und Schwenken Gesten auf Benutzeroberflächen-Steuerelementen unterstützt.

## <a name="localizationlocalizationindexmd"></a>[Lokalisierung](localization/index.md)

Integrierte Lokalisierung .NET Framework kann verwendet werden, um plattformübergreifende mehrsprachige Clientanwendungen mit Xamarin.Forms zu erstellen.

## <a name="local-databasesdatabasesmd"></a>[Lokale Datenbanken](databases.md)

Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code.

## <a name="messaging-centermessaging-centermd"></a>[Messaging-Center](messaging-center.md)

Xamarin.Forms `MessagingCenter` ermöglicht das anzeigen, Modelle und andere Komponenten mit kommunizieren, ohne Informationen über miteinander neben einem einfachen Nachrichtenvertrag kennen zu müssen.

## <a name="navigationnavigationindexmd"></a>[Navigation](navigation/index.md)

Xamarin.Forms stellt eine Reihe von anderen Seite Navigation Erfahrungen, abhängig von der `Page` geben verwendet wird.

## <a name="templatestemplatesindexmd"></a>[Vorlagen](templates/index.md)

Steuerelementvorlagen bieten die Möglichkeit, leicht Design und Re-Design Anwendungsseiten zur Laufzeit während Datenvorlagen definieren die Darstellung von Daten auf unterstützte Steuerelemente werden können.

## <a name="triggerstriggersmd"></a>[Trigger](triggers.md)

Reagieren auf eigenschaftenänderungen und Ereignisse in XAML aus, um Steuerelemente zu aktualisieren.


## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
