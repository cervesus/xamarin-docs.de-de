---
title: Zusammenfassung der Kapitel 6. Schaltflächenklicks
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 6. Schaltflächenklicks'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 0f1da94031e658d42205e6346d41b02c5822d992
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563679"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Zusammenfassung der Kapitel 6. Schaltflächenklicks

Die [ `Button` ](xref:Xamarin.Forms.Button) ist die Ansicht, die dem Benutzer ermöglicht, einen Befehl zu initiieren. Ein `Button` wird durch Text identifiziert (und optional ein Bild wie in [Kapitel 13, Bitmaps](chapter13.md)). Folglich `Button` definiert viele der für die gleichen Eigenschaften wie `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` definiert außerdem drei Eigenschaften, die die Darstellung von dessen Rahmen steuern, aber die Unterstützung dieser Eigenschaften und ihre gegenseitigen Unabhängigkeit ist plattformspezifisch:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) Der Typ `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) Der Typ `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) Der Typ `Double`

`Button` erbt auch alle Eigenschaften des `VisualElement` und `View`, einschließlich `BackgroundColor`, `HorizontalOptions`, und `VerticalOptions`.

## <a name="processing-the-click"></a>Klicken Sie auf Verarbeitung

Die `Button` -Klasse definiert eine [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis, das ausgelöst wird, wenn der Benutzer tippt der `Button`. Die `Click` Handler ist vom Typ `EventHandler`. Das erste Argument ist der `Button` Objekt, das Ereignis generiert, das zweite Argument ist ein `EventArgs` Objekt, das keine zusätzlichen Informationen bereitstellt.

Die [ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) Beispiel zeigt einfache `Clicked` behandeln.

## <a name="sharing-button-clicks"></a>Freigabe Schaltfläche klickt

Mehrere `Button` Ansichten dieselbe `Clicked` Handler, aber der Handler in der Regel muss ermitteln, welche `Button` ist verantwortlich für ein bestimmtes Ereignis. Ein Ansatz ist zum Speichern der verschiedenen `Button` Objekte als Felder und überprüfen Sie die ist das Ereignis im Ereignishandler ausgelöst.

Die [ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) Beispiel wird diese Technik veranschaulicht. Das Programm auch veranschaulicht, wie die [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) Eigenschaft eine `Button` zu `false` beim Drücken der `Button` ist nicht mehr gültig. Eine deaktivierte `Button` generiert keine `Clicked` Ereignis.

## <a name="anonymous-event-handlers"></a>Anonyme-Ereignishandler

Es ist möglich, definieren Sie `Clicked` Handler als anonymer Lambda-Funktionen als die [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) veranschaulicht. Allerdings können anonyme Handler ohne unübersichtliche Reflektionscode eingesetzt werden.

## <a name="distinguishing-views-with-ids"></a>Unterscheiden von Ansichten mit IDs

Mehrere `Button` Objekte können auch durch Festlegen von unterschieden werden die [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) Eigenschaft oder [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) Eigenschaft, um eine `string`. Diese Eigenschaft wird definiert, indem `Element` wird jedoch nicht verwendet in Xamarin.Forms. Es wird ausschließlich von Anwendungen verwendet werden soll.

Die [ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) Beispiel wird mit denselben Ereignishandler für alle 10 Nummerntasten auf eine Zehnertastatur und unterschieden zwischen ihnen mit der `StyleId` Eigenschaft:

[![Dreifacher Screenshot der einfachste Zehnertastatur](images/ch06fg04-small.png "Rechner")](images/ch06fg04-large.png#lightbox "Rechner")

## <a name="saving-transient-data"></a>Speichern vorübergehender Daten

Viele Anwendungen müssen zum Speichern von Daten, wenn ein Programm beendet wird und diese Daten erneut zu laden, wenn die Anwendung neu wird gestartet. Die [ `Application` ](xref:Xamarin.Forms.Application) -Klasse definiert mehrere Member, mit denen das Programm speichern und Wiederherstellen von Daten, die vorübergehend:

- Die [ `Properties` ](xref:Xamarin.Forms.Application.Properties) Eigenschaft ist ein Wörterbuch mit `string` Schlüssel und `object` Elemente. Der Inhalt des Wörterbuchs automatisch im lokalen Anwendungsspeicher vor der Beendigung des Programms gespeichert und erneut geladen, wenn das Programm wird gestartet.
- Die `Application` -Klasse definiert drei geschützte virtuelle Methoden, dass die Anwendung die standard- `App` -Klasse überschreibt: [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart), [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep), und [ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). Diese beziehen sich auf *Anwendungslebenszyklus* Ereignisse.
- Die [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) Methode speichert den Inhalt des Wörterbuchs.

Es ist nicht notwendig, `SavePropertiesAsync`. Der Inhalt des Wörterbuchs automatisch vor der Beendigung des Programms gespeichert und vor dem Programmstart abgerufen. Es ist hilfreich bei der Anwendung zu testen, um Daten zu speichern, wenn das Programm abstürzt.

Sie ist auch nützlich sind:

- [`Application.Current`](xref:Xamarin.Forms.Application.Current), eine statische Eigenschaft, die die aktuelle zurückgibt `Application` -Objekt, das Sie dann, zum Abrufen verwenden können der `Properties` Wörterbuch.

Im ersten Schritt werden alle Variablen auf der Seite angeben, zu bestimmen, beizubehalten, wenn das Programm beendet wird. Wenn Sie alle Stellen kennen, auf denen diese Variablen ändern, können Sie einfach diese hinzufügen, die `Properties` Wörterbuch an diesem Punkt. Im Konstruktor der Seite, Sie können Variablen festlegen, die von der `Properties` Wörterbuch, wenn der Schlüssel vorhanden ist.

Ein größeren Programms müssen wahrscheinlich zur Behandlung der Ereignisse des Anwendungslebenszyklus. Am wichtigsten ist die `OnSleep` Methode. Ein Aufruf dieser Methode gibt an, dass das Programm im Vordergrund verlassen hat. Vielleicht der Benutzer geklickt hat die **Startseite** Schaltfläche auf dem Gerät oder alle Anwendungen angezeigt oder wird heruntergefahren das Telefon. Ein Aufruf von `OnSleep` ist die einzige Benachrichtigung, dass ein Programm empfängt, bevor sie beendet wird. Das Programm dauert die Gelegenheit, um sicherzustellen, dass die `Properties` Wörterbuch auf dem neuesten Stand ist.

Ein Aufruf von `OnResume` gibt an, dass das Programm nicht beendet nach dem letzten Aufruf von `OnSleep` aber ist jetzt im Vordergrund erneut ausführen. Das Programm möglicherweise diese Gelegenheit nutzen, um internetverbindungen (z. B.) zu aktualisieren.

Ein Aufruf von `OnStart` während des Programmstarts auftritt. Es ist nicht erforderlich ist, warten, bis diese Methode aufrufen, für den Zugriff auf die `Properties` Wörterbuch, da der Inhalt bereits wurden wiederhergestellt, wenn die `App` Konstruktor aufgerufen wird.

Die [ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) Beispiel ähnelt **SimplestKeypad** mit dem Unterschied, dass das Programm verwendet die `OnSleep` außer Kraft setzen, um den aktuellen Zehnertastatur-Eintrag zu speichern und der Page-Konstruktor, um diese Daten wiederherzustellen.

> [!NOTE]
> Ein weiteres Verfahren zum Speichern von Einstellungen für das Programm wird bereitgestellt, durch die Xamarin.Essentials [Voreinstellungen](~/essentials/preferences.md) Klasse.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 6 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Kapitel 6-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Kapitel 6 F# Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Schaltfläche "Xamarin.Forms"](~/xamarin-forms/user-interface/button.md)
