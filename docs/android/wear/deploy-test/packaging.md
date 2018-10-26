---
title: Verpacken von Wear-Apps
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 585c276b327a9092bdd13fa633307477017558c5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104400"
---
# <a name="packaging-wear-apps"></a>Verpacken von Wear-Apps

Android Wear-apps werden mit einer vollständigen Android-app für die Verteilung auf Google Play verpackt. 

## <a name="automatic-packaging"></a>Automatische Paketerstellung

Beginnen mit Xamarin Android 5.0, wird Ihre Wear-app automatisch verpackt als Ressource in Ihrer Handheld-app, wenn Sie dem Wear-Projekt einen Projektverweis aus dem Handheld-Projekt erstellen. Sie können die folgenden Schritte verwenden, um diese Zuordnung zu erstellen: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ist Ihre Wear-app nicht bereits Teil Ihrer Handheld-Lösung, mit der rechten Maustaste den Knoten "Projektmappe", und wählen Sie **hinzufügen > vorhandenes Projekt hinzufügen...** .

2. Navigieren Sie zu der **csproj** Datei der Wear-app auszuwählen, und klicken Sie auf **öffnen**. Wear-app-Projekt sollte jetzt in Ihrer Handheld-Lösung angezeigt werden.

3. Mit der rechten Maustaste die **Verweise** Knoten, und wählen **Verweis hinzufügen**.

4. In der **Verweis-Manager** Dialog, aktivieren Sie das Wear-Projekt (klicken Sie mit einem Häkchen versehen hinzufügen), klicken Sie dann auf **OK**.

5. Ändern Sie den Paketnamen für Ihr Projekt Wear so, dass sie den Paketnamen des Handheld-Projekts übereinstimmt (der Paketname kann geändert werden, unter **Eigenschaften > Android-Manifest**).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Ist Ihre Wear-app nicht bereits Teil Ihrer Handheld-Lösung, mit der rechten Maustaste den Knoten "Projektmappe", und wählen Sie **hinzufügen > vorhandenes Projekt hinzufügen...** .

2. Navigieren Sie zu der **csproj** Datei der Wear-app auszuwählen, und klicken Sie auf **öffnen**. Wear-app-Projekt sollte jetzt in Ihrer Handheld-Lösung angezeigt werden.

3. Mit der rechten Maustaste in die Projektmappe, und klicken Sie auf des tragbaren Projektknoten **Verweise bearbeiten...** .

4. In der **Verweise bearbeiten** Dialog, aktivieren Sie das Wear-Projekt (klicken Sie mit einem Häkchen versehen hinzufügen), klicken Sie dann auf **OK**.

5. Ändern Sie den Paketnamen für Ihr Projekt Wear so, dass sie den Paketnamen des Handheld-Projekts übereinstimmt (der Paketname kann geändert werden, unter **Projektoptionen > Android-Anwendung**).

-----


Beachten Sie, die Sie erhalten eine **XA5211** Fehler, wenn der Paketname der Wear-app nicht den Paketnamen der Handheld-app übereinstimmt. Zum Beispiel:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Um diesen Fehler zu beheben, ändern Sie den Paketnamen der Wear-app so, dass sie den Paketnamen der Handheld-app übereinstimmt.

Beim Klicken auf **erstellen > alle erstellen**, diese Zuordnung wird ausgelöst, automatische paketerstellung des Wear-Projekts mit dem Hauptprojekt von Handheld (Telefon). Wear-app wird automatisch erstellt und als Ressource in der Handheld-app enthalten.

Die generierte Assembly mit das Wear-app-Projekt wird nicht als Assemblyverweis im Projekt Handheld (Phone) verwendet. Der Buildprozess führt stattdessen Folgendes aus:

-   Stellt sicher, dass das Paket Übereinstimmung nennt. 

-   Generiert XML-Code, und es die Wear-app Zuordnen der Handheld Projekt hinzugefügt. Zum Beispiel: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Fügt die Wear-app als eine **unformatierten** Ressource zum Handheld-Projekt. 


## <a name="manual-packaging"></a>Manuelle Verpacken

Sie können apps für Android Wear in Xamarin.Android vor Version 5.0 schreiben, aber Sie müssen diese manuelle Verpacken-Anweisungen, um die Verteilung der app befolgen: 

1. Sicherstellen Sie, dass der tragbare Projekt und die Handheld (Phone)-Projekte den gleichen Namen von Version Paket und die Anzahl.

2. Erstellen Sie manuell das tragbare Projekt als eine **Version** erstellen.

3. Manuelles Hinzufügen die Version **. APK** aus Schritt (2) in der **Ressourcen/raw** Verzeichnis des Projekts Handheld (Telefon).

4. Fügen Sie manuell eine neue XML-Ressource **Resources/xml/wearable_app_desc.xml** im portablen Projekt bezieht sich auf tragbare Gerät **APK** aus Schritt (3):

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Manuelles Hinzufügen eine `<meta-data />` Element der Handheld-Projekts **"androidmanifest.xml"** `<application>` -Element, das auf die neue XML-Ressource verweist:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Siehe auch der Android-Entwickler-Website [manuelle Packging Anweisungen](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

