---
title: "Neue Referenzsystem zählen"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 723a9c4a052f7f432ba0f32ec501af3221b2696f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="new-reference-counting-system"></a>Neue Referenzsystem zählen

Verbesserte verweiszählung System für alle Anwendungen standardmäßig, Xamarin.iOS 9.2.1 eingeführt. Es kann viele Speicherprobleme zu vermeiden, die schwer zu verfolgen und korrigieren Sie in früheren Versionen von Xamarin.iOS wurden verwendet werden.

## <a name="enabling-the-new-reference-counting-system"></a>Aktivieren die neue Verweiszählung-System

Zum Zeitpunkt Xamarin 9.2.1 des neue Ref zählen System aktiviert, um **alle** Anwendungen standardmäßig.

Wenn Sie eine vorhandene Anwendung entwickeln, sehen Sie die CSPROJ-Datei, um sicherzustellen, dass alle Vorkommen von `MtouchUseRefCounting` festgelegt `true`, z. B. unten:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Wenn sie, um festgelegt ist `false` wird Ihre Anwendung nicht die neue Tools verwendet werden.

### <a name="using-older-versions-of-xamarin"></a>Ältere Versionen von Xamarin verwenden

Xamarin.iOS 7.2.1 und höher bietet eine verbesserte Vorschau der unsere neue verweiszählung-System.

**Klassische API:**

Um diese neue zählen Referenzsystem aktivieren zu können, überprüfen Sie die **verweiszählung Erweiterung verwenden** Kontrollkästchen gefunden wird, der **erweitert** des Projekts auf der Registerkarte **iOS Buildoptionen** , wie unten dargestellt: 

[![](newrefcount-images/image1.png "Aktivieren Sie die neue zählen Referenzsystem")](newrefcount-images/image1.png#lightbox)

Beachten Sie, dass diese Optionen in neueren Versionen von Visual Studio für Mac entfernt wurden

 **[Einheitliche API:](~/cross-platform/macios/unified/index.md)**

 Neue verweiszählung Erweiterung ist für die einheitliche API erforderlich und sollte standardmäßig aktiviert werden. Ältere Versionen der IDE möglicherweise diesen Wert automatisch überprüft nicht, und Sie möglicherweise eine Prüfung von ihm selbst platzieren.

    
> [!IMPORTANT]
> Eine frühere Version dieser Funktion seit um MonoTouch 5.2 jedoch nur für verfügbar war **Sgen** als eine experimentelle Vorschau. Diese neue, erweiterte Version steht jetzt auch für die **Boehm** Garbage collection.


In der Vergangenheit wurden zwei Arten von Objekten, die von Xamarin.iOS verwaltet: solche, die lediglich einen Wrapper um ein systemeigenes Objekt (Peer-Objekte), und solche, die erweitert oder neue Funktionalität (abgeleitete Objekte) - integriert in der Regel durch die Verwaltung des Zustands der zusätzliche Speicher wurden. Zuvor war es möglich, dass wir eine Peerobjekt, mit dem Status (z. B. durch Hinzufügen eines C#-ereignishandlers) erweitern konnte aber wir informieren, dass das Objekt, das nicht referenzierte und dann gesammelten finden Sie unter. Dies kann verursacht einen Absturz später auf (z. B. wenn die Objective-C-Laufzeit wieder in das verwaltete Objekt aufgerufen).

Das neue System aktualisiert automatisch die Objekte auf derselben Ebene in Objekte, die von der Laufzeit verwaltet werden, wenn sie zusätzlichen Informationen speichern.

Dies löst verschiedene Abstürze (crashes), die in Situationen wie diese aufgetreten sind:

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

Dieser Code würde ohne Verweis Anzahl Erweiterung abstürzen, da `cell` entladbare, wird und daher seine `TouchDown` zu delegieren, die in einen Zeigertyp zurückbleiben übersetzt.

Die Verweis Count-Erweiterung wird sichergestellt, dass das verwaltete Objekt bleibt aktiv, und verhindert, dass die Auflistung, sofern das systemeigene Objekt von systemeigenem Code beibehalten wird.

Das neue System ebenfalls beseitigt die Notwendigkeit *die meisten* private Sichern von Feldern, die in Bindungen - also den Standardansatz für die Instanz am Leben zu erhalten. Der verwaltete Linker ist intelligent genug, um alle Sprachen entfernen *nicht benötigte* Felder von Anwendungen, die mit dem neuen Verweis zählen Erweiterung.

Dies bedeutet, dass jeder verwalteten Objektinstanzen weniger Speicherplatz als vor dem verarbeiten. Sie löst auch verwandte Problem, in denen einige dahinter liegenden Felder Verweise verfügen würde, die von der Objective-C-Laufzeit, somit die Festplatte für eine bestimmte Menge an Arbeitsspeicher nicht mehr erforderlich waren.
