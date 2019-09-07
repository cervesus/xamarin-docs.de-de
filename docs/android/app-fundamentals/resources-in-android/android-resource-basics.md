---
title: Grundlagen zu Android-Ressourcen
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: c248949024d0e13a24863368e88aa559fa496806
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755244"
---
# <a name="android-resource-basics"></a>Grundlagen zu Android-Ressourcen

Fast alle Android-Anwendungen enthalten eine Art von Ressource, zumindest besitzen sie oft die Layouts der Benutzeroberfläche in Form von XML-Dateien. Beim Erstellen einer Xamarin.Android-Anwendung werden Standardressourcen durch die Xamarin.Android-Projektvorlage eingerichtet:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ressourcendateien](android-resource-basics-images/01-resource-files-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ressourcendateien](android-resource-basics-images/01-resource-files-xs.png)

-----

Die fünf Dateien, die die Standardressourcen ausmachen, wurden im Ordner "Resources" (Ressourcen) erstellt:

- **Icon. png** &ndash; das Standard Symbol für die Anwendung

- **Main. axml** &ndash; die standardmäßige Benutzeroberflächen-Layoutdatei für eine Anwendung. Beachten Sie, dass xamarin. Android die Dateierweiterung **. axml** verwendet, während Android die Dateierweiterung **. XML** verwendet.

- **Strings. XML** &ndash; eine Zeichen folgen Tabelle zur Unterstützung der Lokalisierung der Anwendung

- **Abvon sources. txt** &ndash; Dies ist nicht erforderlich und kann problemlos gelöscht werden. Er bietet nur eine allgemeine Übersicht über den Ressourcen Ordner und die darin bereitgestellten Dateien.

- Resource.Designer.cs&ndash; diese Datei wird automatisch von xamarin. Android generiert und verwaltet und enthält die einzelnen Ressourcen zugewiesenen eindeutigen IDs. Dies ist sehr ähnlich und mit der Datei "R. Java" identisch, die eine in Java geschriebene Android-Anwendung hätte. Sie wird automatisch von xamarin. Android-Tools erstellt und wird von Zeit zu Zeit neu generiert.

## <a name="creating-and-accessing-resources"></a>Erstellen von Ressourcen und Zugreifen auf Ressourcen

Das Erstellen von Ressourcen ist so einfach wie das Hinzufügen von Dateien zum Verzeichnis für den fraglichen Ressourcentyp. Der nachfolgende Screenshot zeigt, dass einem Projekt Zeichen folgen Ressourcen für deutsche Gebiets Schemas hinzugefügt wurden. Beim Hinzufügen von " **Strings. XML** " zur Datei wurde die Buildaktion von den xamarin. Android-Tools automatisch auf " **androidresource** " festgelegt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Buildaktion für "Strings. xml" auf "androidresource" festgelegt](android-resource-basics-images/02-build-action-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Buildaktion für "Strings. xml" auf "androidresource" festgelegt](android-resource-basics-images/02-build-action-xs.png)

-----

So können die Ressourcen von den Xamarin.Android-Tools ordnungsgemäß kompiliert und in die APK-Datei eingebettet werden. Falls aus irgendeinem Grund der **Buildvorgang** nicht auf **Android-Ressourcen** festgelegt ist, dann werden die Dateien aus dem APK ausgeschlossen, und jeder Versuch, die Ressource zu laden oder auf diese zuzugreifen, führt zu einem Laufzeitfehler oder zum Absturz der Anwendung.

Auch wenn Android für Ressourcenelemente nur Kleinbuchstaben für Dateinamen unterstützt, ist Xamarin.Android etwas nachsichtiger. Xamarin.Android unterstützt Dateinamen, die jeweils in Groß- und Kleinbuchstaben geschrieben sind. Für Bildnamen gilt die Verwendung von Kleinbuchstaben mit Unterstrichen als Trennzeichen (z. B. **my\_image\_name.png**). Beachten Sie, dass Ressourcennamen nicht verarbeitet werden können, wenn Bindestriche oder Leerzeichen als Trennzeichen verwendet werden.

Sobald Ressourcen zu einem Projekt hinzugefügt wurden, gibt es zwei Möglichkeiten, diese in einer Anwendung zu verwenden &ndash; programmgesteuert (im Code) oder aus XML-Dateien.

## <a name="referencing-resources-programmatically"></a>Programm gesteuertes verweisen auf Ressourcen

Für den programmgesteuerten Zugriff auf diese Dateien wird Ihnen eine eindeutige Ressourcen-ID zugewiesen. Diese Ressourcen-ID ist eine Ganzzahl, die in einer `Resource`speziellen Klasse namens definiert ist, die in der Datei **Resource.Designer.cs**gefunden wird und in etwa wie folgt aussieht:

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

Jede Ressourcen-ID ist in einer geschachtelten Klasse enthalten, die dem Ressourcentyp entspricht. Wenn z. b. die Datei " **Icon. png** " dem Projekt hinzugefügt wurde, hat xamarin `Resource` . Android die-Klasse aktualisiert und eine `Drawable` geschachtelte Klasse mit `Icon`dem Namen mit einer Konstanten innerhalb von mit dem Namen erstellt.
Dadurch kann auf die Datei " **Icon. png** " im Code als `Resource.Drawable.Icon`verwiesen werden. Die `Resource` Klasse sollte nicht manuell bearbeitet werden, da alle daran vorgenommenen Änderungen von xamarin. Android überschrieben werden.

Wenn Sie Programm gesteuert auf Ressourcen verweisen (im Code), können Sie über die Ressourcen Klassenhierarchie auf Sie zugreifen, in der die folgende Syntax verwendet wird:

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

- **PackageName** &ndash; Das Paket, das die Ressource bereitstellt und nur erforderlich ist, wenn Ressourcen aus anderen Paketen verwendet werden.

- **ResourceType** &ndash; Dies ist der geschachtelte Ressourcentyp, der sich innerhalb der oben beschriebenen Ressourcen Klasse befindet.

- **Ressourcen Name** &ndash; Dies ist der Dateiname der Ressource (ohne Erweiterung) oder der Wert des Attributs Android: Name für Ressourcen, die sich in einem XML-Element befinden.

## <a name="referencing-resources-from-xml"></a>Verweisen auf Ressourcen aus XML

Auf Ressourcen in einer XML-Datei wird mit einer folgenden speziellen Syntax zugegriffen:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

- **PackageName** &ndash; das Paket, das die Ressource bereitstellt und nur erforderlich ist, wenn Ressourcen aus anderen Paketen verwendet werden.

- **ResourceType** &ndash; Dies ist der geschachtelte Ressourcentyp innerhalb der Ressourcen Klasse.

- **Ressourcen Name** Dies ist der Dateiname der Ressource (*ohne* Dateityp Erweiterung) oder `android:name` der Wert des-Attributs für Ressourcen, die sich in einem XML-Element befinden. &ndash;

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

Dieses Beispiel enthält [`ImageView`](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) ein-Element, das eine drawable-Ressource mit dem Namen **Flag**erfordert. Das `ImageView` - `src` Attribut ist auf `@drawable/flag`festgelegt. Wenn die Aktivität gestartet wird, sucht Android in der Verzeichnis **Ressource/drawable** für eine Datei mit dem Namen " **Flag. png** " (die Dateierweiterung könnte ein anderes Bildformat sein, z. b `ImageView`. " **Flag. jpg**"), und lädt diese Datei und zeigt Sie im an.
Wenn diese Anwendung ausgeführt wird, würde Sie in etwa wie in der folgenden Abbildung aussehen:

![Lokalisierte ImageView](android-resource-basics-images/03-localized-screenshot.png)
