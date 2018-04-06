---
title: Unittests
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3129302cbb2fbe9e2757986317da0ec30601b492
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="unit-testing"></a>Unittests

Dieses Dokument beschreibt, wie Sie Komponententests für Ihre Xamarin.iOS-Projekte erstellen.
Komponententests mit Xamarin.iOS werden mithilfe des Touch.Unit-Frameworks durchgeführt, das sowohl einen iOS Test Runner als auch eine geänderte Version von NUnit mit dem Namen [Touch.Unit](https://github.com/xamarin/Touch.Unit) enthält, die eine Reihe vertrauter APIs zum Schreiben von Komponententests bietet.

## <a name="setting-up-a-test-project"></a>Einrichten eines Testprojekts

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Sie müssen Ihrer Projektmappe ein Projekt des Typs **iOS-Komponententestprojekt** hinzufügen, um ein Komponententestframework für Ihr Projekt einzurichten. Klicken Sie dafür mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Hinzufügen > Neues Projekt hinzufügen** aus. Wählen Sie aus der Liste **iOS > Tests > Unified API > iOS-Komponententestprojekt** aus. Sie können entweder C# oder F# auswählen.

![](touch.unit-images/00.png "Auswählen von C# oder F#")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie müssen Ihrer Projektmappe ein Projekt des Typs **iOS-Komponententestprojekt** hinzufügen, um ein Komponententestframework für Ihr Projekt einzurichten. Klicken Sie dafür mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Hinzufügen > Neues Projekt...** aus. Wählen Sie aus der Liste **Visual C# > iOS > Komponententest-App (iOS)** aus.

![](touch.unit-images/00a.png "iOS-Komponententest-App")

-----

Dadurch wird ein einfaches Projekt erstellt, dass ein grundlegendes Runnerprogramm enthält und das auf die neue Assembly „MonoTouch.NUnitLite“ verweist. Es wird folgendermaßen aussehen:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](touch.unit-images/01.png "Das Projekt im Projektmappen-Explorer")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](touch.unit-images/01a.png "Das Projekt im Projektmappen-Explorer")

-----

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

[![](touch.unit-images/02.png "Die Liste der registrierten Tests")](touch.unit-images/02.png#lightbox) 

[![](touch.unit-images/03.png "Ein einzelner Test")](touch.unit-images/03.png#lightbox) 

[![](touch.unit-images/04.png "Ergebnisse des Testlaufs")](touch.unit-images/04.png#lightbox)

Sie können einzelne Prüfvorrichtungen ausführen, indem Sie sie aus den geschachtelten Ansichten auswählen, oder Sie können mit „Alles ausführen“ alle Ihre Tests ausführen. Wenn Sie den Standardtest ausführen, sollte dieser je einen Test enthalten, der je einen Test mit den Kriterien "bestanden", "fehlgeschlagen" und "ignoriert" enthalten soll. Die Berichte sehen folgendermaßen aus, und Sie können direkt einen Drilldown zu den fehlgeschlagenen Tests ausführen und mehr über den Fehler herausfinden:

[![](touch.unit-images/05.png "Ein Beispielbericht")](touch.unit-images/05.png#lightbox) [![](touch.unit-images/05.png "A sample report")](touch.unit-images/05.png#lightbox) [![](touch.unit-images/05.png "A sample report")](touch.unit-images/05.png#lightbox)

Im Anwendungsausgabefenster in Ihrer IDE können Sie auch die aktuell ausgeführten Tests und deren gegenwärtigen Status sehen.

## <a name="writing-new-tests"></a>Schreiben neuer Tests

NUnitLite ist eine geänderte Version von NUnit mit dem Namen [Touch.Unit](https://github.com/xamarin/Touch.Unit)-Projekt. Es ist ein einfaches Testframework für .NET, das auf den Ideen von [NUnit](http://nunit.com/) basiert und einen Teil seiner Funktionen bietet.
Es zeichnet sich durch minimalen Ressourcenverbrauch aus und kann so auch auf Plattformen mit eingeschränkten Ressourcen ausgeführt werden, die z.B. für die Entwicklung eingebetteter oder mobiler Lösungen verwendet werden. Sie können [die NUnitLite-API durchsuchen](https://developer.xamarin.com/api/namespace/NUnitLite/), die Ihnen in Xamarin.iOS zur Verfügung steht. Mit dem durch die Komponententestvorlage bereitgestellten grundlegenden Skelett, sind die Methoden [Assert class](https://developer.xamarin.com/api/type/NUnit.Framework.Assert/) Ihr Haupteinstiegspunkt.

Zusätzlich zu den Methoden der Assert-Klasse sind die Funktionen der Komponententests in die folgenden Namespaces aufgeteilt, die Teil von NUnitLite sind:

-   [NUnit.Framework](https://developer.xamarin.com/api/namespace/NUnit.Framework/)
-   [NUnit.Constraints](https://developer.xamarin.com/api/namespace/NUnit.Framework.Constraints/)
-   [NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/)
-   [NUniteLite.Runner](https://developer.xamarin.com/api/namespace/NUnitLite.Runner/)


Der für Xamarin.iOS spezifische Komponententestrunner ist hier dokumentiert:

-   [NUnit.UI.TouchRunner](https://developer.xamarin.com/api/type/NUnit.UI.TouchRunner/)
