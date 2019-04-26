---
title: Arbeiten mit dem Android-Manifest
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 5e354f8271257ab21a855bdf5d576ce3062fadc7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957389"
---
# <a name="working-with-the-android-manifest"></a>Arbeiten mit dem Android-Manifest


## <a name="overview"></a>Übersicht

**"Androidmanifest.xml"** ist eine leistungsstarke-Datei in der Android-Plattform, die Sie beschreiben die Funktionen und Anforderungen Ihrer Anwendung für Android ermöglicht. Das Arbeiten damit ist jedoch nicht einfach. Xamarin.Android unterstützt diese schwierigkeiten minimieren, können Sie zum Hinzufügen benutzerdefinierter Attribute auf Ihre Klassen, die dann verwendet werden wird, um das Manifest für Sie automatisch zu generieren. Unser Ziel ist es, dass 99 % der Benutzer niemals manuell ändern, müssen **"androidmanifest.xml"**. 

**"Androidmanifest.xml"** wird generiert, als Teil des Buildprozesses, und der XML-Code innerhalb **Properties/Androidmanifest.XML** mit XML, das aus benutzerdefinierten Attributen generiert wird zusammengeführt. Die resultierende zusammengeführt **"androidmanifest.xml"** befindet sich in der **Obj** Unterverzeichnis; z. B., dass er sich befindet, auf **obj/Debug/android/AndroidManifest.xml** für Debugbuilds . Der Zusammenführung ist einfach: benutzerdefinierte Attribute in den Code zum Generieren von XML-Elemente, verwendet und *fügt* diese Elemente in **"androidmanifest.xml"**. 



## <a name="the-basics"></a>Die Grundlagen

Zum Zeitpunkt der Kompilierung werden die Assemblys für überprüft nicht`abstract` abgeleitete Klassen [Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/) und die [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) Attribut deklariert werden. Er verwendet dann diese Klassen und Attribute, um das Manifest zu erstellen. Beachten Sie z. B. folgenden Code: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Dies führt zu "nothing" generiert werden **"androidmanifest.xml"**. Wenn Sie möchten eine `<activity/>` Element erzeugt werden kann, müssen Sie die [`[Activity]`](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) 
benutzerdefiniertes Attribut: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

In diesem Beispiel gibt der folgende XML-Fragment hinzuzufügende **"androidmanifest.xml"**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

Die `[Activity]` Attribut hat keine Auswirkungen auf `abstract` -Typen ist. `abstract` Typen werden ignoriert.



### <a name="activity-name"></a>Name der Aktivität

Ab Xamarin.Android 5.1, basiert der Typname der Aktivität auf die MD5SUM, der die Assembly qualifizierten Namen des Typs exportiert wird. Dies ermöglicht den gleichen den vollqualifizierten Namen, die aus zwei verschiedenen Assemblys bereitgestellt werden und keinen Fehler für die paketerstellung. (Vor dem Xamarin.Android 5.1 führt der wurde der Standard-Typname der Aktivität aus der in Kleinbuchstaben Namespace und den Namen der Klasse erstellt.) 

Wenn Sie diesen Standardwert überschreiben und explizit Geben Sie den Namen der Aktivität verwenden möchten die [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) Eigenschaft: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Dieses Beispiel erzeugt die folgende XML-Fragment:

```xml
<activity android:name="awesome.demo.activity" />
```

*Beachten Sie*: Verwenden Sie die `Name` Eigenschaft nur Gründen der Abwärtskompatibilität als solche umbenennen kann, für das Nachschlagen zur Laufzeit verlangsamen. Wenn Sie legacy-Code verfügen, die erwartet, dass der Standardname der Typ der Aktivität auf den Namespace in Kleinbuchstaben basieren und den Klassennamen, finden Sie unter [Android Callable Wrapper Benennung](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) finden Sie Tipps zum Verwalten der Anwendungskompatibilität. 


### <a name="activity-title-bar"></a>Aktivität-Titelleiste

Standardmäßig bietet Android Ihrer Anwendung eine Titelleiste, wenn er ausgeführt wird. Der hierfür verwendete Wert lautet [ `/manifest/application/activity/@android:label` ](https://developer.android.com/guide/topics/manifest/activity-element.html#label). In den meisten Fällen wird dieser Wert von Ihr Klassenname unterscheiden. Verwenden Sie zum Angeben Ihrer app Bezeichnung in der Titelleiste auf die [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) Eigenschaft.
Zum Beispiel: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Dieses Beispiel erzeugt die folgende XML-Fragment:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Zu starten über das Application-Auswahl

Standardmäßig wird die Aktivität nicht in der Android-Anwendung-Startbildschirm angezeigt. Dies ist da Sie wahrscheinlich viele Aktivitäten in Ihrer Anwendung werden und Sie kein Symbol für jeden werden soll. Um anzugeben, welche zu starten über das Anwendungsstartprogramm werden soll, verwenden die [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) Eigenschaft. Zum Beispiel: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Dieses Beispiel erzeugt die folgende XML-Fragment:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>Symbol "Aktivität"

Standardmäßig wird die Aktivität der vom System bereitgestellten Standard-startprogrammsymbol zugewiesen. Um ein benutzerdefiniertes Symbol verwenden zu können, fügen Sie zuerst Ihre **PNG** zu **Ressourcen/drawable**, legen Sie dessen "Buildvorgang" auf **AndroidResource**, verwenden Sie dann die [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) Eigenschaft, die zu verwendende Symbol an. Zum Beispiel: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Dieses Beispiel erzeugt die folgende XML-Fragment:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>Berechtigungen

Wenn Sie Berechtigungen hinzufügen, um das Android-Manifest (siehe [Berechtigungen hinzufügen, um Android-Manifest](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), werden diese Berechtigungen in aufgezeichnet **Properties/Androidmanifest.XML**. Wenn Sie festlegen, z. B. die `INTERNET` -Berechtigung, das folgende Element wird hinzugefügt, um **Properties/Androidmanifest.XML**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Debugbuilds automatisch manche Berechtigungen festlegen, um sicherzustellen, einfacher zu debuggen (z. B. `INTERNET` und `READ_EXTERNAL_STORAGE`) &ndash; legen Sie diese Einstellungen sind nur in der generierten **obj/Debug/android/AndroidManifest.xml** und sind nicht als aktiviert dargestellt die **erforderliche Berechtigungen** Einstellungen. 

Angenommen, Sie überprüfen, dass die generierte Manifestdatei bei **obj/Debug/android/AndroidManifest.xml**, Sie feststellen, dass die folgende Berechtigungselemente wurden hinzugefügt: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Erstellen Sie in der Version die Version des Manifests (am **obj/Debug/android/AndroidManifest.xml**), diese Berechtigungen sind *nicht* automatisch konfiguriert. Wenn Sie feststellen, dass der Wechsel zu einem Releasebuild bewirkt, dass Ihre app eine Berechtigung verliert, die im Debugbuild verfügbar war, stellen Sie sicher, dass Sie diese Berechtigung explizit, in festgelegt haben der **erforderliche Berechtigungen** Einstellungen für Ihre app (siehe  **Erstellen > Android-Anwendung** in Visual Studio für Mac finden Sie unter **Eigenschaften > Android-Manifest** in Visual Studio). 




## <a name="advanced-features"></a>Erweiterte Funktionen


### <a name="intent-actions-and-features"></a>Beabsichtigte Aktionen und Funktionen

Android-Manifest enthält eine Möglichkeit zum Beschreiben der Funktionen von der Aktivität. Dies erfolgt über [Intents](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) und [`[IntentFilter]`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 
das benutzerdefinierte Attribut. Sie können angeben, welche Aktionen für die Aktivität mit entsprechenden sind die [`IntentFilter`](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) 
Konstruktor, und welchen Kategorien eignen, mit der [`Categories`](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) 
-Eigenschaft veranschaulicht. Mindestens eine Aktivität muss bereitgestellt werden (Was ist, warum die Aktivitäten im Konstruktor bereitgestellt werden). `[IntentFilter]` kann sein, sofern mehrere Male auf, und jede Verwendung zu einer separaten führt `<intent-filter/>` Element innerhalb der `<activity/>`. Zum Beispiel:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Dieses Beispiel erzeugt die folgende XML-Fragment:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Application-Element

Android-Manifest enthält auch eine Möglichkeit zum Deklarieren von Eigenschaften für die gesamte Anwendung. Dies erfolgt mithilfe der `<application>` -Element und sein Gegenstück, die [Anwendung](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) benutzerdefinierten Attributs. Beachten Sie, dass diese Einstellungen für den gesamten Anwendung (assemblyweit) statt pro Aktivität Einstellungen sind. Deklarieren Sie normalerweise `<application>` Eigenschaften für die gesamte Anwendung, und klicken Sie dann diese Einstellungen überschreiben (bei Bedarf) auf einer Basis pro Aktivität. 

Beispielsweise die folgenden `Application` -Attribut hinzugefügt, **"AssemblyInfo.cs"** , um anzugeben, dass die Anwendung debuggt werden kann, dass der Benutzer lesbaren Namen **meine App**, und dass er die verwendet`Theme.Light` Stil als das Standarddesign für alle Aktivitäten: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Diese Deklaration führt dazu, dass das folgende XML-Fragment, das im generiert werden **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
In diesem Beispiel werden standardmäßig alle Aktivitäten in der app auf die `Theme.Light` Stil. Wenn Sie auf einer Aktivität Design `Theme.Dialog`, nur das Aktivität verwendet die `Theme.Dialog` formatieren, während alle anderen Aktivitäten in Ihrer app auf festgelegt ist die `Theme.Light` Stil festgelegt die `<application>` Element. 

Die `Application` Element ist nicht die einzige Möglichkeit zum Konfigurieren von `<application>` Attribute. Sie können alternativ einfügen, Attribute, die direkt in die `<application>` Element **Properties/Androidmanifest.XML**. Diese Einstellungen werden in der letzten zusammengeführt `<application>` -Element, das in befindet **obj/Debug/android/AndroidManifest.xml**. Beachten Sie, dass der Inhalt des **Properties/Androidmanifest.XML** immer außer Kraft setzen von benutzerdefinierten Attributen bereitgestellt werden. 

Es gibt viele anwendungsweite-Attribute, die Sie, in konfigurieren können der `<application>` -Element; Weitere Informationen zu diesen Einstellungen finden Sie unter der [öffentliche Eigenschaften](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) Abschnitt [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Liste benutzerdefinierter Attribute

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : Generiert eine [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML-Fragment 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : Generiert eine [/Manifest/Anwendung](https://developer.android.com/guide/topics/manifest/application-element.html) XML-Fragment 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : Generiert eine [/Manifest bzw. Instrumentierung](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML-Fragment 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : Generiert eine [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML-Fragment 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : Generiert eine [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML-Fragment 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : Generiert eine [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML-Fragment 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : Generiert eine [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML-Fragment 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : Generiert eine [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML-Fragment 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : Generiert eine [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML-Fragment 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : Generiert eine [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML-Fragment 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : Generiert eine [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML-Fragment 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : Generiert eine [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML-Fragment 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : Generiert eine [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML-Fragment 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : Generiert eine [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML-Fragment

