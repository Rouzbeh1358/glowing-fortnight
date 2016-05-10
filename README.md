Hi everyone

I need to modify world of zuul code in the following way:

Add this field to the Game class:
private Stack<Room> route;
Use the stack to maintain route information as the player moves from one room to another. Add this method to the Game class.
/** Takes the player back to the previous room, allowing theplayer to retrace the route through the game */
private void goBack()
Add a “back” command to the game, allowing the player to retrace his/her route through the game in the same way that repeatedly clicking on a browser's “Back” button will take the user back to previously-visited web pages. Display information about the current room, as usual. Display an error message if the user attempts to go back past the beginning of the game.
Add a basement room to the game, with “up” and “down” directions connecting it to the lab. Test to be sure your back command works correctly.
