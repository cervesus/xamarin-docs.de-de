---
title: Layout für Tablet und Desktop-apps
description: In diesem Artikel wird erläutert, wie Xamarin.Forms Anwendungslayouts für Tablets, im Gegensatz zu Telefone optimiert werden kann.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: 932b4aa9c865501284b02fc35e12dc8d7e7166fc
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244844"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Layout für Tablet und Desktop-apps

Xamarin.Forms unterstützt alle Gerätetypen, die auf unterstützten Plattformen verfügbar, damit zusätzlich zu den Telefonen apps auch ausgeführt werden können:

* iPads,
* Android-tablets
* Windows-Tablets und desktop-Computer (unter Windows 10).

Auf dieser Seite werden kurz erläutert:

* die unterstützten [Gerätetypen](#Device_Types), und
* wie [optimieren](#optimize) Layouts für Tablets und Telefone.

<a name="Device_Types" />

## <a name="device-types"></a>Gerätetypen

Größere Bildschirm Geräte sind verfügbar für alle Plattformen von Xamarin.Forms unterstützt.

### <a name="ipads-ios"></a>iPads (iOS)

Enthält die Xamarin.Forms-Vorlage automatisch iPad Unterstützung durch Konfigurieren der **"Info.plist" > Geräte** auf **Universal** (d. h. iPhone und iPad werden unterstützt).

Um die Bedienung einer angenehmeren starten und sicherzustellen, dass die Auflösung Vollbild auf allen Geräten verwendet wird, stellen Sie sicher ein [iPad-spezifische Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md) (mithilfe eines Drehbuchs) bereitgestellt wird. Dadurch wird sichergestellt, dass die app auf iPad Mini, iPad und iPad-Pro-Geräte ordnungsgemäß gerendert wird.

Vor iOS 9 alle apps gedauert hat, um den gesamten Bildschirm auf dem Gerät, aber einige iPads können jetzt ausführen [Teilen Bildschirm Multitasking](~/ios/platform/multitasking.md).
Dies bedeutet, dass Ihre app einfach nur eine flache Spalte auf dem Bildschirm, 50 % der Breite des Bildschirms oder den gesamten Bildschirm einnehmen kann.

[![](tablet-images/ipad-sml.png "iPad Split Bildschirm Beispiel")](tablet-images/ipad.png#lightbox "iPad Split-Bildschirm-Beispiel")

Geteiltem Bildschirm Funktionalität bedeutet, dass Sie berücksichtigen sollten, Ihre app auch bei weniger als 320 Pixel breit ist, oder so weit wie 1366 Pixel breit und ordnungsgemäß funktionieren.

### <a name="android-tablets"></a>Android-Tablets

Das Android-Ökosystem verfügt über eine Vielzahl von unterstützten Bildschirmgrößen, aus kleinen Smartphones bis zu großen Tablets. Xamarin.Forms können alle Bildschirmgrößen unterstützt, allerdings als mit den anderen Plattformen sollten Sie zum Anpassen Ihrer Benutzeroberfläche für größere Geräte.

Wenn viele verschiedene bildschirmauflösungen zu unterstützen, können Sie Ihre systemeigene Images Ressourcen in verschiedenen Größen, um die benutzerfreundlichkeit zu optimieren bereitstellen.
Überprüfen Sie die [Android Ressourcen](~/android/app-fundamentals/resources-in-android/index.md) Dokumentation (insbesondere [Erstellen von Ressourcen für die unterschiedlichen Bildschirmgrößen](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Weitere Informationen zum Ordner und alle Dateinamen in der Android-app-Struktur Projekt, um optimierten Images Ressourcen in Ihrer app einzuschließen.

### <a name="windows-tablets-and-desktops"></a>Windows-Tablets und PCs

Unterstützung von Tablets und Desktopcomputer, auf denen Windows ausgeführt wird, müssen mit [Windows uwp-Unterstützung](~/xamarin-forms/platform/windows/installation/index.md), welche builds universelle apps für Windows 10.

Apps auf Windows-Tablets und PCs können auf beliebige Dimensionen darüber hinaus ausgeführt Vollbildmodus verändert werden.

[![](tablet-images/splitscreen-sml.png "Die Fenster teilen Bildschirm Beispiel")](tablet-images/splitscreen.png#lightbox "Bildschirm Beispiel die Fenster teilen")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimieren für Tablet und Desktop

Können Sie je nach Ihrer Xamarin.Forms-Benutzeroberfläche anpassen, ob ein Telefon oder Tablet-Desktop-Gerät verwendet wird. Dies bedeutet, dass die benutzererfahrung für die großen Bildschirm Geräte wie Tablets und desktop-PCs optimiert werden kann.


### <a name="deviceidiom"></a>Device.Idiom

Sie können die [ `Device` ](~/xamarin-forms/platform/device.md) Klasse, um das Verhalten Ihrer app oder-Benutzeroberfläche ändern. Mithilfe der `Device.Idiom` Enumeration Sie können

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Dieser Ansatz kann erweitert werden, wichtige Änderungen an einzelnen Seitenlayouts vornehmen, oder sogar gänzlich andere Seiten auf größere Bildschirme zu rendern.

### <a name="leveraging-masterdetailpage"></a>Nutzung von MasterDetailPage

Die [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) eignet sich ideal für größere Bildschirme, insbesondere auf dem iPad, bei denen es verwendet, die [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) einer systemeigenen iOS zu ermöglichen.

Überprüfen Sie [diesem Blogbeitrag Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) um anzuzeigen, wie Sie Ihre Benutzeroberfläche anpassen, sodass Telefone ein Layout für größere Bildschirme verwenden und eine andere (mit der `MasterDetailPage`).



## <a name="related-links"></a>Verwandte Links

- [Xamarin-Blog](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe-Beispiel](https://github.com/jamesmontemagno/myshoppe)
