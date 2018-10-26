---
title: Neue Verweiszählung-Systems in Xamarin.iOS
description: Dieses Dokument beschreibt die erweiterten verweiszählung Xamarin System, das alle Xamarin.iOS-Anwendungen standardmäßig aktiviert.
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: 3a40605dd58cac0bcf14c156ecf65aa3fec52bc6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103386"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Neue Verweiszählung-Systems in Xamarin.iOS

Xamarin.iOS 9.2.1 eingeführt, die verbesserte verweiszählung System für alle Anwendungen standardmäßig. Es kann verwendet werden, um viele Speicherprobleme zu vermeiden, die schwer zu verfolgen und korrigieren Sie in früheren Versionen von Xamarin.iOS.

## <a name="enabling-the-new-reference-counting-system"></a>Aktivieren die neue Verweiszählung-System

Ab Xamarin 9.2.1 zählen System neue Ref aktiviert ist, um **alle** Anwendungen standardmäßig.

Wenn Sie eine vorhandene Anwendung entwickeln, sehen Sie die CSPROJ-Datei, um sicherzustellen, dass alle Vorkommen von `MtouchUseRefCounting` festgelegt `true`, z.B. unter:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Wenn sie, um festgelegt ist `false` wird Ihre Anwendung nicht die neue Tools verwendet werden.

### <a name="using-older-versions-of-xamarin"></a>Ältere Versionen von Xamarin verwenden

Xamarin.iOS 7.2.1 und Funktionen Sie über eine verbesserte Vorschau auf unser neues verweiszählung-System.

**Klassische-API:**

Um diese neue zählen Referenzsystem zu aktivieren, aktivieren die **verweiszählungserweiterung verwenden** Kontrollkästchen finden Sie in der **erweitert** Ihres Projekts auf der Registerkarte **iOS-Buildoptionen** , wie unten dargestellt: 

[![](newrefcount-images/image1.png "Aktivieren Sie die neue zählen Referenzsystem")](newrefcount-images/image1.png#lightbox)

Beachten Sie, dass diese Optionen in neueren Versionen von Visual Studio für Mac entfernt wurden

 **[Einheitliche API:](~/cross-platform/macios/unified/index.md)**

 Neue verweiszählungserweiterung ist erforderlich, für die Unified API und in der Standardeinstellung aktiviert werden soll. Ältere Versionen Ihrer IDE können keine dieser Wert automatisch aktiviert, und Sie möglicherweise mit einem Häkchen von ihm selbst.

    
> [!IMPORTANT]
> Eine frühere Version dieser Funktion schon seit MonoTouch 5.2 jedoch nur zur **Sgen** als eine experimentelle Vorschau. Diese neue, erweiterte Version steht jetzt auch für die **Boehm** Garbage Collector.


In der Vergangenheit gab es zwei Arten von Objekten, die von Xamarin.iOS verwaltet: solche, die lediglich einen Wrapper für ein systemeigenes Objekt (Peer-Objekte), und diejenigen, die erweitert oder neue Funktionalität (abgeleitete Objekte) - integriert in der Regel durch zusätzliche im Speicher enthaltenen Status beibehalten wurden. Zuvor war es möglich, dass wir ein Peerobjekt, mit dem Status erweitern können (z. B. durch Hinzufügen einer C# -Ereignishandler), aber wir zulassen, dass das Objekt, das nicht referenzierte, und klicken Sie dann die gesammelten finden Sie unter. Dies kann einen Absturz später verursachen (z. B. Wenn Sie die Objective-C-Laufzeit wieder in das verwaltete Objekt aufgerufen wird).

Das neue System aktualisiert automatisch die Objekte auf derselben Ebene in Objekte, die von der Laufzeit verwaltet werden, wenn sie zusätzliche Informationen zu speichern.

Dies löst verschiedene stürzt ab, die in Situationen wie diesen zu aufgetreten ist:

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

Dieser Code würde ohne die Erweiterung der Referenz-Anzahl von Abstürzen, da `cell` entladbaren, wird und daher seine `TouchDown` zu delegieren, die in einen Zeiger zum verbleibende übersetzt.

Die Verweis-Count-Erweiterung wird sichergestellt, das verwaltete Objekt bleibt aktiv, und verhindert, dass die Sammlung, vorausgesetzt das systemeigene Objekt von systemeigenem Code beibehalten wird.

Das neue System auch beseitigt die Notwendigkeit *die meisten* private unterstützungsfelder verwendet Bindungen - der Standardansatz Instanz beibehalten wird. Der verwaltete Linker ist intelligent genug, um alle Mitarbeiter entfernen *nicht benötigte* Felder aus Anwendungen, die mit der neue Verweis zählen Erweiterung.

Dies bedeutet, dass jeder verwaltete Objektinstanzen weniger Arbeitsspeicher als je zuvor nutzen. Es lässt sich auch um ein ähnliches Problem, in dem einige dahinter liegenden Felder Verweise enthalten würde, die nicht mehr von der Objective-C-Laufzeit, macht es schwierig, um Speicherplatz freizugeben erforderlich waren.
