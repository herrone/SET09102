*Code Review*

I have decided to choose this code review challenge for my portfolio:

```
public string GetPlayerStatus(Player player)
{
    static int name = player.name;
    
    if (player.IsOnline) {
        if (player.CurrentGame != null) {
            return "Player is currently in a game";
        } else {    // Player is waiting
            if (player.PendingInvitations.Count > 0) {
                return "Player has pending invitations";
            } else {
                return "Player is online";
            }
        }
    } else {
        return "Player is offline";
    }
}

```
*Issues with this code snippet:*

KISS: This concept is violated by having a triple nested conditional. Although the listed outcomes are fine, they are simply structured in a way that the flow of the loops is not instantly clear to the reader, and confusion and trouble could easily ensue.

YAGNI: This concept is violated by having a "name" variable which is neither needed nor used. This is simply superfluous to requirements. It also is "static" which local variables should not be (only class-level), which might stop the program compiling as well.

Commenting Convention: The code does not follow a consistent commenting convention; there is only one commented line describing a non-complex part of the code, which isn't shown anywhere else and sticks out.

Open/Closed Principle: This may be violated in the future as if someone wanted to add a new condition or change the existing logic, they would need to modify the existing logic (including tricky and annoying 3-layer-deep conditionals), potentially introducing new bugs or issues.

*This code when rewritten:*

```
public string GetPlayerStatus(Player player)
{
    switch (true)
    {
        case !player.IsOnline:
            return "Player is offline";
	case player.CurrentGame != null:
            return "Player is currently in a game";
        case player.PendingInvitations.Count > 0:
            return "Player has pending invitations";
        default:
            return "Player is online";
    }
}

```

*Why this is better and conforms to the previously broken conventions and regulations:*

When approaching this problem, I  wanted to ensure my code was well-written enough that the functionality would be clear without the use of comments, so I removed the three nested loops in favor of a single switch statement. I also rewrote the logic, rendering the new code very simple and clear to anyone who might like to use it, and meaning it no longer violates the KISS principle. The lack of required comments also mean it is no longer betraying the commenting conventions. 

Furthermore, this rewrite better sticks to the Open/Closed Principle, as if a developer needed to extend the functionality by adding new conditions, they could do so by adding new cases to the switch statement while leaving the rest of the method and aforementioned logic alone. This is not perfect though, and I considered using a strategy pattern, but that would probably have violated KISS in itself in this instance. Additionally, the rewrite includes only what it needs, which helps to avoid a violation of the YAGNI principle, as I removed the aforementioned "name" property. The program also compilies without any warning messages as there is no incorrect declaration of a static keyword. 

Finally, switch statements can be marginally faster than conditionals, which may give the program a slight advantage over the old code snippet, as seen here; [C# Helper](http://www.csharphelper.com/howtos/howto_compare_switch_if_speed.html), and is another positive outcome from this task.


