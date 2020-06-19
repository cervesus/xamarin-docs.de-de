---
title: Androidx-Migration inXamarin.Forms
description: In diesem Artikel wird erläutert, warum androidx vorhanden ist und wie Sie in Ihrer APP zu androidx migrieren können Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 98884003-E65A-4EB4-842D-66CFE27344A4
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c2df309a8a12a05a4b492bb66977aa2411142850
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138267"
---
# <a name="androidx-migration-in-xamarinforms"></a>Androidx-Migration inXamarin.Forms

Androidx ersetzt die Android-Unterstützungs Bibliothek. In diesem Artikel wird erläutert, warum androidx vorhanden ist, wie es Auswirkungen Xamarin.Forms hat und wie Sie Ihre Anwendung migrieren, um die androidx-Bibliotheken zu verwenden.

## <a name="history-of-androidx"></a>Verlauf von androidx

Die Android-Unterstützungs Bibliothek wurde erstellt, um neuere Features für ältere Android-Versionen bereitzustellen. Dabei handelt es sich um eine Kompatibilitätsschicht, mit der Entwickler Funktionen verwenden können, die möglicherweise nicht in allen Versionen des Android-Betriebssystems vorhanden sind und für ältere Versionen über ordnungsgemäße Fallbacks verfügen. Die Unterstützungs Bibliothek umfasst auch Hilfs-und Hilfsklassen, Debuggingtools und Hilfsprogramme und ausgereifte Klassen, die von anderen Unterstützungs Bibliotheks Klassen abhängen, die funktionieren.

Obwohl die Unterstützungs Bibliothek ursprünglich eine binäre Binärdatei war, wurde sie zu einer Reihe von Bibliotheken vergrößert und entwickelt, die für die moderne App-Entwicklung fast unverzichtbar sind. Dies sind einige häufig verwendete Features aus der Support Bibliothek:

- Die `Fragment` Support Klasse.
- Der `RecyclerView` zum Verwalten von langen Listen verwendeten.
- Multidex-Unterstützung für apps mit mehr als 65.536 Methoden.
- Die `ActivityCompat`-Klasse.

Androidx ist ein Ersatz für die Unterstützungs Bibliothek, die nicht mehr verwaltet wird. die gesamte neue Bibliotheksentwicklung erfolgt in der androidx-Bibliothek. Androidx ist eine neu gestaltete Bibliothek, die die semantische Versionsverwaltung, klarere Paketnamen und eine bessere Unterstützung für gängige Anwendungsarchitektur Muster verwendet. Androidx Version 1.0.0 ist das binäre Äquivalent zur Support Library-Version 28.0.0. Eine umfassende Liste der Klassen Zuordnungen aus der Unterstützungs Bibliothek zu androidx finden Sie [unter Unterstützung von Bibliotheks Klassen](https://developer.android.com/jetpack/androidx/migrate/class-mappings) Zuordnungen auf Developer.Android.com.

Google hat einen Migrationsprozess mit dem Namen "jetifier" mit "androidx" erstellt. Der jetifier überprüft den JAR-Bytecode während des Buildprozesses und ordnet Verweise auf Bibliotheks Verweise, sowohl im App-Code als auch in Abhängigkeit, auf seine androidx-Entsprechung zu.

In einer- Xamarin.Forms App müssen die JAR-Abhängigkeiten genauso wie in einer Android-Java-App zu androidx migriert werden. Die xamarin-Bindungen müssen jedoch auch migriert werden, um auf die richtigen, zugrunde liegenden JAR-Dateien zu verweisen. Xamarin.FormsUnterstützung für die automatische androidx-Migration in Version 4,5 hinzugefügt.

Weitere Informationen zu "androidx" finden Sie unter " [androidx Overview](https://developer.android.com/jetpack/androidx) " auf Developer.Android.com.

## <a name="automatic-migration-in-xamarinforms"></a>Automatische Migration inXamarin.Forms

Für eine automatische Migration zu androidx muss ein Projekt Folgendes ausführen Xamarin.Forms :

- Android API-Zielversion 29 oder höher.
- Verwenden Sie Xamarin.Forms Version 4,5 oder höher.

Nachdem Sie diese Einstellungen in Ihrem Projekt bestätigt haben, erstellen Sie die Android-App in Visual Studio 2019. Während des Buildprozesses wird die Intermediate Language (IL) überprüft, und die unterstützten Bibliotheksabhängigkeiten und-Bindungen werden mit androidx-Abhängigkeiten ausgetauscht. Wenn Ihre Anwendung alle androidx-Abhängigkeiten aufweist, die zum Erstellen von erforderlich sind, werden Sie feststellen, dass der Buildprozess keine Unterschiede aufweist.

> [!NOTE]
> Sie müssen die Verweise auf die Unterstützungs Bibliothek in Ihrem Projekt beibehalten. Diese werden verwendet, um die Anwendung zu kompilieren, bevor der Migrationsprozess die resultierende IL überprüft und die Abhängigkeiten transformiert.

Wenn androidx-Abhängigkeiten erkannt werden, die nicht Teil des Projekts sind, wird ein Buildfehler gemeldet, der angibt, welche androidx-Pakete fehlen. Im folgenden wird ein Beispiel für einen Buildfehler angezeigt:

```
Could not find 37 AndroidX assemblies, make sure to install the following NuGet packages:
- Xamarin.AndroidX.Lifecycle.LiveData
- Xamarin.AndroidX.Browser
- Xamarin.Google.Android.Material
- Xamarin.AndroidX.Legacy.Supportv4
You can also copy and paste the following snippit into your .csproj file:
 <PackageReference Include="Xamarin.AndroidX.Lifecycle.LiveData" Version="2.1.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Browser" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.Google.Android.Material" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Legacy.Support.V4" Version="1.0.0-rc1" />
```

Die fehlenden nuget-Pakete können entweder über den nuget-Paket-Manager in Visual Studio installiert oder installiert werden, indem Sie Ihre Android. CSPROJ-Datei bearbeiten, um die `PackageReference` im Fehler aufgeführten XML-Elemente einzuschließen.

Nachdem die fehlenden Pakete aufgelöst wurden, lädt das Neuerstellen des Projekts die fehlenden Pakete, und das Projekt wird mithilfe von androidx-Abhängigkeiten anstelle von Unterstützungs Bibliotheksabhängigkeiten kompiliert.

> [!NOTE]
> Wenn Ihr Projekt und die Projekt Abhängigkeiten nicht auf Android-Unterstützungs Bibliotheken verweisen, führt der Migrations Vorgang nichts aus und wird nicht ausgeführt.

## <a name="related-links"></a>Verwandte Links

- [Übersicht über die Android-Support Bibliothek](https://developer.android.com/topic/libraries/support-library/index) auf Developer.Android.com
- [Androidx-Übersicht](https://developer.android.com/jetpack/androidx) auf Developer.Android.com
