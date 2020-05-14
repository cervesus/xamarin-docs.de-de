---
ms.topic: include
author: jamesmontemagno
ms.author: jamont
ms.date: 05/11/2020
ms.openlocfilehash: b5396061de657690fa3f4cdf68e8a94382918613
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149736"
---
Diese API verwendet Laufzeitberechtigungen unter Android. Stellen Sie sicher, dass Xamarin.Essentials vollständig initialisiert und die Berechtigungsbehandlung in Ihrer App eingerichtet ist.

Im `MainLauncher`-Element oder in einem beliebigen `Activity`-Element des Android-Projekts, das gestartet wird, muss Xamarin.Essentials in der `OnCreate`-Methode initialisiert werden:

```csharp
protected override void OnCreate(Bundle savedInstanceState) 
{
    //...
    base.OnCreate(savedInstanceState);
    Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
    //...
}    
```

Um Laufzeitberechtigungen auf Android zu verwalten, muss Xamarin.Essentials `OnRequestPermissionsResult` erhalten. Fügen Sie allen `Activity`-Klassen den folgenden Code hinzu:

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
{
    Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

    base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}
```
