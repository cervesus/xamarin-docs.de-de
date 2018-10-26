---
title: Erste Schritte mit macOS
description: In diesem Dokument wird beschrieben, wie erste Schritte mit MacOS mit Einbetten von .NET. Es erläutert die Anforderungen und führt eine beispielanwendung veranschaulicht, wie die verwaltete Assembly zu binden, und verwenden die generierte Ausgabe in einem Xcode-Projekt vor.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 3de47aa57df29f52f71508977ae017dff009c282
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121567"
---
# <a name="getting-started-with-macos"></a>Erste Schritte mit macOS

## <a name="what-you-will-need"></a>Was Sie benötigen

* Führen Sie die Anweisungen in der [erste Schritte mit Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) Guide.

## <a name="hello-world"></a>Hello World

Erstellen Sie zuerst eine einfache Hello-World-Beispiel in C# geschrieben.

### <a name="create-c-sample"></a>Erstellen einer C#-Beispiel

Öffnen Sie Visual Studio für Mac, erstellen Sie ein neues Mac Class Library-Projekt namens **Hello-aus-Csharp**, und speichern sie **~/Projects/hello-from-csharp**.

Ersetzen Sie den Code in die **MyClass.cs** -Datei mit den folgenden Codeausschnitt:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Erstellen Sie das Projekt. Die resultierende Assembly als gespeichert **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Binden Sie die verwaltete assembly

Nachdem Sie eine verwaltete Assembly haben, binden Sie es durch den Aufruf, Einbetten von .NET.

Siehe die [Installation](~/tools/dotnet-embedding/get-started/install/install.md) Guide, dies kann erfolgen als Postbuildschritt in Ihrem Projekt mit einem benutzerdefinierten MSBuild-Ziel oder manuell:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Das Framework gestellt **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Verwenden Sie die generierte Ausgabe in einem Xcode-Projekt

Öffnen Sie Xcode, und erstellen Sie eine neue Cocoa-Anwendung. Nennen Sie sie **Hello-aus-Csharp** , und wählen Sie die **Objective-C-** Sprache.

Öffnen der **~/Projects/hello-from-csharp/output** Verzeichnis im Finder zeigen, wählen Sie **Hello-aus-csharp.framework**, ziehen Sie es in das Xcode-Projekt, und legen Sie es direkt über die **Hello-aus-Csharp**  Ordner im Projekt.

![Drag & Drop-framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Stellen Sie sicher, dass **Elemente bei Bedarf kopieren** aktiviert ist, in dem angezeigten Dialogfeld, und klicken Sie auf **Fertig stellen**.

![Kopieren von Elementen, bei Bedarf](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Wählen Sie die **Hello-aus-Csharp** Projekt, und navigieren Sie zu der **Hello-aus-Csharp** des Ziels **allgemeine** Registerkarte. In der **eingebettete binäre** Abschnitt, fügen Sie **Hello-aus-csharp.framework**.

![Eingebettete Binärdateien](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Open **"viewcontroller.m"**, und Ersetzen Sie den Inhalt mit:

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Führen Sie abschließend das Xcode-Projekt, und etwa Folgendes angezeigt:

![Grüße aus c#-Beispiel im Simulator ausgeführt](macos-images/hello-from-csharp-mac.png)

Ein Beispiel für umfassendere und kreativer [steht hier zur](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
