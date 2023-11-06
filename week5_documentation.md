# Documentation

##Part 1 - Rewrites of previous code to adhere to conventions
*These 6 examples each depict an explanation of a clean code rule/convention which wasn't followed in my earlier code submission, followed by a snippet of said code, a snippet of the rewrite, and an explanation as to why the rewrite better adheres to the aforementioned convention.*

**1) Rule: Internal Quality Criteria - Exception handling**

<u>Description:</u> Exception handling is an incredibly important aspect of writing good, robust code. It essentially means that if something doesn't behave or appear in the way initially expected (such as if a file is meant to be read in, but the file can't be found), then instead of the entire program collapsing, then the program will continue, and potentially even display a user-friendly error message, as this possibility has been thought of. Additionally, exception handling can include logging messages for future debugging/diagnostics, making the maintenance and understanding of the software easier to achieve.
 
<u>Previous Snippet:</u> 
```csharp
 public async Task<int> AddRota(Rota rota)
 {
     await SetUpDb();
     return await _dbConnection.InsertAsync(rota);
 }

```
<u>Rewrite:</u> 

```csharp
 public async Task<int> AddRota(Rota rota)
    {
        try
        {
            await SetUpDb();
            return await _dbConnection.InsertAsync(rota);
        }
        catch (Exception ex)
        {
            throw;
        }
    }

```
<u>Explanation: </u>This rewrite is superior as it ensures that even if there is an issue with either the setting up of the database, or the inserting of the Rota itself, then the program will continue without issue, fulfilling the clean code practice of having error handling included.


**2) Rule: Long Function**

<u>Description:</u> Long function is a code smell which occurs when a method is simply too long. This trait can lead to code being hard to understand, as it becomes challenging to understand the purpose of the method. They also become difficult to maintain, as editing long methods is usually riskier than shorter ones, as you run the risk of creating more issues than you are fixing/solving. Finally, long functions aren't usually as reusable, as the chances of another part of the program needing the exact functionality within the method decrease with each line. 

<u>Previous Snippet:</u> 

```csharp
 private void SaveButton_Clicked(object sender, EventArgs e)
 {
     if (String.IsNullOrEmpty(txe_rota.Text)) return;

     if (selectedRota == null)
     {
         var rota = new Rota() { Name = txe_rota.Text };
         rotaService.AddRota(rota);
         rotas.Add(rota);
     }
     else
     {
         selectedRota.Name = txe_rota.Text;
         rotaService.UpdateRota(selectedRota);
         var rota = rotas.FirstOrDefault(x => x.ID == selectedRota.ID);
         rota.Name = txe_rota.Text;
     }

     selectedRota = null;
     ltv_rotas.SelectedItem = null;
     txe_rota.Text = "";

 }

```
<u>Rewrite:</u>

```csharp
private void SaveButton_Clicked(object sender, EventArgs e)
{
    if (String.IsNullOrEmpty(txe_rota.Text)) return;

    if (selectedRota == null)
    {
        AddNewRota();
    }
    else
    {
        UpdateExistingRota();
    }

    ClearUIComponents();
}

private void AddNewRota()
{
    var newRota = CreateRotaFromInput();
    rotaService.AddRota(newRota);
    rotas.Add(newRota);
}

private void UpdateExistingRota()
{
    selectedRota.Name = txe_rota.Text;
    rotaService.UpdateRota(selectedRota);
    var existingRota = rotas.FirstOrDefault(x => x.ID == selectedRota.ID);
    existingRota.Name = txe_rota.Text;
}

private Rota CreateRotaFromInput()
{
    return new Rota() { Name = txe_rota.Text };
}e

private void ClearUIComponents()  
{
    selectedRota = null;
    ltv_rotas.SelectedItem = null;
    txe_rota.Text = "";
}

```
<u>Explanation: </u>Even though there are more methods and chunks, the code is now more reusable accross the program, easier to maintain and understand, and generally a lot clearer to those new to the codebase. It has clear and meaningful names for all methods, which  fully explain what is happening in the method. Additionally, each small block of code is now far easier to test properly and carefully ensure works exactly as it should. All of this combined ensures it follows the principle to a much higher degree.


**3) Rule: Single responsibility Principle - SRP** 

<u>Description:</u> The SRP essentially states that each class (or method) should ideally have only one single responsibility, which it should do very well. This again leads to more maintable and understandable code, which is also easier to test. 

<u>Previous Snippet:</u> 
```csharp
private void SaveButton_Clicked(object sender, EventArgs e)
 {
     if (String.IsNullOrEmpty(txe_rota.Text)) return;

     if (selectedRota == null)
     {
         var rota = new Rota() { Name = txe_rota.Text };
         rotaService.AddRota(rota);
         rotas.Add(rota);
     }
     else
     {
         selectedRota.Name = txe_rota.Text;
         rotaService.UpdateRota(selectedRota);
         var rota = rotas.FirstOrDefault(x => x.ID == selectedRota.ID);
         rota.Name = txe_rota.Text;
     }

     selectedRota = null;
     ltv_rotas.SelectedItem = null;
     txe_rota.Text = "";

 }

```
<u>Rewrite:</u>

```csharp
private void SaveButton_Clicked(object sender, EventArgs e)
{
    if (String.IsNullOrEmpty(txe_rota.Text)) return;

    if (selectedRota == null)
    {
        AddNewRota();
    }
    else
    {
        UpdateExistingRota();
    }

    ClearUIComponents();
}

private void AddNewRota()
{
    var newRota = CreateRotaFromInput();
    rotaService.AddRota(newRota);
    rotas.Add(newRota);
}

private void UpdateExistingRota()
{
    selectedRota.Name = txe_rota.Text;
    rotaService.UpdateRota(selectedRota);
    var existingRota = rotas.FirstOrDefault(x => x.ID == selectedRota.ID);
    existingRota.Name = txe_rota.Text;
}

private Rota CreateRotaFromInput()
{
    return new Rota() { Name = txe_rota.Text };
}

private void ClearUIComponents()
{
    selectedRota = null;
    ltv_rotas.SelectedItem = null;
    txe_rota.Text = "";
}

```
<u>Explanation: </u>This rewrite is much improved as now the one method which tried to accomplish several things (checking the new string isn't empty, checking if this is a new or updated "Rota", adding it to the database or updating the value respectively), has been split into several different, smaller methods which are a lot better defined, each of which having one firm purpose. This ensures that if anything is ever needing changed, it will be a simple tweak and fix to complete, and those new to the codebase won't struggle to understand a method with multiple resposibilities and what effect it has on the program.

**4) Rule: Naming conventions**

<u>Description:</u> Naming conventions are an important thing in coding, and wield a lot of power.  Using misleading or non-descriptive names can cause a lot of confusion, or ultimately cause the reader to miss out on assistance in understanding the code base, in that if they are accurately named (ie. calling a list of subjects "subject list" instead of "my favourite apples"), allows those maintaining or adding to the code base to understand exactly what each part does. Furthermore, there are different classes of styles (camel case, snake case, etc), and different styles of capitalisation used across various languages and tiers within. For example, in java class names should always be capitalised, and variables not. Finally, different companies having different conventions, and maintaining one style accross the team/department/company lends itself to a neater, more comprehensive codebase.

Within my code I had several examples of naming conventions not being followed, and also the associated names not being descriptive enough. 

<u>Previous Snippet:</u> 

```csharp
 <Editor x:Name="txe_rota"                 
                Placeholder="Add a Rota"
                HeightRequest="100" />
```
<u>Rewrite:</u>

```csharp
<Editor x:Name="RotaNameTextEditor"
        Placeholder="Add a Rota"
        HeightRequest="100" />

```
<u>Explanation: </u>The rewrite is more appropriate because in XAML, elements follow CamelCase, where the first letter is capitalised, without an underscore. Additionally, it is clear to anyone using this system, that RotaEditor is the element which edits the rotas stored in the data base (or adding a new one). 


**5) Rule: DRY - Don't Repeat Yourself**

<u>Description:</u> This rule is a way of keeping code clean and consise, but attempting to reduce code duplication. It essenitally means that your code should consist of reusable blocks, so that methods and proccesses dont need to include repeated functions as they can call a pre-made one instead.

In my code I wrote a method to clear 3 values used or manipulated by the UI:

<u>Previous Snippet:</u> 
```csharp
private void ClearUIComponents()
        {
            _selectedRota = null;
            RotaListView.SelectedItem = null;
            RotaNameEditor.Text = "";
        }
        
```
And in another method, I did this manually: 

```csharp
 private async void OnDeleteButtonClicked(object sender, EventArgs e)
        {
            if (RotaListView.SelectedItem == null)
            {
                await DisplayAlert("No Rota Selected", "Select the rota you want to delete from the list", "OK");
                return;
            }

            await _rotaService.DeleteRota(_selectedRota);
            _rotaCollection.Remove(_selectedRota);
            RotaListView.SelectedItem = null;
	    _selectedRota = null;
            RotaNameEditor.Text = "";
        }

```
<u>Rewrite:</u>

```csharp
private async void OnDeleteButtonClicked(object sender, EventArgs e)
{
    if (RotaListView.SelectedItem == null)
    {
        await DisplayAlert("No Rota Selected", "Select the rota you want to delete from the list", "OK");
        return;
    }

    await _rotaService.DeleteRota(_selectedRota);
    _rotaCollection.Remove(_selectedRota);

    ClearUIComponents();
}
```

<u>Explanation: </u>I rewrote the method in question "OnDeleteButtonClicked()" to instead simply call the method "ClearUIComponents", which is better as it saves several lines of redundant code. This ensures the principle is followed as it doesn't repeat itself, or host any redundant lines, improving efficency. 

**6) Rule: Dependency Injection**

<u>Description:</u> Dependency Injection is a design pattern where necessary components (for example, a database manager) are provided (typically in the consturctor of an object) to an object, instead of the object needing to create them within themselves. It allows components to be less strongly coupled up, so elements within can be replaced or switched out without issue.

<u>Previous Snippet:</u> 
```csharp
private Rota _selectedRota = null;
        private IRotaService _rotaService;
        private ObservableCollection<Rota> _rotaCollection = new ObservableCollection<Rota>();

        public RotaPage()
        {
            InitializeComponent();
            this.BindingContext = this;
            this._rotaService = new RotaService();

            Task.Run(async () => await LoadRotas());
            RotaNameEditor.Text = "";
        }
```
<u>Rewrite:</u>
```csharp
private Rota _selectedRota = null;
    private IRotaService _rotaService;
    private ObservableCollection<Rota> _rotaCollection = new ObservableCollection<Rota>();

 public RotaPage(IRotaService rotaService)
    {
        InitializeComponent();
        this.BindingContext = this;
        this._rotaService = rotaService;

        Task.Run(async () => await LoadRotas());
        RotaNameEditor.Text = "";
    }
```
<u>Explanation: </u>This rewrite is better as it loosens the coupling between RotaPage and IRotaService by not directly creating a dependency. It also makes the system more flexible, as it can use different implementations of IRotaService without disrupting the functionality of RotaPage. Finally, it aligns with the Diversion Inversion Principle better, as high-level modules (like RotaPage) should not depend on low-level modules (like RotaService). By using DI, I depended on an abstraction (IRotaService), not a concrete implementation, adhering to this principle.

##Part 2 - Doxygen Comments 

*I added a lot of comments, so I have decided to only include 3 examples of Doxygen comments, with a description of the effect they have, and screenshots from the HTML they generate:* 

**1)** 
```
/// <summary>
/// Represents a Rota object.
/// </summary>
public class Rota : INotifyPropertyChanged
{
...
```
This comment provides a high-level description of the Rota class, indicating that it represents an object related to rotas. It serves as an introductory explanation of the class's purpose and role within the application.

Output:

 ![1](/images/represent.png)|
  |:--:|
  Fig 1 - Rota Object


**2)** 
```csharp
/// <summary>
/// Gets or sets the name of the Rota.
/// </summary>
public string Name
{
    get => name;
    set => SetField(ref name, value);
}
```
This comment provides information about the Name property, specifying that it represents the name of a "Rota" object. It explains that the property can be both read and written, giving context to how it can be used within the program.

Output:

 ![2](/images/getset.png)|
  |:--:|
  Fig 2 - Gets and Sets

**3)** 
```csharp
/// <summary>
/// Notifies clients that a property value has changed.
/// </summary>
/// <param name="propertyName">The name of the property that changed.</param>
protected void OnPropertyChanged(string propertyName) =>
    PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
```

This comment describes the purpose of the OnPropertyChanged method, indicating that it's responsible for notifying clients when a property value has changed. Additionally, it explains the parameter propertyName and its role in the method.

Output: 

 ![3](/images/valueChanged.png)|
  |:--:|
  Fig 3 - OnPropertyChanged 

##Part 3 - Clean Code Preventing the Need for Comments

*Each of the 3 examples here shows how adhering to the principles of clean code has negated any need for comments;*

**1) Clear method names:** The method name ClearUIComponents is descriptive of its purpose, which means it's unnecessary to add comments explaining its functionality, as the name itself provides clear documentation.

```csharp
private void ClearUIComponents()
{ 
    RotaListView.SelectedItem = null;
    RotaNameEditor.Text = "";
    _selectedRota = null;
}
```
**2) Descriptive Variable Names:** The variable name _selectedRota is self-explanatory and clearly indicates that it represents the currently selected Rota objects, eliminating the need for comments because it's immediately evident from the variable name what it represents.
```
private Rota _selectedRota = null;
```
**3) Use of Exception Handling and Logging:** The code's structure and the "WriteLine" together show that any exceptions thrown during database operations will be logged to the console, which serves as clear documentation of error handling behavior, negating any need for comments.
```csharp
public async Task<int> AddRota(Rota rota)
{
    try
    {
        await SetUpDb();
        return await _dbConnection.InsertAsync(rota);
    }
    catch (Exception ex)
    {
       await Shell.Current.DisplayAlert("Rota couldn't be inserted to Database");
    }
}
```