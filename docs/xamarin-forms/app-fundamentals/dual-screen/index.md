---
title: Xamarin.Forms-Features für Dual-Screen-Geräte
description: In diesem Leitfaden wird beschrieben, wie Sie Xamarin.Forms-Apps für Dual-Screen-Geräte erstellen.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 737cb819cfd762e81536fba03f3ae5b563416a4e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930740"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms-Features für Dual-Screen-Geräte

![Vorabrelease der API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

Dual-Screen-Geräte wie das Microsoft Surface Duo bieten neue Möglichkeiten für die Benutzeroberfläche Ihrer Anwendungen. Xamarin.Forms enthält die Klassen `TwoPaneView` und `DualScreenInfo`, damit Sie Apps für Dual-Screen-Geräte entwickeln können.

## <a name="get-started"></a>Erste Schritte

Führen Sie die folgenden Schritte aus, um Funktionen für Dual-Screen-Geräte zu einer Xamarin.Forms-App hinzuzufügen:

1. Öffnen Sie das Dialogfeld **NuGet-Paket-Manager** für Ihre Projektmappe.
2. Suchen Sie auf der Registerkarte **Durchsuchen** nach `Xamarin.Forms.DualScreen`.
3. Installieren Sie das Paket `Xamarin.Forms.DualScreen` in Ihrer Projektmappe.
4. Fügen Sie den folgenden Methodenaufruf zur Initialisierung zur `MainActivity`-Klasse im `OnCreate`-Ereignis des Android-Projekts hinzu:

    ```csharp
    Xamarin.Forms.DualScreen.DualScreenService.Init(this);
    ```

    Diese Methode ist erforderlich, damit die App Änderungen am Zustand der App ermitteln kann, z. B. wenn sie auf Dual-Screen-Geräten angezeigt wird.

5. Ändern Sie das `Activity`-Attribut der `MainActivity`-Klasse des Android-Projekts, sodass _alle_ dieser `ConfigurationChanges`-Optionen enthalten sind:

    ```@csharp
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation 
        | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize | ConfigChanges.UiMode
    ```

    Diese Werte sind erforderlich, damit Konfigurationsänderungen und der Anzeigezustand zuverlässiger erkannt wird. Standardmäßig werden nur zwei Optionen zu Xamarin.Forms-Projekten hinzugefügt, vergessen Sie also nicht, die restlichen hinzuzufügen, um die zuverlässige Unterstützung von Dual-Screen-Geräte zu gewährleisten.

## <a name="troubleshooting"></a>Problembehandlung

Wenn die `DualScreenInfo`-Klasse oder das `TwoPaneView`-Layout nicht erwartungsgemäß funktionieren, überprüfen Sie die Anweisungen auf dieser Seite nochmal. Das Auslassen oder fehlerhafte Konfigurieren der `Init`-Methode oder der `ConfigurationChanges`-Attributwerte sind häufig die Ursache für Fehler.

Weitere Anweisungen und eine Referenzimplementierung finden Sie unter [Xamarin.Forms-Beispiele für Dual-Screen-Geräte](https://docs.microsoft.com/dual-screen/xamarin/samples).

## <a name="next-steps"></a>Nächste Schritte

Sobald Sie NuGet hinzugefügt haben, fügen Sie mithilfe der folgenden Artikel Features für Dual-Screen-Geräte zu Ihrer App hinzu:

- [Entwurfsmuster für Dual-Screen-Geräte:](design-patterns.md) Wenn Sie überlegen, wie Sie die Möglichkeiten von zwei Bildschirmen auf Dual-Screen-Geräten am besten nutzen, können Sie den Artikel zu Entwurfsmustern lesen. Dort finden Sie Informationen zur besten Lösung für die Benutzeroberfläche Ihrer Anwendung.
- [TwoPaneView-Layout:](twopaneview.md) Mit der `TwoPaneView`-Klasse von Xamarin.Forms, die vom gleichnamigen UWP-Steuerelement inspiriert ist, wird ein plattformübergreifendes Layout eingeführt, das für Dual-Screen-Geräte optimiert ist.
- [DualScreenInfo-Hilfsklasse:](dual-screen-info.md) Mithilfe der `DualScreenInfo`-Klasse können Sie unter anderem bestimmen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist.
- [Trigger für Dual-Screen-Geräte:](triggers.md) Der [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen)-Namespace enthält zwei Zustandstrigger, die eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung auslösen, wenn sich der Ansichtsmodus des angefügten Layouts oder Fensters ändert.

Weitere Informationen finden Sie in der [Entwicklerdokumentation für Dual-Screen-Geräte](https://docs.microsoft.com/dual-screen/).
