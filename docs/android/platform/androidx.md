---
title: AndroidX
description: Erfahren Sie, wie Sie Xamarin.Android verwenden, um Apps mit AndroidX zu entwickeln.
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: ad6ea2f68fc01183f7ed42e85094f6be5fb3d9f9
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "77618911"
---
# <a name="androidx-with-xamarin"></a>AndroidX mit Xamarin

_Erfahren Sie, wie Sie Xamarin.Android verwenden, um Apps mit AndroidX zu entwickeln._

Bei AndroidX handelt es sich um eine wesentliche Verbesserung der ursprünglichen Android-Unterstützungsbibliothek, die nicht mehr gepflegt und verwaltet wird. Mit **AndroidX**-Paketen wird die Android-Unterstützungsbibliothek vollständig ersetzt, indem Featureparität und neue Bibliotheken bereitgestellt werden, die Sie in Ihren Android-Anwendungen nutzen können.

AndroidX beinhaltet die folgenden Features:

- Alle Pakete innerhalb von AndroidX verfügen jetzt über einen einheitlichen Namespace, der mit `androidx` beginnt. Dies bedeutet, dass alle Pakete der Android-Unterstützungsbibliothek einem entsprechenden `androidx.*`-Paket zugeordnet sind.
- `androidx`-Pakete werden separat verwaltet und aktualisiert. AndroidX-Bibliotheken können also unabhängig voneinander aktualisiert werden.
- Nach v28 der Android-Unterstützungsbibliothek gibt es keine weiteren Releases mehr. Die gesamte Entwicklung ist stattdessen in `androidx` enthalten.

![AndroidX-Logo](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>Anforderungen

Um AndroidX-Features in Xamarin-basierten Apps zu verwenden, müssen folgende Voraussetzungen erfüllt sein:

- **Visual Studio** – Führen Sie unter Windows ein Update auf Visual Studio 2019, Version 16.4 oder höher, durch. Führen Sie unter macOS ein Update auf Visual Studio 2019 für Mac, Version 8.4 oder höher, durch.
- **Xamarin.Android** – Mit Visual Studio muss Xamarin.Android 10.0 oder höher installiert werden (Xamarin.Android wird unter Windows automatisch als Teil der **Mobile Entwicklung mit .NET**-Workload und als Teil des **Visual Studio für Mac-Installationsprogramms** installiert).
- **Java Developer Kit** – Für die Entwicklung mit Xamarin.Android 10.0 ist JDK 8 erforderlich. Die Microsoft-Distribution von OpenJDK wird automatisch als Teil von Visual Studio installiert.
- **Android SDK** – Android SDK API 28 oder höher muss über den Android-SDK-Manager installiert werden.

## <a name="get-started"></a>Erste Schritte

Beginnen Sie Ihre Arbeit mit AndroidX, indem Sie ein beliebiges [AndroidX-NuGet-Paket](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22) zu Ihrem Android-Projekt hinzufügen. Weitere Informationen zur Installation und Verwendung von Paketen in [Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio) oder [Visual Studio für Mac](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)

## <a name="behavior-changes"></a>Verhaltensänderungen

Da AndroidX eine überarbeitete Version der Android-Unterstützungsbibliothek ist, müssen Änderungen und Migrationsschritte berücksichtigt werden, die sich auf Android-Anwendungen auswirken, die mit der Android-Unterstützungsbibliothek erstellt wurden.

### <a name="package-name-change"></a>Änderungen bei Paketnamen
Die Paketnamen wurden überarbeitet und geändert. Im Folgenden finden Sie Beispiele für diese Änderungen:

| Alt                    | Neu                    |
| ---------------------- | ---------------------- |
| android.support.**     | androidx.@             |
| android.design.**      | com.google.android.material.@ |
| android.support.test.** | androidx.test.@       |
| android.arch.**        | androidx.@             |
| android.arch.persistence.room.** | androidx.room.@ |
| android.arch.persistence.** | androidx.sqlite.@ |

Weitere Informationen zu Paketnamen finden Sie in dieser [Dokumentation](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings).

## <a name="migration-tooling"></a>Migrationstools

Es gibt drei wichtige Migrationsschritte für Ihre Anwendung, mit denen Sie vertraut sein sollten.

1. Wenn Ihre Anwendung **Namespaces der Android-Unterstützungsbibliothek umfasst und Sie diese Namespaces zu AndroidX-Namespaces migrieren möchten**, können Sie in den meisten Szenarien unser IDE-Tool **Zu AndroidX migrieren** verwenden. 

Aktivieren Sie den **AndroidX-Migrator** in Visual Studio 2019 über die Optionen **Tools > Optionen > Xamarin > Android-Einstellungen** (in Visual Studio für Mac können Sie diesen Schritt überspringen).

![Aktivieren des AndroidX-Migrators](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Zu AndroidX migrieren** aus.

![Migration zu AndroidX](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> In Szenarien, die nicht von diesem Tool abgedeckt werden, müssen Sie manuell einige Änderungen an den Namespaces vornehmen. Wenngleich wir Pakete korrekt zuordnen, sollten Sie sich für die Migration Ihrer Projekte die offiziellen [Artefaktzuordnungen](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings) und [Klassenzuordnungen](https://developer.android.com/jetpack/androidx/migrate/class-mappings) ansehen.

2. Wenn Ihre Anwendung **Abhängigkeiten umfasst, die nicht zum AndroidX-Namespace migriert wurden**, müssen Sie das Paket für die [Migration der Android-Unterstützungsbibliothek zu AndroidX](https://www.nuget.org/packages/Xamarin.AndroidX.Migration) verwenden.
3. Wenn Ihre Anwendung **keine Abhängigkeiten umfasst, die eine Migration zu AndroidX-Namespaces erfordern**, können Sie sofort die [AndroidX-Bibliotheken in NuGet](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22) verwenden.

## <a name="troubleshooting"></a>Problembehandlung

- Bestimmte Architekturpakete in AndroidX führen zu Konflikten mit der Unterstützungsbibliothek. Um dieses Problem zu beheben, sollten Sie die AndroidX-Version dieser Pakete verwenden und die Unterstützungsbibliothek entfernen. Wenn Sie in Ihrem Projekt z.B. auf `Xamarin.Android.Arch.Work.Runtime` verweisen, führt dies zu einem Konflikt mit den Typen des neu hinzugefügten `AndroidX.Work`-Pakets.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde AndroidX vorgestellt und erläutert, wie die neuesten Tools und Pakete für die Xamarin.Android-Entwicklung mit AndroidX installiert und konfiguriert werden. Sie haben erfahren, was genau AndroidX ist. Darüber hinaus enthält dieser Artikel Links zur API-Dokumentation und zu relevanten Themen für Android-Entwickler, die Ihnen den Einstieg in die App-Entwicklung mit AndroidX erleichtern. Außerdem wurden die wichtigsten Unterschiede von AndroidX im Vergleich zur Unterstützungsbibliothek beschrieben, und es wurde auf Probleme hingewiesen, die sich auf vorhandene Apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Einführung in AndroidX | The Xamarin Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Xamarin/AndroidX im GitHub-Repository](https://github.com/xamarin/AndroidX)
- [XamarinAndroidXMigration im GitHub-Repository](https://github.com/xamarin/XamarinAndroidXMigration)
