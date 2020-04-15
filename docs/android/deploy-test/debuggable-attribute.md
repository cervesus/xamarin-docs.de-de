---
title: Attribut „Debuggable“
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 12/13/2019
ms.openlocfilehash: 6e54dba0c832596818c5163dc4e14ad1e659e040
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "75487970"
---
# <a name="debuggable-attribute"></a>Attribut „Debuggable“

Um das Debuggen zu ermöglichen, unterstützt Android das Java Debug Wire Protocol (JDWP). Dies ist eine Technologie, die es Tools wie adb ermöglicht, mit einer JVM zu kommunizieren. JDWP ist zwar während der Entwicklung wichtig, sollte aber vor der Veröffentlichung der Anwendung deaktiviert werden.

JDWP kann mit dem Wert des `android:debuggable`-Attributs in einer Android-Anwendung konfiguriert werden. Wählen Sie _eine_ der drei folgenden Methoden zum Festlegen dieses Attributs in Xamarin.Android aus:

## <a name="androidmanifestxml"></a>AndroidManifest.xml

Erstellen oder Öffnen einer`AndroidManifext.xml`-Datei und Festlegen des `android:debuggable`-Attributs in dieser Datei. Achten Sie besonders darauf, Ihren Releasebuild nicht mit aktiviertem Debuggen zu liefern.

## <a name="add-an-application-class-attribute"></a>Hinzufügen eines Anwendungsklassenattributs

Wenn Ihre Xamarin.Android-App über eine Klasse mit einem `[Application]`-Attribut verfügt, aktualisieren Sie das Attribut auf `[Application(Debuggable = true)]`. Um es zu deaktivieren, legen Sie es auf `false` fest.

## <a name="add-an-assembly-attribute"></a>Hinzufügen eines Assemblyattributs

Wenn Ihre Xamarin.Android-App noch NICHT über ein `[Application]`-Klassenattribut verfügt, fügen Sie ein Attribut `[assembly: Application(Debuggable=true)]` auf Assemblyebene in einer C#-Datei hinzu. Um es zu deaktivieren, legen Sie es auf `false` fest.

## <a name="summary"></a>Zusammenfassung

Wenn jeweils `AndroidManifest.xml` und `ApplicationAttribute` vorhanden sind, hat der Inhalt von `AndroidManifest.xml` Priorität vor dem, was durch `ApplicationAttribute` angegeben wird.

Wenn Sie sowohl ein Klassenattribut _als auch_ ein Assemblyattribut hinzufügen, tritt ein Compilerfehler auf:

```error
"Error The "GenerateJavaStubs" task failed unexpectedly.
System.InvalidOperationException: Application cannot have both a type with an [Application] attribute and an [assembly:Application] attribute."
```

Wenn weder `AndroidManifest.xml` noch `ApplicationAttribute` vorhanden sind, hängt der Wert des `android:debuggable`-Attributs standardmäßig davon ab, ob Debugsymbole generiert werden oder nicht. Wenn Debugsymbole vorhanden sind, legt Xamarin.Android das `android:debuggable`-Attribut für Sie auf `true` fest.

> [!WARNING]
> Der Wert des `android:debuggable`-Attributs hängt NICHT zwingend von der Buildkonfiguration ab. Es ist möglich, für Releasebuilds das `android:debuggable`-Attribut auf TRUE festzulegen. Wenn Sie ein Attribut verwenden, um diesen Wert festzulegen, können Sie das Attribut in eine Compileranweisung einschließen:
> 
> ```csharp
> #if DEBUG
> [Application(Debuggable = true)]
> #else
> [Application(Debuggable = false)]
> #endif
> ```

## <a name="related-links"></a>Verwandte Links

- [Debuggable apps in the Android market (Debugfähige Apps im Android Market)](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
