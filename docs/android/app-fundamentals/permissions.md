---
title: Berechtigungen In Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1a73896e4f98a6535bcd7ed66f478d168b01157f
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526883"
---
# <a name="permissions-in-xamarinandroid"></a>Berechtigungen In Xamarin.Android


## <a name="overview"></a>Übersicht

Android-Anwendungen in ihren eigenen Sandbox ausgeführt und für die Sicherheit Gründe haben keinen Zugriff auf bestimmte Ressourcen oder die Hardware auf dem Gerät. Der Benutzer muss die Berechtigung für die app explizit gewähren, bevor diese Ressourcen verwendet werden kann. Beispielsweise kann keine Anwendung das GPS auf einem Gerät ohne explizite Berechtigung des Benutzers zugreifen. Android löst eine `Java.Lang.SecurityException` , wenn eine app ohne Berechtigung eine geschützte Ressource zuzugreifen versucht.

Berechtigungen werden in deklariert die **"androidmanifest.xml"** durch den Anwendungsentwickler, wenn die app entwickelt wird. Android hat zwei unterschiedliche Workflows für die Zustimmung des Benutzers für diese Berechtigungen abrufen:
 
* Für apps, die das Ziel Android 5.1 (API-Ebene 22) oder niedriger ist der berechtigungsanforderung aufgetreten, wenn die app installiert wurde. Wenn der Benutzer nicht die Berechtigungen erteilt hat, würde die app nicht installiert werden. Sobald die app installiert ist, ist gibt es keine Möglichkeit, die Berechtigungen mit Ausnahme der zu widerrufen, indem Sie die app deinstalliert.
* Ab Android 6.0 (API-Ebene 23), wurden die Benutzer mehr Kontrolle über Berechtigungen vorgegeben. Sie können die gewähren oder widerrufen Berechtigungen aus, solange die app auf dem Gerät installiert ist. Dieser Screenshot zeigt die berechtigungseinstellungen für die Google-Kontakte-app. Es listet die verschiedenen Berechtigungen und ermöglicht dem Benutzer zum Aktivieren oder deaktivieren die Berechtigungen:

![Beispielbildschirm für Berechtigungen](permissions-images/01-permissions-check.png) 

Android-apps müssen überprüfen, zur Laufzeit zu ermitteln, ob sie über die Berechtigung zum Zugriff auf eine geschützte Ressource ist. Wenn die app die Berechtigung nicht besitzt, muss er Anforderungen mithilfe der neuen APIs, die vom Android SDK für den Benutzer bereitgestellt werden die Berechtigungen ausführen. Berechtigungen sind in zwei Kategorien unterteilt:

* **Berechtigungen der normalen** &ndash; Hierbei handelt es sich um Berechtigungen, die wenig Sicherheitsrisiko für Sicherheits- oder Datenschutzrisiko des Benutzers dar. Android 6.0 wird den normale Berechtigungen zum Zeitpunkt der Installation automatisch erteilt. Wenden Sie sich die Android-Dokumentation für einen [vollständige Liste der normalen Berechtigungen](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Problematische Berechtigungen** &ndash; im Gegensatz zu normalen Berechtigungen problematischen Berechtigungen sind für die Sicherheits- oder Datenschutzrisiko des Benutzers zu schützen. Diese müssen explizit vom Benutzer erteilt sein. Senden oder Empfangen einer SMS-Nachricht ist ein Beispiel für eine Aktion, die eine riskante Berechtigung erfordern.

> [!IMPORTANT]
> Die Kategorie, zu dem eine Berechtigung gehört möglicherweise im Lauf der Zeit ändern.  Es ist möglich, dass eine Berechtigung, die kategorisiert wurde, da möglicherweise eine "normale"-Berechtigung in Zukunft-API-Ebenen auf eine riskante Berechtigung mit erhöhten Rechten.

Problematische Berechtigungen sind nicht weiter unterteilt in [ _Berechtigungsgruppen_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Eine Berechtigungsgruppe enthält Berechtigungen, die logisch verknüpft sind. Wenn der Benutzer die Berechtigung, ein Mitglied einer Berechtigungsgruppe erteilt Android gewährt die Berechtigung automatisch für alle Mitglieder dieser Gruppe. Z. B. die [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) Berechtigungsgruppe enthält sowohl die `WRITE_EXTERNAL_STORAGE` und `READ_EXTERNAL_STORAGE` Berechtigungen. Wenn der Benutzer die Berechtigung erteilt `READ_EXTERNAL_STORAGE`, und klicken Sie dann die `WRITE_EXTERNAL_STORAGE` Berechtigung wird zur gleichen Zeit automatisch gewährt.

Bevor Sie eine oder mehrere Berechtigungen anfordern, ist es eine bewährte Methode, geben Sie eine Begründung, warum die app die Berechtigung, bevor Sie die Berechtigung anfordern muss. Wenn der Benutzer das Grundprinzip verstanden hat, kann die app die Berechtigung vom Benutzer anfordern. Verstehen der Gründe, kann der Benutzer eine fundierte Entscheidung bei Bedarf die Berechtigung erteilen, und die Auswirkungen zu verstehen, wenn dies nicht der Fall. 

Der gesamte Workflow überprüfen und Anfordern von Berechtigungen ist bekannt, wie eine _Berechtigungen zur Laufzeit_ überprüfen, und kann in der folgenden Abbildung zusammengefasst werden: 

[![Flussdiagramm für Laufzeit-Berechtigung überprüfen](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Die Android Support-Bibliothek-Backports einige der neuen APIs für Berechtigungen auf ältere Versionen von Android. Diese zurückportierte APIs überprüft automatisch die Version von Android auf dem Gerät, daher es nicht erforderlich ist, um eine API-Ebene Überprüfung jedes Mal auszuführen.  

In diesem Dokument erfahren Sie, wie Berechtigungen einer Xamarin.Android-Anwendung hinzufügen und wie apps für Android 6.0 (API-Ebene 23) oder höher sollten eine Laufzeit berechtigungsüberprüfung ausführen.


> [!NOTE]
> Es ist möglich, dass Berechtigungen für die Hardware auswirken können, wie die app von Google Play gefiltert wird. Z. B. wenn die app eine Berechtigung für die Kamera erfordert, zeigt Google Play nicht die app in der Google Play Store auf einem Gerät dann, die keine Kamera installiert werden.


<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Es wird dringend empfohlen, dass die Xamarin.Android-Projekte enthalten die [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet-Paket. Dieses Paket wird Backport-Berechtigung, die spezifische APIs auf ältere Versionen von Android, die eine Bereitstellung ohne ständig Schnittstelle überprüfen Sie die Version von Android, die die app ausgeführt wird.


## <a name="requesting-system-permissions"></a>Anfordern von Berechtigungen

Der erste Schritt bei der Arbeit mit Android-Berechtigungen ist deklariert, dass die Berechtigungen im Android-Manifestdatei. Dies muss unabhängig von der API-Ebene erfolgen, dass die app wird.

Apps, die Ausrichtung auf Android 6.0 oder höher können nicht angenommen werden, die dem Benutzer die Berechtigung an einem bestimmten Punkt in der Vergangenheit erteilt, dass die Berechtigung gültig das nächste Mal wird. Eine app, die auf Android 6.0 abzielt, muss immer eine Überprüfung der Berechtigung zur Laufzeit ausführen. Apps für Android Version 5.1 oder niedriger müssen sich nicht um eine berechtigungsüberprüfung der Run-Time-ausführen.

> [!NOTE]
> Anwendungen sollten nur die Berechtigungen anfordern, die sie benötigen.


### <a name="declaring-permissions-in-the-manifest"></a>Deklarieren Sie die Berechtigungen im Manifest

Berechtigungen werden hinzugefügt, um die **"androidmanifest.xml"** mit der `uses-permission` Element. Z. B. wenn eine Anwendung besteht darin, die Position des Geräts, es erfordert gut und Berechtigungen für Positionsdaten Kurs. Das Manifest werden die folgenden zwei Elemente hinzugefügt: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Es ist möglich, deklarieren Sie die Berechtigungen, die mithilfe der toolunterstützung in Visual Studio integriert:

1. Doppelklicken Sie auf **Eigenschaften** in die **Projektmappen-Explorer** , und wählen Sie die **Android-Manifest** Registerkarte im Eigenschaftenfenster angezeigt:

    [![Erforderliche Berechtigungen auf der Registerkarte "Android-Manifest"](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Wenn die Anwendung nicht bereits eine Datei "androidmanifest.xml" verfügt, klicken Sie auf **keine "androidmanifest.xml" gefunden. Klicken Sie auf einen hinzufügen** wie unten dargestellt:

    [![Keine "androidmanifest.xml"-Meldung](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Wählen Sie Ihre Anwendung über muss Berechtigungen der **erforderliche Berechtigungen** aufzulisten und zu speichern:

    [![Beispiele für Berechtigungen zur KAMERA ausgewählt](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Es ist möglich, deklarieren Sie die Berechtigungen, die mithilfe der toolunterstützung in Visual Studio für Mac integriert:

1. Doppelklicken Sie auf das Projekt in der **Lösungspad** , und wählen Sie **Optionen > Erstellen > Android-Anwendung**:

    [![Erforderliche Abschnitt "Berechtigungen" angezeigt](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Klicken Sie auf die **Android-Manifest hinzufügen** Schaltfläche verfügt das Projekt nicht bereits ein **"androidmanifest.xml"**:

    [![Android-Manifest des Projekts ist nicht vorhanden](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Wählen Sie Ihre Anwendung über muss Berechtigungen der **erforderliche Berechtigungen** aus, und klicken Sie auf **OK**:

    [![Beispiele für Berechtigungen zur KAMERA ausgewählt](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android werden bestimmte Berechtigungen zum Zeitpunkt der Erstellung auf Debugbuilds automatisch hinzugefügt. Dies veranlasst die Debuggen der Anwendung einfacher. Insbesondere sind dies zwei Berechtigungen, die wichtige `INTERNET` und `READ_EXTERNAL_STORAGE`. Diese Berechtigungen automatisch festgelegte erscheint nicht aktiviert werden die **erforderliche Berechtigungen** Liste. Releasebuilds, jedoch können Sie nur die Berechtigungen, die explizit auf die **erforderliche Berechtigungen** Liste. 

Für apps für Android 5.1 (API-Ebene 22) oder niedriger ist es nicht mehr, die ausgeführt werden muss. Apps, die auf Android 6.0 (API 23-Ebene 23) oder höher ausgeführt werden, sollten mit dem nächsten Abschnitt zur Laufzeit Durchführung Berechtigung überprüft fortfahren. 


### <a name="runtime-permission-checks-in-android-60"></a>Runtime-Berechtigung überprüft wird, in Android 6.0

Die `ContextCompat.CheckSelfPermission` -Methode (verfügbar mit der Android Support-Bibliothek) Dient zum Überprüfen, ob eine bestimmte Berechtigung erteilt wurde. Diese Methode gibt eine [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) Enumeration, die eine der beiden Werte aufweist:

* **`Permission.Granted`** &ndash; Die angegebene Berechtigung wurde gewährt wurde.
* **`Permission.Denied`** &ndash; Die angegebene Berechtigung wurde nicht erteilt.

Dieser Codeausschnitt ist ein Beispiel für die Kamera-Berechtigung in einer Aktivität überprüfen: 

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

Es ist eine bewährte Methode, informieren Sie den Benutzer, warum eine Berechtigung für eine Anwendung erforderlich ist, damit eine fundierte Entscheidung getroffen werden kann, zum Erteilen der Berechtigung. Ein Beispiel hierfür wäre eine app, die Fotos und Geo-Tags verwendet werden. Es ist für den Benutzer, dass die Kamera-Berechtigung erforderlich ist, aber möglicherweise nicht klar, warum die app außerdem den Speicherort des Geräts benötigt. Das Grundprinzip sollte angezeigt werden eine Meldung an den Benutzer zu verstehen, warum die speicherortberechtigung ist, wünschenswert und die Kamera-Berechtigung erforderlich ist.

Die `ActivityCompat.ShouldShowRequestPermissionRational` Methode wird verwendet, um zu bestimmen, ob die Gründe für den Benutzer angezeigt werden soll. Diese Methode gibt `true` Wenn die Gründe für eine angegebene Berechtigung angezeigt werden soll. Dieser Screenshot zeigt ein Beispiel für eine Snackbar angezeigt, die von einer Anwendung, die erklärt, warum die app muss den Speicherort des Geräts kennen:

![Gründe für Position](permissions-images/07-rationale-snackbar.png) 

Wenn der Benutzer die Berechtigung gewährt der `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` -Methode aufgerufen werden soll. Diese Methode erfordert die folgenden Parameter:

* **Aktivität** &ndash; Dies ist die Aktivität, die Berechtigungen anfordert, und von Android der Ergebnisse informiert werden soll.
* **Berechtigungen** &ndash; eine Liste der Berechtigungen, die angefordert werden.
* **"requestcode"** &ndash; ein ganzzahliger Wert, der verwendet wird, um die Ergebnisse der Anforderung der Berechtigung zum Erfüllen einer `RequestPermissions` aufrufen. Dieser Wert muss größer als null sein.

Dieser Codeausschnitt ist ein Beispiel für die zwei Methoden, die hier erörtert wurden. Zunächst wird eine Überprüfung durchgeführt, um festzustellen, ob die Gründe für die Berechtigungen angezeigt werden soll. Wenn der Grund ist, angezeigt werden soll, wird eine Snackbar mit der Grund angezeigt. Wenn der Benutzer klickt **OK** in die Snackbar, klicken Sie dann die app fordert die Berechtigungen. Wenn der Benutzer die Gründe nicht akzeptiert, sollte die app nicht fortgesetzt, Berechtigungen anfordern. Wenn die Gründe nicht angezeigt wird, wird die Aktivität die Berechtigung anfordern:

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

`RequestPermission` kann aufgerufen werden, auch wenn der Benutzer bereits die Berechtigung gewährt hat. Nachfolgende Aufrufe sind nicht erforderlich, aber sie bieten dem Benutzer die Möglichkeit, die Berechtigung bestätigen (oder aufzuheben). Wenn `RequestPermission` wird aufgerufen, Steuerelement wird übergeben, für das Betriebssystem, der eine Benutzeroberfläche angezeigt wird, akzeptieren Sie die Berechtigungen:  

![Dialogfeld für Berechtigungen](permissions-images/08-location-permission-dialog.png)

Nachdem der Benutzer abgeschlossen ist, Android die Ergebnisse zurück an die Aktivität über eine Rückrufmethode `OnRequestPermissionResult`. Diese Methode ist ein Teil der Schnittstelle `ActivityCompat.IOnRequestPermissionsResultCallback` die von der Aktivität implementiert werden muss. Diese Schnittstelle verfügt über eine einzelne Methode, `OnRequestPermissionsResult`, die von Android, um die Aktivität des Benutzers Entscheidungen darüber zu informieren aufgerufen. Wenn der Benutzer die Berechtigung erteilt hat, kann die app fortfahren und die geschützte Ressource verwenden. Ein Beispiel zum Implementieren `OnRequestPermissionResult` ist unten dargestellt: 

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
            Snackbar.Make(layout, Resource.String.permission_available_camera, Snackbar.LengthShort).Show();            
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

Dieser Leitfaden erläutert das Hinzufügen und überprüfen Sie für Berechtigungen in einem Android-Gerät. Die Unterschiede bei der Funktionsweise von Berechtigungen zwischen alten Android-apps (API-Ebene 23 <) und neue Android-apps (API-Ebene 22 >). Er erläutert, wie im Android 6.0 Runtime-Berechtigung überprüfen.


## <a name="related-links"></a>Verwandte Links

- [Liste mit normalen Berechtigungen](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Beispiel-App von Common Language Runtime-Berechtigungen](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Behandeln von Berechtigungen in Xamarin.Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
