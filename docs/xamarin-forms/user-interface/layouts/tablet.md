---
title: Layout für Tablet-und Desktop-Apps
description: In diesem Artikel wird erläutert, wie Sie Xamarin.Forms Anwendungs Layouts für Tablets im Gegensatz zu Smartphones optimieren.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c53b1e58ad97f7d0e17972a2b232c16e05ecc1a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934887"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Layout für Tablet-und Desktop-Apps

Xamarin.Formsin werden alle Gerätetypen unterstützt, die auf den unterstützten Plattformen verfügbar sind. zusätzlich zu den Smartphones können apps auch unter ausgeführt werden:

- iPads
- Android-Tablets,
- Windows-Tablets und Desktop Computer (auf denen Windows 10 ausgeführt wird).

Auf dieser Seite werden kurz erläutert:

- die unterstützten [Gerätetypen](#device-types)und
- [optimieren](#optimize-for-tablet-and-desktop) von Layouts für Tablets und Smartphones.

## <a name="device-types"></a>Gerätetypen

Größere Bildschirme sind für alle Plattformen verfügbar, die von unterstützt werden Xamarin.Forms .

### <a name="ipads-ios"></a>iPads (IOS)

Die Xamarin.Forms Vorlage enthält automatisch die iPad-Unterstützung durch Konfigurieren der **Info. plist-> Geräte** Einstellung auf **Universal** (was bedeutet, dass iPhone und iPad unterstützt werden).

Um eine angenehme Start Funktion zu gewährleisten und sicherzustellen, dass die voll Bildauflösung auf allen Geräten verwendet wird, sollten Sie sicherstellen, dass ein [iPad-spezifischer Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md) (mit einem Storyboard) bereitgestellt wird. Dadurch wird sichergestellt, dass die APP auf iPad Mini-, iPad-und iPad pro-Geräten korrekt gerendert wird

Vor IOS 9 haben alle apps auf dem Gerät den voll Bildschirm übernommen, aber einige iPads können jetzt [Multitasking mit geteiltem Bildschirm](~/ios/platform/multitasking.md)durchführen.
Dies bedeutet, dass Ihre APP auf der Seite des Bildschirms nur eine schlanke Spalte, 50% der Bildschirmbreite oder den gesamten Bildschirm aufnehmen könnte.

[![Beispiel für den iPad-Split](tablet-images/ipad-sml.png)](tablet-images/ipad.png#lightbox "Beispiel für den iPad-Split")

Die Funktion "Split-Screen" bedeutet, dass Sie Ihre APP so entwerfen müssen, dass Sie mit weniger als 320 Pixel Breite oder mit einer Breite von höchstens 1366 Pixel funktioniert.

### <a name="android-tablets"></a>Android-Tablets

Das Android-Ökosystem hat unzählige unterstützte Bildschirmgrößen, von kleinen Telefonen bis hin zu großen Tablets. Xamarin.Formskann alle Bildschirmgrößen unterstützen, aber wie bei den anderen Plattformen möchten Sie möglicherweise die Benutzeroberfläche für größere Geräte anpassen.

Wenn Sie viele verschiedene Bildschirmauflösungen unterstützen, können Sie Ihre systemeigenen Image Ressourcen in unterschiedlichen Größen bereitstellen, um die Benutzer Leistung zu optimieren.
Weitere Informationen zum Strukturieren der Ordner und Dateinamen in Ihrem Android-App-Projekt, um optimierte Bild Ressourcen in Ihre APP einzubeziehen, finden Sie in der Dokumentation zu [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md) (und insbesondere in [Erstellen von Ressourcen für unterschiedliche Bildschirmgrößen](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)).

### <a name="windows-tablets-and-desktops"></a>Windows-Tablets und-Desktops

Zur Unterstützung von Tablets und Desktop Computern, auf denen Windows ausgeführt wird, müssen Sie die [Windows UWP-Unterstützung](~/xamarin-forms/platform/windows/installation/index.md)verwenden, die universelle Apps erstellt, die unter Windows 10 ausgeführt werden.

Apps, die auf Windows-Tablets und-Desktops ausgeführt werden, können neben dem Vollbildmodus auch in beliebige Dimensionen geändert werden.

[![Beispiel für Windows-Split Screen](tablet-images/splitscreen-sml.png)](tablet-images/splitscreen.png#lightbox "Beispiel für Windows-Split Screen")

## <a name="optimize-for-tablet-and-desktop"></a>Optimieren von Tablet und Desktop

Xamarin.FormsJe nachdem, ob ein Smartphone oder Tablet/Desktop-Gerät verwendet wird, können Sie die Benutzeroberfläche anpassen. Dies bedeutet, dass Sie die Benutzer Funktionen für Geräte mit großen Bildschirmen (z. b. Tablets und Desktop Computern) optimieren können.

### <a name="deviceidiom"></a>Device. Idiom

Sie können die- [`Device`](~/xamarin-forms/platform/device.md) Klasse verwenden, um das Verhalten Ihrer APP oder Benutzeroberfläche zu ändern. Mithilfe der- `Device.Idiom` Enumeration können Sie

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Diese Vorgehensweise kann erweitert werden, um bedeutende Änderungen an den einzelnen Seitenlayouts vorzunehmen, oder sogar um ganz andere Seiten auf größeren Bildschirmen zu erzeugen.

### <a name="leverage-masterdetailpage"></a>Masterdetailpage nutzen

Der [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) eignet sich ideal für größere Bildschirme, insbesondere auf dem iPad, auf dem es verwendet, [`UISplitViewController`](xref:UIKit.UISplitViewController) um eine native IOS-Darstellung bereitzustellen.

Lesen Sie [diesen xamarin-Blogbeitrag](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/) , um zu erfahren, wie Sie Ihre Benutzeroberfläche so anpassen können, dass Telefone ein Layout verwenden und größere Bildschirme andere (mit) verwenden können `MasterDetailPage` .

## <a name="related-links"></a>Ähnliche Themen

- [Xamarin-Blog](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [Myshoppe-Beispiel](https://github.com/jamesmontemagno/myshoppe)
