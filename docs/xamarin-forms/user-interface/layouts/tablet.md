---
title: Layout für Tablet und Desktop-apps
description: In diesem Artikel erläutert das Xamarin.Forms-Anwendung-Layouts für Tablets, im Gegensatz zu Telefone zu optimieren.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: 9d1f54fa4753ba2ef44ba9b8b48a84a3ca932c4b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61369912"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Layout für Tablet und Desktop-apps

Xamarin.Forms unterstützt alle Gerätetypen, die auf den unterstützten Plattformen verfügbar, damit zusätzlich zur Telefonen apps auch ausgeführt werden können:

* iPads,
* Android-tablets
* Windows-Tablets und desktop-PCs (mit Windows 10).

Auf dieser Seite wird kurz erläutert:

* die unterstützten [Gerätetypen](#Device_Types), und
* wie Sie [optimieren](#optimize) Layouts für Tablets und Smartphones.

<a name="Device_Types" />

## <a name="device-types"></a>Gerätetypen

Größere Bildschirm Geräten sind für alle von Xamarin.Forms unterstützte Plattformen verfügbar.

### <a name="ipads-ios"></a>iPads (iOS)

Die Xamarin.Forms-Vorlage enthält automatisch iPad-Unterstützung durch Konfigurieren der **"Info.plist" > Geräte** auf **universelle** (was bedeutet, iPhone und iPad werden unterstützt).

Um eine angenehme Start bieten und sicherzustellen, dass die vollständige Bildschirmauflösung auf allen Geräten verwendet wird, stellen Sie sicher eine [iPad-spezifische Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md) (unter Verwendung eines Storyboards) bereitgestellt wird. Dadurch wird sichergestellt, dass die app für iPad Mini, iPad- und iPad Pro-Geräte ordnungsgemäß gerendert wird.

Vor iOS 9 nahm alle apps im Vollbildmodus auf dem Gerät, aber einige iPads können jetzt durchführen [Teilen Bildschirm Multitasking](~/ios/platform/multitasking.md).
Dies bedeutet, dass Ihre app nur eine schlanke Spalte im Zweifelsfall den Bildschirm, 50 % der Breite des Bildschirms oder den gesamten Bildschirm einnehmen kann.

[![](tablet-images/ipad-sml.png "iPad Split Bildschirm Beispiel")](tablet-images/ipad.png#lightbox "iPad Split-Bildschirm-Beispiel")

Geteilten Bildschirmen bedeutet, dass Sie Ihre app auch mit weniger als 320 Pixel breit ist, oder so weit wie 1366 Pixel breit arbeiten entworfen werden sollten.

### <a name="android-tablets"></a>Android-Tablets

Das Android-Ökosystem verfügt über eine Vielzahl von unterstützten Bildschirmgrößen, von kleinen Smartphones bis zu großen Tablets. Xamarin.Forms können alle Bildschirmgrößen unterstützt, aber als mit anderen Plattformen möglicherweise möchten Ihre Benutzeroberfläche für größeren Geräten anpassen.

Wenn viele verschiedene bildschirmauflösungen zu unterstützen, können Sie Ihre systemeigene Images Ressourcen in unterschiedlichen Größen, die benutzerfreundlichkeit optimieren bereitstellen.
Überprüfen Sie die [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md) Dokumentation (und insbesondere [Erstellen von Ressourcen für unterschiedliche Bildschirmgrößen](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Weitere Informationen zum Strukturieren Sie die Ordner und Dateinamen in Ihrer Android-app Projekt optimierten Image-Ressourcen in Ihrer app angezeigt werden.

### <a name="windows-tablets-and-desktops"></a>Windows-Tablets und Desktops

Zur Unterstützung von Tablets und Desktopcomputer unter Windows müssen Sie verwenden [Windows UWP-Unterstützung](~/xamarin-forms/platform/windows/installation/index.md), welche builds universal-apps, die unter Windows 10 ausgeführt werden.

Apps auf Windows-Tablets und Desktops können beliebiger Dimensionen zusätzlich zum Ausführen der Vollbildmodus verändert werden.

[![](tablet-images/splitscreen-sml.png "Windows Teilen Bildschirm Beispiel")](tablet-images/splitscreen.png#lightbox "Windows Teilen Bildschirm-Beispiel")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimieren für Tablet und Desktop

Können Sie Ihre Xamarin.Forms-Benutzeroberfläche, je nachdem, ob ein Smartphone anpassen oder -Tablet/Desktop-Gerät verwendet wird. Dies bedeutet, dass Sie die benutzerfreundlichkeit für Großbildschirm Geräte wie Tablets und desktop-PCs optimiert werden können.


### <a name="deviceidiom"></a>"Device.Idiom"

Sie können die [ `Device` ](~/xamarin-forms/platform/device.md) Klasse, um das Verhalten Ihrer app oder die Benutzeroberfläche ändern. Mithilfe der `Device.Idiom` Enumeration Sie können

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Dieser Ansatz kann erhebliche Änderungen an einzelnen Seitenlayouts vornehmen oder sogar auf andere Seiten auf größeren Bildschirmen zu rendern, erweitert werden.

### <a name="leveraging-masterdetailpage"></a>Nutzen MasterDetailPage

Die [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) eignet sich ideal für größere Bildschirme, insbesondere auf dem iPad, bei denen es verwendet, die [ `UISplitViewController` ](xref:UIKit.UISplitViewController) einer systemeigenen iOS-Erlebnis zu bieten.

Überprüfen Sie [in diesem Blogbeitrag von Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) angezeigt, wie Sie Ihre Benutzeroberfläche anpassen können, sodass Telefone verwenden Sie ein Layout und größere Bildschirme einen anderen können (mit der `MasterDetailPage`).



## <a name="related-links"></a>Verwandte Links

- [Xamarin-Blog](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe-Beispiel](https://github.com/jamesmontemagno/myshoppe)
