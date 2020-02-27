---
title: Androidx
description: Erfahren Sie mehr über das Entwickeln von apps mit androidx mithilfe von xamarin. Android.
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: ad6ea2f68fc01183f7ed42e85094f6be5fb3d9f9
ms.sourcegitcommit: 2836f2003a5b745b042ee6003a3d6a11b9139e44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618911"
---
# <a name="androidx-with-xamarin"></a>Androidx mit xamarin

_Erfahren Sie mehr über das Entwickeln von apps mit androidx mithilfe von xamarin. Android._

Androidx ist eine wesentliche Verbesserung der ursprünglichen Android-Unterstützungs Bibliothek, die nicht mehr verwaltet wird. **Androidx** -Pakete ersetzen die Android-Unterstützungs Bibliothek vollständig durch Bereitstellen von Featureparität und neuen Bibliotheken, die Sie in ihren Android-Anwendungen verwenden können.

Androidx umfasst die folgenden Features:

- Alle Pakete innerhalb von androidx verfügen jetzt über einen konsistenten Namespace, der mit `androidx`beginnt. Dies bedeutet, dass alle Pakete der Android-Unterstützungs Bibliothek einem entsprechenden `androidx.*` Paket zugeordnet sind.
- `androidx` Pakete werden separat verwaltet und aktualisiert. Dies bedeutet, dass Sie androidx-Bibliotheken unabhängig voneinander aktualisieren können.
- Ab V28 der Android-Unterstützungs Bibliothek gibt es keine weiteren Versionen mehr. Die gesamte Entwicklung wird stattdessen in `androidx` eingeschlossen.

![Androidx-Logo](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>Requirements (Anforderungen)

Die folgende Liste ist erforderlich, um androidx-Features in xamarin-basierten apps zu verwenden:

- **Visual Studio** : Windows Update auf Visual Studio 2019, Version 16,4 oder höher. Aktualisieren Sie unter macOS auf Visual Studio 2019 für Mac, Version 8,4 oder höher.
- **Xamarin. Android** -xamarin. Android 10,0 oder höher muss mit Visual Studio installiert werden (xamarin. Android wird automatisch als Teil der Mobile- **Entwicklung mit .net** -Arbeitsauslastung unter Windows installiert und als Teil des **Visual Studio für Mac Installer**installiert).
- **Java Developer Kit** : die Entwicklung von xamarin. Android 10,0 erfordert JDK 8. Die Microsoft-Distribution von openjdk wird automatisch als Teil von Visual Studio installiert.
- **Android SDK** Android SDK API 28 oder höher muss über den Android SDK Manager installiert werden.

## <a name="get-started"></a>Erste Schritte

Sie können mit androidx beginnen, indem Sie ein beliebiges [androidx-nuget-Paket](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22) in Ihr Android-Projekt einschließen. Weitere Informationen zum Installieren und Verwenden eines Pakets in [Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio) oder [Visual Studio für Mac](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)

## <a name="behavior-changes"></a>Verhaltensänderungen

Da androidx eine Umgestaltung der Android-Unterstützungs Bibliothek ist, umfasst es Migrations Schritte, die sich auf Android-Anwendungen auswirken, die mit der Android-Unterstützungs Bibliothek erstellt wurden.

### <a name="package-name-change"></a>Änderung des Paket namens
Die Paketnamen wurden zwischen dem alten und dem neuen Paket geändert. Im folgenden finden Sie ein Beispiel für diese Änderungen:

| Alt                    | Neu                    |
| ---------------------- | ---------------------- |
| Android. Support. * *     | androidx. @             |
| Android. Design. * *      | com. Google. Android. Material. @ |
| Android. Support. Test. * * | androidx. Test. @       |
| Android. Arch. * *        | androidx. @             |
| Android. Arch. Persistenz. room. * * | androidx. room. @ |
| Android. Arch. Persistenz. * * | androidx. sqlite. @ |

Weitere Informationen zur Benennung von Paketen finden Sie in [der folgenden Dokumentation](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings).

## <a name="migration-tooling"></a>Migrationstools

Es gibt drei Migrations Schritte, die Sie für Ihre Anwendung kennen sollten.

1. Wenn Ihre Anwendung **Android-Unterstützungs Bibliotheks-Namespaces enthält und Sie Sie zu androidx-Namespaces migrieren**möchten, können Sie die meisten Namespace Szenarien mithilfe unserer **Migrieren zu androidx** -IDE-Tools übernehmen. 

Aktivieren Sie den **androidx-migrierer** über **Tools > Optionen > xamarin > Android-Einstellungen** in Visual Studio 2019 (Sie können diesen Schritt auf Visual Studio für Mac überspringen).

![Androidx-migrierer aktivieren](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

Klicken Sie mit der rechten Maustaste auf das Projekt **, und migrieren Sie zu androidx**

![Migrieren zu androidx](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> Sie müssen einige manuelle Namespace Änderungen für Szenarios vornehmen, die vom Tool nicht abgedeckt werden. Während wir das richtige Paket für Sie zuordnen, wird empfohlen, dass Sie sich die offiziellen [artefaktzuordnungen](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings) und [Klassen](https://developer.android.com/jetpack/androidx/migrate/class-mappings) Zuordnungen ansehen, um Ihre Projekt Migration zu unterstützen.

2. Wenn Ihre Anwendung Abhängigkeiten enthält, die **nicht zum androidx-Namespace migriert**wurden, müssen Sie die [Android-Unterstützungs Bibliothek für das androidx-Migrationspaket verwenden.](https://www.nuget.org/packages/Xamarin.AndroidX.Migration)
3. Wenn Ihre Anwendung keine **Abhängigkeiten enthält, die eine androidx-Namespace Migration erfordern**, können Sie die [androidx-Bibliotheken auf nuget noch heute](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)verwenden.

## <a name="troubleshooting"></a>Problembehandlung

- Bestimmte Architektur Pakete in androidx verursachen einen Konflikt mit den Support Bibliotheksversionen. Um dieses Problem zu beheben, verwenden Sie die androidx-Version dieser Pakete, und entfernen Sie die Version der Unterstützungs Bibliothek. Wenn Sie z. b. auf `Xamarin.Android.Arch.Work.Runtime` im Projekt verweisen, tritt ein Konflikt mit den Typen des neu hinzugefügten `AndroidX.Work` Pakets auf.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde androidx vorgestellt und erläutert, wie die neuesten Tools und Pakete für die xamarin. Android-Entwicklung mit androidx installiert und konfiguriert werden. Es wurde eine Übersicht über das, was androidx ist, bereitgestellt. Es enthält Links zu API-Dokumentation und Android-Entwickler Themen, die Ihnen den Einstieg in die Erstellung von apps mit androidx erleichtern. Außerdem wurden die wichtigsten androidx-Verhaltensänderungen und Problem Behandlungs Themen hervorgehoben, die sich auf vorhandene apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Einführung in androidx | Die xamarin-Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [Androidx](https://developer.android.com/jetpack/androidx)
- [GitHub-Repository "xamarin androidx"](https://github.com/xamarin/AndroidX)
- [Xamarin androidx-Migrations GitHub-Repository](https://github.com/xamarin/XamarinAndroidXMigration)
