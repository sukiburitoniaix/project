import java.util.HashMap;
/**
 *  This class is the main class of the "World of Zuul" application. 
 *  "World of Zuul" is a very simple, text based adventure game.  Users 
 *  can walk around some scenery. That's all. It should really be extended 
 *  to make it more interesting!
 * 
 *  To play this game, create an instance of this class and call the "play"
 *  method.
 * 
 *  This main class creates and initialises all the others: it creates all
 *  rooms, creates the parser and starts the game.  It also evaluates and
 *  executes the commands that the parser returns.
 * 
 * @author  Michael Kölling and David J. Barnes
 * @version 2011.08.08
 */

public class Game 
{
    private Parser parser;
    private ParserWithFileInput parserWithFileInput;
    private Room currentRoom;

    static Room hallway;
    static Room gameroom;
    static Room roof; // final destination

    private Room outside, livingroom, kitchen, diningroom; // level 1 rooms
    private Room bathroom1,bedroom1,bedroom2,bathroom2,theater,closet1,closet2,guestroom; // level 2 rooms
    private Room bathroom3, bedroom3, playroom, gym, bedroom4, bathroom4, storageroom, studyroom1; // level 3 rooms
    private Room library, office1, bathroom5, office2, studyroom2, loungeroom;// level 3 rooms

    private SpecialRooms familyroom, laundryroom, bedroom5;

    /**
     * Create the game and initialise its internal map.
     */
    public Game() 
    {
        createRooms();
        parser = new Parser();
        parserWithFileInput = new ParserWithFileInput();

    }

    /**
     * Create all the rooms and link their exits together.
     */
    public void createRooms()
    {   

        // create the rooms first floor
        outside = new Room("It's a dark and rainy day, and you are at the front door of the mansion.");
        livingroom = new Room ("in the living room on the first floor");
        kitchen = new Room ("in the kitchen on the first floor");
        diningroom = new Room ("in the dining room on the first floor");
        familyroom =  new SpecialRooms("in the familyroom" + ". Answer the riddle correctly");
        // create the rooms second floor
        bathroom1 = new Room ("in the first bathroom on the second floor");
        bedroom1 = new Room("in the first bedroom on the second floor");
        bedroom2 = new Room("in the second bedroom on the second floor");
        bathroom2 = new Room ("in the second bathroom on the second floor");
        theater = new Room ("in the theater on the second floor");
        closet1 = new Room("in the first closet on the second floor");
        closet2 = new Room("in the second closet on the second floor"); 
        guestroom = new Room("in the guest room on the second floor");
        laundryroom = new SpecialRooms("in the laundry room on the second floor");
        //create the rooms third floor
        bathroom3 = new Room (" in the third bathroom on the third floor");
        bedroom3 = new Room (" in the third bedroom on the third floor");
        playroom = new Room ("in the play room on the thrid floor");
        gym = new Room ("in the gym on the third floor");
        bedroom4 = new Room (" in the fourth bedroom on the third floor");
        bathroom4 = new Room (" in the fourth bathroom on the third floor");
        storageroom = new Room ("in the storage room on the third floor");
        studyroom1 = new Room ("in the first study room on the third floor");
        office1 = new Room(" in the first office on the third floor");
        office2 =  new Room ("in the second office on the third floor");
        studyroom2 = new Room ("in the second study room on the third floor");
        loungeroom = new Room ("in the lounge room on the third floor");
        bathroom5 = new Room(" in the fifth bathroom on the third floor");
        bedroom5 = new SpecialRooms (" in the fifth bedroom on the third floor" + ". You spot a loose brick " +
            "\non the fireplace. You may type in 'press pull' to pull on the brick or proceed " +
            "\nthrouth one of the exits");
        // create the room in final desintination
        roof = new Room ("Congradulations, you have finished the game!!");

        // Initialise room exits:
        // level 1
        // Exit for outside
        outside.setExit("north", livingroom);
        // Exits for livingroom
        livingroom.setExit("south", outside);
        livingroom.setExit("west", diningroom);
        livingroom.setExit("north", familyroom);
        // Exits for diningroom
        diningroom.setExit("north",kitchen);
        diningroom.setExit("east", livingroom);
        // Exits for kitchen
        kitchen.setExit("east", familyroom);
        kitchen.setExit("south", livingroom);
        // Exits for familyroom
        familyroom.setExit("south", livingroom);
        familyroom.setExit("west", kitchen);

        // level 2
        // Exits for bedroom1
        bedroom1.setExit("south",guestroom);
        bedroom1.setExit("north", bathroom1);
        bedroom1.setExit("west", hallway);
        // Exits for bathroom1
        bathroom1.setExit("south", bedroom1);
        bathroom1.setExit("west", closet2);
        // Exits for closet2
        closet2.setExit("east", bathroom1);
        closet2.setExit("west", bathroom2);
        closet2.setExit("south", hallway);
        // Exits for bathroom2
        bathroom2.setExit("east", closet2);
        bathroom2.setExit("south", bedroom2);
        // Exits for bedroom2
        bedroom2.setExit("north", bathroom2);
        bedroom2.setExit("east", hallway); 
        bedroom2.setExit("south", laundryroom);
        // Exits for laundryroom
        laundryroom.setExit("north", bedroom2);
        laundryroom.setExit("east", theater);
        // Exits for theater
        theater.setExit("west", laundryroom);
        theater.setExit("east", guestroom);
        theater.setExit("south", closet1);
        // Exits for closet1
        closet1.setExit("north", theater);
        // Exits for guestroom
        guestroom.setExit("west", theater);
        guestroom.setExit("north", bedroom1);

        //level 3
        // Exits for office1
        office1.setExit("north", storageroom);
        office1.setExit("west", bedroom5);
        office1.setExit("east", bedroom4);
        // Exits for bedroom5
        bedroom5.setExit("east", office1);
        bedroom5.setExit("north", bathroom5);
        // Exits for bedroom4
        bedroom4.setExit("west", office1);
        bedroom4.setExit("north", bathroom4);
        // Exits for bathroom5
        bathroom5.setExit("south", bedroom5);
        bathroom5.setExit("west", bedroom3);
        bathroom5.setExit("east", storageroom);
        bathroom5.setExit("north", bathroom3);
        // Exits for bedroom3
        bedroom3.setExit("north", studyroom1);
        bedroom3.setExit("east", bathroom3);
        // Exits for bathroom3
        bathroom3.setExit("north", studyroom2);
        bathroom3.setExit("south", bathroom5);
        bathroom3.setExit("east", gameroom); // Does this one work?
        bathroom3.setExit("west", studyroom1);
        // Exits for loungeroom
        loungeroom.setExit("south", bathroom4);
        loungeroom.setExit("north", gym);
        loungeroom.setExit("east", playroom);
        loungeroom.setExit("west", gameroom); // Does this one work?
        // Exits for studyroom1
        studyroom1.setExit("south", bedroom3);
        studyroom1.setExit("east",bathroom3);
        // Exits for storageroom
        storageroom.setExit("north",gameroom); // Does this one work?
        storageroom.setExit("south", office1);
        storageroom.setExit("east", bathroom4);
        storageroom.setExit("west", bathroom5);
        // Exits for bathroom4
        bathroom4.setExit("north", loungeroom);
        bathroom4.setExit("west", storageroom);
        bathroom4.setExit("south", bedroom4);
        // Exit for playroom
        playroom.setExit("west", loungeroom);
        // Exits for gym
        gym.setExit("south", loungeroom);
        gym.setExit("west", office2);
        // Exits for office2
        office2.setExit("west", studyroom2);
        office2.setExit("east", gym);
        // Exits for studyroom2
        studyroom2.setExit("south", bathroom3);
        studyroom2.setExit("east", office2);

        //         String riddle = "What comes down but never goes up?";
        //         familyroom.setRiddle(riddle);

        //         String riddle2 = "What's tall when it's young and short when it's old?";
        //         laundryroom.setRiddle(riddle2);

        familyroom.setRiddle("What comes down but never goes up?", "rain");
        laundryroom.setRiddle("What's tall when it's young and short when it's old?", "candle");
        bedroom5.setRiddle("What is at the end of a rainbow?", "w");

        currentRoom = bedroom2;  // start game outside
    }

    /**
     *  Main play routine.  Loops until end of play.
     */
    public void play() 
    {            
        printWelcome();

        // Enter the main command loop.  Here we repeatedly read commands and
        // execute them until the game is over.

        boolean finished = false;
        while (! finished) {
            Command command = parser.getCommand();
            finished = processCommand(command);
        }
        System.out.println("Thank you for playing The Maze World.  Good bye.");
    }
    //     public void playWithFileInput() 
    //     {            
    //         printWelcome();
    //         // Enter the main command loop.  Here we repeatedly read commands and
    //         // execute them until the game is over.
    // 
    //         boolean finished = false;
    //         while (! finished) {
    //             Command command = parserWithFileInput.getCommand();
    //             finished = processCommand(command);
    //         }
    //         System.out.println("Thank you for playing.  Good bye.");
    //     }

    /**
     * Print out the opening message for the player.
     */
    private void printWelcome()
    {
        System.out.println();
        System.out.println("Welcome to The Maze World");
        System.out.println("In the Maze World, there are three different levels consisting of different rooms"); 
        System.out.println("and the goal is to reach the rooftop - the final destination. In this game, there will be riddles that"); 
        System.out.println("you must solve that will help you win the game");
        System.out.println("Type 'help' if you need help.");
        System.out.println();
        System.out.println(currentRoom.getLongDescription());
    }
    boolean riddleCorrect = false;
    /**
     * Given a command, process (that is: execute) the command.
     * @param command The command to be processed.
     * @return true If the command ends the game, false otherwise.
     */
    private boolean processCommand(Command command) 
    {
        boolean wantToQuit = false;

        if(command.isUnknown()) {
            System.out.println("I don't know what you mean...");
            return false;
        }

        String commandWord = command.getCommandWord();
        if (commandWord.equals("help")) {
            printHelp();
        }
        else if (commandWord.equals("go")) {

            goRoom(command);

        }
        else if (commandWord.equals("press")) {
            System.out.println(riddleCorrect);

            if(riddleCorrect == false)
            {
                System.out.println("You have either not answered the riddle or answered the riddle incorrectly! Try Again!");
                return false;
            }

            currentRoom.press(command);
            System.out.println(currentRoom.getLongDescription());
        }
        else if (commandWord.equals("quit")) {
            wantToQuit = quit(command);
        }
        else if(commandWord.equals("answer"))
        {
            riddleCorrect = riddleCheck(command);
        }
        // else command not recognised.
        return wantToQuit;
    }

    // implementations of user commands:

    /**
     * Print out some help information.
     * Here we print some stupid, cryptic message and a list of the 
     * command words.
     */
    private void printHelp() 
    {
        System.out.println("You are are currently lost in the mansion. To get to the other door, follow the riddle.");
        System.out.println("around at the mansion.");
        System.out.println();
        System.out.println("Your command words are:");
        parser.showCommands();
    }

    /** 
     * Try to in to one direction. If there is an exit, enter the new
     * room, otherwise print an error message.
     */
    private void goRoom(Command command) 
    {
        if(!command.hasSecondWord()) 
        {
            // if there is no second word, we don't know where to go...
            System.out.println("Go where?");
            return;
        }

        String direction = command.getSecondWord();

        // Try to leave current room.
        Room nextRoom = currentRoom.getExit(direction);

        if (nextRoom == null) 
        {
            System.out.println(" This is the incorrect door! Try again! ");
        }
        else 
        {

            if(nextRoom.getShortDescription().contains("hallway"))
            {
                Game.hallway.setExit("north",closet2);
                Game.hallway.setExit("south",theater);
                Game.hallway.setExit("west",bedroom2);
                Game.hallway.setExit("east",bedroom1);

            }
            if(nextRoom.getShortDescription().contains("gameroom"))
            {
                Game.gameroom.setExit("north", office2);
                Game.gameroom.setExit("south", storageroom);
                Game.gameroom.setExit("east", loungeroom);
                Game.gameroom.setExit("west", bathroom3);
            }

            currentRoom = nextRoom;
            System.out.println(currentRoom.getLongDescription());
            if(currentRoom.riddleMapSize() != 0)
            {
                System.out.println(currentRoom.getRiddle());

            }
        }

    }

    /** 
     * "Quit" was entered. Check the rest of the command to see
     * whether we really quit the game.
     * @return true, if this command quits the game, false otherwise.
     */
    private boolean quit(Command command) 
    {
        if(command.hasSecondWord()) {
            System.out.println("Quit what?");
            return false;
        }
        else {
            return true;  // signal that we want to quit
        }
    }

    public boolean riddleCheck(Command command)
    {
        String answer = command.getSecondWord();

        if(answer ==  null)
        {
            System.out.println("Please respond to the riddle");
            return false;
        }

        if(answer.equals(currentRoom.getRiddleAnswer()))
        {
            System.out.println("You have solved the riddle! A button has magically appeared." +
                    "\nYou may press the button by typing in 'press button'.");
                System.out.println(currentRoom.getLongDescription());
                return true;
            
        }
        else
        {
            System.out.println("You have given the wrong answer, try again");
            return false;
        }

       
    }
}

