---
title: Zusammenfassung der Kapitel 6. Auf eine Schaltfläche
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a45d1f1663b141912744e83763e7d618866159d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>Zusammenfassung der Kapitel 6. Auf eine Schaltfläche

Die [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ist die Ansicht, die dem Benutzer ermöglicht, einen Befehl zu initiieren. Ein `Button` von Text bezeichnet wird (und optional ein Image wie in [Kapitel 13, Bitmaps](chapter13.md)). Folglich `Button` definiert viele dieselben Eigenschaften wie `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` Außerdem definiert drei Eigenschaften, die die Darstellung von Rahmen steuern, aber die Unterstützung für diese Eigenschaften und ihre gegenseitigen Unabhängigkeit ist plattformspezifisch:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) vom Typ `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) vom Typ `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) vom Typ `Double`

`Button` erbt auch die Eigenschaften des `VisualElement` und `View`, einschließlich `BackgroundColor`, `HorizontalOptions`, und `VerticalOptions`.

## <a name="processing-the-click"></a>Klicken Sie auf Verarbeitung

Die `Button` Klasse definiert ein [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) Ereignis, das ausgelöst wird, wenn der Benutzer tippt der `Button`. Die `Click` Handler ist vom Typ `EventHandler`. Das erste Argument ist der `Button` Objekt, das Ereignis generiert; das zweite Argument ist ein `EventArgs` Objekt, das keine zusätzlichen Informationen bereitstellt.

Die [ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) Beispiel zeigt einfache `Clicked` behandeln.

## <a name="sharing-button-clicks"></a>Freigabe der Schaltfläche klickt

Mehrere `Button` Ansichten dieselbe `Clicked` Handler, aber der Handler muss im Allgemeinen ermitteln, welche `Button` für ein bestimmtes Ereignis verantwortlich ist. Ein Ansatz besteht darin, die verschiedenen speichern `Button` Objekte als Felder und überprüfen, welches das Ereignis im Handler auslösen wird.

Die [ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) Beispiel wird diese Technik veranschaulicht. Das Programm auch veranschaulicht, wie die [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) Eigenschaft eine `Button` auf `false` beim Drücken der `Button` ist nicht mehr gültig. Deaktiviert ein `Button` generiert kein `Clicked` Ereignis.

## <a name="anonymous-event-handlers"></a>Anonyme Ereignishandler

Es ist möglich, definieren Sie `Clicked` Handler als anonymen Lambda-Funktionen als die [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) demonstriert. Allerdings können die anonyme Handler ohne einige unübersichtlichen Reflektionscode freigegeben werden.

## <a name="distinguishing-views-with-ids"></a>Unterscheiden von Sichten mit IDs

Mehrere `Button` Objekte können auch durch Festlegen von unterschieden werden die [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) Eigenschaft oder [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) Eigenschaft, um eine `string`. Diese Eigenschaft wird definiert, indem `Element` jedoch nicht in Xamarin.Forms verwendet. Sie dient ausschließlich von der Anwendungsprogrammen verwendet werden.

Die [ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) Beispiel verwendet die gleichen Ereignishandler für alle 10 Nummerntasten auf eine Zehnertastatur und unterschieden zwischen ihnen mit der `StyleId` Eigenschaft:

[![Dreifacher Screenshot der einfachste Zehnertastatur](images/ch06fg04-small.png "Rechner")](images/ch06fg04-large.png#lightbox "Rechner")

## <a name="saving-transient-data"></a>Speichern vorübergehender Daten

Viele Anwendungen müssen zum Speichern von Daten, wenn ein Programm beendet wird und diese Daten erneut geladen, beim nächsten der Anwendung Start. Die [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Klasse definiert mehrere Elemente, mit deren Hilfe das Programm speichern und Wiederherstellen vorübergehende Daten:

- Die [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) Eigenschaft ist ein Wörterbuch mit `string` Schlüssel und `object` Elemente. Die Inhalte des Wörterbuchs automatisch im lokalen Speicher von Anwendung vor der Beendigung des Programms gespeichert und erneut geladen, wenn das Programm wird gestartet.
- Die `Application` Klasse definiert drei geschützte virtuelle Methoden, die das Programm mit der standardmäßigen `App` -Klasse überschreibt: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), und [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). Diese beziehen sich auf *Anwendungslebenszyklus* Ereignisse.
- Die [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) Methode speichert den Inhalt des Wörterbuchs.

Es ist nicht notwendig, `SavePropertiesAsync`. Die Inhalte des Wörterbuchs vor Beendigung des Programms gespeichert und abgerufen, die vor dem Programmstart automatisch. Es ist hilfreich, während die Anwendung testen, um Daten zu speichern, wenn das Programm abstürzt.

Sie ist auch nützlich sind:

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), eine statische Eigenschaft, die das aktuelle zurückgibt `Application` -Objekt, das Sie dann, zum Abrufen verwenden können der `Properties` Wörterbuch.

Der erste Schritt werden alle Variablen auf der Seite zu identifizieren, die beim Beenden des Programms beibehalten werden sollen. Wenn Sie alle Orte kennen, in denen diese Variablen ändern, Sie können einfach hinzufügen der `Properties` Wörterbuch an diesem Punkt. In der Page-Konstruktor, legen Sie die Variablen aus der `Properties` Wörterbuch, wenn der Schlüssel vorhanden ist.

Ein größeres Programms müssen wahrscheinlich Lebenszyklusereignisse für die Anwendung zu verarbeiten. Am wichtigsten ist die `OnSleep` Methode. Ein Aufruf dieser Methode gibt an, dass das Programm im Vordergrund verlassen hat. Vielleicht der Benutzer geklickt hat die **Home** Schaltfläche auf dem Gerät oder alle Anwendungen, die angezeigt oder Telefon heruntergefahren wird. Ein Aufruf von `OnSleep` ist die einzige Benachrichtigung, dass ein Programm empfängt, bevor er beendet wird. Das Programm sollte diese Gelegenheit um sicherzustellen, dass die `Properties` Wörterbuch ist auf dem neuesten Stand.

Ein Aufruf von `OnResume` gibt an, dass das Programm nicht beendet nach dem letzten Aufruf von `OnSleep` jedoch jetzt erneut im Vordergrund ausgeführt. Das Programm möglicherweise Gelegenheit verwenden, um Internet-Verbindungen (z. B.) zu aktualisieren.

Ein Aufruf von `OnStart` während des Programmstarts auftritt. Es ist nicht notwendig, warten Sie, bis diese Methode aufgerufen wird, für den Zugriff auf die `Properties` Wörterbuch, da der Inhalt bereits wurden wiederhergestellt, wenn die `App` Konstruktor aufgerufen wird.

Die [ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) Beispiel ähnelt **SimplestKeypad** mit dem Unterschied, dass die Anwendung verwendet die `OnSleep` überschreiben, um den aktuellen Zehnertastatur-Eintrag zu speichern und Der Seitenkonstruktor zum Wiederherstellen von Daten.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 6 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Kapitel 6-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Kapitel 6 f#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
