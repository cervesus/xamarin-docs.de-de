---
title: Grundlagen der Android-Ressourcen
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: 2673021fae2f0a0b45761bf4ed619c92fb826b13
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110133"
---
# <a name="android-resource-basics"></a>Grundlagen der Android-Ressourcen

Fast alle Android-Anwendungen enthalten eine Art von Ressource, zumindest besitzen sie oft die Layouts der Benutzeroberfläche in Form von XML-Dateien. Beim Erstellen einer Xamarin.Android-Anwendung werden Standardressourcen durch die Xamarin.Android-Projektvorlage eingerichtet:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ressourcendateien](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ressourcendateien](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Die fünf Dateien, die die Standardressourcen ausmachen, wurden im Ordner "Resources" (Ressourcen) erstellt:

-  **Icon.PNG** &ndash; das Standardsymbol für die Anwendung

-  **Main.axml** &ndash; die standardmäßige Layout Benutzeroberflächendatei für eine Anwendung. Beachten Sie, dass Android verwendet die **XML** Dateierweiterung Xamarin.Android verwendet das **axml** Dateierweiterung.

-  **Von "Strings.xml"** &ndash; eine Zeichenfolgentabelle zur Unterstützung der Lokalisierung der Anwendung

-  **AboutResources.txt** &ndash; Dies ist nicht erforderlich und kann gelöscht werden. Er bietet nur eine allgemeine Übersicht über den Ordner "Resources" und die darin enthaltenen Dateien.

-  **Resource.Designer.cs** &ndash; diese Datei wird automatisch generiert und verwaltet, die von Xamarin.Android- und enthält die eindeutige IDs, die jeder Ressource zugewiesen. Dies ist sehr ähnlich und Zweck der R.java-Datei, die eine Android-Anwendung in Java geschrieben hätte identisch. Es wird automatisch von den Xamarin.Android-Tools erstellt und wird von Zeit zu Zeit neu generiert werden.


## <a name="creating-and-accessing-resources"></a>Erstellen und den Zugriff auf Ressourcen

Erstellen von Ressourcen ist so einfach wie das Hinzufügen von Dateien in das Verzeichnis für den betreffenden Ressourcentyps. Der nachstehende Screenshot zeigt, dass die Zeichenfolgenressourcen für deutsche Gebietsschemas zu einem Projekt hinzugefügt wurden. Wenn **von "Strings.xml"** wurde hinzugefügt, um die Datei, die **Buildvorgang** auf automatisch festgelegt wurde **AndroidResource** von den Xamarin.Android-Tools:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Buildvorgang für die von "Strings.xml" AndroidResource festgelegt](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Buildvorgang für die von "Strings.xml" AndroidResource festgelegt](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

So können die Ressourcen von den Xamarin.Android-Tools ordnungsgemäß kompiliert und in die APK-Datei eingebettet werden. Falls aus irgendeinem Grund der **Buildvorgang** nicht auf **Android-Ressourcen** festgelegt ist, dann werden die Dateien aus dem APK ausgeschlossen, und jeder Versuch, die Ressource zu laden oder auf diese zuzugreifen, führt zu einem Laufzeitfehler oder zum Absturz der Anwendung.

Darüber hinaus ist es wichtig zu beachten, dass Android für Ressourcenelemente nur Kleinbuchstabe Dateinamen unterstützt, Xamarin.Android ist ein wenig mehr Fehler toleriert; Es wird die Groß- und Kleinbuchstabe Dateinamen unterstützen. Die Konvention für imagenamen Kleinbuchstaben mit Unterstrichen als Trennzeichen verwendet wird (z. B. **meine\_Image\_name.png**). Beachten Sie, dass Ressourcennamen können nicht verarbeitet werden, wenn Striche oder Leerzeichen als Trennzeichen verwendet werden.

Sobald Ressourcen zu einem Projekt hinzugefügt wurden, gibt es zwei Möglichkeiten, diese in einer Anwendung zu verwenden &ndash; programmgesteuert (im Code) oder aus XML-Dateien.


## <a name="referencing-resources-programmatically"></a>Programmgesteuertes Verweisen auf Ressourcen

Um den programmgesteuerten Zugriff auf diese Dateien werden sie über eine eindeutige Ressourcen-ID zugewiesen. Diese Ressourcen-ID ist eine ganze Zahl, die in eine spezielle Klasse namens definierten `Resource`, befindet sich in der Datei **Resource.designer.cs**, und sieht etwa folgendermaßen aus:

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Jede Ressource-ID ist innerhalb einer geschachtelten Klasse enthalten, die den Ressourcentyp entspricht. Z. B., wenn die Datei **Icon.png** wurde dem Projekt hinzugefügt, aktualisiert Xamarin.Android das `Resource` Klasse, erstellen eine geschachtelte Klasse mit dem Namen `Drawable` mit einer internen Konstante mit dem Namen `Icon`.
Dadurch können die Datei **Icon.png** im Code verwiesen werden `Resource.Drawable.Icon`. Die `Resource` Klasse sollte nicht manuell bearbeitet werden, da alle Änderungen, die vorgenommen werden von Xamarin.Android überschrieben werden.

Beim Verweisen auf Ressourcen, programmgesteuert (im Code), können sie über die Klassenhierarchie Ressourcen zugegriffen werden die folgende Syntax verwendet:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **Paketname** &ndash; das Paket, das die Ressource bereitstellt und ist nur erforderlich, wenn Ressourcen von anderen Paketen verwendet werden.

-  **ResourceType** &ndash; Dies ist der geschachtelten Ressource-Typ, der in der oben beschriebenen Ressourcenklasse ist.

-  **Ressourcenname** &ndash; Dies ist der Dateiname der Ressource (ohne Erweiterung) oder den Wert des Attributs "Android: Name" für Ressourcen in ein XML-Element.


## <a name="referencing-resources-from-xml"></a>Verweisen auf Ressourcen aus XML

Ressourcen in eine XML-Datei werden aufgerufen, indem ein eine spezielle Syntax:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **Paketname** &ndash; das Paket, das die Ressource bereitstellt und ist nur erforderlich, wenn Ressourcen von anderen Paketen verwendet werden.

-  **ResourceType** &ndash; Dies ist der geschachtelten Ressource-Typ, der in der Ressourcenklasse ist.

-  **Ressourcenname** &ndash; Dies ist der Dateiname der Ressource (*ohne* der dateityperweiterung) oder den Wert der `android:name` -Attribut für Ressourcen in ein XML-Element.

Z. B. den Inhalt einer Datei Layout **Main.axml**, lauten wie folgt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

Dieses Beispiel besteht aus einem [ `ImageView` ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) , erfordert eine drawable-Ressource mit dem Namen **Flag**. Die `ImageView` hat seine `src` -Attributsatz auf **@drawable/flag**. Wenn die Aktivität gestartet wird, sieht Android innerhalb des Verzeichnisses **Ressource/Drawable** für eine Datei namens **flag.png** (die Dateierweiterung ist möglicherweise ein anderes Bildformat, z. B. **flag.jpg**) Diese Datei zu laden und Anzeigen der Daten in die `ImageView`.
Wenn diese Anwendung ausgeführt wird, würde dies etwa der folgenden Abbildung aussehen:

![Lokalisierte ImageView](android-resource-basics-images/03-localized-screenshot.png)

