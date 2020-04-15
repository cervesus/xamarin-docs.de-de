---
title: 'Zusammenfassung von Kapitel 6: Schaltflächenklicks'
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 6: Schaltflächenklicks'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 12c8cdc19f9e6765ca25ade97bcfdbffb7b60381
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "61334716"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Zusammenfassung von Kapitel 6: Schaltflächenklicks

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)

Ein [`Button`](xref:Xamarin.Forms.Button)-Element stellt die Ansicht dar, mit der Benutzer einen Befehl initiieren können. Ein `Button` wird durch Text identifiziert (optional auch durch ein Bild, wie in [Kapitel 13: Bitmaps](chapter13.md) ersichtlich). Somit werden durch `Button` viele der Eigenschaften definiert, die auch durch `Label` definiert werden:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

Durch `Button` werden auch drei Eigenschaften definiert, die die Darstellung des Rahmens der Schaltfläche steuern. Die Unterstützung dieser Eigenschaften und ihrer gegenseitigen Unabhängigkeit ist jedoch plattformspezifisch:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) vom Typ `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) vom Typ `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) vom Typ `Double`

`Button` erbt zudem alle Eigenschaften von `VisualElement` und `View`, einschließlich `BackgroundColor`, `HorizontalOptions` und `VerticalOptions`.

## <a name="processing-the-click"></a>Verarbeiten des Klicks

Mit der `Button`-Klasse wird ein [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis definiert, das durch Tippen des Benutzers auf den `Button` ausgelöst wird. Der `Click`-Handler ist vom Typ `EventHandler`. Das erste Argument ist das `Button`-Objekt, das das Ereignis erzeugt. Das zweite Argument ist ein `EventArgs`-Objekt, das keine zusätzlichen Informationen bereitstellt.

Im Beispiel [**ButtonLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) wird die einfache Verarbeitung von `Clicked` veranschaulicht.

## <a name="sharing-button-clicks"></a>Freigeben von Schaltflächenklicks

Mehrere `Button`-Ansichten können denselben `Clicked`-Handler verwenden. Jedoch muss der Handler im Allgemeinen bestimmen, welcher `Button` für ein bestimmtes Ereignis verantwortlich ist. Eine Möglichkeit besteht darin, die unterschiedlichen `Button`-Objekte als Felder zu speichern und zu prüfen, durch welches das Ereignis im Handler ausgelöst wird.

Diese Prozedur wird im Beispiel [**TwoButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) veranschaulicht. Es wird zudem gezeigt, wie die [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)-Eigenschaft eines `Button` auf `false` festgelegt wird, wenn das Klicken auf den `Button` nicht mehr gültig ist. Über einen deaktivierten `Button` wird kein `Clicked`-Ereignis generiert.

## <a name="anonymous-event-handlers"></a>Anonyme Ereignishandler

`Clicked`-Handler können auch, wie im Beispiel [**ButtonLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) veranschaulicht, als anonyme Lambdafunktionen definiert werden. Anonyme Handler können jedoch nur mit unübersichtlichem Reflektionscode freigegeben werden.

## <a name="distinguishing-views-with-ids"></a>Unterscheiden von Ansichten mit IDs

Mehrere `Button`-Objekte können auch durch Festlegen der Eigenschaften [`StyleId`](xref:Xamarin.Forms.Element.StyleId) oder [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) auf einen `string` unterschieden werden. Diese Eigenschaft wird durch `Element` definiert. Sie wird in Xamarin.Forms jedoch nicht verwendet. Sie wird ausschließlich in Anwendungsprogrammen verwendet.

Im Beispiel [**SimplestKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) wird derselbe Ereignishandler für alle zehn Zifferntasten auf einer Zehnertastatur verwendet, die über die Eigenschaft `StyleId` unterschieden werden:

[![Dreifacher Screenshot eines Beispiels für „SimplestKeypad“](images/ch06fg04-small.png "Rechner")](images/ch06fg04-large.png#lightbox "Rechner")

## <a name="saving-transient-data"></a>Speichern vorübergehender Daten

Viele Anwendungen müssen beim Beenden eines Programms Daten speichern, die beim nächsten Programmstart nochmals geladen werden. In der Klasse [`Application`](xref:Xamarin.Forms.Application) werden mehrere Member definiert, mit denen das Programm vorübergehende Daten speichern und wiederherstellen kann:

- Die [`Properties`](xref:Xamarin.Forms.Application.Properties)-Eigenschaft ist ein Wörterbuch mit `string`-Schlüsseln und `object`-Elementen. Der Inhalt des Wörterbuchs wird vor Beendigung des Programms automatisch im lokalen Anwendungsspeicher gespeichert und noch mal geladen, wenn das Programm gestartet wird.
- In der `Application`-Klasse werden drei geschützte virtuelle Methoden definiert, die von der `App`-Standardklasse des Programms überschrieben werden: [`OnStart`](xref:Xamarin.Forms.Application.OnStart), [`OnSleep`](xref:Xamarin.Forms.Application.OnSleep) und [`OnResume`](xref:Xamarin.Forms.Application.OnResume). Diese beziehen sich auf Ereignisse des *Anwendungslebenszyklus*.
- Mit der Methode [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) wird der Inhalt des Wörterbuchs gespeichert.

Der Aufruf von `SavePropertiesAsync` ist nicht erforderlich. Der Inhalt des Wörterbuchs wird vor Beendigung des Programms automatisch gespeichert und vor einem erneuten Programmstart abgerufen. Während Sie das Programm testen, ist es nützlich, Ihre Daten für den Fall eines Programmabsturzes zu speichern.

Folgende Option kann ebenfalls nützlich sein:

- [`Application.Current`](xref:Xamarin.Forms.Application.Current): eine statische Eigenschaft, die das aktuelle `Application`-Objekt zurückgibt. Mit diesem Objekt können Sie anschließend das `Properties`-Wörterbuch abrufen.

Identifizieren Sie zunächst auf der Seite alle Variablen, die beim Beenden des Programms beibehalten werden sollen. Wenn Sie bereits alle Stellen kennen, an denen sich diese Variablen ändern, können Sie sie einfach zu diesem Zeitpunkt dem `Properties`-Wörterbuch hinzufügen. Im Seitenkonstruktor können Sie die Variablen aus dem `Properties`-Wörterbuch festlegen, sofern der Schlüssel vorhanden ist.

Größere Programme müssen vermutlich in der Lage sein, mit Ereignissen des Anwendungslebenszyklus umzugehen. Das wichtigste Ereignis ist dabei die `OnSleep`-Methode. Ein Aufruf dieser Methode zeigt an, dass das Programm nicht mehr im Vordergrund ausgeführt wird. Möglicherweise hat der Benutzer zuvor auf dem Gerät auf die Schaltfläche **Start** geklickt, alle Anwendungen angezeigt oder das Smartphone heruntergefahren. Ein Aufruf von `OnSleep` ist die einzige Benachrichtigung, die ein Programm erhält, bevor es beendet wird. Dies ist die letzte Möglichkeit für das Programm zu überprüfen, ob das `Properties`-Wörterbuch auf dem neuesten Stand ist.

Ein Aufruf von `OnResume` zeigt an, dass das Programm nach dem letzten Aufruf von `OnSleep` nicht beendet wurde, sondern noch mal im Vordergrund ausgeführt wird. Bei dieser Gelegenheit kann das Programm (beispielsweise) Internetverbindungen aktualisieren.

Zu einem Aufruf von `OnStart` kommt es während des Programmstarts. Sie müssen nicht bis zum Aufruf dieser Methode warten, bevor Sie auf das `Properties`-Wörterbuch zugreifen, da dessen Inhalt bereits beim Aufruf des `App`-Konstruktors wiederhergestellt wurde.

Die Beispiele [**PersistentKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) und **SimplestKeypad** ähneln sich sehr. Der einzige Unterschied besteht darin, dass das Programm die Überschreibung durch `OnSleep` dazu verwendet, den aktuellen Tastatureintrag zu speichern sowie den Seitenkonstruktor, um diese Daten wiederherzustellen.

> [!NOTE]
> Eine weitere Möglichkeit zum Speichern von Programmeinstellungen bietet die Xamarin.Essentials-Klasse [Einstellungen](~/essentials/preferences.md).

## <a name="related-links"></a>Verwandte Links

- [Kapitel 6: Vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Kapitel 6: Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Kapitel 6: F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Xamarin.Forms-Button-Element](~/xamarin-forms/user-interface/button.md)
