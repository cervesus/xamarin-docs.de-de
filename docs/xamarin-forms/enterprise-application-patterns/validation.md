---
title: Die Validierung in Unternehmens-Apps
description: In diesem Kapitel wird erläutert, wie die mobilen Anwendung für eShopOnContainers Validierung von Benutzereingaben ausführt. Dies schließt Validierungsregeln angeben, das Auslösen der Überprüfung und das Anzeigen von Validierungsfehlern.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6a7f244b78d5b48dd219f59f1191993d62663bbf
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243176"
---
# <a name="validation-in-enterprise-apps"></a>Die Validierung in Unternehmens-Apps

Jede app, die Benutzereingaben akzeptiert, sollten sicherstellen, dass die Eingabe gültig ist. Eine app könnten Sie z. B. Eingabe überprüfen, die eine bestimmte Länge ist, enthält nur Zeichen in einem bestimmten Bereich oder entspricht ein bestimmtes Format. Ohne Validierung kann ein Benutzer Daten angeben, die bewirkt, die app dass zu Fehlern. Validierung erzwingt Geschäftsregeln, und verhindert, dass einen Angreifer Räumen schädliche Daten.

Im Kontext des Modell-ViewModel-Modell (MVVM), das einem Ansichtsmodell Muster oder Modell müssen häufig zum Ausführen von datenvalidierung und Validierungsfehler in die Ansicht zu signalisieren, damit der Benutzer, die sie korrigieren kann. Die mobile app eShopOnContainers synchrone clientseitige überprüft der Modell-Eigenschaften anzeigen und benachrichtigt den Benutzer des Validierungsfehler durch das Steuerelement, das den ungültigen Daten hervorheben und Anzeigen von Fehlermeldungen, die den Benutzer zu informieren Warum die Daten ungültig ist. Abbildung 6-1 gibt Aufschluss über die Klassen beim Ausführen einer Überprüfung in der mobilen Anwendung für eShopOnContainers.

[![](validation-images/validation.png "Überprüfung von Klassen in der mobilen Anwendung für eShopOnContainers")](validation-images/validation-large.png#lightbox "Überprüfung Klassen in der mobilen Anwendung für eShopOnContainers")

**Abbildung 6 – 1**: Überprüfung Klassen in der mobilen Anwendung für eShopOnContainers

Anzeigen von Modelleigenschaften, die Validierung erforderlich ist, sind vom Typ `ValidatableObject<T>`, und jede `ValidatableObject<T>` Instanz hat Validierungsregeln hinzugefügt, seine `Validations` Eigenschaft. Die Validierung wird aus dem Ansichtsmodell aufgerufen, durch Aufrufen der `Validate` Methode der `ValidatableObject<T>` -Instanz, die die Überprüfung ruft Regeln und führt sie für die `ValidatableObject<T>` `Value` Eigenschaft. Validierungsfehler abgelegt werden die `Errors` Eigenschaft von der `ValidatableObject<T>` Instanz, und die `IsValid` Eigenschaft der `ValidatableObject<T>` Instanz wird aktualisiert, um anzugeben, ob die Überprüfung erfolgreich war oder fehlgeschlagen ist.

Änderungsbenachrichtigung für die Eigenschaft wird bereitgestellt, indem die `ExtendedBindableObject` -Klasse, und daher ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement binden kann, an die `IsValid` Eigenschaft `ValidatableObject<T>` Instanz im Modell Ansichtsklasse ob benachrichtigt zu werden Die eingegebenen Daten sind gültig.

## <a name="specifying-validation-rules"></a>Angeben von Validierungsregeln

Validierungsregeln werden angegeben, durch das Erstellen einer Klasse, abgeleitet wird, die `IValidationRule<T>` -Schnittstelle, die im folgenden Codebeispiel gezeigt wird:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Diese Schnittstelle gibt an, dass eine Validierungsregelklasse bereitstellen, muss ein `boolean` `Check` -Methode, die verwendet wird, um die erforderliche Überprüfung ausführen und eine `ValidationMessage` -Eigenschaft, deren Wert der Überprüfungsfehlermeldung, die Wenn angezeigt wird Tritt ein Überprüfungsfehler auf.

Das folgende Codebeispiel zeigt die `IsNotNullOrEmptyRule<T>` Validierungsregel, die verwendet wird, die Validierung von Benutzername und Kennwort, die vom Benutzer eingegeben werden, auf die `LoginView` bei Verwendung von simulierten Dienste in der mobilen Anwendung für eShopOnContainers:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

Die `Check` Methode gibt ein `boolean` , der angibt, ob die Wertargument ist `null`, leer ist oder nur aus Leerzeichen besteht.

Zwar nicht von der mobilen Anwendung für eShopOnContainers verwendet wird, wird im folgenden Codebeispiel wird eine Validierungsregel für die Überprüfung von e-Mail-Adressen:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

Die `Check` Methode gibt ein `boolean` , der angibt, ob die Wertargument eine gültige e-Mail-Adresse ist. Dies wird erreicht, indem Sie das Argument "Value" für das erste Vorkommen im angegebenen Muster für reguläre Ausdrücke suchen der `Regex` Konstruktor. Gibt an, ob das reguläre Ausdrucksmuster in der Eingabezeichenfolge gefunden wurde kann bestimmt werden, durch Überprüfung des Werts, der die `Match` des Objekts `Success` Eigenschaft.

> [!NOTE]
> Überprüfung der Eigenschaften kann manchmal abhängige Eigenschaften umfassen. Ein Beispiel für die abhängigen Eigenschaften ist, wenn der Satz gültiger Werte für eine Eigenschaft der betreffende Wert abhängig ist, die in der Eigenschaft b festgelegt wurde Um sicherzustellen, dass der Wert der Eigenschaft A einer der zulässigen Werte ist würde das Abrufen des Werts der Eigenschaft b Darüber hinaus bei Änderung des Werts der Eigenschaft B müssten Eigenschaft A erneute Überprüfung wird erwartet.

## <a name="adding-validation-rules-to-a-property"></a>Hinzufügen von Validierungsregeln auf eine Eigenschaft

In der mobilen app eShopOnContainers Ansicht Modelleigenschaften, die Validierung erforderlich ist vom Typ deklariert werden `ValidatableObject<T>`, wobei `T` ist der Typ der Daten überprüft werden. Das folgende Codebeispiel zeigt ein Beispiel für diese zwei Eigenschaften:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Validierung ausgeführt wird, müssen Sie Validierungsregeln hinzugefügt werden die `Validations` Auflistung der einzelnen `ValidatableObject<T>` Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Diese Methode fügt die `IsNotNullOrEmptyRule<T>` Überprüfungsregel auf die `Validations` Auflistung der einzelnen `ValidatableObject<T>` Instanz, Angeben von Werten für der Überprüfungsregel `ValidationMessage` Eigenschaft, die die Überprüfungsfehlermeldung angibt, der Wenn angezeigt wird Tritt ein Überprüfungsfehler auf.

## <a name="triggering-validation"></a>Auslösen der Validierung

Überprüfung Ansatz wird in der mobilen Anwendung für eShopOnContainers kann Überprüfung einer Eigenschaft und Trigger Überprüfung automatisch bei Änderung einer Eigenschaft manuell auslösen.

### <a name="triggering-validation-manually"></a>Überprüfung manuell auslösen

Überprüfung kann für eine Modelleigenschaft Ansicht manuell auslösen. Beispielsweise dieser Fehler tritt in der mobilen Anwendung für eShopOnContainers beim Tippen auf die **Anmeldung** Schaltfläche auf der `LoginView`, bei Verwendung von simulierten Services. Der Befehl Delegieren von Aufrufen der `MockSignInAsync` Methode in der `LoginViewModel`, ruft die die Validierung durch Ausführen der `Validate` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

Die `Validate` -Methode führt die Überprüfung von Benutzername und Kennwort, die vom Benutzer eingegeben werden, auf die `LoginView`, durch den Aufruf der Validate-Methode für jede `ValidatableObject<T>` Instanz. Das folgende Codebeispiel zeigt die Validate-Methode aus der `ValidatableObject<T>` Klasse:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Diese Methode löscht die `Errors` Auflistung und anschließend abgerufen, die Validierungsregeln, wurden hinzugefügt, um des Objekts `Validations` Auflistung. Die `Check` Methode für jede abgerufene Validierungsregel ausgeführt wird, und die `ValidationMessage` Eigenschaftswert für jede Validierungsregel, die nicht zum Überprüfen der Daten hinzugefügt wird die `Errors` Auflistung von der `ValidatableObject<T>` Instanz. Schließlich die `IsValid` Eigenschaft festgelegt ist, und sein Wert wird zurückgegeben, an die aufrufende Methode, die angibt, ob die Überprüfung erfolgreich war oder fehlgeschlagen ist.

### <a name="triggering-validation-when-properties-change"></a>Validierung auslösen, wenn Eigenschaften ändern

Überprüfung kann auch ausgelöst werden, wenn eine gebundene Eigenschaft ändert. Beispielsweise, wenn eine bidirektionale Bindung in der `LoginView` legt die `UserName` oder `Password` -Eigenschaft, Überprüfung wird ausgelöst. Im folgenden Codebeispiel wird veranschaulicht, wie in diesem Fall:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

Die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement bindet an die `UserName.Value` Eigenschaft von der `ValidatableObject<T>` Instanz und die Control `Behaviors` Auflistung verfügt über eine `EventToCommandBehavior` Instanz hinzugefügt wird. Dieses Verhalten führt die `ValidateUserNameCommand` als Antwort auf die [`TextChanged`] auf Auslösen des Ereignisses die `Entry`, die ausgelöst wird, wenn der Text in der `Entry` Änderungen. Wiederum die `ValidateUserNameCommand` Delegaten ausführt der `ValidateUserName` -Methode, die ausgeführt wird die `Validate` Methode für die `ValidatableObject<T>` Instanz. Deshalb jedes Mal der Benutzer gibt ein Zeichen in der `Entry` Steuerelement für den Benutzernamen, die Validierung der eingegebenen Daten erfolgt.

Weitere Informationen zu Verhalten, finden Sie unter [Verhalten implementieren](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Anzeigen von Validierungsfehlern

Die mobile app eShopOnContainers benachrichtigt den Benutzer der Validierungsfehler, da das Steuerelement, die ungültigen Daten mit einer roten Linie enthält, hervorgehoben, und durch eine Fehlermeldung angezeigt, mit denen den Benutzer, warum informiert die Daten sind ungültig, unter das Steuerelement mit dem Ungültige Daten. Wenn die ungültigen Daten korrigiert werden, wird die Zeile in Schwarz geändert, und die Fehlermeldung wird entfernt. Abbildung 6 – 2 zeigt die LoginView in der mobilen Anwendung für eShopOnContainers, wenn Validierungsfehler vorhanden sind.

![](validation-images/validation-login.png "Anzeigen von Validierungsfehlern während der Anmeldung")

**Abbildung 6 – 2:** Anzeigen von Validierungsfehlern während der Anmeldung

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Hervorhebung eines Steuerelements, die ungültige Daten enthält.

Die `LineColorBehavior` angefügten Verhalten wird verwendet, um markieren [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelemente, in denen Validierungsfehler aufgetreten. Im folgenden Codebeispiel wird veranschaulicht wie die `LineColorBehavior` angefügten Verhalten angefügt ist ein `Entry` Steuerelement:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

Die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement nutzt eine explizite-Formatvorlage, die im folgenden Codebeispiel gezeigt wird:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Dieses Format wird die `ApplyLineColor` und `LineColor` angefügten Eigenschaften der `LineColorBehavior` angefügten Verhalten von der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement. Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

Bei den Wert des der `ApplyLineColor` angefügte Eigenschaft ist, festlegen oder ändern, die `LineColorBehavior` angefügten Verhalten führt die `OnApplyLineColorChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Die Parameter für diese Methode bereit, die Instanz des Steuerelements, das das Verhalten angefügt ist und die alten und neuen Werte von der `ApplyLineColor` -Eigenschaft. Die `EntryLineColorEffect` Klasse hinzugefügt, um des Steuerelements [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung Wenn die `ApplyLineColor` angefügte Eigenschaft ist `true`, andernfalls wird er aus des Steuerelements entfernt `Effects` Auflistung. Weitere Informationen zu Verhalten, finden Sie unter [Verhalten implementieren](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

Die `EntryLineColorEffect` Unterklassen der [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasse, und wird im folgenden Codebeispiel gezeigt:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

Die [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasse stellt einen plattformunabhängigen Effekt, der einen inneren Effekt umschließt, der plattformspezifischen ist. Dies vereinfacht das Effekt entfernen, da es keinen Zeitpunkt der Kompilierung Zugriff auf die Typinformationen für eine plattformspezifische Auswirkung gibt. Die `EntryLineColorEffect` Ruft den Basisklassenkonstruktor übergeben eines Parameters, bestehend aus einer Verkettung der Name der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen Klasse angegeben ist.

Das folgende Codebeispiel zeigt die `eShopOnContainers.EntryLineColorEffect` Implementierung für iOS:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

Die `OnAttached` Methode ruft das systemeigene Steuerelement ab, für die Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) steuern und aktualisieren Sie die Linienfarbe durch Aufrufen der `UpdateLineColor` Methode. Die `OnElementPropertyChanged` Außerkraftsetzung reagiert auf bindbare eigenschaftenänderungen auf die `Entry` Steuerelement, indem Sie die Linienfarbe aktualisieren, wenn die angefügte `LineColor` eigenschaftsänderungen, oder die [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) Eigenschaft von der `Entry`ändert. Weitere Informationen zu Auswirkungen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

Bei der Eingabe gültiger Daten der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) -Steuerelement, es gelten schwarze Linie für den unteren Rand des Steuerelements, um anzugeben, dass keine Validierungsfehler vorliegt. Abbildung 6 – 3 zeigt ein Beispiel hierfür.

![](validation-images/validation-blackline.png "Schwarze Linie, die keine Validierungsfehler")

**Abbildung 6 – 3**: schwarze Linie, die keine Validierungsfehler

Die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) -Steuerelement verfügt über eine [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) hinzugefügt, dessen [ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) Auflistung. Das folgende Codebeispiel zeigt die `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Dies [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) Monitore die `UserName.IsValid` -Eigenschaft, und wird der Wert ist `false`, führt er die [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/), welche Änderungen der `LineColor` angefügt Eigenschaft der `LineColorBehavior` Verhalten sich in Rot angefügt. Abbildung 6 – 4 zeigt ein Beispiel hierfür.

![](validation-images/validation-redline.png "Rote Linie Validierungsfehler")

**Abbildung 6 – 4**: rote Linie Validierungsfehler

Die Zeile in der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement wird rot bleiben, während die eingegebenen Daten ungültig sind, andernfalls ändert er auf Schwarz festgelegt, um anzugeben, dass die eingegebenen Daten gültig sind.

Weitere Informationen zu Triggern finden Sie unter [Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Anzeigen von Fehlermeldungen

Die Benutzeroberfläche zeigt Überprüfungsfehlermeldungen in Bezeichnungsfelder unten jedes Steuerelement, dessen Daten Fehler bei der Überprüfung. Das folgende Codebeispiel zeigt die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , das eine Überprüfungsfehlermeldung anzeigt, wenn der Benutzer einen gültigen Benutzernamen nicht eingegeben hat:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Jede [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bindet an die `Errors` Eigenschaft des Model View-Objekts, das überprüft wird. Die `Errors` Eigenschaft wird bereitgestellt, indem Sie die `ValidatableObject<T>` Klasse, und ist vom Typ `List<string>`. Da die `Errors` Eigenschaft kann mehrere Validierungsfehler, enthalten die `FirstValidationErrorConverter` Instanz wird verwendet, um den ersten Fehler aus der Auflistung für die Anzeige abzurufen.

## <a name="summary"></a>Zusammenfassung

Die mobile app eShopOnContainers synchrone clientseitige überprüft der Modell-Eigenschaften anzeigen und benachrichtigt den Benutzer des Validierungsfehler durch das Steuerelement, das den ungültigen Daten hervorheben und Anzeigen von Fehlermeldungen, die den Benutzer zu informieren Warum die Daten ungültig sind.

Anzeigen von Modelleigenschaften, die Validierung erforderlich ist, sind vom Typ `ValidatableObject<T>`, und jede `ValidatableObject<T>` Instanz hat Validierungsregeln hinzugefügt, seine `Validations` Eigenschaft. Die Validierung wird aus dem Ansichtsmodell aufgerufen, durch Aufrufen der `Validate` Methode der `ValidatableObject<T>` -Instanz, die die Überprüfung ruft Regeln und führt sie für die `ValidatableObject<T>` `Value` Eigenschaft. Validierungsfehler abgelegt werden die `Errors` Eigenschaft von der `ValidatableObject<T>`Instanz, und die `IsValid` Eigenschaft der `ValidatableObject<T>` Instanz wird aktualisiert, um anzugeben, ob die Überprüfung erfolgreich war oder fehlgeschlagen ist.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
