# Documentation
## Applying clean code principles
This section focuses on applying clean code principles to the code we wrote in [week 2](./week2_setup.md).

#### Dependency Inversion Principle
The Dependency Inversion Principle suggest that we should not depend on the specific implementation of lower level module but should instead depend on the abstraction.

In the code from week 2 we instanciate the class `ContinentService` in the constructor. Instead we should use dependency injection of the interface so we only need to depend on the abstraction instead of the concrete service class.

##### Old Code
```csharp
    private IContinentService _continentService;

    public ContinentPage() {
        InitializeComponent();
        this.BindingContext = this;
        this._continentService = new ContinentService();

        Task.Run(async () => await LoadContinents());
        txe_continent.Text = "";
    }
```

##### Fixed Code
```csharp
    private IContinentService _continentService;

    public ContinentPage(IContinentService continentService) {
        InitializeComponent();
        this.BindingContext = this;
        this._continentService = continentService;

        Task.Run(async () => await LoadContinents());
        txe_continent.Text = "";
    }
```


#### DRY
The DRY (Don't Repeat Yourself) principle suggest that we should never write the same or similar code in two different places but instead use methods or parent classes to make the code reusable. Otherwise, if we would have to make any changes we would need to make them in several places.

In this example we are using the same two lines of code at the end of both methods. To fix it we simply need to create a new private method with those two lines and call it at the bottom of the initial methods.

##### Old Code
```csharp
private void SaveButton_Clicked(object sender, EventArgs e) {
    if (String.IsNullOrEmpty(txe_continent.Text)) return;

    if(selectedContinent == null) {
        var continent = new Continent() { Name=txe_continent.Text};
        continentService.AddContinent(continent);
        continents.Add(continent);
    } else {
        selectedContinent.Name = txe_continent.Text;
        continentService.UpdateContinent(selectedContinent);
        var continent = continents.FirstOrDefault(x => x.ID == selectedContinent.ID);
        continent.Name = txe_continent.Text;
    }

    
    selectedContinent = null;
    ltv_continents.SelectedItem = null;
    txe_continent.Text = "";
}

private async void DeleteButton_Clicked(object sender, EventArgs e) {
    if(ltv_continents.SelectedItem == null) {
        await Shell.Current.DisplayAlert("No Continent Selected", "Select the continent you want to delete from the list", "OK");
        return;
    }

    await continentService.DeleteContinent(selectedContinent);
    continents.Remove(selectedContinent);

    ltv_continents.SelectedItem = null;
    txe_continent.Text = "";
}
```

##### Fixed Code
```csharp
private void SaveButton_Clicked(object sender, EventArgs e) {
    if (String.IsNullOrEmpty(txe_continent.Text)) return;

    if(selectedContinent == null) {
        var continent = new Continent() { Name=txe_continent.Text};
        continentService.AddContinent(continent);
        continents.Add(continent);
    } else {
        selectedContinent.Name = txe_continent.Text;
        continentService.UpdateContinent(selectedContinent);
        var continent = continents.FirstOrDefault(x => x.ID == selectedContinent.ID);
        continent.Name = txe_continent.Text;
    }

    
    selectedContinent = null;
    
    SetContinentSelectionNull();
}

private async void DeleteButton_Clicked(object sender, EventArgs e) {
    if(ltv_continents.SelectedItem == null) {
        await Shell.Current.DisplayAlert("No Continent Selected", "Select the continent you want to delete from the list", "OK");
        return;
    }

    await continentService.DeleteContinent(selectedContinent);
    continents.Remove(selectedContinent);

    SetContinentSelectionNull();
}

private void SetContinentSelectionNull() {
    ltv_continents.SelectedItem = null;
    txe_continent.Text = "";
}
```



#### Single Responsibility Principle
The Single Responsibility Principle suggest that one thing should only be responsibly to do one task (have responsibility). That means a class or object should only do one thing but that thing well.

In our code from week 2 we can see that our `ContinentService` sets up a new `SQLiteAsyncConnection` every time it does an operation. The ContinentService should not be responsible for creating the database connection. Instead, we should inject the database connection as a singleton into the constructor of the service and make it reusable but give the responsibility of establishing the connection to someone else.

##### Old Code
```csharp
public class ContinentService : IContinentService
{
    private SQLiteAsyncConnection _dbConnection;

    private async Task SetUpDb() {
        if (_dbConnection != null)
            return;
        

        _dbConnection = new SQLiteAsyncConnection(DatabaseSettings.DBPath, DatabaseSettings.Flags);
        await _dbConnection.CreateTableAsync<Continent>();
    }

    public async Task<int> AddContinent(Continent continent) {
        await SetUpDb();
        return await _dbConnection.InsertAsync(continent);
         
    }

    ...
}
```

##### Fixed Code
```csharp
public class ContinentService : IContinentService
{
    private SQLiteAsyncConnection _dbConnection;

    public ContinentService(SQLiteAsyncConnection dbConnection) {
        _dbConnection = dbConnection;
        await _dbConnection.CreateTableAsync<Continent>();
    }

    public async Task<int> AddContinent(Continent continent) {
        return await _dbConnection.InsertAsync(continent);
    }

    ...
}
```

#### Naming Convention
It is important to have clear and consitent naming conventions for variables, methods, classes and interfaces. If we name everything correctly other programmers can orient themselve better in our codebase.

In our code from week two we have a private variable `selectedContinent`. It is almost the correct name but in C# it is [convention](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/identifier-names#camel-case) to name private variables with an prefix underscore.

##### Old Code
```csharp
public partial class ContinentPage : ContentPage {

    private Continent selectedContinent = null;

    ...
}
```

##### Fixed Code
```csharp
public partial class ContinentPage : ContentPage {

    private Continent _selectedContinent = null;

    ...
}
```



## Doxygen comments
For automatic code documentation we can add [Doxygen](https://www.doxygen.nl/) comments to our code.

Doxygen comments are unique comment blocks that start with `/*!` and end with `*/`. They can be multiline.
Inside the comment block we can use html-like tags to describe the code. Commonly used tags are `summary`, `returns` and `param`. For more information have at look in the [documentation](https://www.doxygen.nl/manual/index.html).

Here are some examples of Doxygen comments.

#### Interface
Briefly describes what the interface is for, what the methods should do and what the methods return.
```csharp
   /*! <summary>
       Interface that exposes methods of ContinentService
   </summary> */
   public interface IContinentService
   {
       /*! <summary>
        Method responsible for quering database, for entries in Continent table
       </summary> 
        <returns>Returns Task containing List of Continents present in the database.</returns>*/
       Task<List<Continent>> GetContinentList();

       /*! <summary>
           Method responsible for addtion of entry into Continent table.
       </summary> 
        <param name="org">Continent class instance that is attempted to be inserted into Continent table.</param>
        <returns>Returns Task containing number of rows inserted into Continent table.</returns> */
       Task<int> AddContinent(Continent continent);

       ...
   }
```

#### Class
Similar to the interface the comments briefly describe what the class is for. Additionally to comments over methods we also have comments over variables to describe what they are for.
```csharp
/*! <summary>
    ContinentService extending IContinentService Interface
</summary> */
public class ContinentService : IContinentService
{
    /*! <summary>
        Variable storing dbConnection to SQLite database.
    </summary> */
    private SQLiteAsyncConnection _dbConnection;

    /*! <summary>
        Method that initiates connection to SQLite database, and creates Continent class table, if none is present.
    </summary> */
    private async Task SetUpDb() {
        if (_dbConnection != null)
            return;
        

        _dbConnection = new SQLiteAsyncConnection(DatabaseSettings.DBPath, DatabaseSettings.Flags);
        await _dbConnection.CreateTableAsync<Continent>();
    }

...
}
```

## Clean Code
Here are some examples of clean code that do not require any comments because the code itself is descriptive enought.

#### Example 1: Method with naming conventions
The method and variable names in this example are descriptive enough so we do not need any more methods.

```csharp
private void ltv_continents_ItemSelected(object sender, SelectedItemChangedEventArgs e) {
        selectedContinent = e.SelectedItem as Continent;
        if (selectedContinent == null) return;

        txe_continent.Text = selectedContinent.Name;
    }
```

#### Example 2: Model class with namespace
Because of the namespace in the first line we know that this class is supposed to be a model class, hence we do not need any more description with comments. Additionally, all the members have clearly defined names.

```csharp
namespace MauiApp1.Models {
    public class Continent : INotifyPropertyChanged {
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        private string name;
        public string Name {
            get => name;
            set => SetField(ref name, value);
        }

        public event PropertyChangedEventHandler? PropertyChanged;

        protected void OnPropertyChanged(string propertyName) =>
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));

        protected bool SetField<T>(ref T field, T value, [CallerMemberName] string propertyName = "") {
            if (EqualityComparer<T>.Default.Equals(field, value)) return false;
            field = value;
            OnPropertyChanged(propertyName);
            return true;
        }
    }
}
```

#### Example 2: Interface with naming convention
Similar to the examples above we do not need any comments to make the reader understand what is going on. The name of the interface clarifies that the interface is used to expose the methods of the ContinentService. We also have clear an consitently named methods inside the interface.

```csharp
    public interface IContinentService
    {
        Task<List<Continent>> GetContinentList();

        Task<int> AddContinent(Continent continent);

        Task<int> DeleteContinent(Continent continent);

        Task<int> UpdateContinent(Continent continent);
    }
```

___
This section is related to your work on clean code and documentation in week 5.

First, choose six rules of clean code and explain them. For each one,

* Summarise the rule in your own words.
* Provide an example from the code that you wrote in week 2 and then refined in week 4.
* Explain how your code implements the rule. 

Second, copy the doxygen comments from your code into your portfolio and provide some 
descriptive commentary on their purpose and structure. Use screenshots showing the HTML 
content that is generated from your code to illustrate your explanation.

Finally, highlight three examples from your code where you have eliminated the need
for comments by adhering to the principles of clean code.
 
