---
title: Layoutoptionen in xamarin. IOS
description: In diesem Dokument werden verschiedene Möglichkeiten zum Anordnen von Benutzeroberflächen in xamarin. IOS beschrieben. Dabei werden die automatische Größenanpassung und das automatische Layout erläutert.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b825b0d7f9e18350514e3f84e70f901f5fcb6724
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655928"
---
# <a name="layout-options-in-xamarinios"></a>Layoutoptionen in xamarin. IOS

Es gibt zwei verschiedene Mechanismen, um das Layout zu steuern, wenn die Größe einer Ansicht geändert oder gedreht wird:

-  **Automatische Größen** Anpassung – mit dem Filter für die automatische Größenanpassung im Designer können die `AutoresizingMask` Eigenschaften festgelegt werden. Dadurch kann ein Steuerelement an die Ränder des Containers verankert und/oder seine Größe behoben werden. Die automatische Größenanpassung funktioniert in allen Versionen von IOS. Dies wird im folgenden ausführlicher beschrieben.
-  **Automatisches Layout** – eine Funktion in ios 6, die eine differenzierte Steuerung der Beziehungen der UI-Steuerelemente ermöglicht. Sie ermöglicht die Steuerung der Positionen von Elementen im Verhältnis zu anderen Elementen auf der Entwurfs Oberfläche. Dieses Thema wird im Handbuch zum [automatischen Layout mit dem xamarin IOS-Designer](~/ios/user-interface/designer/designer-auto-layout.md) ausführlicher beschrieben.

## <a name="autosizing"></a>Automatisches Anpassen

Wenn ein Benutzer die Größe eines Fensters ändert, z. b. wenn das Gerät gedreht wird und sich die Ausrichtung ändert, ändert das System automatisch die Größe der Ansichten innerhalb dieses Fensters gemäß den Regeln für die automatische Größenanpassung. Diese C# Regeln können in mithilfe der `AutoresizingMask` -Eigenschaft von `UIView` oder in der **Eigenschaftenpad** des IOS-Designers festgelegt werden, wie unten dargestellt:

 [![](layout-options-images/image41.png "Visual Studio für Mac-Designer")](layout-options-images/image41.png#lightbox)

Wenn Sie ein Steuerelement auswählen, können Sie den Speicherort und die Dimensionen des Steuer Elements manuell angeben und das Verhalten für die **Automatische Größen** Anpassung auswählen. Wie im folgenden Screenshot gezeigt, können wir die Quellen und Strukturen im Steuerelement für die automatische Größenanpassung verwenden, um die Beziehung der ausgewählten Ansicht mit ihrem übergeordneten Element zu definieren:

 [![](layout-options-images/image42.png "Visual Studio für Mac-Designer")](layout-options-images/image42.png#lightbox)

Das Anpassen einer *Spring* -Ansicht bewirkt, dass die Größe der Ansicht basierend auf der Breite oder Höhe der übergeordneten Ansicht geändert wird. Wenn Sie einen *Strumpf* anpassen, wird die Ansicht in der Ansicht einen konstanten Abstand zwischen sich selbst und der übergeordneten Ansicht an diesem bestimmten Rand beibehalten.

Diese Einstellungen können auch im Code festgelegt werden:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Um die Einstellungen für die automatische Größenanpassung zu testen, aktivieren Sie in den Optionen des Projekts verschiedene **Unterstützte Geräte Ausrichtungen** :

 [![](layout-options-images/image43a.png "Einstellungen für automatische Größenanpassung")](layout-options-images/image43a.png#lightbox)

Im Code Behind können wir den folgenden Code verwenden, der bewirkt, dass die Größe der beiden Text Steuerelemente horizontal geändert wird:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Wir können die Steuerelemente auch mithilfe des Designers anpassen. Wenn Sie die unten gezeigten Strukturen auswählen, wird das Bild rechtsbündig ausgerichtet, ohne am unteren Rand der Ansicht abgeschnitten zu werden:

 [![](layout-options-images/autoresize.png "Autorotierung")](layout-options-images/autoresize.png#lightbox)

Diese Screenshots zeigen, wie sich die Größe der Steuerelemente ändert, wenn der Bildschirm gedreht wird.

 [![](layout-options-images/image44a.png "Autorotierung")](layout-options-images/image44a.png#lightbox)

Beachten Sie, dass die Textansicht und das Textfeld gestreckt werden, um die gleichen linken und rechten Ränder aufgrund `FlexibleWidth` der-Einstellung beizubehalten. Das Bild hat den oberen und den linken Rand flexibel, d. h., er behält den unteren und rechten Rand bei – behält das Bild beim Drehen des Bildschirms bei. Komplexe Layouts erfordern in der Regel eine Kombination dieser Einstellungen auf jedem sichtbaren Steuerelement, um die Benutzeroberfläche konsistent zu halten und zu verhindern, dass sich Steuerelemente überlappen, wenn sich die Begrenzungen der Ansicht ändern (aufgrund der Drehung oder eines anderen Ereignisses zur Änderungs Änderung).





## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
