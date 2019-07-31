---
title: Arbeiten mit tvos-Status Indikatoren in xamarin
description: In diesem Dokument wird beschrieben, wie Sie in einer mit xamarin erstellten tvos-App mit Status Indikatoren arbeiten. Es werden sowohl Status-als auch Aktivitätsindikatoren erläutert.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/25/2018
ms.openlocfilehash: 6ab1b4ad5493075e8806190e77f6d234354af9ff
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648972"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>Arbeiten mit tvos-Status Indikatoren in xamarin

_In diesem Artikel wird das Entwerfen und arbeiten mit Status Indikatoren in einer xamarin. tvos-App behandelt._

Es kann vorkommen, dass Ihre xamarin. tvos-App neuen Inhalt laden oder einen langwierigen Verarbeitungsvorgang ausführen muss. Während dieser Zeit sollten Sie entweder einen Aktivitätsindikator oder eine Statusanzeige darstellen, um dem Benutzer mitzuteilen, dass die APP weiterhin ausgeführt wird, und Ihnen einen Hinweis auf die Länge der Aufgabe geben, die ausgeführt wird.

![Beispiel Status Indikatoren](progress-indicators-images/intro01.png "Beispiel Status Indikatoren")

## <a name="about-activity-indicators"></a>Informationen zu Aktivitätsindikatoren

Ein Aktivitätsindikator ist als drehendes Zahnrad dargestellt und wird verwendet, um eine Aufgabe mit einer unbestimmten Länge darzustellen. Der Indikator wird angezeigt, wenn die Aufgabe gestartet wird und nach Abschluss der Aufgabe nicht mehr angezeigt wird.

Apple hat die folgenden Vorschläge zum Arbeiten mit Aktivitätsindikatoren:

- **Verwenden Sie nach Möglichkeit stattdessen** Status anzeigen, da der Benutzer in einem Aktivitätsindikator kein Feedback darüber gibt, wie lange der Prozess ausgeführt wird. verwenden Sie immer eine Statusanzeige, wenn die Länge bekannt ist (z. b., wie viele Bytes in einer Datei heruntergeladen werden sollen).
- **Den Indikator animieren** : Benutzer verknüpfen einen Indikator für eine stationäre Aktivität mit einer angehaltenen app. Daher sollten Sie den Indikator immer animieren, während er angezeigt wird.
- **Beschreiben Sie die verarbeitete Aufgabe** . das Anzeigen des Aktivitäts Indikators allein ist nicht ausreichend. der Benutzer muss über den Prozess informiert werden, auf den Sie warten. Fügen Sie eine aussagekräftige Bezeichnung (in der Regel einen einzelnen, vollständigen Satz) ein, die die Aufgabe eindeutig definiert.

## <a name="about-progress-bars"></a>Informationen zu Status anzeigen

Eine Statusanzeige wird als Linie dargestellt, die die Farbe füllt, um den Zustand und die Länge einer zeitaufwändigen Aufgabe anzugeben. Status leisten sollten immer verwendet werden, wenn die Länge der Aufgaben bekannt ist oder berechnet werden kann.

Apple hat die folgenden Vorschläge zum Arbeiten mit Status leisten:

- Genaue **Berichts** Status: Status leisten sollten immer eine genaue Darstellung der zum Ausführen einer Aufgabe benötigten Zeit darstellen. Geben Sie niemals die Zeit an, die die APP als ausgelastet angezeigt werden soll.
- **Verwendung für klar definierte Zeiträume** : Status leisten sollten nicht nur anzeigen, dass eine lange Aufgabe stattfindet, sondern auch den Benutzer und die Angabe, wie viel der Aufgabe abgeschlossen ist, und eine Schätzung der verbleibenden Zeit anzeigen.

## <a name="progress-indicators-and-storyboards"></a>Status Indikatoren und Storyboards

Die einfachste Möglichkeit, mit einer Statusanzeige in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
1. Doppelklicken Sie im **Lösungspad**auf die Datei **Main. Storyboard** , und öffnen Sie Sie zur Bearbeitung.

2. Ziehen Sie einen **Aktivitätsindikator** aus der **Toolbox** , und legen Sie ihn in der Ansicht ab: 

    ![Ein Aktivitätsindikator](progress-indicators-images/activity01.png "Ein Aktivitätsindikator")

3. Auf der Registerkarte **Widget** des **Eigenschaftenpad**können Sie verschiedene Eigenschaften des Aktivitäts Indikators anpassen, z. b. den **Stil**, das **Verhalten**und den **Namen**: 

    ![Die Widget-Registerkarte für einen Aktivitätsindikator](progress-indicators-images/activity02.png "Die Widget-Registerkarte für einen Aktivitätsindikator")
    
    Der **Name** bestimmt den Namen der Eigenschaft, die den Aktivitätsindikator im C# Code darstellt.

4. Ziehen Sie eine **Fortschritts Ansicht** aus der **Toolbox** , und legen Sie Sie in der Ansicht ab: 

    ![Eine Fortschritts Ansicht](progress-indicators-images/activity03.png "Eine Fortschritts Ansicht")

5. Auf der Registerkarte **Widget** des **Eigenschaften-Explorers**können Sie mehrere Eigenschaften der Fortschritts Ansicht anpassen, z. b. den **Stil**, den **Fortschritt** (Prozent abgeschlossen) und den **Namen**: 

    ![Die Widget-Registerkarte für eine Fortschritts Ansicht](progress-indicators-images/activity04.png "Die Widget-Registerkarte für eine Fortschritts Ansicht")
    
    Der **Name** bestimmt den Namen der Eigenschaft, die die Fortschritts Ansicht im C# Code darstellt.

6. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Doppelklicken Sie im **Projektmappen-Explorer**auf die Datei **Main. Storyboard** , und öffnen Sie Sie zur Bearbeitung.

2. Ziehen Sie einen **Aktivitätsindikator** aus der **Toolbox** , und legen Sie ihn in der Ansicht ab: 

    ![Einen Aktivitätsindikator](progress-indicators-images/activity01-vs.png
    "für einen Aktivitätsindikator")

3. Auf der Registerkarte **Widget** des **Eigenschaften-Explorers**können Sie verschiedene Eigenschaften des Aktivitäts Indikators anpassen, z. b. den **Stil**, das **Verhalten**und den **Namen**: 

    ![Die Widget-Registerkarte für einen Aktivitätsindikator](progress-indicators-images/activity02-vs.png "Die Widget-Registerkarte für einen Aktivitätsindikator")

    Der **Name** bestimmt den Namen der Eigenschaft, die den Aktivitätsindikator im C# Code darstellt.

4. Ziehen Sie eine **Fortschritts Ansicht** aus der **Toolbox** , und legen Sie Sie in der Ansicht ab: 

   ![Eine Fortschritts Ansicht](progress-indicators-images/activity03-vs.png "Eine Fortschritts Ansicht")

5. Auf der Registerkarte **Widget** des **Eigenschaften-Explorers**können Sie mehrere Eigenschaften der Fortschritts Ansicht anpassen, z. b. den **Stil**, den **Fortschritt** (Prozent abgeschlossen) und den **Namen**: 

    ![Die Widget-Registerkarte für eine Fortschritts Ansicht](progress-indicators-images/activity04-vs.png "Die Widget-Registerkarte für eine Fortschritts Ansicht")
    
    Der **Name** bestimmt den Namen der Eigenschaft, die die Fortschritts Ansicht im C# Code darstellt.

6. Speichern Sie die Änderungen.

-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Arbeiten mit Aktivitätsindikatoren

Wie bereits erwähnt, sollten Aktivitätsindikatoren angezeigt werden, wenn Ihre APP einen langen Prozess der unbestimmten Länge aufweist.

An jedem Punkt können Sie sehen, ob ein Aktivitätsindikator animiert wird, indem Sie die `IsAnimating` zugehörige-Eigenschaft überprüfen. Wenn die `HidesWhenStopped` -Eigenschaft `true`ist, wird der Aktivitätsindikator automatisch ausgeblendet, wenn die Animation angehalten wird.

Sie können den folgenden Code verwenden, um die Animation zu starten: 

```csharp
ActivityIndicator.StartAnimating();
```

Und die folgende Animation wird beendet:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Diese Code Ausschnitte gehen davon aus, dass der **Name** des Aktivitäts Indikators auf der Registerkarte **Widget** des IOS-Designers auf **activityindicator** festgelegt wurde.

## <a name="working-with-progress-bars"></a>Arbeiten mit Status leisten

Eine Statusanzeige sollte immer dann verwendet werden, wenn Ihre APP eine lange Ausführungs Aufgabe einer bekannten Dauer ausführt. 

Die `Progress` -Eigenschaft wird verwendet, um den Umfang der Aufgabe festzulegen, die von 0% auf 100% (0,0 bis 1,0) abgeschlossen wurde. Verwenden Sie `ProgressTintColor` die-Eigenschaft, um die Farbe der abgeschlossenen Amount-Leiste `TrackTintColor` festzulegen, und die-Eigenschaft, um die Hintergrundfarbe (nicht abgeschlossene Menge) festzulegen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Status Indikatoren in einer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
