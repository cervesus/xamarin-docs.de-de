---
title: Benutzerdefinierte Schaltfläche
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: ecb745f2f50b5aa0e22e331a4def0be9d8f86aa5
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510395"
---
# <a name="custom-button"></a>Benutzerdefinierte Schaltfläche

In diesem Abschnitt erstellen Sie eine Schaltfläche mit einem benutzerdefinierten Bild anstelle von Text, wobei das [`Button`](xref:Android.Widget.Button) Widget und eine XML-Datei verwendet werden, die drei verschiedene Bilder definiert, die für die verschiedenen Schaltflächen Zustände verwendet werden sollen. Wenn die Schaltfläche gedrückt wird, wird eine kurze Meldung angezeigt.

Klicken Sie mit der rechten Maustaste, und laden Sie die drei Images unten herunter, und kopieren Sie Sie in das Verzeichnis **Ressourcen/drawable** Ihres Projekts. Diese werden für die verschiedenen Schaltflächen Zustände verwendet.

 [Grünes Android-Symbol für normales Bundesland Orange Android Symbol für Fokus Zustand Gelb Android Symbol für gedrückten Zustand ![](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Erstellen Sie eine neue Datei im **Ressourcen/drawable-** Verzeichnis mit dem Namen **android_button. XML**. Fügen Sie folgenden XML-Code ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Dadurch wird eine einzelne drawable-Ressource definiert, die Ihr Bild basierend auf dem aktuellen Zustand der Schaltfläche ändert. Der erste `<item>` definiert **android_pressed. png** als Bild, wenn die Schaltfläche gedrückt wird (Sie wurde aktiviert); die zweite `<item>` definiert **android_focused. png** als Bild, wenn die Schaltfläche fokussiert ist (wenn die Schaltfläche mit trackballunterstützung oder direktionalem Pad hervorgehoben); Das dritte `<item>` definiert **android_normal. png** als Bild für den normalen Zustand (wenn weder gedrückt noch fokussiert). Diese XML-Datei stellt nun eine einzelne drawable-Ressource dar. Wenn [`Button`](xref:Android.Widget.Button) Sie von einem für den Hintergrund referenziert wird, ändert sich das angezeigte Bild basierend auf diesen drei Zuständen.


> [!NOTE]
> Die Reihenfolge `<item>` der Elemente ist wichtig. Wenn auf diese drawable verwiesen wird, `<item>`werden die e in der Reihenfolge durchlaufen, um zu bestimmen, welche für den aktuellen Schaltflächen Zustand geeignet ist.
> Da das "normale" Bild zuletzt verwendet wird, wird es nur angewendet, wenn `android:state_pressed` die `android:state_focused` Bedingungen und beide als "false" ausgewertet werden.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und [`Button`](xref:Android.Widget.Button) fügen Sie das-Element hinzu:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Das `android:background` -Attribut gibt die drawable-Ressource an, die für den Schaltflächen Hintergrund verwendet werden soll (bei Speichern unter " **Resources/drawable/Android. XML**" wird als `@drawable/android`bezeichnet). Dadurch wird das normale Hintergrundbild ersetzt, das für Schaltflächen im gesamten System verwendet wird. Damit das Bild auf der Grundlage des Schaltflächen Zustands geändert werden kann, muss das Bild auf den Hintergrund angewendet werden.

Fügen Sie den folgenden Code am Ende von hinzu, um die Schaltfläche zu ändern.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Dadurch wird der [`Button`](xref:Android.Widget.Button) aus dem Layout erfasst, und anschließend [`Toast`](xref:Android.Widget.Toast) wird eine Meldung hinzugefügt, [`Button`](xref:Android.Widget.Button) die angezeigt wird, wenn auf das geklickt wird.

Führen Sie nun die Anwendung aus.


*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.
