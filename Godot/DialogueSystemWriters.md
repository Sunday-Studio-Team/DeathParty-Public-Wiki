# Table of Contents
1. [Characters](#characters)
2. [Custom Commands in Ink](#custom-commands)
3. [Variables and Conditionals](#variables-conditionals)

Hi writers! You will be using Ink and exporting your files as `.json`. Here is a guide to all the commands and ways to control the game flow!

<a id="characters"></a>
## Characters

Use `[<name (case-sensitive)>] <text>` to designate a speaker.

![image](https://github.com/user-attachments/assets/cce9f776-7333-46d7-9094-80700a6734a2)

These will automatically link to ingame files that store the character's properties (such as their text color or their images.) Make sure the name is correct!

<a id="custom-commands"></a>
## Custom Commands in Ink

| Command | Functionality
| ---- | ---- 
| /give_item <item_name (case sensitive!)> | Gives the player 1x item (if it is a valid item.)
| /remove_item <item_name> | Removes 1x item from the player (sets `remove_item_flag` to `false` if the player did not have enough items, and `true` if they did)
| /has_item <item_name> | Checks that the player has at least 1x item. Sets `has_item_flag` to `false` if not, and `true` if so.

<a id="variables-conditionals"></a>
## Variables and Conditionals
You will use variables and conditionals to decide how you branch your story. **You can access and edit both your own Ink-created variables and variables from the rest of the game, such as the time or player stats.**

You can access ingame variables by declaring them (case-sensitive) at the top of your Ink file. Declaring them the first time won't change their value in game; it will just give you access to their value. Performing a separate operation, however, will.

The second operation would set the time ingame to midnight:

![image](https://github.com/user-attachments/assets/443dcf10-dc1c-42d9-9355-d5d1021ffe62)

The following increases the time by **5 hours:**

![image](https://github.com/user-attachments/assets/32ab025d-e726-405a-9c12-4713a61ea679)

A 5-minute increase would be a decimal-- 5/60 = .08333333 (but the code rounds down so make it 0.09 instead)

You can use this to check player stats; for example, this will only show the first option if the player's intelligence is greater than 13:

![image](https://github.com/user-attachments/assets/92abb0cc-5764-417b-8371-6d1fcbc0afba)

In the future I will probably make a command to roll a die and add the player's stats.

If you want to check that the player has the Key before moving on:

![image](https://github.com/user-attachments/assets/774b7b19-10d8-414b-9b2c-208e6566daaa)

The above Ink commands each set a `_flag` variable that you can check for a true/false result after you run them!