---
title: Benutzerdefinierte Schaltfläche
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: d85c67cf18c61af04cf12bfab58a5b516d380f62
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029353"
---
# <a name="custom-button"></a>Benutzerdefinierte Schaltfläche

In diesem Abschnitt erstellen Sie eine Schaltfläche mit einem benutzerdefinierten Bild anstelle von Text, wobei das [`Button`](xref:Android.Widget.Button) -Widget und eine XML-Datei verwendet werden, die drei verschiedene Bilder definiert, die für die verschiedenen Schaltflächen Zustände verwendet werden sollen. Wenn die Schaltfläche gedrückt wird, wird eine kurze Meldung angezeigt.

Klicken Sie mit der rechten Maustaste, und laden Sie die drei Images unten herunter, und kopieren Sie Sie in das Verzeichnis **Ressourcen/drawable** Ihres Projekts. Diese werden für die verschiedenen Schaltflächen Zustände verwendet.

 [![grünes Android-Symbol für den normalen Zustand](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [![orangefarbenes Android-Symbol für den fokussierten Zustand](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [![gelben Android-Symbol für gedrückten](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

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

Dadurch wird eine einzelne drawable-Ressource definiert, die Ihr Bild basierend auf dem aktuellen Zustand der Schaltfläche ändert. Der erste `<item>` definiert **android_pressed. png** als Bild, wenn die Schaltfläche gedrückt wird (Sie wurde aktiviert); der zweite `<item>` definiert **android_focused. png** als Bild, wenn die Schaltfläche fokussiert ist (wenn die Schaltfläche mit dem trackballunterstützung oder direktionalen Pad hervorgehoben wird). und der dritte `<item>` definiert **android_normal. png** als Bild für den normalen Zustand (wenn weder gedrückt noch fokussiert). Diese XML-Datei stellt nun eine einzelne drawable-Ressource dar, und wenn von einer [`Button`](xref:Android.Widget.Button) auf den Hintergrund verwiesen wird, ändert sich das angezeigte Bild basierend auf diesen drei Zuständen.

> [!NOTE]
> Die Reihenfolge der `<item>` Elemente ist wichtig. Wenn auf diese drawable verwiesen wird, werden die `<item>`s in der Reihenfolge durchlaufen, um zu bestimmen, welche für den aktuellen Schaltflächen Zustand geeignet ist.
> Da das "normale" Bild zuletzt verwendet wird, wird es nur angewendet, wenn die Bedingungen `android:state_pressed` und `android:state_focused` beide als "false" ausgewertet werden.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie das [`Button`](xref:Android.Widget.Button) -Element hinzu:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Das `android:background`-Attribut gibt die drawable-Ressource an, die für den Schaltflächen Hintergrund verwendet werden soll (bei der Speicherung unter " **Resources/drawable/Android. XML**" wird als `@drawable/android`bezeichnet). Dadurch wird das normale Hintergrundbild ersetzt, das für Schaltflächen im gesamten System verwendet wird. Damit das Bild auf der Grundlage des Schaltflächen Zustands geändert werden kann, muss das Bild auf den Hintergrund angewendet werden.

Fügen Sie den folgenden Code am Ende [`OnCreate()`](xref:Android.App.Activity.OnCreate*) der hinzu, um die Schaltfläche zu ändern.
anzuwenden

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Dadurch wird die [`Button`](xref:Android.Widget.Button) aus dem Layout erfasst und dann eine [`Toast`](xref:Android.Widget.Toast) Meldung hinzugefügt, die angezeigt wird, wenn auf die [`Button`](xref:Android.Widget.Button) geklickt wird.

Führen Sie nun die Anwendung aus.

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.
