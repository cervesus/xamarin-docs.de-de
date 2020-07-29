---
title: Komponententests bei Xamarin.iOS-Apps
description: Dieses Dokument enthält eine Übersicht über das Durchführen von Komponententests für eine Xamarin.iOS-App. Es wird beschrieben, wie Sie ein Komponententestprojekt erstellen und Tests schreiben und ausführen.
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: f5796ee17e947494d1e22f750bc43ff823d56d55
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937279"
---
# <a name="unit-testing-xamarinios-apps"></a>Komponententests bei Xamarin.iOS-Apps

Dieses Dokument beschreibt, wie Sie Komponententests für Ihre Xamarin.iOS-Projekte erstellen.
Komponententests mit Xamarin.iOS werden mithilfe des Touch.Unit-Frameworks durchgeführt, das sowohl einen iOS Test Runner als auch eine geänderte Version von NUnit mit dem Namen [Touch.Unit](https://github.com/xamarin/Touch.Unit) enthält, die eine Reihe vertrauter APIs zum Schreiben von Komponententests bietet.

## <a name="setting-up-a-test-project-in-visual-studio-for-mac"></a>Einrichten eines Testprojekts in Visual Studio für Mac

Sie müssen Ihrer Projektmappe ein Projekt des Typs **iOS-Komponententestprojekt** hinzufügen, um ein Komponententestframework für Ihr Projekt einzurichten. Klicken Sie dafür mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Hinzufügen > Neues Projekt hinzufügen** aus. Wählen Sie aus der Liste **iOS > Tests > Unified API > iOS-Komponententestprojekt** aus. Sie können entweder C# oder F# auswählen.

![Auswählen von C# oder F#](touch.unit-images/00.png)

Dadurch wird ein einfaches Projekt erstellt, dass ein grundlegendes Runnerprogramm enthält und das auf die neue Assembly „MonoTouch.NUnitLite“ verweist. Es wird folgendermaßen aussehen:

![Das Projekt im Projektmappen-Explorer](touch.unit-images/01.png)

Die Klasse `AppDelegate.cs` enthält den Test Runner und sieht folgendermaßen aus:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TouchRunner runner;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        runner = new TouchRunner (window);

        // register every tests included in the main application/assembly
        runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

        window.RootViewController = new UINavigationController (runner.GetViewController ());

        // make the window visible
        window.MakeKeyAndVisible ();

        return true;
    }
}
```

> [!NOTE]
> Der iOS-Projekttyp „Komponententest“ ist in Visual Studio 2019 oder in Visual Studio 2017 unter Windows nicht verfügbar.

## <a name="writing-some-tests"></a>Schreiben einiger Tests

Nun da die grundlegende Shell vorhanden ist, sollten Sie Ihre erste Reihe von Tests schreiben.

Tests werden geschrieben, indem Klassen erstellt werden, auf die das Attribut `[TestFixture]` angewendet wird. Sie sollten innerhalb jeder TestFixture-Klasse das Attribut `[Test]` für jede Methode anwenden, die den Test Runner aufrufen soll. Die tatsächlichen Prüfvorrichtungen können in jeder Datei in Ihrem Testprojekt vorhanden sein.

Klicken Sie für einen schnellen Start auf **Hinzufügen/Neue Datei hinzufügen**, und wählen Sie in der Xamarin.iOS-Gruppe **UnitTests** aus. Dadurch wird eine Skelettdatei hinzugefügt, die einen bestandenen, einen fehlgeschlagenen und einen ignorierten Test enthält. Dies sieht folgendermaßen aus:

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

    [TestFixture]
    public class Tests {

        [Test]
        public void Pass ()
        {
                Assert.True (true);
        }

        [Test]
        public void Fail ()
        {
                Assert.False (true);
        }

        [Test]
        [Ignore ("another time")]
        public void Ignore ()
        {
                Assert.True (false);
        }
    }
}
```

## <a name="running-your-tests"></a>Ausführen Ihrer Tests

Klicken Sie in Ihrer Projektmappe mit der rechten Maustaste auf das Projekt, um es auszuführen, und wählen Sie **Debug Item** (Element debuggen) oder **Element ausführen** aus.

Mit dem Test Runner können Sie sehen, welche Tests registriert sind, und Sie können einzeln auswählen, welche Tests ausgeführt werden können.

[![Die Liste der registrierten Tests](touch.unit-images/02-sml.png)](touch.unit-images/02.png#lightbox) 
[![Individueller Text](touch.unit-images/03-sml.png)](touch.unit-images/03.png#lightbox) 

[![Ergebnisse der Ausführung](touch.unit-images/04-sml.png)](touch.unit-images/04.png#lightbox)

Sie können einzelne Prüfvorrichtungen ausführen, indem Sie sie aus den geschachtelten Ansichten auswählen, oder Sie können mit „Alles ausführen“ alle Ihre Tests ausführen. Wenn Sie den Standardtest ausführen, sollte dieser je einen Test enthalten, der je einen Test mit den Kriterien "bestanden", "fehlgeschlagen" und "ignoriert" enthalten soll. Die Berichte sehen folgendermaßen aus, und Sie können direkt einen Drilldown zu den fehlgeschlagenen Tests ausführen und mehr über den Fehler herausfinden:

[![Beispielbericht](touch.unit-images/05-sml.png)](touch.unit-images/05.png#lightbox) [![Beispielbericht](touch.unit-images/06-sml.png)](touch.unit-images/06.png#lightbox) [![Beispielbericht](touch.unit-images/07-sml.png)](touch.unit-images/07.png#lightbox)

Im Anwendungsausgabefenster in Ihrer IDE können Sie auch die aktuell ausgeführten Tests und deren gegenwärtigen Status sehen.

## <a name="writing-new-tests"></a>Schreiben neuer Tests

NUnitLite ist eine geänderte Version von NUnit mit dem Namen [Touch.Unit](https://github.com/xamarin/Touch.Unit)-Projekt. Es ist ein einfaches Testframework für .NET, das auf den Ideen von [NUnit](https://nunit.com/) basiert und einen Teil seiner Funktionen bietet.
Es zeichnet sich durch minimalen Ressourcenverbrauch aus und kann so auch auf Plattformen mit eingeschränkten Ressourcen ausgeführt werden, die z.B. für die Entwicklung eingebetteter oder mobiler Lösungen verwendet werden. Die NUnitLite-API steht Ihnen in Xamarin.iOS zur Verfügung. Mit dem durch die Komponententestvorlage bereitgestellten grundlegenden Skelett, sind die Methoden [Assert class](xref:NUnit.Framework.Assert) Ihr Haupteinstiegspunkt.

Zusätzlich zu den Methoden der Assert-Klasse sind die Funktionen der Komponententests in die folgenden Namespaces aufgeteilt, die Teil von NUnitLite sind:

- [NUnit.Framework](xref:NUnit.Framework)
- [NUnit.Constraints](xref:NUnit.Framework.Constraints)
- [NUniteLite.Runner](xref:NUnitLite.Runner)
