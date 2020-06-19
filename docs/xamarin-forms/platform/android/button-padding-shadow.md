---
title: Schaltflächen Auffüll Flächen und Schatten unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Gerät verwenden, das die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwendet.
ms.prod: xamarin
ms.assetid: BD2B60D1-DE6E-4691-A777-8EC5F560A4E9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5554341493b52d20c946a4bcfe2d1230e4a02759
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135563"
---
# <a name="button-padding-and-shadows-on-android"></a>Schaltflächen Auffüll Flächen und Schatten unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser Android-plattformspezifischen Steuerung wird gesteuert, ob Xamarin.Forms Schaltflächen die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwenden Sie wird in XAML verwendet, indem die [`Button.UseDefaultPadding`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) [`Button.UseDefaultShadow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) angefügten Eigenschaften und auf-Werte festgelegt werden `boolean` :

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

Die- `Button.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `Button.SetUseDefaultPadding` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. Button. setusedefaultpadding ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Button}, System. Boolean)) und [ `Button.SetUseDefaultShadow` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. Button. setusedefaultshadow ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Button}, System. Boolean))-Methoden im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace werden verwendet, um zu steuern, ob Schaltflächen Xamarin.Forms die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwenden. Außerdem ist [ `Button.UseDefaultPadding` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. Button. usedefaultpadding ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Button})) und [ `Button.UseDefaultShadow` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. Button. usedefaultshadow ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Button}))-Methoden können verwendet werden, um zurückzugeben, ob eine Schaltfläche den Standard Auffüll Wert bzw. den Standard Schattenwert verwendet.

Das Ergebnis ist, dass Xamarin.Forms Schaltflächen die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwenden können:

![](button-padding-shadow-images/button-padding-and-shadow.png "Default Padding and Shadow Values on Android Buttons")

Beachten Sie, dass im obigen Screenshot jeweils [`Button`](xref:Xamarin.Forms.Button) identische Definitionen vorliegen, mit dem Unterschied, dass die Rechte Seite `Button` die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwendet.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
