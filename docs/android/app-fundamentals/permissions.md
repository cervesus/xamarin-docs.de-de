---
title: Berechtigungen in xamarin. Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 911f56026a1495099e81a542b30b280f26b6a9e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025463"
---
# <a name="permissions-in-xamarinandroid"></a>Berechtigungen in xamarin. Android

## <a name="overview"></a>Übersicht

Android-Anwendungen werden in einem eigenen Sandkasten ausgeführt und haben aus Sicherheitsgründen keinen Zugriff auf bestimmte Systemressourcen oder Hardware auf dem Gerät. Der Benutzer muss der APP explizit Berechtigungen erteilen, bevor diese Ressourcen verwendet werden können. Beispielsweise kann eine Anwendung nicht auf das GPS auf einem Gerät zugreifen, ohne dass der Benutzer explizit berechtigt ist. Android löst eine `Java.Lang.SecurityException` aus, wenn eine APP versucht, ohne Berechtigung auf eine geschützte Ressource zuzugreifen.

Berechtigungen werden vom Anwendungsentwickler in der Datei " **androidmanifest. XML** " deklariert, wenn die APP entwickelt wird. Android verfügt über zwei unterschiedliche Workflows zum Abrufen der Zustimmung des Benutzers für diese Berechtigungen:

- Für apps, die auf Android 5,1 (API-Ebene 22) oder niedriger abzielen, erfolgte die Berechtigungs Anforderung, als die APP installiert wurde. Wenn der Benutzer die Berechtigungen nicht erteilt hat, würde die APP nicht installiert werden. Nachdem die APP installiert wurde, gibt es keine Möglichkeit, die Berechtigungen aufzuheben, außer indem Sie die APP deinstallieren.
- Ab Android 6,0 (API-Ebene 23) haben Benutzer mehr Kontrolle über Berechtigungen erhalten. Sie können Berechtigungen erteilen oder widerrufen, solange die APP auf dem Gerät installiert ist. Dieser Screenshot zeigt die Berechtigungseinstellungen für die Google Contacts-app. Sie listet die verschiedenen Berechtigungen auf und ermöglicht dem Benutzer das Aktivieren oder Deaktivieren von Berechtigungen:

![Bildschirm für Beispiel Berechtigungen](permissions-images/01-permissions-check.png) 

Android-Apps müssen sich zur Laufzeit überprüfen, um festzustellen, ob Sie über die Berechtigung zum Zugriff auf eine geschützte Ressource verfügen. Wenn die APP nicht über die erforderliche Berechtigung verfügt, muss Sie mithilfe der neuen APIs, die von der Android SDK bereitgestellt werden, damit der Benutzer die Berechtigungen erteilt. Berechtigungen werden in zwei Kategorien unterteilt:

- **Normale Berechtigungen** &ndash; Dies sind Berechtigungen, die ein wenig Sicherheitsrisiko für die Sicherheit und den Datenschutz des Benutzers darstellen. Android 6,0 gewährt zum Zeitpunkt der Installation automatisch normale Berechtigungen. Eine [komplette Liste der normalen Berechtigungen](https://developer.android.com/guide/topics/permissions/normal-permissions.html)finden Sie in der Android-Dokumentation.
- **Gefährliche Berechtigungen** &ndash; im Gegensatz zu normalen Berechtigungen sind gefährliche Berechtigungen, mit denen die Sicherheit oder der Datenschutz des Benutzers geschützt wird. Diese müssen explizit vom Benutzer erteilt werden. Das Senden oder Empfangen einer SMS-Nachricht ist ein Beispiel für eine Aktion, die eine gefährliche Berechtigung erfordert.

> [!IMPORTANT]
> Die Kategorie, zu der eine Berechtigung gehört, kann sich im Laufe der Zeit ändern.  Es ist möglich, dass eine Berechtigung, die als "normale"-Berechtigung kategorisiert wurde, in zukünftigen API-Ebenen auf eine gefährliche Berechtigung erhöht werden kann.

Gefährliche Berechtigungen sind weiter unterteilt in [_Berechtigungs Gruppen_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Eine Berechtigungs Gruppe enthält Berechtigungen, die logisch verknüpft sind. Wenn der Benutzer eine Berechtigung für ein Mitglied einer Berechtigungs Gruppe erteilt, erteilt Android automatisch die Berechtigung für alle Mitglieder dieser Gruppe. Die [`STORAGE`](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) Berechtigungs Gruppe enthält z. b. die Berechtigungen `WRITE_EXTERNAL_STORAGE` und `READ_EXTERNAL_STORAGE`. Wenn der Benutzer die Berechtigung für `READ_EXTERNAL_STORAGE`erteilt, wird die Berechtigung `WRITE_EXTERNAL_STORAGE` automatisch zur gleichen Zeit erteilt.

Vor dem Anfordern einer oder mehrerer Berechtigungen ist es eine bewährte Vorgehensweise, eine Begründung anzugeben, warum die APP die Berechtigung erfordert, bevor Sie die Berechtigung anfordert. Sobald der Benutzer die Begründung verstanden hat, kann die APP die Berechtigung vom Benutzer anfordern. Wenn Sie die Begründung verstanden haben, kann der Benutzer eine fundierte Entscheidung treffen, wenn er die Berechtigung erteilen möchte und die Auswirkungen zu verstehen, wenn dies nicht der Fall ist. 

Der gesamte Workflow zum Überprüfen und Anfordern von Berechtigungen wird als _Lauf Zeit Berechtigungs_ Überprüfung bezeichnet und kann im folgenden Diagramm zusammengefasst werden: 

[Flussdiagramm zur Überprüfung der![Lauf Zeit Berechtigung](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Die Android-Unterstützungs Bibliothek unterstützt einige der neuen APIs für Berechtigungen für ältere Android-Versionen. Diese backportiert-APIs überprüfen automatisch die Android-Version auf dem Gerät, sodass Sie nicht jedes Mal eine Überprüfung auf API-Ebene durchführen müssen.  

In diesem Dokument wird erläutert, wie Sie einer xamarin. Android-Anwendung Berechtigungen hinzufügen und wie apps, die auf Android 6,0 (API-Ebene 23) oder höher abzielen, eine Lauf Zeit Berechtigungsüberprüfung ausführen sollten.

> [!NOTE]
> Es ist möglich, dass die Berechtigungen für Hardware sich darauf auswirken können, wie die APP nach Google Play gefiltert wird. Wenn die APP z. b. eine Berechtigung für die Kamera erfordert, zeigt Google Play die APP nicht in der Google Play Store auf einem Gerät an, auf dem keine Kamera installiert ist.

<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Es wird dringend empfohlen, dass xamarin. Android-Projekte das nuget-Paket [xamarin. Android. Support. compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) enthalten. Mit diesem Paket werden Berechtigungs spezifische APIs für ältere Versionen von Android backportieren. dabei wird eine gemeinsame Schnittstelle bereitgestellt, ohne dass die Android-Version, auf der die app ausgeführt wird, ständig überprüft werden muss.

## <a name="requesting-system-permissions"></a>Anfordern von System Berechtigungen

Der erste Schritt bei der Arbeit mit Android-Berechtigungen besteht darin, die Berechtigungen in der Android-Manifest-Datei zu deklarieren. Dies muss unabhängig von der API-Ebene erfolgen, für die die APP vorgesehen ist.

Apps, die auf Android 6,0 oder höher ausgerichtet sind, können nicht davon ausgehen, dass der Benutzer die Berechtigung zu einem bestimmten Zeitpunkt in der Vergangenheit erteilt hat, dass die Berechtigung beim nächsten Mal gültig ist. Eine APP, die auf Android 6,0 abzielt, muss immer eine Lauf Zeit Berechtigungsüberprüfung durchführen. Apps, die auf Android 5,1 oder niedriger abzielen, müssen keine Lauf Zeit Berechtigungsüberprüfung durchführen.

> [!NOTE]
> Anwendungen sollten nur die Berechtigungen anfordern, die Sie benötigen.

### <a name="declaring-permissions-in-the-manifest"></a>Deklarieren von Berechtigungen im Manifest

Die Berechtigungen werden der Datei " **androidmanifest. XML** " mit dem `uses-permission`-Element hinzugefügt. Wenn eine Anwendung z. b. die Position des Geräts Auffinden soll, sind die Berechtigungen für die-und Kurs Orte erforderlich. Die folgenden beiden Elemente werden dem Manifest hinzugefügt: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Es ist möglich, die Berechtigungen mithilfe der in Visual Studio integrierten Tool Unterstützung zu deklarieren:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf **Eigenschaften** , und wählen Sie im Eigenschaftenfenster die Registerkarte **Android-Manifest** aus:

    [![erforderlichen Berechtigungen auf der Registerkarte "Android-Manifest"](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Wenn die Anwendung nicht bereits über die Datei "androidmanifest. xml" verfügt, klicken Sie auf " **keine androidmanifest. xml". Klicken Sie, um einen** wie unten gezeigt hinzuzufügen:

    [![keine "androidmanifest. xml"-Meldung](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Wählen Sie in der Liste der **erforderlichen Berechtigungen** alle Berechtigungen aus, die Ihre Anwendung benötigt, und speichern Sie

    [![Beispiel für Kamera Berechtigungen ausgewählt](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Es ist möglich, die Berechtigungen mithilfe der in Visual Studio für Mac integrierten Tool Unterstützung zu deklarieren:

1. Doppelklicken Sie im **Lösungspad** auf das Projekt, und wählen Sie **Optionen > Build > Android-Anwendung**aus:

    [![Abschnitt "erforderliche Berechtigungen" angezeigt](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Klicken Sie auf die Schaltfläche **Android-Manifest hinzufügen** , wenn das Projekt nicht bereits über eine " **androidmanifest. XML**" verfügt:

    [![das Android-Manifest des Projekts fehlt.](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Wählen Sie in der Liste **erforderliche Berechtigungen** alle Berechtigungen aus, die Ihre Anwendung benötigt, und klicken Sie auf **OK**:

    [![Beispiel für Kamera Berechtigungen ausgewählt](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin. Android fügt bei der Buildzeit automatisch einige Berechtigungen zum Debuggen von Builds hinzu. Dadurch wird das Debuggen der Anwendung vereinfacht. Insbesondere sind zwei relevante Berechtigungen `INTERNET` und `READ_EXTERNAL_STORAGE`. Diese automatisch festgelegten Berechtigungen werden in der Liste **erforderliche Berechtigungen** nicht als aktiviert angezeigt. Releasebuilds verwenden jedoch nur die Berechtigungen, die explizit in der Liste **erforderliche Berechtigungen** festgelegt sind. 

Für apps, die auf Android 5.1 (API-Ebene 22) oder niedriger abzielen, müssen Sie nichts weiter tun. Apps, die unter Android 6,0 (API 23 Level 23) oder höher ausgeführt werden, sollten mit dem nächsten Abschnitt zum Ausführen von Berechtigungs Prüfungen zur Laufzeit fortfahren. 

### <a name="runtime-permission-checks-in-android-60"></a>Lauf Zeit Berechtigungs Prüfungen in Android 6,0

Die `ContextCompat.CheckSelfPermission`-Methode (verfügbar mit der Android-Unterstützungs Bibliothek) wird verwendet, um zu überprüfen, ob eine bestimmte Berechtigung erteilt wurde. Diese Methode gibt eine [`Android.Content.PM.Permission`](xref:Android.Content.PM.Permission) Enumeration zurück, die einen von zwei Werten hat:

- **`Permission.Granted`** &ndash; die angegebene Berechtigung erteilt wurde.
- **`Permission.Denied`** &ndash; die angegebene Berechtigung nicht erteilt wurde.

Dieser Code Ausschnitt ist ein Beispiel dafür, wie die Kamera Berechtigung in einer Aktivität überprüft wird: 

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

Es wird empfohlen, den Benutzer darüber zu informieren, warum eine Berechtigung für eine Anwendung erforderlich ist, damit eine fundierte Entscheidung getroffen werden kann, um die Berechtigung zu erteilen. Ein Beispiel hierfür wäre eine APP, die Fotos aufnimmt und Sie als georedunziert. Dem Benutzer ist klar, dass die Kamera Berechtigung erforderlich ist, aber es ist nicht klar, warum die APP auch den Speicherort des Geräts benötigt. Der Grund dafür ist, dass eine Meldung angezeigt wird, die dem Benutzer hilft, zu verstehen, warum die Location-Berechtigung erwünscht ist und dass die Kamera Berechtigung erforderlich ist.

Die `ActivityCompat.ShouldShowRequestPermissionRationale`-Methode wird verwendet, um zu bestimmen, ob dem Benutzer die Begründung angezeigt werden soll. Diese Methode gibt `true` zurück, wenn die Begründung für eine bestimmte Berechtigung angezeigt werden soll. Dieser Screenshot zeigt ein Beispiel für eine Snackbar, die von einer Anwendung angezeigt wird, in der erläutert wird, warum die APP den Speicherort des Geräts kennen muss:

![Begründung für Speicherort](permissions-images/07-rationale-snackbar.png) 

Wenn der Benutzer die Berechtigung erteilt, sollte die `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)`-Methode aufgerufen werden. Diese Methode erfordert die folgenden Parameter:

- **Aktivität** &ndash; Dies ist die Aktivität, die die Berechtigungen anfordert und von Android der Ergebnisse informiert werden soll.
- **Berechtigungen** &ndash; eine Liste der angeforderten Berechtigungen.
- **requestcode** &ndash; einen ganzzahligen Wert, der verwendet wird, um die Ergebnisse der Berechtigungs Anforderung einem `RequestPermissions`-Befehl zuzuordnen. Dieser Wert muss größer als null sein.

Dieser Code Ausschnitt ist ein Beispiel für die beiden Methoden, die besprochen wurden. Zuerst wird geprüft, ob die Berechtigungs Begründung angezeigt werden soll. Wenn die Begründung angezeigt werden soll, wird eine Snackbar mit der Begründung angezeigt. Wenn der Benutzer in der Snackbar auf **OK** klickt, werden die Berechtigungen von der APP angefordert. Wenn der Benutzer die Begründung nicht annimmt, sollte die APP nicht mit den Anforderungs Berechtigungen fortfahren. Wenn die Begründung nicht angezeigt wird, wird die-Berechtigung von der-Aktivität angefordert:

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

`RequestPermission` können auch aufgerufen werden, wenn der Benutzer bereits die Berechtigung erteilt hat. Nachfolgende Aufrufe sind nicht erforderlich, bieten jedoch dem Benutzer die Möglichkeit, die Berechtigung zu bestätigen (oder aufzuheben). Wenn `RequestPermission` aufgerufen wird, wird die Steuerung an das Betriebssystem übergeben, das eine Benutzeroberfläche zum Akzeptieren der Berechtigungen anzeigt:  

![Permession (Dialog Feld)](permissions-images/08-location-permission-dialog.png)

Nachdem der Benutzer fertig ist, gibt Android die Ergebnisse über eine Rückruf Methode (`OnRequestPermissionResult`) an die Aktivität zurück. Diese Methode ist Teil der-Schnittstelle `ActivityCompat.IOnRequestPermissionsResultCallback` die von der-Aktivität implementiert werden muss. Diese Schnittstelle verfügt über eine einzelne Methode, `OnRequestPermissionsResult`, die von Android aufgerufen wird, um die Aktivitäten der Benutzeroptionen zu informieren. Wenn der Benutzer die Berechtigung erteilt hat, kann die APP die geschützte Ressource verwenden. Ein Beispiel für die Implementierung `OnRequestPermissionResult` finden Sie unten: 

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

In diesem Leitfaden wurde erläutert, wie Sie Berechtigungen auf einem Android-Gerät hinzufügen und überprüfen. Die Unterschiede in der Funktionsweise von Berechtigungen zwischen alten Android-Apps (API-Ebene < 23) und neuen Android-Apps (API-Ebene > 22). Es wurde erläutert, wie Lauf Zeit Berechtigungs Überprüfungen in Android 6,0 ausgeführt werden.

## <a name="related-links"></a>Verwandte Links

- [Liste der normalen Berechtigungen](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Beispiel-App für Lauf Zeit Berechtigungen](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Behandeln von Berechtigungen in xamarin. Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
