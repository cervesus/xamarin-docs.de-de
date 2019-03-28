---
title: Validierung in Unternehmens-Apps
description: In diesem Kapitel wird erläutert, wie die "eshoponcontainers" mobile app die Validierung der Benutzereingabe ausführt. Dies umfasst das Angeben von Validierungsregeln, Auslösen von Validierung und Anzeigen von Überprüfungsfehlern.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 22b5fe703486f0ded3a5b91241e3fe5ce41bbc98
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507096"
---
# <a name="validation-in-enterprise-apps"></a>Validierung in Unternehmens-Apps

Jede app, die Eingaben von Benutzern akzeptiert sorgen dafür, dass die Eingabe gültig ist. Eine app könnte beispielsweise für die Eingabe überprüfen, die eine bestimmte Länge ist, enthält nur Zeichen in einem bestimmten Bereich oder entspricht ein bestimmtes Format. Ohne Validierung kann ein Benutzer Daten bereitstellen, die bewirkt, dass die app fehl. Überprüfung von Geschäftsregeln erzwingt und verhindert, dass einen Angreifer schädliche Daten einfügt.

Klicken Sie im Kontext der Model-View-ViewModel (MVVM) Muster, ein Ansichtsmodell oder Modell ist häufig erforderlich, um eine datenüberprüfung durchführen und Validierungsfehler an die Ansicht zu signalisieren, sodass der Benutzer, die sie korrigieren kann. Die eShopOnContainers-mobile-app führt die synchronen die clientseitige Validierung von Eigenschaften und benachrichtigt den Benutzer über Validierungsfehler aus, durch das Steuerelement, das die ungültigen Daten enthält, hervorheben und Anzeigen von Fehlermeldungen, die den Benutzer zu informieren Warum die Daten ungültig ist. Abbildung 6 – 1 zeigt die Klassen, die beim Ausführen der Überprüfung in der mobilen app für "eshoponcontainers".

[![](validation-images/validation.png "Überprüfung von Klassen in der mobilen app von eShopOnContainers")](validation-images/validation-large.png#lightbox "Validierungsklassen in der eShopOnContainers-mobile-app")

**Abbildung 6 – 1**: Überprüfung von Klassen in der eShopOnContainers-mobile-app

Eigenschaften, für die Validierung erforderlich sind, vom Typ `ValidatableObject<T>`, und jede `ValidatableObject<T>` Instanz verfügt über Regeln zur Überprüfung hinzugefügt, die `Validations` Eigenschaft. Überprüfung von des Ansichtsmodells aufgerufen wird, durch den Aufruf der `Validate` -Methode der der `ValidatableObject<T>` -Instanz, die die Überprüfung ruft Regeln und führt sie anhand der `ValidatableObject<T>` `Value` Eigenschaft. Validierungsfehler befinden sich in der `Errors` Eigenschaft der `ValidatableObject<T>` -Instanz und die `IsValid` Eigenschaft der `ValidatableObject<T>` Instanz wird aktualisiert, um anzugeben, ob die Überprüfung erfolgreich war oder nicht.

Benachrichtigung der Eigenschaftenänderung erfolgt über die `ExtendedBindableObject` -Klasse, und daher ein [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement binden kann, an der `IsValid` Eigenschaft `ValidatableObject<T>` -Instanz in die Ansichtsmodell-Klasse, ob benachrichtigt werden Die eingegebenen Daten gilt.

## <a name="specifying-validation-rules"></a>Angeben von Validierungsregeln

Validierungsregeln werden angegeben, indem Sie eine abgeleitete Klasse erstellen den `IValidationRule<T>` -Schnittstelle, die im folgenden Codebeispiel gezeigt wird:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Diese Schnittstelle gibt an, dass eine Validierungsregelklasse bereitstellen, muss ein `boolean` `Check` -Methode, die zum Ausführen der erforderlichen Überprüfung, und ein `ValidationMessage` Eigenschaft, deren Wert die Überprüfungsfehlermeldung, die Wenn angezeigt wird Tritt ein Überprüfungsfehler auf.

Das folgende Codebeispiel zeigt die `IsNotNullOrEmptyRule<T>` Validierungsregel, die verwendet wird, für die Validierung von Benutzername und Kennwort, die vom Benutzer eingegeben wird, für die `LoginView` pseudodienste Verwendung in der eShopOnContainers-mobile-app:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

Die `Check` Methode gibt eine `boolean` , der angibt, ob das Wertargument ist `null`, leer ist oder nur aus Leerzeichen besteht.

Zwar nicht von der mobilen app für "eshoponcontainers" verwendet, wird im folgenden Codebeispiel wird die Validierungsregel für die Überprüfung von e-Mail-Adressen:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

Die `Check` Methode gibt eine `boolean` , der angibt, ob das Wertargument eine gültige e-Mail-Adresse ist. Dies wird erreicht, indem Sie das Argument "Value" für das erste Vorkommen im angegebenen Muster eines regulären Ausdrucks suchen der `Regex` Konstruktor. Gibt an, ob das Muster für reguläre Ausdrücke in der Eingabezeichenfolge gefunden wurde anhand des Werts, der bestimmt werden kann die `Match` des Objekts `Success` Eigenschaft.

> [!NOTE]
> Überprüfung der Eigenschaften kann manchmal abhängige Eigenschaften umfassen. Ein Beispiel der abhängigen Eigenschaften ist der Satz gültiger Werte für die Eigenschaft ein von der betreffende Wert abhängig ist, die in b-Eigenschaft festgelegt wurde Um sicherzustellen, dass der Wert der Eigenschaft ein, die einer der zulässigen Werte ist enthält z. B. Abrufen des Werts der Eigenschaft b Darüber hinaus, wenn der Wert der Eigenschaft B ändert, müssen Eigenschaft A erneut überprüft werden.

## <a name="adding-validation-rules-to-a-property"></a>Hinzufügen von Validierungsregeln auf eine Eigenschaft

In der mobilen Anwendung "eshoponcontainers", Eigenschaften, die Validierung des Typs deklariert werden `ValidatableObject<T>`, wobei `T` ist der Typ der Daten, die überprüft werden. Das folgende Codebeispiel zeigt ein Beispiel für diese zwei Eigenschaften:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Für die Überprüfung ausgeführt, Validierungsregeln müssen hinzugefügt werden die `Validations` Auflistung der einzelnen `ValidatableObject<T>` -Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Diese Methode fügt die `IsNotNullOrEmptyRule<T>` Überprüfungsregel auf die `Validations` Auflistung der einzelnen `ValidatableObject<T>` Instanz, die Sie Werte für der Validierungsregel angeben `ValidationMessage` -Eigenschaft, die die Überprüfungsfehlermeldung gibt an, der Wenn angezeigt wird Tritt ein Überprüfungsfehler auf.

## <a name="triggering-validation"></a>Auslösen von Validierung

Überprüfung Ansatz wird in der mobilen app von eShopOnContainers kann manuell auslösen, Validierung einer Eigenschaft und Trigger Überprüfung automatisch bei Änderung einer Eigenschaft.

### <a name="triggering-validation-manually"></a>Manuelles Auslösen von Validierung

Überprüfung kann für eine Eigenschaft der Ansicht-Modell manuell ausgelöst werden. Angenommen, dieser Fehler tritt in der mobilen app für "eshoponcontainers" Wenn der Benutzer tippt der **Anmeldung** auf auf die Schaltfläche der `LoginView`, bei der Verwendung mock-Diensten. Der Befehl Delegat ruft die `MockSignInAsync` -Methode in der die `LoginViewModel`, die Ruft die Validierung durch Ausführen der `Validate` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

Die `Validate` Methode führt eine Überprüfung von Benutzername und Kennwort, die vom Benutzer eingegeben wird, für die `LoginView`, durch den Aufruf der Validate-Methode für jede `ValidatableObject<T>` Instanz. Das folgende Codebeispiel zeigt die Validate-Methode aus der `ValidatableObject<T>` Klasse:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Diese Methode löscht die `Errors` Sammlung und anschließend abgerufen, die alle Validierungsregeln, wurden hinzugefügt, um die Objekteigenschaften `Validations` Auflistung. Die `Check` -Methode für jede abgerufene Validierungsregel ausgeführt wird, und die `ValidationMessage` Eigenschaftswert für jede Validierungsregel, die nicht zum Überprüfen der Daten wird hinzugefügt. die `Errors` Auflistung von der `ValidatableObject<T>` Instanz. Zum Schluss die `IsValid` Eigenschaft wird festgelegt, und sein Wert wird zurückgegeben, an die aufrufende Methode, der angibt, ob die Überprüfung erfolgreich war oder nicht.

### <a name="triggering-validation-when-properties-change"></a>Auslösen von Validierung, wenn Eigenschaften ändern

Überprüfung kann auch ausgelöst werden, wenn eine gebundene Eigenschaft ändert. Z. B., wenn eine bidirektionale Bindung in der `LoginView` legt die `UserName` oder `Password` -Eigenschaft, Überprüfung wird ausgelöst. Im folgenden Codebeispiel wird veranschaulicht, wie in diesem Fall:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

Die [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement gebunden wird, die `UserName.Value` Eigenschaft der `ValidatableObject<T>` -Instanz und des Steuerelements `Behaviors` Auflistung verfügt über eine `EventToCommandBehavior` Instanz hinzugefügt wird. Dieses Verhalten führt die `ValidateUserNameCommand` als Reaktion auf die [`TextChanged`] ereignisauslösung der `Entry`, die ausgelöst wird, wenn der Text in die `Entry` Änderungen. Im Gegenzug die `ValidateUserNameCommand` Delegaten ausführt der `ValidateUserName` -Methode, die ausgeführt wird die `Validate` Methode für die `ValidatableObject<T>` Instanz. Daher jedes Mal der Benutzer gibt ein Zeichen in der `Entry` -Steuerelement für den Benutzernamen, die Überprüfung der eingegebenen Daten erfolgt.

Weitere Informationen zu Verhalten finden Sie unter [implementieren Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Anzeigen von Überprüfungsfehlern

Die eShopOnContainers-mobile-app benachrichtigt den Benutzer über Fehler bei der Validierung durch das Steuerelement, die ungültigen Daten mit einer roten Linie enthält, hervorheben und durch eine Fehlermeldung angezeigt, die den Benutzer, warum informieren die Daten sind ungültig, unter das Steuerelement mit dem Ungültige Daten. Bei der die ungültigen Daten behoben wurde, wird die Zeile in Schwarz geändert, und die Fehlermeldung wird entfernt. Abbildung 6 – 2 zeigt die LoginView in der mobilen app für "eshoponcontainers", wenn Validierungsfehler vorhanden sind.

![](validation-images/validation-login.png "Anzeigen von Überprüfungsfehlern bei der Anmeldung")

**Abbildung 6 – 2:** Anzeigen von Überprüfungsfehlern bei der Anmeldung

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Hervorheben eines Steuerelements, die ungültige Daten enthält.

Die `LineColorBehavior` angefügten Verhalten wird verwendet, um hervorzuheben [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelemente, in dem Validierungsfehler aufgetreten. Das folgende Codebeispiel zeigt wie die `LineColorBehavior` angefügten Verhalten angefügt ist, um eine `Entry` Steuerelement:

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

Die [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement nutzt expliziten Stil, die in der im folgenden Codebeispiel gezeigt wird:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Dieser Stil legt die `ApplyLineColor` und `LineColor` angefügten Eigenschaften von der `LineColorBehavior` Verhalten angefügt der [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement. Weitere Informationen zu Formatvorlagen finden Sie unter [Styles (Formatvorlagen)](~/xamarin-forms/user-interface/styles/index.md).

Wenn den Wert des der `ApplyLineColor` angefügte Eigenschaft ist die Menge oder Änderungen der `LineColorBehavior` angefügten Verhalten führt die `OnApplyLineColorChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Geben Sie die Parameter für diese Methode die Instanz des Steuerelements, das das Verhalten zugeordnet ist, und die alten und neuen Werte der `ApplyLineColor` angefügte Eigenschaft. Die `EntryLineColorEffect` Klasse des Steuerelements hinzugefügt [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung Wenn die `ApplyLineColor` angefügte Eigenschaft `true`, andernfalls wird er aus des Steuerelements entfernt `Effects` Auflistung. Weitere Informationen zu Verhalten finden Sie unter [implementieren Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

Die `EntryLineColorEffect` Unterklassen der [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Klasse, und wird im folgenden Codebeispiel dargestellt:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

Die [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Klasse stellt eine plattformunabhängige ab, der einen inneren Effekt umschließt, die plattformspezifische ist. Dadurch wird das Entfernen eines Effekts vereinfacht, da während der Kompilierzeit nicht auf die Typinformationen für einen plattformspezifischen Effekt zugegriffen werden kann. Die `EntryLineColorEffect` ruft der Konstruktor der Basisklasse übergeben eines Parameters, bestehend aus einer Verkettung der Namen der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen-Klasse angegeben wird.

Das folgende Codebeispiel zeigt die `eShopOnContainers.EntryLineColorEffect` Implementierung für iOS:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

Die `OnAttached` Methode ruft das native Steuerelement ab, für die Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) steuern und aktualisiert die Linienfarbe durch Aufrufen der `UpdateLineColor` Methode. Die `OnElementPropertyChanged` Außerkraftsetzung reagiert auf Änderungen der bindbare Eigenschaften auf der `Entry` Steuerelement, indem Sie die Linienfarbe aktualisieren, wenn die angefügte `LineColor` Eigenschaft geändert wird, oder die [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) Eigenschaft der `Entry`Änderungen. Weitere Informationen zu Effekten finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

Wenn gültige Daten eingegeben werden, der [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement, es wird eine schwarze Linie auf den unteren Rand des Steuerelements, um anzugeben, dass keine Validierungsfehler angewendet. Abbildung 6 – 3 zeigt ein Beispiel hierfür.

![](validation-images/validation-blackline.png "Schwarze Linie, der angibt, der kein Validierungsfehler")

**Abbildung 6 – 3**: Schwarze Linie, der angibt, der kein Validierungsfehler

Die [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement verfügt auch über eine [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) hinzugefügt, die [ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers) Auflistung. Das folgende Codebeispiel zeigt die `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Dies [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) überwacht die `UserName.IsValid` -Eigenschaft, und wenn es Wert ist, wird `false`, Ausführen der [ `Setter` ](xref:Xamarin.Forms.Setter), welche Änderungen der `LineColor` angefügt Eigenschaft der `LineColorBehavior` Red Verhalten hinzugefügt. Abbildung 6-4 zeigt ein Beispiel hierfür.

![](validation-images/validation-redline.png "Rote Linie, der angibt, Fehler bei der Validierung")

**Abbildung 6 – 4**: Rote Linie, der angibt, Fehler bei der Validierung

Die Zeile in der [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement wird rot bleiben, während die eingegebenen Daten ungültig sind, ändert es andernfalls in Schwarz festgelegt, um anzugeben, dass die eingegebenen Daten ungültig sind.

Weitere Informationen zu Triggern finden Sie unter [Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Anzeigen von Fehlermeldungen

Die Benutzeroberfläche zeigt Meldungen für Validierungsfehler in Label-Steuerelemente unter jedes Steuerelement, dessen Daten Fehler bei der Überprüfung. Das folgende Codebeispiel zeigt die [ `Label` ](xref:Xamarin.Forms.Label) , tritt ein Validierungsfehler anzeigt, wenn der Benutzer nicht auf einen gültigen Benutzernamen eingegeben hat:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Jede [ `Label` ](xref:Xamarin.Forms.Label) bindet an die `Errors` Eigenschaft das ViewModel-Objekt, das überprüft wird. Die `Errors` Eigenschaft wird bereitgestellt, durch die `ValidatableObject<T>` Klasse, und weist den Typ `List<string>`. Da die `Errors` -Eigenschaft kann mehrere Validierungsfehler, enthalten die `FirstValidationErrorConverter` Instanz wird verwendet, um den ersten Fehler aus der Auflistung für die Anzeige abzurufen.

## <a name="summary"></a>Zusammenfassung

Die eShopOnContainers-mobile-app führt die synchronen die clientseitige Validierung von Eigenschaften und benachrichtigt den Benutzer über Validierungsfehler aus, durch das Steuerelement, das die ungültigen Daten enthält, hervorheben und Anzeigen von Fehlermeldungen, die den Benutzer zu informieren Warum die Daten ungültig sind.

Eigenschaften, für die Validierung erforderlich sind, vom Typ `ValidatableObject<T>`, und jede `ValidatableObject<T>` Instanz verfügt über Regeln zur Überprüfung hinzugefügt, die `Validations` Eigenschaft. Überprüfung von des Ansichtsmodells aufgerufen wird, durch den Aufruf der `Validate` -Methode der der `ValidatableObject<T>` -Instanz, die die Überprüfung ruft Regeln und führt sie anhand der `ValidatableObject<T>` `Value` Eigenschaft. Validierungsfehler befinden sich in der `Errors` Eigenschaft der `ValidatableObject<T>`-Instanz und die `IsValid` Eigenschaft der `ValidatableObject<T>` Instanz wird aktualisiert, um anzugeben, ob die Überprüfung erfolgreich war oder nicht.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
