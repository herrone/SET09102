# Testing

##The goal of the code being tested: 
The ResetDisplay function is crucial to the Hangman game as it clears the game display each time a new word is chosen. In order to prepare the screen for the player to guess the new word, it restores the original state of the Hangman image and adds enough underlines for the new word. Without this the game wouldn't really work, and would be frustrating for all users.

## Tests and Explanation 

### Test 1
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
#### Explanation of the Test(s):

This test tests the ResetDisplay method, by checking that when it is called, it resets the imageTracker variable to 1, updating the corresponding HangmanImage to the first one in the program, which is the initial state of the game.

#### Importance of Testing:

This test is crucial because it ensures that the ResetDisplay method completes a very basic yet important task. If this method fails and doesn't reset the image, it would lead to a confusing experience for the player and impact the overall gaming experience negatively.

#### Limitations of the Tests:

The limitations of the test are quite simple, such as the test relying on external image files, which might make it fail if these resources are missing, moved, or renamed. The test also only focuses on the visual aspect of the game. It doesn't cover the entire functionality of the ResetDisplay method, so there might be other behaviors or side effects caused by this method that are not tested here. Finally, I could have mocked or stubbed the GamePage dependency to the tested behaviour, to prevent the above from happening as well.

### Test 2
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

#### Explanation of the Test:

This test evaluates if the method resets the letters and underline labels after being called by iterating through the letter and underline labels and verifying that their content is simply empty strings and underscores (respectively). This ensures that the displayed letters are cleared when the game is reset, and that each underline label's text is set to an underscore after the ResetDisplay method is executed. 

#### Importance of Testing:

This test is important because it ensures that each  letter and underline label is correctly reset to their initial states, as if the labels are not reset properly, it could lead to player confusion about which letters have been guessed and which are still hidden from view.

#### Limitations of the Test:

The test relies on a hardcoded naming convention for the labels (Letter{i} and Underline{i}). It also only checks the initial state of the labels after calling ResetDisplay, and not during the game (e.g.after correct guesses), although it could be argued that this is simply outwith the scope needed.


