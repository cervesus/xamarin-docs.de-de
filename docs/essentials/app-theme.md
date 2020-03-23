---
title: 'Xamarin.Essentials: App-Design'
description: In diesem Dokument wird die RequestedTheme-API in Xamarin.Essentials beschrieben, die Informationen dazu enthält, welcher Designstil für die ausgeführte App angefordert wird.
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: e31cae6ff639dbe261599a7cf78ae31fc09318b3
ms.sourcegitcommit: c83b55f60ece20e9163b3e587130250fdf113a16
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2020
ms.locfileid: "79190313"
---
# <a name="xamarinessentials-app-theme"></a>Xamarin.Essentials: App-Design

Die **RequestedTheme**-API ist Teil der [`AppInfo`](app-information.md)-Klasse und liefert Informationen darüber, welches Design vom System für Ihre ausgeführte Anwendung angefordert wird.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-requestedtheme"></a>Verwenden von RequestedTheme

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-theme-information"></a>Abrufen der Designinformationen

Das angeforderte Anwendungsdesign kann mithilfe der folgenden API erkannt werden:

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

Dadurch wird das aktuell angeforderte Design vom System für Ihre Anwendung bereitgestellt. Der Rückgabewert ist einer der folgenden:

* Nicht angegeben.
* Hell
* Dunkel

„Nicht angegeben“ wird zurückgegeben, wenn das Betriebssystem nicht über einen bestimmten Benutzeroberflächenstil verfügt, der angefordert werden kann. Ein Beispiel hierfür sind Geräte, auf denen Versionen von iOS vor 13.0 ausgeführt werden.


## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

Android verwendet Konfigurationsmodi, um den Typ des Designs festzulegen, der vom Benutzer angefordert werden soll. Je nach Android-Version kann er vom Benutzer geändert werden oder wird geändert, wenn der Stromsparmodus aktiviert ist.

Weitere Informationen finden Sie in der offiziellen [Android-Dokumentation zum dunklen Design](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).


# <a name="ios"></a>[iOS](#tab/ios)

Bei iOS-Versionen vor 13.0 wird stets „Nicht angegeben“ zurückgegeben. 


# <a name="uwp"></a>[UWP](#tab/uwp)

Standardmäßig wird Ihre App mit dem vom Benutzer in den Windows-Einstellungen festgelegten Design ausgeführt (**Einstellungen > Personalisierung > Farben > Standard-App-Modus auswählen**). Sie können die RequestedTheme-Eigenschaft der App so festlegen, dass der Benutzerstandard überschrieben wird, und angeben, welches Design verwendet wird.

Weitere In finden Sie in der [UWP-Dokumentation zum angeforderten Design](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme).

--------------

## <a name="api"></a>API

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)
