---
title: Benutzerdefinierte Schaltfläche
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b5ccefa1eb7e659584c1c82481bbd4473a3a8abc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122854"
---
# <a name="custom-button"></a>Benutzerdefinierte Schaltfläche

In diesem Abschnitt erstellen Sie eine Schaltfläche mit einem benutzerdefinierten Image anstelle von Text, mit der [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) -Widget und einer XML-Datei, die drei verschiedene Bilder, die für die verschiedenen Zustände definiert. Wenn die Schaltfläche gedrückt wird, wird eine kurze Nachricht angezeigt werden.

Mit der rechten Maustaste die drei Bilder unten herunterladen, und kopieren Sie sie in der **Ressourcen/drawable** für Ihr Projekt. Diese werden für die verschiedenen Zustände verwendet werden.

 [![Grün-Android-Symbol für den normalen Zustand](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![Android Orange Symbol für fokussierte Zustand](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![Android gelbes Symbol für gedrückten Zustand](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Erstellen Sie eine neue Datei in die **Ressourcen/drawable** Verzeichnis mit dem Namen **android_button.xml**. Fügen Sie den folgenden XML-Code ein:

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

Definiert eine einzelne drawable-Ressource, die wodurch das Image basierend auf den aktuellen Zustand der Schaltfläche geändert werden. Die erste `<item>` definiert **android_pressed.png** wie das Bild aus, wenn die Schaltfläche gedrückt wird (es ist wurde aktiviert); die zweite `<item>` definiert **android_focused.png** wie das Bild bei der Schaltfläche "konzentriert sich (wenn die Schaltfläche markiert wird, mit dem Trackball oder Steuerkreuz); und der dritte `<item>` definiert **android_normal.png** als Abbild für den Normalzustand (wenn gedrückt weder mit Fokus). Diese XML-Datei stellt jetzt eine einzelne drawable-Ressource und verweist eine [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) für den Hintergrund, das angezeigte Bild ändern sich basierend auf diesen drei Zuständen.


> [!NOTE]
> Die Reihenfolge der `<item>` Elemente ist wichtig. Wenn diesem drawable verwiesen wird, die `<item>`sind in der richtigen Reihenfolge zu bestimmen, welche für den aktuellen Zustand der Schaltfläche zu durchlaufen.
> Da das Image "normale" letzte ist, ist es nur angewendet, wenn die Bedingungen `android:state_pressed` und `android:state_focused` haben beide "false" ausgewertet.

Öffnen der **Resources/layout/Main.axml** -Datei und fügen die [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) Element:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Die `android:background` Attribut gibt an, die drawable-Ressource, die für den Hintergrund der Schaltfläche verwendet (die beim Speichern auf **Resources/drawable/android.xml**, verwiesen wird, als `@drawable/android`). Dies ersetzt das normale Hintergrundbild für Schaltflächen im gesamten System verwendet. In der Reihenfolge für die drawable um das Image basierend auf den Zustand der Schaltfläche zu ändern muss das Abbild auf den Hintergrund angewendet werden.

Damit wird die Schaltfläche mit den etwas Wenn gedrückt, fügen Sie den folgenden Code am Ende der [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)
Methode:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Hierbei werden zusammengefasst, die [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) aus dem Layout an, und fügt dann ein [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) anzuzeigende Nachricht bei der [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) geklickt wird.

Die Anwendung jetzt ausführen.


*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
