# Code review

I chose the code review challenge **Play to win** to demonstrate my coding skills.

```csharp
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

#### Coding standard violations
##### KISS
The code contains a lot of nested if statement which makes it hard to follow the flow of the program and intruduces a lot of unnecessary complexity. 

##### YAGNI
In the first line of the method there is a static name variable assigned. The variable is not used inside the bounds of the method.

##### Use recognised commenting conventions
The comment "Player is waiting" that is used above one of the if-conditions is unnecessary.

___

The following code snippet shows how to fix the issues described in the paragraphs above.

```csharp
public string GetPlayerStatus(Player player)
{
    if(!player.IsOnline) {
        return "Player is offline";
    }
    if(player.CurrentGame != null) {
        return "Player is currently in a game";
    }
    if (player.PendingInvitations.Count > 0) {
        return "Player has pending invitations";
    }
    return "Player is online";
}
```

#### Resolving the coding standard violations
##### KISS
Keeping it simple in this case means that we want to reduce the amount of nested conditional layers to decrease the complexity. Here it makes a lot of sense to use [Guard Clauses](https://en.wikipedia.org/wiki/Guard_(computer_science)) as way to make the code more readable.

Guard clauses will flatten the layers of nesting because they will exit the method as soon as a condition is met and return a status.

##### YAGNI
Simply removing the unused variable will resolve this coding standard violation.

##### Use recognised commenting conventions
In this case it is feasable to remove the comment altogether as the code itself tells us what is going on. We do not need that extra information.
