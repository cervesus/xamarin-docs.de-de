---
title: "Benutzerdefinierte Schaltfläche"
ms.topic: article
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f77a9b8d3bb69bb47d973a56aed5ad1d49f9a02d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="custom-button"></a>Benutzerdefinierte Schaltfläche

In diesem Abschnitt erstellen Sie eine Schaltfläche mit benutzerdefinierten Bilds anstelle von Text, mit der [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) Widget und eine XML-Datei, die drei verschiedene Bilder, die für die verschiedenen Zustände definiert. Wenn die Schaltfläche aufgerufen werden, wird eine kurze Nachricht angezeigt.

Mit der rechten Maustaste die folgenden drei Images herunterzuladen, und kopieren Sie sie nach der **Ressourcen und Ausgaben möglich** Verzeichnis des Projekts. Diese werden für die verschiedenen Zustände verwendet werden.

 [![Android grüne Symbol für Zustand "normal"](custom-button-images/android-normal.png)](custom-button-images/android-normal.png) [ ![Orange Android Symbol für fokussierte Zustand](custom-button-images/android-focused.png)](custom-button-images/android-focused.png) [ ![Android gelbe Symbol für Zustand "gedrückt"](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png)

Erstellen Sie eine neue Datei in die **Ressourcen und Ausgaben möglich** Verzeichnis mit dem Namen **android_button.xml**. Fügen Sie die folgenden XML-Code:

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

Definiert eine einzelne zeichenbare Ressource, die wodurch das Bild basierend auf den aktuellen Zustand der Schaltfläche geändert werden. Die erste `<item>` definiert **android_pressed.png** wie das Bild aus, wenn die Schaltfläche aufgerufen werden (es ist aktiviert); das zweite `<item>` definiert **android_focused.png** als Image bei der Schaltfläche konzentriert sich (wenn die Schaltfläche mit dem Trackball oder Steuerkreuz markiert ist); und der dritte `<item>` definiert **android_normal.png** als Bild für den normalen Zustand (wenn gedrückt weder mit Fokus). Diese XML-Datei stellt nun eine einzelne zeichenbare Ressource und verweist eine [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) für den Hintergrund das angezeigte Bild ändern sich basierend auf diesen drei Zuständen.


> [!NOTE]
> **Hinweis:** die Reihenfolge der der `<item>` Elemente ist wichtig. Wenn diesem zeichenbaren verwiesen wird, die `<item>`sind durchlaufen Sie die Reihenfolge bestimmen, welcher Schlüssel für den aktuellen Schaltflächenstatus geeignet ist.
> Da das Bild "normale" zuletzt angezeigt wird, ist es nur angewendet, wenn die Bedingungen `android:state_pressed` und `android:state_focused` haben beide "false" ausgewertet.

Öffnen der **Resources/layout/Main.axml** und fügen die [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) Element:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Die `android:background` Attribut gibt die zeichenbaren Ressource an, die für den Hintergrund der Schaltfläche verwendet (, die beim Speichern auf **Resources/drawable/android.xml**, verwiesen wird, als `@drawable/android`). Dies ersetzt die normale Hintergrundbild für die Schaltflächen im gesamten System verwendet. In der Reihenfolge für die zeichenbaren seines Abbilds basierend auf den Zustand der Schaltfläche zu ändern muss das Image auf den Hintergrund angewendet werden.

Um die Schaltfläche "" Wenn gedrückt Aktionen zu machen, fügen Sie den folgenden Code am Ende der [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/) Methode:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Zeichnet die [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) aus dem Layout und fügt dann ein [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Meldung, die bei der [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) geklickt wird.

Die Anwendung jetzt ausführen.


*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
