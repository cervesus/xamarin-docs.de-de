---
title: Arbeiten mit dem Android-Manifest
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 1438c012608b367c21ebcc401c058b186b917f53
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027805"
---
# <a name="working-with-the-android-manifest"></a>Arbeiten mit dem Android-Manifest

**AndroidManifest.xml** ist eine leistungsfähige Datei auf der Android-Plattform, die es Ihnen ermöglicht, die Funktionalität und die Anforderungen Ihrer App für Android zu beschreiben. Es ist jedoch nicht einfach, mit einer solchen Datei zu arbeiten. Xamarin.Android hilft, diese Schwierigkeit zu minimieren, indem es Ihnen ermöglicht, Ihren Klassen benutzerdefinierte Attribute hinzuzufügen, die dann dazu verwendet werden, das Manifest automatisch für Sie zu generieren. Das Ziel ist, dass es für 99 % der Benutzer niemals notwendig sein soll, **AndroidManifest.xml** manuell zu ändern. 

**AndroidManifest.xml** wird während des Buildprozesses generiert, und der in **Properties/AndroidManifest.xml** enthaltene XML-Code wird mit dem XML-Code zusammengeführt, der aus benutzerdefinierten Attributen generiert wurde. Die resultierende zusammengeführte **AndroidManifest.xml**-Datei befindet sich im Unterverzeichnis **obj**. Sie befindet sich z. B. in **obj/Debug/android/AndroidManifest.xml** für Debugbuilds. Der Zusammenführungsprozess ist trivial: In ihm werden benutzerdefinierte Attribute im Code verwendet, um XML-Elemente zu generieren, und diese Elemente werden in **AndroidManifest.xml** *eingefügt*. 

## <a name="the-basics"></a>Die Grundlagen

Zur Kompilierzeit werden Assemblys auf Nicht-`abstract`-Klassen überprüft, die aus [Activity](xref:Android.App.Activity) abgeleitet sind und für die das [`[Activity]`](xref:Android.App.ActivityAttribute)-Attribut deklariert ist. Anschließend werden diese Klassen und Attribute verwendet, um das Manifest zu erstellen. Beachten Sie z. B. folgenden Code: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Das Ergebnis ist, dass nichts in **AndroidManifest.xml** generiert wird. Wenn Sie ein `<activity/>`-Element generieren möchten, müssen Sie das benutzerdefinierte Attribut [`[Activity]`](xref:Android.App.Activity) 
verwenden: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

In diesem Beispiel wird das folgende XML-Fragment zu **AndroidManifest.xml** hinzugefügt:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

Das `[Activity]`-Attribut wirkt sich nicht auf `abstract`-Typen aus. `abstract`-Typen werden ignoriert.

### <a name="activity-name"></a>Name der Aktivität

Ab Xamarin.Android 5.1 basiert der Typname einer Aktivität auf dem MD5SUM-Wert des durch die Assembly qualifizierten Namens des exportierten Typs. Dadurch kann derselbe vollqualifizierte Name von zwei verschiedenen Assemblys bereitgestellt werden, ohne dass ein Paketfehler auftritt. (Vor Xamarin.Android 5.1 wird der Standardtypname der Aktivität aus dem kleingeschriebenen Namespace- und dem Klassennamen erstellt.) 

Wenn Sie diese Standardeinstellung außer Kraft setzen und den Namen der Aktivität explizit angeben möchten, verwenden Sie die [`Name`](xref:Android.App.ActivityAttribute.Name)-Eigenschaft: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

In diesem Beispiel wird das folgende XML-Fragment erstellt:

```xml
<activity android:name="awesome.demo.activity" />
```

> [!NOTE]
> Sie sollten die `Name`-Eigenschaft nur aus Gründen der Abwärtskompatibilität verwenden, da eine solche Umbenennung die Typsuche zur Laufzeit verlangsamen kann. Wenn Sie Legacycode haben, für den erwartet wird, dass der Standardtypname auf dem kleingeschriebenen Namespace- und dem Klassennamen basiert, und Tipps zur Beibehaltung der Kompatibilität wünschen, lesen Sie [Android Callable Wrapper Naming](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming). 

### <a name="activity-title-bar"></a>Titelleiste für eine Aktivität

Android zeigt für Ihrer App, wenn diese ausgeführt wird, standardmäßig eine Titelleiste an. Dafür wird der folgende Werte verwendet: [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label). In den meisten Fällen wird sich dieser Wert vom Namen Ihrer Klasse unterscheiden. Um die Bezeichnung Ihrer App auf der Titelleiste anzugeben, verwenden Sie die [`Label`](xref:Android.App.ActivityAttribute.Label)-Eigenschaft.
Zum Beispiel: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

In diesem Beispiel wird das folgende XML-Fragment erstellt:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

### <a name="launchable-from-application-chooser"></a>Startbar über die App-Auswahl

Standardmäßig wird Ihre Aktivität nicht auf dem App-Startbildschirm von Android angezeigt. Dies liegt daran, dass in Ihrer Anwendung wahrscheinlich viele Aktivitäten vorhanden sind und Sie kein Symbol für jede dieser Aktivitäten wünschen. Um anzugeben, welche Aktivität aus dem App-Startprogramm startbar sein soll, verwenden Sie die [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher)-Eigenschaft. Zum Beispiel: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

In diesem Beispiel wird das folgende XML-Fragment erstellt:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### <a name="activity-icon"></a>Symbol für eine Aktivität

Standardmäßig wird Ihrer Aktivität das standardmäßige Startprogrammsymbol zugewiesen, das vom System bereitgestellt wird. Um ein benutzerdefiniertes Symbol zu verwenden, fügen Sie zunächst Ihre **.png**-Datei zu **Resources/drawable** hinzu, legen dessen „Build Action“ auf **AndroidResource** fest, und verwenden dann die [`Icon`](xref:Android.App.ActivityAttribute.Icon)-Eigenschaft, um das zu verwendende Symbol anzugeben. Zum Beispiel: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

In diesem Beispiel wird das folgende XML-Fragment erstellt:

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

Wenn Sie dem Android-Manifest Berechtigungen hinzufügen (wie unter [Add Permissions to Android Manifest](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)beschrieben), werden diese Berechtigungen in **Properties/AndroidManifest.xml** festgehalten. Wenn Sie z. B. die `INTERNET`-Berechtigung festlegen, wird das folgende Element zu **Properties/AndroidManifest.xml** hinzugefügt: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Für Debugbuilds werden automatisch einige Berechtigungen festgelegt, um das Debuggen zu vereinfachen (z. B. `INTERNET` und `READ_EXTERNAL_STORAGE`) &ndash; diese Einstellungen sind nur in der generierten **obj/Debug/android/AndroidManifest.xml**-Datei festgelegt und werden in den **Erforderliche Berechtigungen**-Einstellungen nicht als aktiviert angezeigt. 

Wenn Sie sich z. B. die generierte Manifestdatei unter **obj/Debug/android/AndroidManifest.xml** ansehen, sind möglicherweise die folgenden hinzugefügten Berechtigungselemente zu sehen: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

In der Releasebuildversion des Manifests (unter **obj/Debug/android/AndroidManifest.xml**) werden diese Berechtigungen *nicht* automatisch konfiguriert. Wenn Sie feststellen, dass der Wechsel zu einem Releasebuild bewirkt, dass Ihre App eine Berechtigung verliert, die im Debugbuild verfügbar war, vergewissern Sie sich, dass Sie diese Berechtigung explizit in den **Erforderliche Berechtigungen**-Einstellungen für Ihre App festgelegt haben (siehe **Build > Android-Anwendung** in Visual Studio für Mac bzw. **Eigenschaften > Android-Manifest** in Visual Studio). 

## <a name="advanced-features"></a>Erweiterte Funktionen

### <a name="intent-actions-and-features"></a>Intent-Aktionen und -Funktionen

Das Android-Manifest bietet Ihnen einen Weg, die Möglichkeiten Ihrer Aktivität zu beschreiben. Dies erfolgt über [Intents](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) und das
benutzerdefinierte Attribut . Die für Ihre Aktivität geeigneten Aktionen können Sie mit dem [`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)-Konstruktor
und die geeigneten Kategorien können Sie mit der [`Categories`](xref:Android.App.IntentFilterAttribute.Categories)-Eigenschaft
-Eigenschaft veranschaulicht. Mindestens eine Aktivität muss bereitgestellt werden (aus diesem Grund werden Aktivitäten im Konstruktor bereitgestellt). `[IntentFilter]` kann mehrmals bereitgestellt werden, und jede Verwendung führt zu einem separaten `<intent-filter/>`-Element innerhalb der `<activity/>`. Zum Beispiel:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

In diesem Beispiel wird das folgende XML-Fragment erstellt:

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

Das Android-Manifest bietet Ihnen auch einen Weg, Eigenschaften für Ihre gesamte Anwendung (App) zu deklarieren. Dies erfolgt über das `<application>`-Element und dessen Gegenstück, das benutzerdefinierte Attribut [Application](xref:Android.App.ApplicationAttribute). Beachten Sie, dass es sich dabei um anwendungsweite (assemblyweite) Einstellungen und nicht um Einstellungen pro Aktivität handelt. Üblicherweise deklarieren Sie `<application>`-Eigenschaften für Ihre gesamte Anwendung und überschreiben diese Einstellungen dann (bei Bedarf) auf Aktivitätsbasis. 

Beispielsweise wird das folgende `Application`-Attribut zu **AssemblyInfo.cs** hinzugefügt, um anzugeben, dass die Anwendung debuggt werden kann, dass für sie der Name **My App** angezeigt wird und dass der `Theme.Light`-Stil als Standarddesign für alle Aktivitäten verwendet wird: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Diese Deklaration bewirkt, dass das folgende XML-Fragment in **obj/Debug/android/AndroidManifest.xml** generiert wird:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

In diesem Beispiel werden alle Aktivitäten in der App standardmäßig auf den `Theme.Light`-Stil eingestellt. Wenn Sie das Design einer Aktivität auf `Theme.Dialog` festgelegt haben, wird der `Theme.Dialog`-Stil nur für diese Aktivität verwendet, während für alle anderen Aktivitäten in der App standardmäßig der `Theme.Light`-Stil verwendet wird, der im `<application>`-Element festgelegt ist. 

Das `Application`-Element ist nicht die einzige Möglichkeit, `<application>`-Attribute zu konfigurieren. Alternativ können Sie Attribute direkt in das `<application>`-Element von **Properties/AndroidManifest.xml** einfügen. Diese Einstellungen werden mit dem letzten `<application>`-Element zusammengeführt, das sich in **obj/Debug/android/AndroidManifest.xml** befindet. Beachten Sie, dass Daten, die von benutzerdefinierten Attributen bereitgestellt werden, immer mit dem Inhalt von **Properties/AndroidManifest.xml** überschrieben werden. 

Es gibt viele anwendungsweite Attribute, die Sie im `<application>`-Element konfigurieren können. Weitere Informationen zu diesen Einstellungen finden Sie im Abschnitt [Public Properties](xref:Android.App.ApplicationAttribute) von [ApplicationAttribute](xref:Android.App.ApplicationAttribute). 

## <a name="list-of-custom-attributes"></a>Liste der benutzerdefinierten Attribute

- [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute): Generiert ein [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html)-XML-Fragment 
- [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute): Generiert ein [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html)-XML-Fragment 
- [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute): Generiert ein [/manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html)-XML-Fragment 
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute): Generiert ein [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html)-XML-Fragment 
- [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute): Generiert ein [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html)-XML-Fragment 
- [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute): Generiert ein [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html)-XML-Fragment 
- [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute): Generiert ein [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html)-XML-Fragment 
- [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute): Generiert ein [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html)-XML-Fragment 
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute): Generiert ein [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html)-XML-Fragment 
- [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute): Generiert ein [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html)-XML-Fragment 
- [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute): Generiert ein [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html)-XML-Fragment 
- [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute): Generiert ein [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html)-XML-Fragment 
- [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute): Generiert ein [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html)-XML-Fragment 
- [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute): Generiert ein [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html)-XML-Fragment
