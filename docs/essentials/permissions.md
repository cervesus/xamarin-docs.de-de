---
title: 'Xamarin.Essentials: Berechtigungen'
description: In diesem Dokument wird die Permissions-Klasse in Xamarin.Essentials beschrieben, die die Möglichkeit bietet, Laufzeitberechtigungen zu prüfen und anzufordern.
ms.assetid: 34062D84-3E55-4AF7-A688-8551068B1E57
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 01/06/2020
ms.openlocfilehash: fbce02300363c3ec68c35c11afb25342f06f4be1
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83150069"
---
# <a name="xamarinessentials-permissions"></a>Xamarin.Essentials: Berechtigungen

Die **Permissions**-Klasse bietet die Möglichkeit, Laufzeitberechtigungen zu prüfen und anzufordern.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

[!include[](~/essentials/includes/android-permissions.md)]

## <a name="using-permissions"></a>Verwenden von Berechtigungen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="checking-permissions"></a>Überprüfen von Berechtigungen

Um den aktuellen Status einer Berechtigung zu prüfen, verwenden Sie die `CheckStatusAsync`-Methode zusammen mit der jeweiligen Berechtigung, um den Status abzurufen.

```csharp
var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
```

Eine `PermissionException` wird ausgelöst, wenn die erforderliche Berechtigung nicht deklariert ist.

Sie sollten den Status der Berechtigung vor der Anforderung zu überprüfen. Jedes Betriebssystem gibt einen anderen Standardstatus zurück, wenn dem Benutzer noch keine Eingabeaufforderung angezeigt wurde. iOS gibt `Unknown` zurück, während andere `Denied` zurückgeben.

## <a name="requesting-permissions"></a>Anfordern von Berechtigungen

Um eine Berechtigung von den Benutzern anzufordern, verwenden Sie die `RequestAsync`-Methode zusammen mit der spezifischen anzufordernden Berechtigung. Wenn der Benutzer zuvor eine Berechtigung erteilt und diese nicht widerrufen hat, gibt diese Methode sofort `Granted` zurück, ohne ein Dialogfeld einzublenden.

```csharp
var status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
```

Eine `PermissionException` wird ausgelöst, wenn die erforderliche Berechtigung nicht deklariert ist.

Beachten Sie, dass auf einigen Plattformen eine Berechtigungsanforderung nur einmalig aktiviert werden kann. Weitere Aufforderungen müssen vom Entwickler behandelt werden, um zu prüfen, ob sich eine Berechtigung den Status `Denied` hat, und den Benutzer aufzufordern, sie manuell zu aktivieren.

## <a name="permission-status"></a>Berechtigungsstatus

Bei Verwenden von `CheckStatusAsync` oder `RequestAsync` wird ein `PermissionStatus` zurückgegeben, der zur Festlegung der nächsten Schritte verwendet werden kann:

* Unbekannt: Die Berechtigung hat einen unbekannten Status.
* Verweigert: Der Benutzer hat die Berechtigungsanforderung verweigert.
* Deaktiviert: Das Feature ist auf dem Gerät deaktiviert.
* Erteilt: Der Benutzer hat die Berechtigung erteilt, oder sie wird automatisch erteilt.
* Eingeschränkt: eingeschränkter Status.

## <a name="available-permissions"></a>Verfügbare Berechtigungen

Xamarin.Essentials versucht, so viele Berechtigungen wie möglich zu abstrahieren. Jedoch hat jedes Betriebssystem eine andere Gruppe von Laufzeitberechtigungen. Darüber hinaus gibt es Unterschiede, wenn eine einzelne API für einige Berechtigungen bereitgestellt wird. Es folgt eine Übersicht über die derzeit verfügbaren Berechtigungen:

Symbole und ihre Bedeutung:

* ![Vollständige Unterstützung](~/media/shared/yes.png "Vollständige Unterstützung"): unterstützt
* ![Nicht unterstützt](~/media/shared/no.png "Nicht unterstützt oder erforderlich"): nicht unterstützt/erforderlich

| Berechtigung | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---:
| CalendarRead   | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS](~/media/shared/yes.png "Unterstützt watchOS") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| CalendarWrite | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS](~/media/shared/yes.png "Unterstützt watchOS") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Camera | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen](~/media/shared/yes.png "Unterstützt Tizen") |
| ContactsRead | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP](~/media/shared/yes.png "Unterstützt UWP") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| ContactsWrite | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP](~/media/shared/yes.png "Unterstützt UWP") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Taschenlampe | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS nicht](~/media/shared/no.png "Unterstützt iOS nicht") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen](~/media/shared/yes.png "Unterstützt Tizen") |
| LocationWhenInUse | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP](~/media/shared/yes.png "Unterstützt UWP") | ![Unterstützt watchOS](~/media/shared/yes.png "Unterstützt watchOS") | ![Unterstützt tvOS](~/media/shared/yes.png "Unterstützt tvOS")  | ![Unterstützt Tizen](~/media/shared/yes.png "Unterstützt Tizen") |
| LocationAlways | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP](~/media/shared/yes.png "Unterstützt UWP") | ![Unterstützt watchOS](~/media/shared/yes.png "Unterstützt watchOS") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen](~/media/shared/yes.png "Unterstützt Tizen") |
| Medien | ![Unterstützt Android nicht](~/media/shared/no.png "Unterstützt Android nicht") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Mikrofon | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP](~/media/shared/yes.png "Unterstützt UWP") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen](~/media/shared/yes.png "Unterstützt Tizen") |
| Phone | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Fotos | ![Unterstützt Android nicht](~/media/shared/no.png "Unterstützt Android nicht") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS](~/media/shared/yes.png "Unterstützt tvOS") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Erinnerungen | ![Unterstützt Android nicht](~/media/shared/no.png "Unterstützt Android nicht") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS](~/media/shared/yes.png "Unterstützt watchOS") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Sensoren | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP](~/media/shared/yes.png "Unterstützt UWP") | ![Unterstützt watchOS](~/media/shared/yes.png "Unterstützt watchOS") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| SMS | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| Spracheingabe | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS](~/media/shared/yes.png "Unterstützt iOS") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| StorageRead | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS nicht](~/media/shared/no.png "Unterstützt iOS nicht") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |
| StorageWrite | ![Unterstützt Android](~/media/shared/yes.png "Unterstützt Android") | ![Unterstützt iOS nicht](~/media/shared/no.png "Unterstützt iOS nicht") | ![Unterstützt UWP nicht](~/media/shared/no.png "Unterstützt UWP nicht") | ![Unterstützt watchOS nicht](~/media/shared/no.png "Unterstützt watchOS nicht") | ![Unterstützt tvOS nicht](~/media/shared/no.png "Unterstützt tvOS nicht") | ![Unterstützt Tizen nicht](~/media/shared/no.png "Unterstützt Tizen nicht") |

Falls eine Berechtigung als ![nicht unterstützt](~/media/shared/no.png "wird nicht unterstützt") markiert ist, wird stets `Granted` zurückgegeben, wenn sie geprüft oder angefordert wird.

## <a name="general-usage"></a>Allgemeine Verwendung
Es folgt ein allgemeines Nutzungsmuster zum Umgang mit Berechtigungen.

```csharp
public async Task<PermissionStatus> CheckAndRequestLocationPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
    }

    // Additionally could prompt the user to turn on in settings

    return status;
}
```

Von jedem Berechtigungstyp kann eine Instanz so erstellt werden, dass die die Methoden direkt aufgerufen werden können.

```csharp
public async Task GetLocationAsync()
{
    var status = await CheckAndRequestPermissionAsync(new Permissions.LocationWhenInUse());
    if (status != PermissionStatus.Granted)
    {
        // Notify user permission was denied
        return;
    }

    var location = await Geolocation.GetLocationAsync();
}

public async Task<PermissionStatus> CheckAndRequestPermissionAsync<T>(T permission)
            where T : BasePermission
{
    var status = await permission.CheckStatusAsync();
    if (status != PermissionStatus.Granted)
    {
        status = await permission.RequestAsync();
    }

    return status;
}
```

## <a name="extending-permissions"></a>Erweitern von Berechtigungen

Die Permissions-API wurde mit Blick auf Flexibilität und Erweiterbarkeit für Anwendungen erstellt, die eine zusätzliche Überprüfung oder Berechtigungen erfordern, die nicht in Xamarin.Essentials enthalten sind. Erstellen Sie eine neue Klasse, die von `BasePermission` erbt, und implementieren Sie die erforderlichen abstrakten Methoden. Then

```csharp
public class MyPermission : BasePermission
{
    // This method checks if current status of the permission
    public override Task<PermissionStatus> CheckStatusAsync()
    {
        throw new System.NotImplementedException();
    }

    // This method is optional and a PermissionException is often thrown if a permission is not declared
    public override void EnsureDeclared()
    {
        throw new System.NotImplementedException();
    }

    // Requests the user to accept or deny a permission
    public override Task<PermissionStatus> RequestAsync()
    {
        throw new System.NotImplementedException();
    }
}
```

Bei Implementierung einer Berechtigung auf einer bestimmten Plattform kann die `BasePlatformPermission`-Klasse von dieser geerbt werden. Dies ermöglicht zusätzliche Plattformhilfsmethoden zur automatischen Überprüfung der Deklarationen. Dies kann beim Erstellen benutzerdefinierter Berechtigungen zum Vornehmen einer Gruppierung hilfreich sein. Beispielsweise können Sie mit der folgenden benutzerdefinierten Berechtigung Lese- und Schreibzugriff auf Speicher unter Android anfordern.

Erstellen Sie in Ihrem Projekt eine neue Berechtigung, von der Sie Berechtigungen aufrufen.

```csharp
public partial class ReadWriteStoragePermission  : Xamarin.Essentials.Permissions.BasePlatformPermission
{

}
```

Erweitern Sie in Ihrem Android-Projekt die Berechtigung mit den Berechtigungen, die Sie anfordern möchten.

```csharp
public partial class ReadWriteStoragePermission : Xamarin.Essentials.Permissions.BasePlatformPermission
{
    public override (string androidPermission, bool isRuntime)[] RequiredPermissions => new List<(string androidPermission, bool isRuntime)>
    {
        (Android.Manifest.Permission.ReadExternalStorage, true),
        (Android.Manifest.Permission.WriteExternalStorage, true)
    }.ToArray();
}
```

Anschließend können Sie Ihre neue Berechtigung von der freigegebenen Logik abrufen.

```csharp
await Permissions.RequestAsync<ReadWriteStoragePermission>();
```

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

Die Berechtigungen müssen in der Android-Manifestdatei über die entsprechenden Attribute verfügen.

Weitere Informationen finden Sie in der Dokumentation zu [Berechtigungen in Xamarin.Android](https://docs.microsoft.com/xamarin/android/app-fundamentals/permissions).

# <a name="ios"></a>[iOS](#tab/ios)

Berechtigungen müssen in der Datei `Info.plist` über eine übereinstimmende Zeichenfolge verfügen. Nachdem eine Berechtigung angefordert und verweigert wurde, wird kein Popupfenster mehr angezeigt, wenn Sie die Berechtigung ein zweites Mal anfordern. Sie müssen Benutzer auffordern, die Einstellung auf dem Bildschirm mit den Anwendungseinstellungen unter iOS manuell anzupassen.

Weitere Informationen finden Sie in der Dokumentation [Sicherheits- und Datenschutzfunktionen für iOS](https://docs.microsoft.com/xamarin/ios/app-fundamentals/security-privacy).

# <a name="uwp"></a>[UWP](#tab/uwp)

Berechtigungen müssen über übereinstimmende im Paketmanifest deklarierte Funktionen verfügen.

Weitere Informationen finden Sie in der Dokumentation zur [Deklaration von App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

--------------

## <a name="api"></a>API

- [Quellcode für Berechtigungen](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Permissions)
- [Dokumentation zur Permissions-API](xref:Xamarin.Essentials.Permissions)


## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Permissions-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
