---
title: Erste Schritte mit iOS
description: Dieses Dokument beschreibt, wie die ersten Schritte mit iOS mit Einbetten von .NET. Es erläutert die Anforderungen und stellt eine Beispiel-app, um zu veranschaulichen, wie Sie eine verwaltete Assembly zu binden, und verwenden Sie die Ausgabe in einem Xcode-Projekt.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 009772ac88ad57bab53fb71c9705b71f0f8acc8b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118291"
---
# <a name="getting-started-with-ios"></a>Erste Schritte mit iOS

## <a name="requirements"></a>Anforderungen

Zusätzlich zu den Anforderungen aus unserer [erste Schritte mit Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) Guide, Sie müssen außerdem:

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) oder höher

## <a name="hello-world"></a>Hello World

Erstellen Sie zuerst eine einfache Hello-World-Beispiel in C# geschrieben.

### <a name="create-c-sample"></a>Erstellen einer C#-Beispiel

Öffnen Sie Visual Studio für Mac, ein neues iOS-Klassenbibliotheksprojekt zu erstellen, nennen Sie sie **Hello-aus-Csharp**, und speichern sie **~/Projects/hello-from-csharp**.

Ersetzen Sie den Code in die **MyClass.cs** -Datei mit den folgenden Codeausschnitt:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Erstellen Sie das Projekt, und die resultierende Assembly als gespeichert **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Binden Sie die verwaltete assembly

Nachdem Sie eine verwaltete Assembly haben, binden Sie es durch den Aufruf, Einbetten von .NET.

Siehe die [Installation](~/tools/dotnet-embedding/get-started/install/install.md) Guide, dies kann erfolgen als Postbuildschritt in Ihrem Projekt mit einem benutzerdefinierten MSBuild-Ziel oder manuell:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Das Framework gestellt **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Verwenden Sie die generierte Ausgabe in einem Xcode-Projekt

Öffnen Sie Xcode, ein neues iOS Einzelansicht-Anwendungsprojekt zu erstellen, nennen Sie es **Hello-aus-Csharp**, und wählen Sie die **Objective-C-** Sprache.

Öffnen der **~/Projects/hello-from-csharp/output** Verzeichnis im Finder zeigen, wählen Sie **Hello-aus-csharp.framework**, ziehen Sie es in das Xcode-Projekt, und legen Sie es direkt über die **Hello-aus-Csharp**  Ordner im Projekt.

![Drag & Drop-framework](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Stellen Sie sicher, dass **Elemente bei Bedarf kopieren** aktiviert ist, in dem angezeigten Dialogfeld, und klicken Sie auf **Fertig stellen**.

![Kopieren von Elementen, bei Bedarf](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Wählen Sie die **Hello-aus-Csharp** Projekt, und navigieren Sie zu der **Hello-aus-Csharp** des Ziels **Registerkarte "Allgemein"**. In der **eingebettete binäre** Abschnitt, fügen Sie **Hello-aus-csharp.framework**.

![Eingebettete Binärdateien](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Open **"viewcontroller.m"**, und Ersetzen Sie den Inhalt mit:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Einbetten von .NET unterstützt zurzeit Bitcode unter iOS, nicht die für einige Vorlagen der Xcode-Projekt aktiviert ist. 

Deaktivieren Sie es in Ihren projekteinstellungen:

![Bitcode-Option](../../images/ios-bitcode-option.png)

Führen Sie abschließend das Xcode-Projekt, und etwa Folgendes angezeigt:

![Grüße aus c#-Beispiel im Simulator ausgeführt](ios-images/hello-from-csharp-ios.png)
