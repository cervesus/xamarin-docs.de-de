---
title: Fehler bei fehlenden Paketen nach dem Aktualisieren von nuget-Paketen
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: a1ed4a2be63973e9e26dcb06163a3f9fd7eae649
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728095"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Fehler bei fehlenden Paketen nach dem Aktualisieren von nuget-Paketen

Dieses Problem wurde hauptsächlich bei xamarin. Forms-Beispiel-App-Lösungen gemeldet, aber das Potenzial dieses Problems kann für jedes Projekt auftreten, das nuget-Pakete verwendet.

Wenn nach dem Aktualisieren von nuget-Paketen im Projekt oder in der Projekt Mappe eine Fehlermeldung angezeigt wird, die auf die alten Paket Versionsnummern verweist, z. b.:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

In diesem Beispiel ist *xamarin. Forms. 1.3.1.6296* die alte Versionsnummer, die mit dem nuget-Paket Update entfernt wurde.

Dies kann vorkommen, wenn die XML-Elemente in der CSPROJ-Datei, die auf die alte Paket Versionsnummer verweisen, manuell hinzugefügt oder bearbeitet wurden. nuget entfernt oder aktualisiert sie nicht, wenn Sie manuell hinzugefügt oder bearbeitet wurden. das Projekt sucht nun nach Paketen, die gelöscht wurden.

Um dieses Problem zu beheben, bearbeiten Sie die CSPROJ-Datei (en) manuell, und löschen Sie alle Elemente, die auf die alte Versionsnummer verweisen.

Zu entfern gende Beispiel Elemente (wenn Sie die alte Paket Versionsnummer aufweisen):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```
