---
title: Neues Verweis zählungs System in xamarin. IOS
description: In diesem Dokument wird das erweiterte Verweis zählungs System von xamarin beschrieben, das in allen xamarin. IOS-Anwendungen standardmäßig aktiviert ist.
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 221c3a3bb82b5b46f4afea5ec43fcdd5c00b0556
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199326"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Neues Verweis zählungs System in xamarin. IOS

Xamarin. IOS 9.2.1 hat das erweiterte Verweis Zählsystem standardmäßig für alle Anwendungen eingeführt. Sie kann verwendet werden, um viele Speicherprobleme zu vermeiden, die in früheren Versionen von xamarin. IOS schwer zu verfolgen und zu beheben waren.

## <a name="enabling-the-new-reference-counting-system"></a>Aktivieren des neuen Verweis zählungs Systems

Ab xamarin 9.2.1 wird das neue Verweis Zählsystem standardmäßig für **alle** Anwendungen aktiviert.

Wenn Sie eine vorhandene Anwendung entwickeln, können Sie die CSPROJ-Datei überprüfen, um sicherzustellen, dass `MtouchUseRefCounting` alle Vorkommen von `true`auf festgelegt sind, wie unten dargestellt:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Wenn Sie auf festgelegt `false` ist, werden die neuen Tools nicht von der Anwendung verwendet.

### <a name="using-older-versions-of-xamarin"></a>Verwenden älterer Versionen von xamarin

Xamarin. IOS 7.2.1 und höher bietet eine erweiterte Vorschau unseres neuen Referenz zählungs Systems.

**Classic API:**

Aktivieren Sie zum Aktivieren dieses neuen Verweis zählungs Systems das Kontrollkästchen **Verweis Zähl Erweiterung verwenden** auf der Registerkarte **erweitert** der **IOS**-Buildoptionen Ihres Projekts, wie unten dargestellt: 

[![](newrefcount-images/image1.png "Aktivieren des neuen Verweis zählungs Systems")](newrefcount-images/image1.png#lightbox)

Beachten Sie, dass diese Optionen in neueren Versionen von Visual Studio für Mac entfernt wurden.

 **[Unified API:](~/cross-platform/macios/unified/index.md)**

 Die neue Verweis Zähl Erweiterung ist für die Unified API erforderlich und sollte standardmäßig aktiviert sein. Bei älteren Versionen Ihrer IDE ist dieser Wert möglicherweise nicht automatisch aktiviert, und Sie müssen möglicherweise selbst eine Prüfung durchgeführt haben.


> [!IMPORTANT]
> Eine frühere Version dieses Features ist seit MonoTouch 5,2 verfügbar, war aber nur für **Sgen** als experimentelle Vorschau verfügbar. Diese neue, erweiterte Version ist jetzt auch für den **Boehm** -Garbage Collector verfügbar.


In der Vergangenheit gab es zwei Arten von Objekten, die von xamarin. IOS verwaltet werden: solche, die lediglich ein Wrapper um ein natives Objekt (Peer-Objekte) waren, und diejenigen, die neue Funktionen (abgeleitete Objekte) erweitert oder integriert haben (in der Regel durch die Beibehaltung des zusätzlichen Speicher internen Zustands). Bisher war es möglich, ein Peer Objekt mit dem Zustand zu vergrößern (z. b. durch C# Hinzufügen eines-Ereignis Handlers), aber das Objekt kann nicht referenziert und dann gesammelt werden. Dies kann später zu einem Absturz führen (z. b. wenn die Ziel-C-Laufzeit wieder in das verwaltete Objekt aufgerufen wurde).

Das neue System aktualisiert Peer Objekte automatisch in Objekte, die von der Laufzeit verwaltet werden, wenn zusätzliche Informationen gespeichert werden.

Dadurch werden verschiedene Abstürze gelöst, die in Situationen wie der folgenden aufgetreten sind:

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

Ohne die Verweis Zähler Erweiterung würde dieser Code abstürzen, `cell` weil entladbar ist, und `TouchDown` somit seinen Delegaten, der in einen verbleibenden Zeiger übersetzt wird.

Die Erweiterung für Verweis Zähler stellt sicher, dass das verwaltete Objekt aktiv bleibt und seine Auflistung verhindert, vorausgesetzt, dass das systemeigene Objekt durch nativen Code beibehalten wird.

Das neue System entfernt auch die Notwendigkeit der *meisten* privaten Unterstützungs Felder, die in Bindungen verwendet werden. Dies ist der Standardansatz, um die Instanz aktiv zu halten. Der verwaltete Linker ist intelligent genug, um alle *nicht benötigten* Felder aus Anwendungen mithilfe der neuen Verweis Anzahl Erweiterung zu entfernen.

Dies bedeutet, dass jede Instanz des verwalteten Objekts weniger Arbeitsspeicher beansprucht als zuvor. Außerdem wird ein verwandtes Problem gelöst, bei dem einige Sicherungs Felder Verweise enthalten, die von der Ziel-C-Laufzeit nicht mehr benötigt werden. dadurch ist es schwierig, Arbeitsspeicher freizugeben.
