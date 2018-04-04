---
title: Permissions In Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: b8a8005c69c8aaee5d92bdabb3429bd52fc76b4a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="permissions-in-xamarinandroid"></a>Permissions In Xamarin.Android


## <a name="overview"></a>Übersicht

Ausführen von Android-Anwendungen in ihren eigenen Sandbox und aus Sicherheitsgründen Gründe haben keinen Zugriff auf bestimmte Systemressourcen verfügbar sind oder die Hardwarekonfiguration auf dem Gerät. Der Benutzer muss die Berechtigung für die app explizit gewähren, bevor sie diese Ressourcen verwenden kann. Eine Anwendung kann nicht z. B. GPS auf einem Gerät ohne explizite Berechtigung des Benutzers zugreifen. Android löst eine `Java.Lang.SecurityException` , wenn eine Anwendung versucht, eine geschützte Ressource ohne Berechtigung Zugriff auf.

Berechtigungen werden deklariert, der **AndroidManifest.xml** vom Anwendungsentwickler, wenn die Anwendung entwickelt wird. Android bietet zwei unterschiedliche Workflows zum Abrufen der Benutzer seine Zustimmung für diese Berechtigungen:
 
* Für apps, die das Ziel von Android 5.1 (API-Ebene 22) oder niedriger aufweist, ist die Erlaubnis aufgetreten, wenn die app installiert wurde. Wenn der Benutzer nicht die Berechtigungen erteilt hat, würde die app nicht installiert werden. Sobald die app installiert ist, wird gibt es keine Möglichkeit, Berechtigungen außer widerrufen, indem Sie die app deinstalliert.
* Android 6.0 (API-Ebene 23) ab, erhielten Benutzer mehr Kontrolle über Berechtigungen; Sie können gewähren oder widerrufen von Berechtigungen, solange die app auf dem Gerät installiert ist. Diese bildschirmabbildung zeigt die berechtigungseinstellungen für die Google-Kontakte-app. Listet die verschiedenen Berechtigungen, und ermöglicht es dem Benutzer aktivieren oder Deaktivieren von Berechtigungen:

![Beispiel-Berechtigungen-Bildschirm](permissions-images/01-permissions-check.png) 

Android-apps müssen überprüfen, zur Laufzeit, um festzustellen, ob sie die Berechtigung, auf eine geschützte Ressource zuzugreifen. Wenn die app nicht berechtigt ist, muss dann Anforderungen mit den neuen APIs von Android SDK für den Benutzer zum Erteilen der Berechtigungen erleichtern. Berechtigungen sind in zwei Kategorien unterteilt:

* **Normal Berechtigungen** &ndash; Dies sind die Berechtigungen, die wenig für des Benutzers Sicherheits- oder Sicherheitsrisiko. Android 6.0 wird zum Zeitpunkt der Installation automatisch zu normale Berechtigungen erteilen. Weitere Informationen finden Sie in der Android-Dokumentation für einen [vollständige Liste der normalen Berechtigungen](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Problematische Berechtigungen** &ndash; im Gegensatz zu normalen Berechtigungen problematische Berechtigungen sind für die Sicherheit oder den Datenschutz des Benutzers zu schützen. Diese müssen explizit vom Benutzer erteilt sein. Eine SMS-Nachricht sendet oder empfängt ist ein Beispiel für eine Aktion erfordern eine problematische Berechtigung.

> [!IMPORTANT]
> Die Kategorie aus, der eine Berechtigung zu gehört möglicherweise im Laufe der Zeit ändern.  Es ist möglich, eine Berechtigung, die kategorisiert wurde, da eine "normale" Berechtigung möglicherweise in Zukunft API-Ebenen in ein Berechtigungsobjekt gefährlich hochgestuft.

Problematische Berechtigungen sind nicht weiter unterteilt in [ _Berechtigungsgruppen_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Eine Berechtigungsgruppe, Berechtigungen gespeichert werden, die logisch verknüpft sind. Wenn der Benutzer die Berechtigung zu einem Mitglied einer Berechtigungsgruppe erteilt, die Berechtigung Android automatisch an alle Mitglieder dieser Gruppe. Z. B. die [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) Berechtigungsgruppe enthält sowohl die `WRITE_EXTERNAL_STORAGE` und `READ_EXTERNAL_STORAGE` Berechtigungen. Wenn der Benutzer die Berechtigung erteilt, `READ_EXTERNAL_STORAGE`, und klicken Sie dann die `WRITE_EXTERNAL_STORAGE` Berechtigung wird zur gleichen Zeit automatisch erteilt.

Bevor Sie eine oder mehrere Berechtigungen anfordern, ist es eine bewährte Methode, geben Sie eine Begründung, warum die Berechtigung, bevor Sie die Berechtigung für eine app erforderlich. Sobald der Benutzer die Begründung erkennt, kann die app Berechtigung vom Benutzer anzufordern. Verstehen Sie die Gründe, kann der Benutzer eine fundierte Entscheidung treffen, wenn sie möchten, die Berechtigung zu gewähren und Verstehen der Konsequenzen, wenn dies nicht der Fall. 

Der gesamte Workflow überprüfen und Anfordern von Berechtigungen wird als bezeichnet eine _zur Laufzeit Berechtigungen_ überprüfen und kann in der folgenden Abbildung zusammengefasst werden: 

[![Flussdiagramm für Laufzeit-Berechtigung überprüfen](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Die Android-Unterstützungsbibliothek Backports einige der neuen APIs für Berechtigungen auf ältere Versionen von Android. Diese mehr APIs prüft automatisch die Version von Android auf dem Gerät, daher ist es nicht notwendig, um einen API-Überprüfung jedes Mal auszuführen.  

Dieses Dokument wird beschrieben, wie Berechtigungen zu einer Xamarin.Android-Anwendung hinzufügen und wie apps für Android 6.0 (API-Ebene 23) oder höher sollten eine berechtigungsüberprüfung zur Laufzeit ausführen.


> [!NOTE]
> Es ist möglich, dass Berechtigungen für Hardware auswirken können, wie die app von Google Play gefiltert werden. Z. B. wenn die app für die Verwendung der Kamera Berechtigung erfordert, zeigt klicken Sie dann Google Play die app im Google Play Store auf einem Gerät nicht, die über keinen Kamera installiert.


<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Es wird dringend empfohlen, dass Xamarin.Android Projekte umfassen die [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet-Paket. Dieses Paket wird Backport-Berechtigung, die bestimmte APIs den Zugriff auf frühere Versionen von Android, die eine gemeinsame Bereitstellung ohne die Notwendigkeit, ständig Schnittstelle überprüfen die Version von Android, die die app ausgeführt wird.


## <a name="requesting-system-permissions"></a>Anfordern von Berechtigungen

Der erste Schritt beim Arbeiten mit Android Berechtigungen wird deklariert, dass die Berechtigungen in der Android-Manifestdatei. Dies muss unabhängig von der API-Ebene erfolgen, dass die app Anwendung ist.

Apps, die als Ziel Android 6.0 oder höher können nicht angenommen werden, die dem Benutzer die Berechtigung irgendwann in der Vergangenheit erteilt, dass die Berechtigung gültig das nächste Mal sind. Eine app für Android 6.0 auf muss immer eine Überprüfung der Berechtigung zur Laufzeit ausführen. Apps für Android 5.1 oder niedriger müssen sich nicht um Berechtigung zur Laufzeit zu überprüfen.

> [!NOTE]
> Anwendungen sollten nur die Berechtigungen anfordern, die sie benötigen.


### <a name="declaring-permissions-in-the-manifest"></a>Deklarieren von Berechtigungen im Manifest

Berechtigungen werden hinzugefügt, um die **AndroidManifest.xml** mit der `uses-permission` Element. Z. B. wenn eine Anwendung besteht darin, die Position des Geräts, es erfordert einwandfrei und Kurs Speicherort Berechtigungen. Die folgenden beiden Elemente werden dem Manifest hinzugefügt: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Es ist möglich, die Berechtigungen mithilfe der in Visual Studio integrierten Toolsupport deklarieren:

1. Doppelklicken Sie auf **Eigenschaften** in der **Projektmappen-Explorer** , und wählen Sie die **Android-Manifest** Registerkarte im Eigenschaftenfenster angezeigt:

    [![Erforderliche Berechtigungen auf der Registerkarte "Android-Manifest"](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Wenn die Anwendung nicht bereits über ein AndroidManifest.xml verfügt, klicken Sie auf **keine AndroidManifest.xml gefunden. Klicken Sie zum Hinzufügen einer** wie unten dargestellt:

    [![Keine AndroidManifest.xml-Meldung](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Wählen Sie alle Berechtigungen, die Ihre Anwendung benötigt werden, aus der **erforderliche Berechtigungen** angezeigt, und speichern:

    [![Beispiele für Berechtigungen zur KAMERA ausgewählt](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Es ist möglich, die Berechtigungen mithilfe der in Visual Studio für Mac integrierten Toolsupport deklarieren:

1. Doppelklicken Sie auf das Projekt in der **Lösung Pad** , und wählen Sie **Optionen > Erstellen > Android-Anwendung**:

    [![Erforderliche Abschnitt "Berechtigungen" angezeigt](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Klicken Sie auf die **Android-Manifest hinzufügen** Schaltfläche verfügt das Projekt nicht bereits ein **AndroidManifest.xml**:

    [![Android-Manifest des Projekts ist nicht vorhanden](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Wählen Sie alle Berechtigungen, die Ihre Anwendung benötigt werden, aus der **erforderliche Berechtigungen** aus, und klicken Sie auf **OK**:

    [![Beispiele für Berechtigungen zur KAMERA ausgewählt](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android wird automatisch einige Berechtigungen zur Buildzeit Debug-Builds hinzugefügt werden. Dies veranlasst das Debuggen der Anwendung einfacher. Zwei wichtige Berechtigungen gibt insbesondere `INTERNET` und `READ_EXTERNAL_STORAGE`. Diese Berechtigungen automatisch Satz erscheinen nicht im aktiviert werden die **erforderliche Berechtigungen** Liste. Releasebuilds jedoch, verwenden Sie nur die Berechtigungen, die explizit, in festgelegt sind der **erforderliche Berechtigungen** Liste. 

Für apps für Android 5.1 (API-Ebene 22) oder niedriger aufweist, ist es nichts weiter, die ausgeführt werden muss. Apps, die für Android 6.0 (API 23 Stufe 23) oder höher ausgeführt werden sollten fahren Sie mit dem nächsten Abschnitt zum zur Laufzeit ausführen Berechtigung überprüft. 


### <a name="runtime-permission-checks-in-android-60"></a>Common Language Runtime-Berechtigung überprüft in Android 6.0

Die `ContextCompat.CheckSelfPermission` -Methode (mit der Android-Unterstützungsbibliothek verfügbar) Dient zum Überprüfen, ob eine bestimmte Berechtigung erteilt wurde. Diese Methode zurückgegeben wird ein [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) Enum besitzt einen von zwei Werten:

* **`Permission.Granted`** &ndash; Verfügt über die angegebene Berechtigung erteilt wurde.
* **`Permission.Denied`** &ndash; Die angegebene Berechtigung wurde nicht erteilt.

Dieser Codeausschnitt ist ein Beispiel dafür, wie die Kamera-Berechtigung in einer Aktivität zu überprüfen: 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

Es ist eine bewährte Methode, informieren Sie den Benutzer, warum eine Berechtigung für eine Anwendung erforderlich sind, damit eine fundierte Entscheidung erfolgen kann, um die Berechtigung gewähren. Ein Beispiel hierfür wäre eine app, die Fotos und Geo-Tags akzeptiert werden. Es ist für den Benutzer, dass die Kamera-Berechtigung erforderlich ist, aber es möglicherweise nicht eindeutig sein, warum die app außerdem den Speicherort des Geräts benötigt. Die Begründung sollte besser verstehen, warum die Ort-Berechtigung ist, wünschenswert und die Kamera-Berechtigung erforderlich ist eine Nachricht angezeigt werden.

Die `ActivityCompat.ShouldShowRequestPermissionRational` Methode wird verwendet, um zu bestimmen, ob der Grund für den Benutzer angezeigt werden sollen. Von dieser Methode zurückgegeben `true` Wenn die Gründe für eine bestimmte Berechtigung angezeigt werden soll. Diese bildschirmabbildung zeigt ein Beispiel für eine Snackbar angezeigt, die von einer Anwendung, die erklärt, warum die app, die den Speicherort des Geräts kennen muss:

![Begründung für Speicherort](permissions-images/07-rationale-snackbar.png) 

Wenn der Benutzer die Berechtigung gewährt der `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` Methode sollte aufgerufen werden. Diese Methode erfordert die folgenden Parameter:

* **Aktivität** &ndash; Dies ist die Aktivität, die die Berechtigungen anfordert, und wird von der Ergebnisse Android informiert werden.
* **Berechtigungen** &ndash; eine Liste der Berechtigungen, die angefordert werden.
* **RequestCode** &ndash; ein Integer-Wert, der verwendet wird, um die Ergebnisse der berechtigungsanforderung entsprechen einem `RequestPermissions` aufrufen. Dieser Wert muss größer als null sein.

Dieser Codeausschnitt ist ein Beispiel für die beiden Methoden, die behandelt wurden. Zunächst wird eine Überprüfung durchgeführt, um zu bestimmen, ob die Berechtigung Begründung angezeigt werden sollen. Wenn der Grund ist, angezeigt werden soll, wird eine Snackbar mit Begründung angezeigt. Wenn der Benutzer klickt **OK** in der Snackbar klicken Sie dann die app wird der Berechtigungen anfordern. Wenn der Benutzer keine Begründung annimmt, sollte die app nicht fortgesetzt, um Berechtigungen zu erhalten. Wenn der Grund nicht angezeigt wird, wird die Aktivität die Berechtigung anfordern:

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` kann aufgerufen werden, auch wenn der Benutzer bereits die Berechtigung gewährt hat. Nachfolgende Aufrufe sind nicht erforderlich, aber sie bieten den Benutzer die Möglichkeit, die Berechtigung bestätigen (oder aufzuheben). Wenn `RequestPermission` wird aufgerufen, Steuerelement deaktiviert für das Betriebssystem, der eine Benutzeroberfläche angezeigt wird, für die Berechtigungen akzeptieren übergeben wird:  

![Dialogfeld für Berechtigungen](permissions-images/08-location-permission-dialog.png)

Nach Abschluss der Benutzer Android die Ergebnisse zurück an die Aktivität über eine Rückrufmethode `OnRequestPermissionResult`. Diese Methode ist ein Teil der Schnittstelle `ActivityCompat.IOnRequestPermissionsResultCallback` die von der Aktivität implementiert werden muss. Diese Schnittstelle verfügt über eine einzelne Methode `OnRequestPermissionsResult`, dem wird aufgerufen, die von Android, um die Aktivität der Auswahl des Benutzers zu informieren. Wenn der Benutzer die Berechtigung erteilt wurde, kann die app fahren Sie fort und verwenden Sie die geschützte Ressource. Ein Beispiel zum Implementieren von `OnRequestPermissionResult` wird unten gezeigt: 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  


## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert das Hinzufügen und überprüfen Sie Berechtigungen in einem Android-Gerät. Die Unterschiede in der Funktionsweise von Berechtigungen zwischen alten Android-apps (API-Ebene < 23) und neuen Android-apps (API-Ebene > 22). Erläutert, zur Laufzeit berechtigungsprüfungen in Android 6.0 auszuführen.


## <a name="related-links"></a>Verwandte Links

- [Liste mit normalen Berechtigungen](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Beispiel-App der Common Language Runtime-Berechtigungen](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Behandlung von Berechtigungen in Xamarin.Android](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
