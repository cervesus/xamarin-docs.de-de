---
title: Metadaten für Java-Bindungen
description: C#-Code im Xamarin.Android ruft Java-Bibliotheken über Bindungen, d. h. einen Mechanismus, der die Details auf niedriger Ebene abstrahiert, die in Java Native Interface (JNI) angegeben werden. Xamarin.Android bietet ein Tool, das diese Bindungen generiert. Diese Tools kann Entwickler-Steuerelements, wie eine Bindung erstellt wird, mithilfe von Metadaten, die Prozeduren, z. B. Ändern von Namespaces und Umbenennen von Elementen ermöglicht. Dieses Dokument erläutert die Funktionsweise von Metadaten, werden die Attribute, die Metadaten unterstützt, und erläutert deren Bindungsprobleme zu beheben, indem Sie diese Metadaten zu ändern.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 6dea13fcda43cad22b8bea9838bbcb23b97820c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="java-bindings-metadata"></a>Metadaten für Java-Bindungen

_C#-Code im Xamarin.Android ruft Java-Bibliotheken über Bindungen, d. h. einen Mechanismus, der die Details auf niedriger Ebene abstrahiert, die in Java Native Interface (JNI) angegeben werden. Xamarin.Android bietet ein Tool, das diese Bindungen generiert. Diese Tools kann Entwickler-Steuerelements, wie eine Bindung erstellt wird, mithilfe von Metadaten, die Prozeduren, z. B. Ändern von Namespaces und Umbenennen von Elementen ermöglicht. Dieses Dokument erläutert die Funktionsweise von Metadaten, werden die Attribute, die Metadaten unterstützt, und erläutert deren Bindungsprobleme zu beheben, indem Sie diese Metadaten zu ändern._


## <a name="overview"></a>Übersicht

Eine Xamarin.Android **Java Bindung Bibliothek** versucht, der zum Binden einer vorhandenen Android-Bibliothek mithilfe eines Tools auch bezeichnet als erforderliche Arbeitsaufwand größtenteils automatisiert die _Bindungen Generator_. Beim Binden einer Java-Bibliothek Xamarin.Android Java-Klassen überprüfen und generieren Sie eine Liste aller Pakete, Typen und Member wird die gebunden werden soll. Diese Liste der APIs wird gespeichert, in eine XML-Datei, die am befinden  **\{Projekt directory}\obj\Release\api.xml** für eine **Version** erstellen und zur  **\{Projekt Directory}\obj\Debug\api.XML** für eine **DEBUGGEN** erstellen.

![Speicherort der Datei im Ordner "Obj/Debug" api.xml](java-bindings-metadata-images/java-bindings-metadata-01.png)

Bindungen-Generator verwendet die **api.xml** Datei als Richtlinie zum Generieren der erforderlichen C#-Wrapperklassen. Der Inhalt dieser XML-Datei ist eine Variation des Google _Android Open Source Project_ Format.
Der folgende Codeausschnitt zeigt ein Beispiel für den Inhalt der **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

In diesem Beispiel **api.xml** deklariert eine Klasse in der `android` Paket mit dem Namen `Manifest` , reicht die `java.lang.Object`.

In vielen Fällen ist die menschliche Unterstützung nach "erforderlich, damit der Meinung sind, dass weitere".NET wie"Java-API oder um Probleme zu beheben, die verhindern, die bindungsassembly kompilieren dass. Beispielsweise kann es erforderlich sein, ändern Sie die Java-Paketnamen in .NET Namespaces, benennen Sie eine Klasse oder ändern Sie den Rückgabetyp einer Methode.

Diese Änderungen werden nicht erzielt, indem ändern **api.xml** direkt.
Stattdessen werden die Änderungen in speziellen XML-Dateien aufgezeichnet, die von der Java binden Bibliotheksvorlage bereitgestellt werden. Beim Kompilieren der Assemblybindung Xamarin.Android wird den Bindungen Generator durch diesen Zuordnungsdateien entnehmen beeinflusst werden beim Erstellen der bindungsassembly

Diese XML-Zuordnungsdateien finden Sie in der **transformiert** Ordner des Projekts:

-   **MetaData.xml** &ndash; können Änderungen an der endgültigen-API, z. B. Ändern des Namespaces der generierten Bindung vorgenommen werden müssen. 

-   **EnumFields.xml** &ndash; enthält die Zuordnung zwischen Java `int` Konstanten und C#- `enums` . 

-   **EnumMethods.xml** &ndash; ermöglicht es, ändern die Parameter und Rückgabetypen von Java `int` Konstanten in c# `enums` . 

Die **MetaData.xml** Datei ist die meisten Importieren dieser Dateien, da der hinauszögern allgemeiner Änderungen an der Bindung, z. B.:

-   Umbenennen von Namespaces, Klassen, Methoden oder Felder aus, damit sie .NET Konventionen entsprechen. 

-   Entfernen von Namespaces, Klassen, Methoden oder Felder, die nicht benötigt werden. 

-   Verschieben Sie Klassen in verschiedenen Namespaces. 

-   Hinzufügen von zusätzlichen Unterstützungsklassen, stellen den Entwurf der Bindung führen Sie die .NET Framework-Muster. 

Zu besprechen wechseln können **Metadata.xml** im Detail.


## <a name="metadataxml-transform-file"></a>Metadata.XML-Transform-Datei

Da wir haben bereits berücksichtigt, die Datei **Metadata.xml** von den Bindungen-Generator verwendet, um die Erstellung der bindungsassembly beeinflussen.
Dem Metadatenformat identisch verwendet [XPath](https://www.w3.org/TR/xpath/) Syntax und ist nahezu identisch mit der *GAPI Metadaten* in beschriebenen [GAPI Metadaten](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) Handbuch. Diese Implementierung ist fast eine vollständige Implementierung von XPath 1.0 und unterstützt daher Elemente in der 1.0-standard. Diese Datei ist ein XPath-basierte ein leistungsstarker Mechanismus zu ändern, hinzufügen, ausblenden oder Verschieben von einem Element oder Attribut in der API-Datei. Alle Regelelemente in den Metadaten-Spezifikationen enthalten ein Pfadattribut zum Identifizieren des Knotens, für die die Regel angewendet wird. Die Regeln werden in der folgenden Reihenfolge angewendet:

* **Hinzufügen von Knoten** &ndash; Fügt einen untergeordneten Knoten auf den Knoten, die von der Path-Attribut angegeben.
* **Attr** &ndash; legt den Wert eines Attributs des Elements durch den Path-Attribut angegeben.
* **Entfernen von Knoten** &ndash; Knoten, die mit einem angegebenen XPath entfernt.

Im folgenden ist ein Beispiel für eine **Metadata.xml** Datei:

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Die folgende Liste enthält einige der häufiger verwendeten XPath-Elemente für die Java-API:

-   `interface` &ndash; Zum Suchen einer Java-Schnittstelle verwendet. z. B. `/interface[@name='AuthListener']`.

-   `class` &ndash; Zum Suchen einer Klasse verwendet. z. B. `/class[@name='MapView']`.

-   `method` &ndash; Zum Suchen einer Methode in einer Java-Klasse oder Schnittstelle verwendet. z. B. `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Identifizieren Sie einen Parameter für eine Methode an. Z. B. `/parameter[@name='p0']`



### <a name="adding-types"></a>Hinzufügen von Typen

Die `add-node` Element informiert die Xamarin.Android bindungsprojekt hinzufügen zu eine neuen Wrapperklasse **api.xml**. Beispielsweise wird der folgende Codeausschnitt den Binding-Generator zum Erstellen einer Klasse einen Konstruktor mit einem einzelnen Feld weitergeleitet:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Entfernen von Typen

Es ist möglich, weisen Sie den Xamarin.Android Bindungen-Generator einen Java-Typ ignoriert und nicht gebunden. Dies erfolgt durch Hinzufügen einer `remove-node` XML-Element, das **metadata.xml** Datei:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Umbenennen von Elementen

Umbenennen von Elementen kann nicht durchgeführt werden, indem Sie direkt bearbeiten der **api.xml** Datei, da Xamarin.Android den Originalnamen-Java Native Interface (JNI) erforderlich ist. Aus diesem Grund die `//class/@name` Attribut kann nicht geändert werden, wenn dies der Fall, wird die Bindung nicht funktionsfähig.

Betrachten Sie den Fall, in dem wir, benennen Sie einen Typ möchten `android.Manifest`.
Zu diesem Zweck könnten wir versuchen für eine direkte Bearbeitung **api.xml** und Umbenennen der Klasse wie folgt:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Dies führt zu den Bindungen-Generator den folgenden c#-Code für die Wrapperklasse erstellen:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Beachten Sie, die die Wrapperklasse um umbenannt wurde `NewName`, während die ursprünglichen Java-Datentyp weiterhin `Manifest`. Es ist nicht mehr möglich, dass die Xamarin.Android Bindungsklasse zum Zugriff auf alle Methoden in `android.Manifest`; die Wrapperklasse an eine nicht existierende Java-Typ gebunden ist.

Um die verwalteten Namen einer umschlossenen Typ (oder Methode) ordnungsgemäß zu ändern, es ist erforderlich, legen Sie die `managedName` -Attribut wie im folgenden Beispiel gezeigt:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Umbenennen von `EventArg` Wrapperklassen

Bei der Xamarin.Android Bindung Generator identifiziert eine `onXXX` Setter-Methode für eine _Listenertyp_, ein C#-Ereignis und `EventArgs` Unterklasse generiert werden Zusatz Sie zur Unterstützung von .NET API für den Java-basierten Listener Muster. Betrachten Sie beispielsweise die folgenden Java-Klasse und Methode ein:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android löscht das Präfix `on` aus der Settermethode und stattdessen `2DSignNextManuever` als Grundlage für den Namen des der `EventArgs` Unterklasse. Die Unterklasse wird etwa benannt werden:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Dies ist nicht zulässigen C#-Klassennamen. Um dieses Problem zu beheben, muss der Autor Bindung verwenden die `argsType` Attribut, und geben Sie einen gültigen C#-Namen für die `EventArgs` Unterklasse:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Unterstützte Attribute

In den folgenden Abschnitten werden einige der Attribute zum Transformieren von Java-APIs beschrieben.

### <a name="argstype"></a>argsType

Dieses Attribut befindet sich auf Setter-Methoden zum Benennen der `EventArg` Unterklasse, die zur Unterstützung von Java-Listener generiert werden. Hierzu finden Sie unten im Abschnitt detaillierter [EventArg Wrapperklassen umbenennen](#Renaming_EventArg_Wrapper_Classes) weiter unten in diesem Handbuch.

### <a name="eventname"></a>eventName

Gibt einen Namen für ein Ereignis. Falls leer, wird verhindert, dass es Ereigniserzeugung und Sammlung.
Dies wird ausführlicher in den Abschnittstiteln beschrieben [EventArg Wrapperklassen umbenennen](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Dies wird verwendet, um den Namen des Pakets, Klasse, Methode oder Parameter zu ändern. Z. B. so ändern Sie den Namen der Java-Klasse `MyClass` auf `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

Das folgende Beispiel veranschaulicht einen XPath-Ausdruck für die Methode umbenennen `java.lang.object.toString` auf `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` wird verwendet, um den Rückgabetyp einer Methode zu ändern. In einigen Situationen wird der Generator Bindungen falsch den Rückgabetyp einer Java-Methode, abzuleiten, der einen Fehler zur Kompilierzeit verursacht. Eine mögliche Lösung in dieser Situation ist den Rückgabetyp der Methode zu ändern.

Bindungen-Generator z. B. der Meinung ist, die die Java-Methode `de.neom.neoreadersdk.resolution.compareTo()` zurückgeben sollte ein `int`, was dazu führt, in der Fehlermeldung angegebene **Fehler CS0535: "DE. Neom.Neoreadersdk.Resolution "implementiert den Schnittstellenmember 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' nicht**. Der folgende Codeausschnitt veranschaulicht, wie so ändern Sie den Rückgabetyp der generierten C#-Methode von einer `int` auf eine `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Ändert den Rückgabetyp einer Methode an. Dies ändert die return-Attribut nicht (wie Änderungen zurückgegeben, dass Attribute mit der Signatur JNI inkompatible Änderungen führen können). Im folgenden Beispiel der Rückgabetyp von den `append` Methode von geändert wird `SpannableStringBuilder` auf `IAppendable` (Denken Sie daran, dass c# nicht covariant-Rückgabetypen unterstützt):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>verborgen

Tools, die Bibliotheken für Java zu verbergen können mit der Bindung Xamarin.Android-Generator und der Fähigkeit zum Generieren von C#-Wrapperklassen beeinträchtigen. Eigenschaften von verborgenen Klassen umfassen: * der Klassenname enthält eine **$**, d. h. **eine$ .class** * der Klassennamen vollständig gefährdet ist von Kleinbuchstaben, d. h.  **a.Class**

Dieser Codeausschnitt ist ein Beispiel für eine "nicht verborgenen" C#-Typ zu generieren:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Dieses Attribut kann zum Ändern des Namens einer verwalteten Eigenschaft verwendet werden.

Ein spezialisierter Fall der Verwendung von `propertyName` umfasst die Situation, in eine Javaklasse nur eine Getter-Methode für ein Feld hat. In diesem Fall möchte der Generator Bindung eine Nur-Schreiben-Eigenschaft, ein Element zu erstellen, die in .NET abgeraten wird. Der folgende Codeausschnitt zeigt, wie die Eigenschaften von .NET durch Festlegen von "entfernen" die `propertyName` auf eine leere Zeichenfolge:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Beachten Sie, dass die Setter-Methode und der Getter-Methode noch durch die Bindungen-Generator erstellt werden.

### <a name="sender"></a>sender

Gibt an, welche Parameter einer Methode sollte die `sender` -Parameter, wenn die Methode mit einem Ereignis zugeordnet ist. Der Wert kann `true` oder `false`. Zum Beispiel:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>Sichtbarkeit

Dieses Attribut wird verwendet, um die Sichtbarkeit einer Klasse, die Methode oder die Eigenschaft zu ändern. Angenommen, es kann erforderlich sein, Heraufstufen einer `protected` Java-Methode, damit sie C#-Wrapper entsprechende ist ist `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml und EnumMethods.xml

Es gibt Fälle, in denen Android Bibliotheken ganzzahlige Konstanten zu verwenden, um Zustände darstellen, die auf Eigenschaften oder Methoden der Clientbibliotheken übergeben werden. In vielen Fällen ist es sinnvoll, diese ganzzahlige Konstanten für Enumerationen in c# zu binden. Um diese Zuordnung zu ermöglichen, verwenden Sie die **EnumFields.xml** und **EnumMethods.xml** Dateien in Ihrem bindungsprojekt. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Definieren einer Enumeration mit EnumFields.xml

Die **EnumFields.xml** -Datei enthält die Zuordnung zwischen Java `int` Konstanten und C#- `enums`. Werfen wir das folgende Beispiel einer c#-Enumeration, die erstellt wird, für einen Satz von `int` Konstanten: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Hier haben wir die Java-Klasse ergriffen `SKRealReachSettings` und definiert eine c#-Enumeration namens `SKMeasurementUnit` im Namespace `Skobbler.Ngx.Map.RealReach`. Die `field` Einträge definiert den Namen der Java-Konstante (Beispiel `UNIT_SECOND`), den Namen des Eintrags Enum (Beispiel `Second`), und der Ganzzahlwert durch beide Entitäten dargestellt (Beispiel `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Definition Getter/Setter-Methoden, die mithilfe von EnumMethods.xml

Die **EnumMethods.xml** Datei ermöglicht es, ändern die Parameter und Rückgabetypen von Java `int` Konstanten in c# `enums`. Das heißt, es das Lesen und Schreiben von C#-Enumerationen zugeordnet (definiert der **EnumFields.xml** Datei) Java `int` konstant `get` und `set` Methoden.

Erhält die `SKRealReachSettings` -Enumeration definiert, über die folgenden **EnumMethods.xml** Datei würde die Getter/Setter für diese Enumeration definieren:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

Die erste `method` Zeile zugeordnet, den Rückgabewert von der Java `getMeasurementUnit` Methode, um die `SKMeasurementUnit` Enum. Die zweite `method` Zeile ordnet den ersten Parameter der `setMeasurementUnit` der gleichen-Enumeration.

Alle diese Änderungen vorhanden, können Sie den folgenden Code in Xamarin.Android legen Sie die `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie Xamarin.Android Metadaten verwendet, um einen API-Definition aus zu transformieren der *Google* *AOSP Format*. Nach dem abdecken der Änderungen, die möglich sind, indem *Metadata.xml*, er überprüft die Einschränkungen, die beim Umbenennen von Elementen auftreten und sie angezeigt, dass die Liste der unterstützten XML-Attribute, die beschreiben, wenn jedes Attribut verwendet werden soll.



## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [GAPI Metadaten](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
