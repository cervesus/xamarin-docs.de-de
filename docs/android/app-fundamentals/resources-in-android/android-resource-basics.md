---
title: Grundlagen zu Android-Ressourcen
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/01/2018
ms.openlocfilehash: ac228e6f0c251ae6f0fcabe1be855c6ed4a85d35
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025347"
---
# <a name="android-resource-basics"></a>Grundlagen zu Android-Ressourcen

Fast alle Android-Anwendungen verfügen über einige Ressourcen. häufig verfügen Sie über die Benutzeroberflächen Layouts in Form von XML-Dateien. Wenn eine xamarin. Android-Anwendung erstmalig erstellt wird, werden Standard Ressourcen von der xamarin. Android-Projektvorlage eingerichtet:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ressourcendateien](android-resource-basics-images/01-resource-files-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ressourcendateien](android-resource-basics-images/01-resource-files-xs.png)

-----

Die fünf Dateien, aus denen sich die Standard Ressourcen bilden, wurden im Ressourcen Ordner erstellt:

- **Icon. png** &ndash; das Standard Symbol für die Anwendung

- **Main. axml** &ndash; die Standard-Benutzeroberflächen-Layoutdatei für eine Anwendung. Beachten Sie, dass xamarin. Android die Dateierweiterung **. axml** verwendet, während Android die Dateierweiterung **. XML** verwendet.

- **Strings. XML** &ndash; eine Zeichen folgen Tabelle, um die Lokalisierung der Anwendung zu unterstützen.

- **Abder sources. txt** &ndash; Dies ist nicht erforderlich und kann problemlos gelöscht werden. Er bietet nur eine allgemeine Übersicht über den Ressourcen Ordner und die darin bereitgestellten Dateien.

- **Resource.Designer.cs** &ndash; diese Datei wird automatisch von xamarin. Android generiert und verwaltet und enthält die einzelnen Ressourcen zugewiesenen eindeutigen IDs. Dies ist sehr ähnlich und mit der Datei "R. Java" identisch, die eine in Java geschriebene Android-Anwendung hätte. Sie wird automatisch von xamarin. Android-Tools erstellt und wird von Zeit zu Zeit neu generiert.

## <a name="creating-and-accessing-resources"></a>Erstellen von Ressourcen und Zugreifen auf Ressourcen

Das Erstellen von Ressourcen ist so einfach wie das Hinzufügen von Dateien zum Verzeichnis für den fraglichen Ressourcentyp. Der nachfolgende Screenshot zeigt, dass einem Projekt Zeichen folgen Ressourcen für deutsche Gebiets Schemas hinzugefügt wurden. Beim Hinzufügen von " **Strings. XML** " zur Datei wurde die Buildaktion von den xamarin. Android-Tools automatisch auf " **androidresource** " festgelegt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Buildaktion für "Strings. xml" auf "androidresource" festgelegt](android-resource-basics-images/02-build-action-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Buildaktion für "Strings. xml" auf "androidresource" festgelegt](android-resource-basics-images/02-build-action-xs.png)

-----

Dadurch können die xamarin. Android-Tools die Ressourcen in die APK-Datei ordnungsgemäß kompilieren und einbetten. Wenn die **Buildaktion** aus irgendeinem Grund nicht auf **Android-Ressource**festgelegt ist, werden die Dateien aus dem APK ausgeschlossen, und jeder Versuch, die Ressourcen zu laden oder darauf zuzugreifen, führt zu einem Laufzeitfehler, und die Anwendung stürzt ab.

Es ist auch wichtig zu beachten, dass xamarin. Android bei der Unterstützung von Dateinamen in Kleinbuchstaben für Ressourcen Elemente eine etwas größere Anzahl von Informationen bietet. Es werden sowohl groß-als auch Kleinbuchstaben unterstützt. Die Konvention für Image Namen ist die Verwendung von Kleinbuchstaben mit unterstrichen als Trennzeichen (z. b **. My\_Image\_Name. png**). Beachten Sie, dass Ressourcennamen nicht verarbeitet werden können, wenn Bindestriche oder Leerzeichen als Trennzeichen verwendet werden.

Nachdem Ressourcen zu einem Projekt hinzugefügt wurden, gibt es zwei Möglichkeiten, Sie in einer Anwendung &ndash; Programm gesteuert (innerhalb von Code) oder in XML-Dateien zu verwenden.

## <a name="referencing-resources-programmatically"></a>Programm gesteuertes verweisen auf Ressourcen

Für den programmgesteuerten Zugriff auf diese Dateien wird Ihnen eine eindeutige Ressourcen-ID zugewiesen. Diese Ressourcen-ID ist eine Ganzzahl, die in einer speziellen Klasse namens `Resource`definiert ist, die in der Datei **Resource.Designer.cs**gefunden wird und in etwa wie folgt aussieht:

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

Jede Ressourcen-ID ist in einer geschachtelten Klasse enthalten, die dem Ressourcentyp entspricht. Wenn z. b. die Datei " **Icon. png** " dem Projekt hinzugefügt wurde, aktualisiert xamarin. Android die `Resource`-Klasse und erstellt eine geschachtelte Klasse mit dem Namen `Drawable` mit einer Konstanten innerhalb des Namens `Icon`.
Dadurch kann auf die Datei " **Icon. png** " im Code als `Resource.Drawable.Icon`verwiesen werden. Die `Resource` Klasse sollte nicht manuell bearbeitet werden, da alle daran vorgenommenen Änderungen von xamarin. Android überschrieben werden.

Wenn Sie Programm gesteuert auf Ressourcen verweisen (im Code), können Sie über die Ressourcen Klassenhierarchie auf Sie zugreifen, in der die folgende Syntax verwendet wird:

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

- **PackageName** &ndash; das Paket, das die Ressource bereitstellt, und ist nur erforderlich, wenn Ressourcen aus anderen Paketen verwendet werden.

- **ResourceType** &ndash; Dies ist der geschachtelte Ressourcentyp, der sich innerhalb der oben beschriebenen Ressourcen Klasse befindet.

- **Ressourcen Name** &ndash; Dies ist der Dateiname der Ressource (ohne Erweiterung) oder der Wert des Attributs "Android: Name" für Ressourcen, die sich in einem XML-Element befinden.

## <a name="referencing-resources-from-xml"></a>Verweisen auf Ressourcen aus XML

Auf Ressourcen in einer XML-Datei wird mit einer folgenden speziellen Syntax zugegriffen:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

- **PackageName** &ndash; das Paket, das die Ressource bereitstellt, und ist nur erforderlich, wenn Ressourcen aus anderen Paketen verwendet werden.

- **ResourceType** &ndash; Dies ist der geschachtelte Ressourcentyp innerhalb der Ressourcen Klasse.

- **Ressourcen Name** &ndash; Dies ist der Dateiname der Ressource (*ohne* Dateityp Erweiterung) oder der Wert des `android:name`-Attributs für Ressourcen, die sich in einem XML-Element befinden.

Der Inhalt der Layoutdatei **Main. axml**lautet z. b. wie folgt:

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

Dieses Beispiel enthält eine [`ImageView`](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) , die eine drawable-Ressource mit dem Namen **Flag**erfordert. Das `src`-Attribut des `ImageView` ist auf `@drawable/flag`festgelegt. Wenn die Aktivität gestartet wird, sucht Android in der Verzeichnis **Ressource/drawable** für eine Datei mit dem Namen " **Flag. png** " (die Dateierweiterung könnte ein anderes Bildformat sein, z. b. " **Flag. jpg**"), und lädt diese Datei und zeigt Sie im `ImageView`an.
Wenn diese Anwendung ausgeführt wird, würde Sie in etwa wie in der folgenden Abbildung aussehen:

![Lokalisierte ImageView](android-resource-basics-images/03-localized-screenshot.png)
