---
title: Verpacken von Abnutzung-Apps
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: af96c0f8cf862b7a208beb5b91ecbb30598b09d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767712"
---
# <a name="packaging-wear-apps"></a>Verpacken von Abnutzung-Apps

Android Abnutzung-apps werden mit einer vollständigen Android-app für die Verteilung auf Google Play verpackt. 

## <a name="automatic-packaging"></a>Automatische Paketerstellung

Beginnend mit Xamarin Android 5.0, wird Ihre app Abnutzung automatisch verpackt, als Ressource in Ihrer app Handheld Wenn Sie einen Projektverweis auf das Projekt Abnutzung aus dem Handheld-Projekt erstellen. Sie können die folgenden Schritte verwenden, um diese Zuordnung zu erstellen: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Wenn Ihre app Abnutzung nicht Teil der Handheld-Projektmappe ist, mit der rechten Maustaste des Knotens Projektmappe, und wählen Sie **hinzufügen > vorhandenes Projekt hinzufügen...** .

2. Navigieren Sie zu der **csproj** Datei Ihrer App Abnutzung auszuwählen, und klicken Sie auf **öffnen**. Abnutzung-app-Projekt sollten nun in der Projektmappe Handheld sichtbar sein.

3. Mit der rechten Maustaste die **Verweise** Knoten, und wählen **Verweis hinzufügen**.

4. In der **Verweis-Manager** Dialog, Abnutzung Projekt (klicken Sie zum Hinzufügen ein Häkchen), klicken Sie dann auf, aktivieren **OK**.

5. Der Paketname für das Projekt Abnutzung zu ändern, damit diese den Paketnamen des Handheld-Projekts übereinstimmt (der Paketname kann geändert werden, unter **Eigenschaften > Android-Manifest**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Wenn Ihre app Abnutzung nicht Teil der Handheld-Projektmappe ist, mit der rechten Maustaste des Knotens Projektmappe, und wählen Sie **hinzufügen > vorhandenes Projekt hinzufügen...** .

2. Navigieren Sie zu der **csproj** Datei Ihrer App Abnutzung auszuwählen, und klicken Sie auf **öffnen**. Abnutzung-app-Projekt sollten nun in der Projektmappe Handheld sichtbar sein.

3. Mit der rechten Maustaste in der Projektmappe und klicken Sie auf des Handheld Projektknoten **Verweise bearbeiten...** .

4. In der **Verweise bearbeiten** Dialog, Abnutzung Projekt (klicken Sie zum Hinzufügen ein Häkchen), klicken Sie dann auf, aktivieren **OK**.

5. Der Paketname für das Projekt Abnutzung zu ändern, damit diese den Paketnamen des Handheld-Projekts übereinstimmt (der Paketname kann geändert werden, unter **Projektoptionen > Android-Anwendung**).

-----


Beachten Sie, die Sie erhalten eine **XA5211** Fehler, wenn der Paketname der app Abnutzung der Paketname der Handheld-app nicht übereinstimmt. Zum Beispiel:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Um diesen Fehler zu beheben, Ändern der Paketname der Abnutzung app, damit diese den Paketnamen der Handheld app übereinstimmt.

Beim Klicken auf **erstellen > Build All**, diese Zuordnung wird ausgelöst, automatische paketerstellung des Projekts Abnutzung in das Hauptprojekt Handheld (Phone). Die Abnutzung-app wird automatisch erstellt und als Ressource in der Handheld-app enthalten.

Die Assembly, die das Abnutzung-app-Projekt generiert wird nicht als Assemblyverweis im Projekt Handheld (Phone) verwendet. Stattdessen führt während des Erstellungsprozesses Folgendes aus:

-   Stellt sicher, dass das Paket Übereinstimmung benennt. 

-   Generiert XML-Code, und fügt es der Handheld Projekt, um es der app Abnutzung zuordnen hinzu. Zum Beispiel: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Fügt die Abnutzung app als eine **unformatierten** Ressource Handheld-Projekt. 


## <a name="manual-packaging"></a>Manuelles Verpacken

Sie können apps für Android Dach in Xamarin.Android schreiben, vor Version 5.0, aber Sie müssen diese manuelle Packaging-Anweisungen, um die app zu verteilen befolgen: 

1. Stellen Sie sicher, dass Wearable Projekt und Handheld (Phone)-Projekte haben die gleiche Version und Paket ein.

2. Erstellen Sie manuell das Wearable Projekt als eine **Version** erstellen.

3. Manuelles Hinzufügen die Version **. APK** aus Schritt (2) in der **Ressourcen/raw** Verzeichnis des Projekts Handheld (Phone).

4. Fügen Sie manuell eine neue XML-Ressource **Resources/xml/wearable_app_desc.xml** im Handheld-Projekt bezieht sich auf tragbar **APK** aus Schritt (3):

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Manuell hinzufügen einer `<meta-data />` Element Handheld-Projekt **AndroidManifest.xml** `<application>` Element, das auf die neue XML-Ressource verweist:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Siehe auch die Android-Entwickler-Website [manuelle Packging Anweisungen](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

