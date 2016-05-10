Hi everyone

I need to modify world of zuul code in the following way:

Add this field to the Game class:
private Stack<Room> route;
Use the stack to maintain route information as the player moves from one room to another. Add this method to the Game class.
/** Takes the player back to the previous room, allowing theplayer to retrace the route through the game */
private void goBack()
Add a “back” command to the game, allowing the player to retrace his/her route through the game in the same way that repeatedly clicking on a browser's “Back” button will take the user back to previously-visited web pages. Display information about the current room, as usual. Display an error message if the user attempts to go back past the beginning of the game.
Add a basement room to the game, with “up” and “down” directions connecting it to the lab. Test to be sure your back command works correctly.


import java.util.Stack;

/**
 * This class is the main class of the "World of Zuul" application.
 * "World of Zuul" is a very simple, text based adventure game. Users can walk
 * around some scenery. That's all. It should really be extended to make it more
 * interesting!
 * 
 * To play this game, create an instance of this class and call the "play"
 * method.
 * 
 * This main class creates and initialises all the others: it creates all rooms,
 * creates the parser and starts the game. It also evaluates and executes the
 * commands that the parser returns.
 * 
 * @author Michael Kolling and David J. Barnes
 * @author Colleen Penrowley added "back" command
 * @version 2010.01.05
 */

public class Game {
	private Parser parser;
	private Room currentRoom;
	private Stack<Room> route;

	/**
	 * Create the game and initialise its internal map.
	 */
	public Game() {
		createRooms();
		parser = new Parser();
		route = new Stack<>();
	}

	/**
	 * Create all the rooms and link their exits together.
	 */
	private void createRooms() {
		Room outside, theatre, pub, lab, office, basement;

		// create the rooms
		outside = new Room("outside the main entrance of the university");
		theatre = new Room("in a lecture theatre");
		pub = new Room("in the campus pub");
		lab = new Room("in a computing lab");
		office = new Room("in the computing admin office");
		basement = new Room("in the basement");
  
		// initialise room exits
		outside.setExit("east", theatre);
		outside.setExit("south", lab);
		outside.setExit("west", pub);

		theatre.setExit("west", outside);

		pub.setExit("east", outside);

		lab.setExit("north", outside);
		lab.setExit("east", office);
		lab.setExit("down", basement);

		basement.setExit("up", lab);

		office.setExit("west", lab);

		currentRoom = outside; // start game outside

	}

	/**
	 * Main play routine. Loops until end of play.
	 */
	public void play() {
		printWelcome();

		// Enter the main command loop. Here we repeatedly read commands and
		// execute them until the game is over.

		boolean finished = false;
		while (!finished) {
			Command command = parser.getCommand();
			finished = processCommand(command);
		}
		System.out.println("Thank you for playing.  Good bye.");
	}

	/**
	 * Print out the opening message for the player.
	 */
	private void printWelcome() {
		System.out.println();
		System.out.println("Welcome to the World of Zuul!");
		System.out.println("World of Zuul is a new, incredibly boring adventure game.");
		System.out.println("Type 'help' if you need help.");
		System.out.println();
		System.out.println(currentRoom.getLongDescription());
	}

	/**
	 * Given a command, process (that is: execute) the command. If this command
	 * ends the game, true is returned, otherwise false is returned.
	 */
	private boolean processCommand(Command command) {
		boolean wantToQuit = false;

		if (command.isUnknown()) {
			System.out.println("I don't know what you mean...");
			return false;
		}

		String commandWord = command.getCommandWord();
		if (commandWord.equals("help")) {
			printHelp();
		} else if (commandWord.equals("go")) {
			goRoom(command);
		} else if (commandWord.equals("back")) {
			goBack();
		} else if (commandWord.equals("quit")) {
			wantToQuit = quit(command);
		}
		return wantToQuit;
	}

	// implementations of user commands:

	/**
	 * Print out some help information. Here we print some stupid, cryptic
	 * message and a list of the command words.
	 */
	private void printHelp() {
		System.out.println("You are lost. You are alone. You wander");
		System.out.println("around at the university.");
		System.out.println();
		System.out.println("Your command words are:");
		parser.showCommands();
	}

	/**
	 * Try to go to one direction. If there is an exit, enter the new room,
	 * otherwise print an error message.
	 */
	private void goRoom(Command command) {

		if (!command.hasSecondWord()) {
			// if there is no second word, we don't know where to go...
			System.out.println("Go where?");
			return;
		}

		String direction = command.getSecondWord();

		// Try to leave current room.
		
		Room nextRoom = currentRoom.getExit(direction);
		//route.push(nextRoom);
		if (nextRoom == null)
			System.out.println("There is no door!");
		else {
			//route.push(currentRoom);
			currentRoom = nextRoom;
			route.push(nextRoom);
			System.out.println(currentRoom.getLongDescription());
		}
	}

	/**
	 * "Quit" was entered. Check the rest of the command to see whether we
	 * really quit the game. Return true, if this command quits the game, false
	 * otherwise.
	 */
	private boolean quit(Command command) {
		if (command.hasSecondWord()) {
			System.out.println("Quit what?");
			return false;
		} else
			return true; // signal that we want to quit
	}

	/**
	 * Takes the player back to the previous room.
	 */
	private void goBack() {
	//  route.push(currentRoom);
		
		if (route.empty()) {
			System.out.println("You are already outside of building!" + ".\n" +"You can not go back furthur." );
			//System.out.println(currentRoom.getLongDescription());
			//route.push(currentRoom);
		}
		else {
			//route.push(currentRoom);
			currentRoom = route.pop();
			//route.pop();
		System.out.println(currentRoom.getLongDescription());
		}
	}
	
}
