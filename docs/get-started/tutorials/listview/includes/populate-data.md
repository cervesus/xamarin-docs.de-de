Eine [`ListView`](xref:Xamarin.Forms.ListView)-Klasse wird unter Verwendung der Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource), die vom Typ `IEnumerable` ist, mit Daten aufgefüllt. Im vorherigen Schritt wurde die `ListView`-Klasse in XAML mit einem Zeichenfolgenarray aufgefüllt. Eine `ListView`-Klasse wird jedoch üblicherweise mit Daten aus einer Sammlung aufgefüllt, die im CodeBehind definiert ist, der `IEnumerable` implementiert.

In dieser Übung ändern Sie das Projekt **ListViewTutorial** so, dass die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse mit Daten aus einer Sammlung von Objekten aufgefüllt wird, die in einer `List`-Klasse gespeichert ist.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Fügen Sie im **Projektmappen-Explorer** im Projekt **ListViewTutorial** eine Klasse mit dem Namen `Monkey` hinzu, die den folgenden Code enthält:

    ```csharp
    public class Monkey
    {
        public string Name { get; set; }
        public string Location { get; set; }
        public string ImageUrl { get; set; }

        public override string ToString()
        {
            return Name;
        }
    }
    ```

    Dieser Code definiert ein `Monkey`-Objekt, das den Namen, den Speicherort und die URL eines Images speichert, das das Monkey-Objekt repräsentiert. Zudem überschreibt die Klasse die `ToString`-Methode, um die `Name`-Eigenschaft zurückzugeben.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **ListViewTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace ListViewTutorial
    {
        public partial class MainPage : ContentPage
        {
            public IList<Monkey> Monkeys { get; private set; }

            public MainPage()
            {
                InitializeComponent();

                Monkeys = new List<Monkey>();
                Monkeys.Add(new Monkey
                {
                    Name = "Baboon",
                    Location = "Africa & Asia",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Capuchin Monkey",
                    Location = "Central & South America",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Blue Monkey",
                    Location = "Central and East Africa",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Squirrel Monkey",
                    Location = "Central & South America",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Saimiri_sciureus-1_Luc_Viatour.jpg/220px-Saimiri_sciureus-1_Luc_Viatour.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Lion Tamarin",
                    Location = "Brazil",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Golden_lion_tamarin_portrait3.jpg/220px-Golden_lion_tamarin_portrait3.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Howler Monkey",
                    Location = "South America",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/Alouatta_guariba.jpg/200px-Alouatta_guariba.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Japanese Macaque",
                    Location = "Japan",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Macaca_fuscata_fuscata1.jpg/220px-Macaca_fuscata_fuscata1.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Mandrill",
                    Location = "Southern Cameroon, Gabon, Equatorial Guinea, and Congo",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Mandrill_at_san_francisco_zoo.jpg/220px-Mandrill_at_san_francisco_zoo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Proboscis Monkey",
                    Location = "Borneo",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Proboscis_Monkey_in_Borneo.jpg/250px-Proboscis_Monkey_in_Borneo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Red-shanked Douc",
                    Location = "Vietnam, Laos",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Portrait_of_a_Douc.jpg/159px-Portrait_of_a_Douc.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gray-shanked Douc",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Cuc.Phuong.Primate.Rehab.center.jpg/320px-Cuc.Phuong.Primate.Rehab.center.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg/165px-Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Black Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/RhinopitecusBieti.jpg/320px-RhinopitecusBieti.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Tonkin Snub-nosed Monkey",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg/320px-Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Thomas's Langur",
                    Location = "Indonesia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Thomas%27s_langur_Presbytis_thomasi.jpg/142px-Thomas%27s_langur_Presbytis_thomasi.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Purple-faced Langur",
                    Location = "Sri Lanka",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Semnopithèque_blanchâtre_mâle.JPG/192px-Semnopithèque_blanchâtre_mâle.JPG"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gelada",
                    Location = "Ethiopia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Gelada-Pavian.jpg/320px-Gelada-Pavian.jpg"
                });

                BindingContext = this;
            }
        }
    }
    ```

    Dieser Code definiert eine `Monkeys`-Eigenschaft vom Typ `IList<Monkey>` und initialisiert sie in eine leere Liste im Klassenkonstruktor. Dann werden `Monkey`-Objekte der Sammlung `Monkeys` hinzugefügt, und die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite wird auf das `MainPage`-Objekt festgelegt. Weitere Informationen zur `BindingContext`-Eigenschaft finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md) im Abschnitt [Bindungen mit Bindungskontext](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context).

    > [!IMPORTANT]
    > Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) wird über die visuelle Struktur vererbt. Da sie auf das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt festgelegt wurde, erben deshalb untergeordnete Objekte von `ContentPage` ihren Wert, darunter auch die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse.

1.  Ändern Sie in **MainPage.xaml** die [`ListView`](xref:Xamarin.Forms.Image)-Deklaration, um die Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) auf die Sammlung `Monkeys` festzulegen:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}" />
    ```

    Dieser Code stellt eine Datenbindung zwischen der Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) und der Sammlung `Monkeys` her. Zur Laufzeit sucht die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse in ihrer [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft nach der Sammlung `Monkeys` und wird mit Daten aus dieser Sammlung aufgefüllt. Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Eine ListView-Klasse wird unter iOS und Android mit Daten aus einer Sammlung aufgefüllt](../images/populate-data.png "ListView mit Daten aus einer Sammlung")](../images/populate-data-large.png#lightbox "ListView mit Daten aus einer Sammlung")

    Die Klasse [`ListView`](xref:Xamarin.Forms.ListView) zeigt die Eigenschaft `Name` für jedes `Monkey`-Objekt in der `Monkeys`-Sammlung an. Dies liegt daran, dass in der Standardeinstellung die `ListView`-Klasse die `ToString`-Methode aufruft, wenn sie die Objekte aus einer Sammlung anzeigt (dies wurde in der `Monkey`-Klasse überschrieben, um den Eigenschaftswert `Name` zurückzugeben).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Fügen Sie im **Lösungspad** im Projekt **ListViewTutorial** eine Klasse mit dem Namen `Monkey` hinzu, die den folgenden Code enthält:

    ```csharp
    public class Monkey
    {
        public string Name { get; set; }
        public string Location { get; set; }
        public string ImageUrl { get; set; }

        public override string ToString()
        {
            return Name;
        }
    }
    ```

    Dieser Code definiert ein `Monkey`-Objekt, das den Namen, den Speicherort und die URL eines Images speichert, das das Monkey-Objekt repräsentiert. Zudem überschreibt die Klasse die `ToString`-Methode, um die `Name`-Eigenschaft zurückzugeben.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **ListViewTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace ListViewTutorial
    {
        public partial class MainPage : ContentPage
        {
            public IList<Monkey> Monkeys { get; private set; }

            public MainPage()
            {
                InitializeComponent();

                Monkeys = new List<Monkey>();
                Monkeys.Add(new Monkey
                {
                    Name = "Baboon",
                    Location = "Africa & Asia",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Capuchin Monkey",
                    Location = "Central & South America",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Blue Monkey",
                    Location = "Central and East Africa",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Squirrel Monkey",
                    Location = "Central & South America",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Saimiri_sciureus-1_Luc_Viatour.jpg/220px-Saimiri_sciureus-1_Luc_Viatour.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Lion Tamarin",
                    Location = "Brazil",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Golden_lion_tamarin_portrait3.jpg/220px-Golden_lion_tamarin_portrait3.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Howler Monkey",
                    Location = "South America",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/Alouatta_guariba.jpg/200px-Alouatta_guariba.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Japanese Macaque",
                    Location = "Japan",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Macaca_fuscata_fuscata1.jpg/220px-Macaca_fuscata_fuscata1.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Mandrill",
                    Location = "Southern Cameroon, Gabon, Equatorial Guinea, and Congo",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Mandrill_at_san_francisco_zoo.jpg/220px-Mandrill_at_san_francisco_zoo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Proboscis Monkey",
                    Location = "Borneo",
                    ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Proboscis_Monkey_in_Borneo.jpg/250px-Proboscis_Monkey_in_Borneo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Red-shanked Douc",
                    Location = "Vietnam, Laos",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Portrait_of_a_Douc.jpg/159px-Portrait_of_a_Douc.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gray-shanked Douc",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Cuc.Phuong.Primate.Rehab.center.jpg/320px-Cuc.Phuong.Primate.Rehab.center.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg/165px-Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Black Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/RhinopitecusBieti.jpg/320px-RhinopitecusBieti.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Tonkin Snub-nosed Monkey",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg/320px-Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Thomas's Langur",
                    Location = "Indonesia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Thomas%27s_langur_Presbytis_thomasi.jpg/142px-Thomas%27s_langur_Presbytis_thomasi.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Purple-faced Langur",
                    Location = "Sri Lanka",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Semnopithèque_blanchâtre_mâle.JPG/192px-Semnopithèque_blanchâtre_mâle.JPG"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gelada",
                    Location = "Ethiopia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Gelada-Pavian.jpg/320px-Gelada-Pavian.jpg"
                });

                BindingContext = this;
            }
        }
    }
    ```

    Dieser Code definiert eine `Monkeys`-Eigenschaft vom Typ `IList<Monkey>` und initialisiert sie in eine leere Liste im Klassenkonstruktor. Dann werden `Monkey`-Objekte der Sammlung `Monkeys` hinzugefügt, und die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite wird auf das `MainPage`-Objekt festgelegt. Weitere Informationen zur `BindingContext`-Eigenschaft finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md) im Abschnitt [Bindungen mit Bindungskontext](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context).

    > [!IMPORTANT]
    > Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) wird über die visuelle Struktur vererbt. Da sie auf das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt festgelegt wurde, erben deshalb untergeordnete Objekte von `ContentPage` ihren Wert, darunter auch die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse.

1.  Ändern Sie in **MainPage.xaml** die [`ListView`](xref:Xamarin.Forms.Image)-Deklaration, um die Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) auf die Sammlung `Monkeys` festzulegen:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}" />
    ```

    Dieser Code stellt eine Datenbindung zwischen der Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) und der Sammlung `Monkeys` her. Zur Laufzeit sucht die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse in ihrer [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft nach der Sammlung `Monkeys` und wird mit Daten aus dieser Sammlung aufgefüllt. Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    [![Screenshot: Eine ListView-Klasse wird unter iOS und Android mit Daten aus einer Sammlung aufgefüllt](../images/populate-data.png "ListView mit Daten aus einer Sammlung")](../images/populate-data-large.png#lightbox "ListView mit Daten aus einer Sammlung")

    Die Klasse [`ListView`](xref:Xamarin.Forms.ListView) zeigt die Eigenschaft `Name` für jedes `Monkey`-Objekt in der `Monkeys`-Sammlung an. Dies liegt daran, dass in der Standardeinstellung die `ListView`-Klasse die `ToString`-Methode aufruft, wenn sie die Objekte aus einer Sammlung anzeigt (dies wurde in der `Monkey`-Klasse überschrieben, um den Eigenschaftswert `Name` zurückzugeben).
