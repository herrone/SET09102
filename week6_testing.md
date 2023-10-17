# Testing

###The goal of the code being tested: 
The ResetDisplay function is crucial to the Hangman game as it clears the game display each time a new word is chosen. In order to prepare the screen for the player to guess the new word, it restores the original state of the Hangman image and adds enough underlines for the new word. 

##Tests and Explanation 

```csharp
 [Fact]
 public void ResetDisplay_ResetImage()
 { 
     var gamePage = new GamePage("Easy");
     var word = "hangman"; 
     gamePage.ResetDisplay(word);
     Assert.Equal(1, gamePage.imageTracker);
     Assert.Equal($"hangman{gamePage.imageTracker}.png", gamePage.HangmanImage.Source);
 }
```
###

```csharp
[Fact]
public void ResetDisplay_ResetLabels()
{
   
    var gamePage = new GamePage("Easy");
    var word = "hangman"; 
    gamePage.ResetDisplay(word);
    for (int i = 1; i <= word.Length; i++)
    {
        var letterLabel = gamePage.FindByName<Label>($"Letter{i}");
        var underlineLabel = gamePage.FindByName<Label>($"Underline{i}");

        Assert.Equal(string.Empty, letterLabel.Text); 
        Assert.Equal("_", underlineLabel.Text); 
    }
}
```


##Importance of Testing:
These simplified tests ensure that the ResetDisplay method performs its essential functions of resetting the image and labels. Testing these aspects is crucial to maintaining the correct state of the Hangman game's display.

##Limitations of the Tests:
These tests are simplified and focus on the core functionality of the ResetDisplay method. More comprehensive testing may be needed to cover all edge cases and other parts of the game logic, depending on the complexity of your application.

For each example

* Summarise the purpose of the code you were testing
* Include the test code
* Provide a brief explanation of the test(s) that are performed
* Explain why this is an important aspect of the code to test
* Identify any limitations of your tests (this may be something that you realised after
  the evaluation).

Did you manage to write a test which failed during the final evaluation? If so, that would make an excellent example. You should briefly discuss why the writer of the code might 
have overlooked the particular test case that failed.
