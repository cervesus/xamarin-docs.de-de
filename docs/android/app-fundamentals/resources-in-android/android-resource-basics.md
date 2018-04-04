---
title: Grundlagen der Android-Ressource
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: f6be1001e5d3455a94e677f1bb5dc52ca574b873
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="android-resource-basics"></a>Grundlagen der Android-Ressource

Fast alle Android-Anwendungen werden irgendeine Ressourcen enthalten; zumindest müssen häufig Benutzer Schnittstelle Layouts in Form von XML-Dateien. Wenn eine Anwendung Xamarin.Android zuerst erstellt wird, sind Standardressourcen Setup über die Projektvorlage Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ressourcendateien](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Ressourcendateien](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Die fünf Dateien, aus denen die Standardressourcen besteht, die im Ordner "Ressourcen" erstellt wurden:

-  **Datei "Icon.png"** &ndash; das Standardsymbol für die Anwendung

-  **Main.axml** &ndash; -Schnittstelle der Standardeinstellung Benutzerdatei für Layout für eine Anwendung. Beachten Sie, dass Android verwendet die **XML** Dateierweiterung Xamarin.Android verwendet die **.axml** Dateierweiterung.

-  **Strings.XML** &ndash; eine Zeichenfolgentabelle, Lokalisierung der Anwendung verwenden können

-  **AboutResources.txt** &ndash; Dies ist nicht erforderlich und kann bedenkenlos gelöscht werden. Es Übersicht nur eine auf hoher Ebene Ressourcenordner und die darin enthaltenen Dateien.

-  **Resource.Designer.cs** &ndash; diese Datei wird automatisch generiert und Beibehalten von Xamarin.Android und enthält die eindeutige IDs jeder Ressource zugewiesen. Dies ist sehr ähnlich und ihres Verwendungszwecks identisch, mit der R.java-Datei, die eine Android-Anwendung in Java geschrieben würden. Es wird automatisch erstellt, indem die Xamarin.Android Tools und wird von Zeit zu Zeit neu generiert werden.


## <a name="creating-and-accessing-resources"></a>Erstellen und den Zugriff auf Ressourcen

Erstellen von Ressourcen ist so einfach wie das Hinzufügen von Dateien in das Verzeichnis für den betreffenden Ressourcentyp. Der Screenshot zeigt, dass die Zeichenfolgenressourcen für deutsche Gebietsschemas zu einem Projekt hinzugefügt wurden. Wenn **Strings.xml** wurde hinzugefügt, um die Datei der **Buildvorgang** wurde auf automatisch festgelegt **AndroidResource** von Xamarin.Android-Tools:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Buildvorgang für Strings.xml AndroidResource festgelegt](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Buildvorgang für Strings.xml AndroidResource festgelegt](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Dadurch wird die Xamarin.Android Tools ordnungsgemäß kompiliert und die Ressourcen in der APK-Datei einzubetten. If aus irgendeinem Grund die **Buildvorgang** nicht festgelegt ist, um **Android Ressource**, dann werden die Dateien aus der APK ausgeschlossen, und jeder Versuch zum Laden oder die Ressourcen zugreifen, führt zu einem Laufzeitfehler und Anwendung abstürzt.

Darüber hinaus ist es wichtig zu beachten, dass Android Kleinbuchstabe Dateinamen nur für Ressourcenelemente unterstützt, Xamarin.Android ist ein wenig mehr Datentypkonflikte; Es wird die Groß- und Kleinbuchstabe Dateinamen unterstützen. Die Konvention für Bildnamen Kleinbuchstabe Unterstriche als Trennzeichen verwendet wird (z. B. **meine\_Image\_name.png**). Beachten Sie, dass Ressourcennamen nicht verarbeitet werden können, wenn Striche oder Leerzeichen als Trennzeichen verwendet werden.

Nach der Ressourcen zu einem Projekt hinzugefügt wurden, stehen zwei Möglichkeiten, Ihre Verwendung in einer Anwendung &ndash; programmgesteuert (im Code) oder aus XML-Dateien.


## <a name="referencing-resources-programmatically"></a>Programmgesteuertes Verweisen auf Ressourcen

Um programmgesteuert auf diese Dateien zugreifen zu können, werden sie eine eindeutige Ressourcen-ID zugewiesen. Diese Ressourcen-ID ist eine ganze Zahl, die in einer speziellen Klasse namens definiert `Resource`, befindet sich in der Datei **Resource.designer.cs**, und sieht ungefähr so aus:

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

Jeder Ressourcen-ID ist innerhalb einer geschachtelten Klasse enthalten, die den Ressourcentyp entspricht. Beispielsweise, wenn die Datei **Datei "Icon.png"** wurde dem Projekt hinzugefügt, aktualisiert der Xamarin.Android der `Resource` -Klasse, erstellen eine geschachtelte Klasse aufgerufen `Drawable` eine Konstante innerhalb mit dem Namen `Icon`.
Dadurch wird die Datei **Datei "Icon.png"** im Code als bezeichnet werden `Resource.Drawable.Icon`. Die `Resource` Klasse sollte nicht manuell bearbeitet werden, wie die ihm vorgenommenen Änderungen werden durch Xamarin.Android überschrieben werden.

Wenn Ressourcen programmgesteuert (im Code) verwiesen wird, kann darauf über die Klassenhierarchie Ressourcen zugegriffen werden, die folgende Syntax verwendet:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash; das Paket, das die Ressource wird bereitgestellt und ist nur erforderlich, wenn Ressourcen von anderen Paketen verwendet werden.

-  **ResourceType** &ndash; Dies ist der geschachtelten Ressource-Typ, der innerhalb der oben beschriebenen Ressourcenklasse.

-  **Ressourcenname** &ndash; Dies ist der Dateiname der Ressource (ohne Erweiterung) oder den Wert des Attributs "Android: Name" für Ressourcen, die in einem XML-Element befinden.


## <a name="referencing-resources-from-xml"></a>Verweisen auf Ressourcen aus XML

Ressourcen in eine XML-Datei werden aufgerufen, indem eine nachfolgende eine spezielle Syntax:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash; das Paket, das die Ressource wird bereitgestellt und ist nur erforderlich, wenn Ressourcen von anderen Paketen verwendet werden.

-  **ResourceType** &ndash; Dies ist der geschachtelten Ressource-Typ, der in die Ressourcenklasse.

-  **Ressourcenname** &ndash; Dies ist der Dateiname der Ressource (*ohne* die Typ-Dateierweiterung) oder der Wert von der `android:name` Attribut für Ressourcen, die in einem XML-Element sind.

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

Dieses Beispiel umfasst ein [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview) , erfordert eine zeichenbare Ressource mit dem Namen **Flag**. Die `ImageView` hat seine `src` -Attributsatz zur **@drawable/flag**. Wenn die Aktivität gestartet wird, sieht in das Verzeichnis Android **Ressource/Drawable** nach einer Datei namens **flag.png** (die Dateierweiterung ist möglicherweise ein anderes Bildformat, z. B. **flag.jpg**) Laden Sie die Datei, und zeigen Sie es in der `ImageView`.
Wenn diese Anwendung ausgeführt wird, würde es etwa der folgenden Abbildung aussehen:

![Lokalisierte ImageView](android-resource-basics-images/03-localized-screenshot.png)

