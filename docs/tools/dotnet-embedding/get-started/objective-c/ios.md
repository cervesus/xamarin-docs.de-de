---
title: Erste Schritte mit iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: c1b21e36d314b79402702f29428f2c337ddc1069
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-ios"></a>Erste Schritte mit iOS


## <a name="requirements"></a>Anforderungen

Zusätzlich zu den aus unserem [Einstieg in Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) Handbuch Sie auch müssen:

* [Xamarin.iOS 10.11 +](https://jenkins.mono-project.com/view/Xamarin.MaciOS/job/xamarin-macios-builds-master/) aus unserem _master_ Verzweigung.

## <a name="hello-world"></a>Hello World

Zuerst erstellen Sie ein einfaches Hello World-Beispiel in C# geschrieben.

### <a name="create-c-sample"></a>Erstellen der C#-Beispiel

Öffnen Sie Visual Studio für Mac, erstellen Sie eine neue iOS Class Library-Projekts, nennen Sie sie `hello-from-csharp`, und speichern Sie es `~/Projects/hello-from-csharp`.

Ersetzen Sie den Code in der `MyClass.cs` -Datei mit den folgenden Codeausschnitt:

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

Erstellen Sie das Projekt, die sich ergebende Assembly als gespeichert `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`.

### <a name="bind-the-managed-assembly"></a>Die verwaltete Assembly binden

Führen Sie die Embeddinator um ein natives Framework für die verwaltete Assembly zu erstellen:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Das Framework wird in abgelegt `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Verwenden Sie die generierte Ausgabe in einem Xcode-Projekt

Xcode öffnen und eine neue iOS einzelne Ansicht-Anwendung erstellen möchten, nennen Sie sie `hello-from-csharp` , und wählen Sie die **Objective-C** Sprache.

Öffnen der `~/Projects/hello-from-csharp/output` Verzeichnis im Finder wählen `hello-from-csharp.framework`, ziehen Sie es in Xcode-Projekt, und legen Sie sie direkt über die `hello-from-csharp` Ordner des Projekts.

! [Drag & Drop-Framework] Images/Hello-from-CSharp-IOS-Drag-Drop-Framework.PNG)

Stellen Sie sicher, dass `Copy items if needed` in der angezeigten Dialogfeld aktiviert ist, und klicken Sie auf `Finish`.

![Kopieren von Elementen, bei Bedarf](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Wählen Sie die `hello-from-csharp` Projekt, und navigieren Sie zu der `hello-from-csharp` des Ziels **Registerkarte "Allgemein"**. In der **eingebettete binäre** Abschnitt, fügen Sie `hello-from-csharp.framework`.

![Eingebettete Binärdateien](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Öffnen Sie ViewController.m, und Ersetzen Sie den Inhalt mit:

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

Führen Sie schließlich die Xcode-Projekt, und etwa Folgendes angezeigt wird:

![Hallo von C#-Beispiel im Simulator ausführen](ios-images/hello-from-csharp-ios.png)
