---
title: Arbeiten mit der Android-Manifest
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 18817063900437baa625d8572f0ae28fec77be1e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769805"
---
# <a name="working-with-the-android-manifest"></a>Arbeiten mit der Android-Manifest


## <a name="overview"></a>Übersicht

**AndroidManifest.xml** ist eine leistungsstarke Datei in der Android-Plattform, die Beschreibung der Funktionen und Anforderungen der Anwendung, um Android ermöglicht. Arbeiten mit es ist jedoch nicht einfach. Xamarin.Android hilft diese Schwierigkeit zu minimieren, können Sie benutzerdefinierte Attribute in Ihren Klassen hinzufügen, die zum automatischen Generieren von Manifest für Sie verwendet werden soll. Unser Ziel ist es, dass 99 % unserer Benutzer sollten nie manuell ändern, müssen **AndroidManifest.xml**. 

**AndroidManifest.xml** wird generiert, als Teil des Buildprozesses und den XML-Code in gefunden **Properties/AndroidManifest.xml** zusammengeführt wird, mit XML, das aus benutzerdefinierten Attributen generiert wird. Das resultierende zusammengeführt **AndroidManifest.xml** befindet sich in der **Obj** Unterverzeichnis; z. B. dem er sich am **obj/Debug/android/AndroidManifest.xml** für Debug-Builds . Die Zusammenführung ist trivial: benutzerdefinierte Attribute innerhalb des Codes zum Generieren von XML-Elementen, verwendet und *fügt* diese Elemente in **AndroidManifest.xml**. 



## <a name="the-basics"></a>Die Grundlagen

Zum Zeitpunkt der Kompilierung werden die Assemblys für gescannt nicht-`abstract` abgeleitete Klassen [Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/) und haben die [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) Attribut deklariert werden. Es verwendet dann diese Klassen und Attribute, um das Manifest zu erstellen. Beachten Sie z. B. folgenden Code: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Dies führt zu ' Nothing ' generiert **AndroidManifest.xml**. Wenn Sie möchten eine `<activity/>` Element erzeugt werden kann, müssen Sie die [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) benutzerdefiniertes Attribut: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

In diesem Beispiel wird die folgende XML-Fragment hinzuzufügende **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

Die `[Activity]` Attribut hat keine Auswirkungen auf `abstract` Typen; `abstract` Typen werden ignoriert.



### <a name="activity-name"></a>Name der Aktivität

Beginnen mit dem Xamarin.Android 5.1, basiert der Typname der Aktivität auf die von der Assembly qualifizierte Name des Typs, der zu exportierenden MD5SUM. Dies ermöglicht den gleichen vollqualifizierten Namen aus zwei verschiedenen Assemblys bereitgestellt werden und nicht erhalte die Fehlermeldung verpacken. (Vor Xamarin.Android 5.1 wurde der Standardname der Typ der Aktivität aus der lowercased Namespace und Name der Klasse erstellt.) 

Wenn Sie möchten diesen Standardwert überschreiben und explizit Geben Sie den Namen der Aktivität, verwenden die [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) Eigenschaft: 

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

*Hinweis*: Verwenden Sie die `Name` Eigenschaft nur Gründen der Abwärtskompatibilität als solche umbenennen Typ Nachschlagen zur Laufzeit verlangsamen kann. Wenn Sie die legacy-Code, der erwartet, der Standardname der Typ der Aktivität verfügen dass auf der Grundlage des lowercased-Namespaces und Klassennamen finden Sie unter [Android Callable Wrapper Naming](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) Tipps zur Verwaltung von Kompatibilität. 


### <a name="activity-title-bar"></a>Aktivität-Titelleiste

Standardmäßig gibt Android der Anwendung eine Titelleiste bei der Ausführung. Der verwendete Wert für diesen [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). In den meisten Fällen wird dieser Wert von Ihrem Klassennamen unterscheiden. Verwenden Sie zum Angeben Ihrer app Bezeichnung in der Titelleiste auf die [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) Eigenschaft.
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


### <a name="launchable-from-application-chooser"></a>Ausführbare in Anwendung-Auswahl

Standardmäßig wird die Aktivität nicht im Startbildschirm für Android-Anwendung angezeigt. Dies liegt daran Sie wahrscheinlich viele Aktivitäten in der Anwendung werden, und ein Symbol für jeden einzelnen soll nicht. Um anzugeben, welcher Typ von Anwendungsstartprogramm ausführbare werden soll, verwenden Sie die [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) Eigenschaft. Zum Beispiel: 

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



### <a name="activity-icon"></a>Symbol "Codeaktivität"

Standardmäßig wird die Aktivität die Standard-Startsymbol, die vom System bereitgestellte zugewiesen. Um ein benutzerdefiniertes Symbol zu verwenden, fügen Sie zuerst Ihre **PNG** auf **Ressourcen und Ausgaben möglich**, legen Sie seine Buildaktion auf **AndroidResource**, verwenden Sie dann die [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) Eigenschaft an, die zu verwendende Symbol. Zum Beispiel: 

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

Wenn Sie Berechtigungen auf das Android-Manifest hinzufügen (wie in beschrieben [Berechtigungen hinzufügen, um Android-Manifest](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/)), diese Berechtigungen werden in aufgezeichnet **Properties/AndroidManifest.xml**. Wenn Sie festlegen, z. B. die `INTERNET` Berechtigung, das folgende Element wird hinzugefügt **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Debug-Builds automatisch einige Berechtigungen zum Erstellen einfacher Debuggen festgelegt (wie z. B. `INTERNET` und `READ_EXTERNAL_STORAGE`) &ndash; legen Sie diese Einstellungen sind nur in der generierten **obj/Debug/android/AndroidManifest.xml** und sind nicht erreichte im aktiviert die **erforderliche Berechtigungen** Einstellungen. 

Angenommen, Sie überprüfen, dass die generierte Manifestdatei bei **obj/Debug/android/AndroidManifest.xml**, wird möglicherweise die folgende Berechtigungselemente wurden hinzugefügt: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Erstellen Sie in der Version die Version des Manifests (am **obj/Debug/android/AndroidManifest.xml**), diese Berechtigungen sind *nicht* automatisch konfiguriert. Wenn Sie feststellen, dass der Wechsel zu einem Releasebuild bewirkt, dass Ihre app eine Berechtigung zu verlieren, die im Debugbuild verfügbar war, stellen Sie sicher, dass Sie diese Berechtigung explizit, in festgelegt haben der **erforderliche Berechtigungen** Einstellungen für Ihre app (siehe  **Erstellen Sie > Android-Anwendung** in Visual Studio für Mac; finden Sie unter **Eigenschaften > Android-Manifest** in Visual Studio). 




## <a name="advanced-features"></a>Erweiterte Funktionen


### <a name="intent-actions-and-features"></a>Beabsichtigte Aktionen und Funktionen

Die Android-Manifest bietet eine Möglichkeit für Sie beschreiben die Funktionen der Aktivität. Dies geschieht über [Intents](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) und [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) benutzerdefinierten Attributs. Sie können angeben, welche Aktionen für die Aktivität mit geeignet sind die [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) Konstruktor und welche Kategorien mit geeignet sind der [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) Eigenschaft. Mindestens eine Aktivität muss bereitgestellt werden (also Warum Aktivitäten im Konstruktor bereitgestellt werden). `[IntentFilter]` kann mehrere Male auf, und jede Verwendung resultiert in einer separaten `<intent-filter/>` Element innerhalb der `<activity/>`. Zum Beispiel:

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

Die Android-Manifest enthält auch eine Möglichkeit zum Deklarieren von Eigenschaften für die gesamte Anwendung. Dies erfolgt über die `<application>` Element und seine Entsprechung, die [Anwendung](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) benutzerdefinierten Attributs. Beachten Sie, dass es eine anwendungsweite (assemblyweit) Einstellungen statt aktivitätsbasierte Einstellungen handelt. In der Regel deklarieren Sie `<application>` Eigenschaften für die gesamte Anwendung und überschreiben Sie dann diese Einstellungen (wie erforderlich) auf Basis eines pro-Aktivität. 

Beispielsweise die folgenden `Application` Attribut hinzugefügt wird **AssemblyInfo.cs** , um anzugeben, dass die Anwendung gedebuggt werden kann, wird der Benutzer lesbaren Namen **meine App**, und dass er die verwendet`Theme.Light` Stil als Standarddesign für alle Aktivitäten: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Diese Deklaration führt dazu, dass das folgende XML-Fragment in generiert werden **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
In diesem Beispiel werden standardmäßig alle Aktivitäten in der app auf die `Theme.Light` Stil. Wenn Sie eine Aktivität Design auf `Theme.Dialog`, nur die Aktivität verwendet die `Theme.Dialog` formatieren, während alle anderen Aktivitäten in Ihrer app standardmäßig generiert die `Theme.Light` Format wie in der `<application>` Element. 

Die `Application` Element ist nicht die einzige Möglichkeit zum Konfigurieren von `<application>` Attribute. Alternativ können Sie fügen Sie Attribute direkt in die `<application>` Element des **Properties/AndroidManifest.xml**. Diese Einstellungen in die endgültige zusammengeführt `<application>` Element, das befindet **obj/Debug/android/AndroidManifest.xml**. Beachten Sie, dass der Inhalt des **Properties/AndroidManifest.xml** immer Vorrang vor Daten, die von benutzerdefinierten Attributen bereitgestellt. 

Es gibt viele anwendungsweite-Attribute, die Sie in konfigurieren können die `<application>` Element; Weitere Informationen zu diesen Einstellungen finden Sie unter der [öffentlichen Eigenschaften](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) Teil [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Liste benutzerdefinierter Attribute

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : generiert eine [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML-Fragment 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : generiert eine [/Manifest/Anwendung](http://developer.android.com/guide/topics/manifest/application-element.html) XML-Fragment 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : Generates a  [/manifest/instrumentation](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML fragment 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : Generates a  [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML fragment 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : Generates a  [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML fragment 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : Generates a  [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML fragment 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : Generates a  [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML fragment 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : Generates a  [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML fragment 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : generiert eine [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML-Fragment 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : Generates a  [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML fragment 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : Generates a  [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML fragment 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : Generates a  [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML fragment 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : Generates a  [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML fragment 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : Generates a  [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML fragment

