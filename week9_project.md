

# Project work 1

## Descriptive summary

My Jira is "As an UNDAC Team Leader I want to view security alerts so that I can inform appropriate partners", meaning that I needed to build a new "Security Alert" model class, and a very basic view page of CRUD operations to add an alert. Additionally I needed to add functionality for an actual "alert" pop-up and sound, which could be snoozed or marked as resolved. I also then needed to display two sets of alerts: current and resolved, so the model class needed to contain a "resolved" boolean property. Finally, I needed to add a filter for the view, which was to be clearable.


## Snippets from my code with commentary showing how I have used good software design practice.


#### Task


#### Snippet

```csharp
 private async void CreateAlertButton_Clicked(object sender, EventArgs e)
{
    if (string.IsNullOrEmpty(txe_securityAlert.Text))
        return;

    var securityAlert = CreateSecurityAlert(txe_securityAlert.Text);

    _currentAlerts.Insert(0, securityAlert);
    txe_securityAlert.Text = "";

    await DisplaySecurityAlerts(securityAlert);
}
```
#### Explanation of good design practice 

The method begins with a check to ensure that the txe_securityAlert.Text is not empty. If it's empty, the method returns then, avoiding unnecessary processing or issues. This is a good practice to handle edge cases and ensure the method is robust. Then the method follows a clear separation of concerns, creating a new security alert, updating _currentAlerts, and then calling the method to display the alert. Each step has a distinct responsibility, making the code more maintainable and easy to follow and read without comments, without violating SRP. 

##### Task:

 I created a snooze function, as per requirements, to snooze the alerts as they pop up, and so they pop up again at the end of the snooze timer.

#### Snippet:
##### C#: 
```csharp
 private async void SnoozeButton_Clicked(object sender, EventArgs e)
{
    if (_selectedAlert != null)
    {
        btnSnooze.IsVisible = false;
        btnResolve.IsVisible = false;
        _snoozeTokenSource = new CancellationTokenSource();
        bool timerElapsed = await StartSnoozeTimer(TimeSpan.FromMinutes(5), _snoozeTokenSource.Token);
        btnSnooze.IsVisible = true;
        btnResolve.IsVisible = true;

        if (timerElapsed)
        {
            await DisplayAlert("Alert", _selectedAlert.Message, "OK");
            PlaySound();
        }
    }
}
```
#### Explanation of good design practice 

Asynchronous Operation:

This method is marked as async, which is important as it means the UI is not blocked while waiting for the snooze timer to complete, which is a good design practice. Additionally, the method hides the "Snooze" and "Resolve" buttons (btnSnooze and btnResolve) when the snooze operation is initiated and shows them again when it completes, providing a better user experience by preventing accidental clicks while the snooze is in progress and causing annoying repeated popups out of turn.

#### Task

 Additionally, when a team leader, or deputy, wishes to view those in their team, it is easy to see only their members.

#### Snippet

```csharp 
private void FilterAlerts_TextChanged(object sender, TextChangedEventArgs e)
{
    string searchText = e.NewTextValue;
    var filteredAlerts = _currentAlerts.Where(alert => alert.Message.Contains(searchText)).ToList();
    ltv_currentAlerts.ItemsSource = new ObservableCollection<SecurityAlert>(filteredAlerts);
}

private void ClearFilterButton_Clicked(object sender, EventArgs e)
{
    ltv_currentAlerts.ItemsSource = _currentAlerts;
}

```
#### Explanation of good design practice 
This snippet shows the FilterAlerts_TextChanged method filtering the displayed alerts in real-time as the user types into a search or filter input field by databinding. This practice provides immediate feedback to the user and enhances the user experience by helping users find relevant information, and doesn't ever actually changed the values held in _currentAlerts, which prevents loss of both data and user experience. As the "Clear Filter" method (ClearFilterButton_Clicked) is used to reset the ltv_currentAlerts ListView to display all alerts, there is enhanced usability.


 [Test]
 public void ClearFilterButton_Clicked_ClearsFilter()
 {
     var page = new SecurityAlertsPage();

     var filteredAlerts = new ObservableCollection<SecurityAlert>
 {
     new SecurityAlert { Message = "Security Alert 1" },
     new SecurityAlert { Message = "Security Alert 2" },
 };

     page.ltv_currentAlerts.ItemsSource = filteredAlerts;

     page.ClearFilterButton_Clicked(null, EventArgs.Empty);

     CollectionAssert.AreEqual(page._currentAlerts, page.ltv_currentAlerts.ItemsSource as ObservableCollection<SecurityAlert>);
 }

## A descriptive summary of the test code that you have written.
  ```csharp
  
 [Test]
 public void FilterAlerts_TextChanged_FiltersAlerts()
 {
     var page = new SecurityAlertsPage();
     var filteredText = "security";

     var alerts = new ObservableCollection<SecurityAlert>
 {
     new SecurityAlert { Message = "Security Alert 1" },
     new SecurityAlert { Message = "Test Alert" },
     new SecurityAlert { Message = "Security Alert 2" },
 };

     page._currentAlerts = alerts;

     page.FilterAlerts_TextChanged(null, new TextChangedEventArgs(null, filteredText, null));

     CollectionAssert.AreEqual(new ObservableCollection<SecurityAlert>
 {
     new SecurityAlert { Message = "Security Alert 1" },
     new SecurityAlert { Message = "Security Alert 2" },
 }, page.ltv_currentAlerts.ItemsSource as ObservableCollection<SecurityAlert>);
 }
  ```
The first test, FilterAlerts_TextChanged_FiltersAlerts, checks if the FilterAlerts_TextChanged method correctly filters alerts based on the entered text. It sets up a test collection of alerts, filters them, and then checks if the ltv_currentAlerts data source is updated with the filtered alerts.

```csharp
 [Test]
        public void ClearFilterButton_Clicked_ClearsFilter()
        {
            var page = new SecurityAlertsPage();

            var filteredAlerts = new ObservableCollection<SecurityAlert>
            {
                new SecurityAlert { Message = "Security Alert 1" },
                new SecurityAlert { Message = "Security Alert 2" },
            };

            page.ltv_currentAlerts.ItemsSource = filteredAlerts;

            page.ClearFilterButton_Clicked(null, EventArgs.Empty);

            CollectionAssert.AreEqual(page._currentAlerts, page.ltv_currentAlerts.ItemsSource as ObservableCollection<SecurityAlert>);
        }
    
```

The second test, ClearFilterButton_Clicked_ClearsFilter, verifies that the ClearFilterButton_Clicked method correctly clears the filter and resets the data source to the original collection. It simulates a situation where filtered alerts are displayed and checks if clicking the clear filter button resets the data source.

##  Summary of any changes that were requested during the code review, with your fixes.

I was requested to change this method in my code:

```csharp
  private async void CreateAlertButton_Clicked(object sender, EventArgs e)
 {
     if (string.IsNullOrEmpty(txe_securityAlert.Text))
         return;

     var securityAlert = new SecurityAlert
     {
         Message = txe_securityAlert.Text,
         CreatedTime = DateTime.Now,
         Resolved = false
     };

     _currentAlerts.Insert(0, securityAlert);
     txe_securityAlert.Text = "";

     await DisplayAlert("Alert", securityAlert.Message, "OK");

     using (var soundPlayer = new SoundPlayer("Resources/Sounds/alert.mp3"))
     {
         soundPlayer.Play();
     }

     string emailRecipient = "crisismanager@UNSAC.com";
     string emailSubject = "Security Alert";
     string emailBody = securityAlert.Message;

     try
     {
         var message = new EmailMessage
         {
             Subject = emailSubject,
             Body = emailBody,
             To = { emailRecipient }
         };

         await Email.ComposeAsync(message);
     }
     catch (Exception ex)
     {
         DisplayAlert("Email Error", $"Error sending email: {ex.Message}", "OK");
     }
 
 }
    
```

This change was requested as it violates SRP in every way. I hadn't realised, whilst coding, that this had happened as technically all of these things have to happen As it was included in multiple methods which had other functionalities, meaning it both violated DRY and SRP. I changed this to smaller methods, which each have a defined purpose, which mean they are less likely to break, easier to test and change, and easily maintained. For example, I made seperate methods for emailing management, and for playing the alerting noise. 

```csharp

  private async void CreateAlertButton_Clicked(object sender, EventArgs e)
{
    if (string.IsNullOrEmpty(txe_securityAlert.Text))
        return;

    var securityAlert = CreateSecurityAlert(txe_securityAlert.Text);

    _currentAlerts.Insert(0, securityAlert);
    txe_securityAlert.Text = "";

    await DisplaySecurityAlerts(securityAlert);
}

private SecurityAlert CreateSecurityAlert(string message)
{
    return new SecurityAlert
    {
        Message = message,
        CreatedTime = DateTime.Now,
        Resolved = false
    };
}

private async Task DisplaySecurityAlerts(SecurityAlert securityAlert)
{
    await DisplayAlert("Alert", securityAlert.Message, "OK");

    PlayAlertSound();
}

private void PlayAlertSound()
{
    using (var soundPlayer = new SoundPlayer("Resources/Sounds/alert.mp3"))
    {
        soundPlayer.Play();
    }
}

private async void NotifySeniorManagement(SecurityAlert securityAlert)
{
    string emailRecipient = "crisismanager@UNSAC.com";
    string emailSubject = "Security Alert";
    string emailBody = securityAlert.Message;

    try
    {
        var message = new EmailMessage
        {
            Subject = emailSubject,
            Body = emailBody,
            To = { emailRecipient }
        };

        await Email.ComposeAsync(message);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Email Error", $"Error sending email: {ex.Message}", "OK");
    }
}
```

##  Summary of any changes that I requested during the code review I did for a teammember.

I requested my team member change this method in their code:

```csharp
  public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            return value is bool ? ((bool)value ? "Available" : "Not Available") : "Not Available";
        }

  public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            return value is string ? ((string)value == "Available" ? true : false) : false;
        }
    
```

I requested this change as it was really unreadable for the user. Whilst impressive to have such a long, single-line piece of logic, it isn't the most helpful thing to have, particularly when coupled with no comments. As a result I asked the teammate to change this into something which was over multiple lines for usability and proper structure, which was done.


Secondly, I asked for this change to be made :

```csharp
    Text="{Binding Available,Converter={StaticResource boolToAvailability}}" 
    
```

I requested this as when I tried to run the app, an error was thrown surrounding this object, saying Available could not be found. As I looked a little further into it, the only thing which seemed like it might have been an issue was the ValueCounter within the code. I asked for this to be reviewed, although it wasn't so much a "style" or "SOLID" violation, more than simply the code not running and there being an issue with the setup itself.



## General reflective

Firstly, this week I decided to take a more structured approach to my own workflow. I took my JIRA, and looked at what our application already had (in this case, nothing). I then began at the beginning, by creating a model for the Objects I would need (Security Alerts). I then created the view class and method basics (just the name, parameters, and anything else needed). I then created the XAML code, before working to integrate the needed functionality with this and the cs code. Finally, I created the service classes, and the tests. This workflow ensured that the work was completed properly, and that I had a strong understanding of my goal and what I needed to do, so I could iteratively improve on this. I think this is a solid, systematic workflow to take with me into future projects and a career. 

Secondly, I took longer to review my teammates code as last week I found nothing to comment on. Instead of being intimidated by his code, and how it looked, I decided to evaluate it more through the lens of "do I, someone who at least has a basic understanding of code, understand what method x is doing without undue effort". When It became obvious that I didn't, that was worth mentioning in his review. 


