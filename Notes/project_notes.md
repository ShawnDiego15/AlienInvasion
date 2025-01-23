# Alien Invasion; Page 223
The Alien Invasion project covers chapters 12, 13 and 14. It will utilize the Pygame package to develop a 2D game in which the aim is to shoot down a fleet of aliens. 

# Chapter 12 - A Ship That Fires Bullets
For this project we will be using Pygame, a collection of fun, powerful Python modules that manage graphics, animation, and even sound, making it easier for you to build sophisticatecd games. With Pygame handling tasks like drawing images to the screen, you can focus on the higher-level logic of game dynamics.

In this chapter we will set up Pygame, and then create a rocket ship that moves right and left and fires bullets in response to player input. In the next 2 chapters, we will create a fleet of aliens to destroy, and then continue to refine the game by setting limits on the number of ships you can use and adding a scoreboard.

While building this game, we will also learn how to manage large projects that span multiple files. We'll refactor a lot of code and manage file contetns to roganize the project and make the code efficient.

## Planning Your Project; Page 228
When building a large project, it's important to prepare a plan before you begin to write code. This will keep you focused and make it more likely that you will complete the project.

Lets write a description of the general gameplay. This will provide an idea of how to start building the game:
*In Alien Invasion, the player controls a rocket ship that appears at the bottom center of the screen. The player can move the ship right and left using the arrow keys and shoot bullets using the spacebar. When the game begins, a fleet of aliens fills the sky and moves across and dwon the screen. The player shoots and destroys aliens. If the player shoots all the alients, a new fleet appears that moves faster than the previous fleet. If any alien hits the player's ship or reaches the bottom of the screen, the player loses a ship. If the player loses three ships, the game ends.*

For the first development phase, we'll make a ship that can move left and right and fires bullets when the player presses the spacebar.

## Installing Pygame; Page 228
The `pip` module helps you download and install Python packages. To install Pygame, enter the following command at a terminal prompt: `$ python -m pip install --user pygame`

**NOTE:** This did not work as intended working on my PC through VSCode. Instead, I ensured Python and pip were installed on my device and then entered `pip install pygame` into a terminal within VSCode. I also had to select an interpreter within VSCode, which was just my standard Python download. This appears to have resolved the issue and installed Pygame. On a separate machine, I was able to install this by installing direct to the Python terminal and it resulted in working in VSCode.

## Starting the Game Project; Page 229
We'll begin building the game by creating an empty Pygame window. Later on we will add more elements and build out funtionality of this window.

### Creating a Pygame Window and Responding to User Input; Page 229
We'll make a n empty Pygame window by creating a class to represent the game.

#### *alien_invasion.py*
```
import sys

import pygame

class AlienInvasion:
    """Overall class to manage game assets and behavior."""

    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()

        self.screen = pygame.display.set_mode((1200, 800))
        pygame.display.set_caption("Alien Invasion")

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Make the most recently drawn screen visible.
            pygame.display.flip()

if __name__ == '__main__':
    # Make a new game instance, and run the game.
    ai = AlienInvasion()
    ai.run_game()
```

1. First we import the `sys` and `pygame` modules. `pygame` contains the functinality we need to make a game while `sys` is used to exit the game when the player quits.
2. We start off with the `AlienInvasion` class. The `__init__()` method contains the `pygame.init()` function to initialize the background settings that Pygame needs to work properly. 
3. We then call `pygame.display.set_mode(())` to create a display window, on which we'll draw all the game's graphical elements. The argument used here (1200, 800) is a tuple that defines the dimensions of the game window, which will be 1200 pixels wide by 800 pixels high. These can be adjusted depending on your system display size.
    - We assign this to the attribute `self.screen`, so it will be available in all methods in the class.
    - The object we assigned to `self.screen` is called a **surface**. A surface in Pygame is a part of the screen where a game element can be displayed. Each element in the game, like an alien or a ship, is its own surface. The surface returned by `display_set_mode()` represents the entire game window. When we activate the games animation loop, this surface will be redrawn on every pass through the loop, so it can be updated with any changes triggered by user input.
4. The game is controlled by the `run_game()` method. This method contains a while loop that runs continually. The while loop contains an event loop (the for loop) and code that manages screen updates.
    - An **event* is an action that the user performs while playing the game, such as pressing a key or moving the mouse.
    - To make our program respond to events, we write this event loop to listen for events and perform appropriate tasks depending on the kinds of events that occur.
5. To access events that Pygame detects, we will use the `pygame.event.get()` function.
    - This function returns a list of events that have taken place since the last time this function was called.
    - We will later add a series of if statements to detect and respond to different events.
6. The call to `pygame.display.flip()` tells Pygame to make the most recently drawn screen visible. In this case, it simply draws an empty screen on each pass through the while loop, erasing the old screen so only the new screen is visible. When we move the game elements around, `pygame.display.flip()` continually updates the display to show the new positions of game elements and hides the old ones, creating the illusion of smooth movement.
7. At the end of the file, we create an instance of the game, and then call `run_game()`. We place this `run_game()` in an if block that only runs if the file is called directly.

### Setting the Background Color; Page 230
Pygame creates a black screen by default. We will do this at the end of the `__init__()` method.

#### *alien_invasion.py*
```
class AlienInvasion:
    """Overall class to manage game assets and behavior."""

    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()

        self.screen = pygame.display.set_mode((1200, 800))
        pygame.display.set_caption("Alien Invasion")

        # Set the background color.
        self.bg_color = (230, 230, 230)

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.bg_color)

            # Make the most recently drawn screen visible.
            pygame.display.flip()
```

- Colors in Pygame are specified as RGB colors. Each color value can range from 0 to 255 and are in the order of red, green and blue. For example (255, 0, 0) is red.
- We assign a gray color (230, 230, 230) to `self.bg_color`
- At `self.screen.fill(self.bg_color)` we fill the screen with the background color using the fill() method, which acts on a surface and takes only one argument, a color.

### Creating a Settings Class; Page 231
Each time we introduce new functionality into the game, we'll typically create some new settings as well. Instead of adding settings throughout the code, let's write a module called *settings* that contains a class called `Settings` to store all of these in one place. This approach allows us to work with just one settings object any time we need to access any individual setting and will make it easier to modify the game as the project grows.

#### *settings.py*
```
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (230, 230, 230)
```

To make an instance of Settings in the project and use it to access our settings, we need to modify *alien_invasion.py*.

#### *alien_invasion.py*
```
import sys

import pygame

from settings import Settings

class AlienInvasion:
    """Overall class to manage game assets and behavior."""

    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((self.settings.screen_width, self.settings.screen_height))
        pygame.display.set_caption("Alien Invasion")

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.settings.bg_color)

            # Make the most recently drawn screen visible.
            pygame.display.flip()

if __name__ == '__main__':
    # Make a new game instance, and run the game.
    ai = AlienInvasion()
    ai.run_game()
```

1. We import Settings into the main program file.
2. Then we create an instance of Settings and assign it to `self.settings`.
3. We then use the attributes of `self.settings`, `screen_width` and `screen_height` to create a screen alongside the background colors.

## Adding the Ship Image; Page 232
Let's add the ship to our game. To draw the player's ship on the screen, we'll load an image and then use the Pygame `blit()` method to draw the image.

When choosing artwork for your games, there are a few things to consider:
- Licensing: Safest and cheapest way is to start to use freely licensed graphics that you can use and modify. https://pixabay.com/ is a recommended site.
- File type: Almost any can be used, but its easiest to use a bitmap (.bmp) file because Pygame loads bitmaps by default. Other types can be configured but may depend on certain image librariers that must be installed on your system. .jpg or .png files could be used but would need to be converted in a tool like Photoshop.
- Background Color: Transparent or solid that can be replaced in an image editor is best.

### Creating the Ship Class; Page 233
We now need to display it on the screen. To use our ship, we'll create a new ship module that will contain the class Ship. This class will manage most of the behavior of the players ship.

#### *ship.py*
```
import pygame

class Ship:
    """A class to manage the ship."""

    def __init__(self, ai_game):
        """Initialize the ship and set its starting position."""
        self.screen = ai_game.screen
        self.screen_rect = ai_game.screen.get_rect()

        # Load the ship image and get its rect.
        self.image = pygame.image.load('images/ship.bmp')
        self.rect = self.image.get_rect()

        # Start each new ship at the bottom center of the screen.
        self.rect.midbottom = self.screen_rect.midbottom

    def blitme(self):
        """Draw the ship at its current location."""
        self.screen.blit(self.image, self.rect)
```

Pygame is efficient beccause it lets you treat all game elements like rectangles (*rects*), even if they're not exactly shaped like rectangles. This eases tasks as rectangles are simple shapes and can be used for various items such as validating if a collision occurs when 2 rectangles touch eachother.
1. We import the pygame module before defining the class.
2. The `__init__()` method of Ship takes two parameters: the self reference and a reference to the current instance of the AlienInvasion class. This will give Ship access to all the game resources defined in AlienInvasion.
3. At `self.screen = ai_game.screen` we assign the screen to an attribute of Ship, so we can access it easily in all the methods in this class.
4. At `self.screen_rect = ai_game.screen.get_rect()` we access the screen's rect attribute using the `get_rect()` method and assign it to `self.screen_rect`. Doing this allows us to place the ship in the correct location on the screen.
5. To load our image, we call `pygame.image.load()` and give the filepath for our image. This function returns a surface representing the ship, which we assign to `self.image`. When the image is loaded we call `get_rect()` to access the ship surface's rect attribute so we can later use it to place the ship.
6. We want to place the ship at the bottom center of the screen, to do so we make the value of `self.rect.midbottom` match the `midbottom` attribute of the screen's rect. This is done at `self.rect.midbottom = self.screen_rect.midbottom`.
7. Lastly, at `def blitme(self):` we define this method which draws the image to the screen at the position specified by `self.rect`.

When working with a rect object, you can use the x- and y-coordiantes of the top, bottom, left and right edges of the rectangle as well as the center. Any of these values can be used to place the object in a position. In Pygame, the origin (0,0) is the top-left corner of the game window, increasing as you move right and down.

### Drawing the Ship to the Screen; Page 235
Now we will update the *alien_invasion.py* file so it creates a ship and calls the ship's `blitme()` method.

#### *alien_invasion.py*
```
import sys

import pygame

from settings import Settings
from ship import Ship

class AlienInvasion:
    """Overall class to manage game assets and behavior."""

    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((self.settings.screen_width, self.settings.screen_height))
        pygame.display.set_caption("Alien Invasion")

        self.ship = Ship(self)

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.settings.bg_color)
            self.ship.blitme()

            # Make the most recently drawn screen visible.
            pygame.display.flip()

```

1. We import Ship and then make an instance of Ship after the screen has been created. `self.ship = Ship(self)` The call to `Ship()` requires one argument, an instance of AlienInvasion. The self argument refers to the current instance of AlienInvasion. We assign this instance of Ship to `self.ship`.
2. After filling the background, we draw the ship on the screen by calling `ship.blitme()`, so the ship appears on top of the background.

## Refactoring: The `_check_events()` and `_update_screen()` Methods; Page 236
In large projects, you'll often refactor code you've written before adding more code. Refactoring makes it code easier to build upon as it simplifies the structure.

In this section, we will break the `run_game()` method into 2 helper methods. A ***helper method*** does work inside a class but isn't meant to be called through an instance. In Python, **a single leading underscore indicates a helper method.**

### The `_check_events()` Method; Page 236
We'll move the code that manages events to a separate method called `_check_events()`. This will simplify `run_game()` and isolate the event management loop. Isolating the loop allows you to manage events separately from other aspects of the game, such as updating the screen.

#### *alien_invasion.py*
```
    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            self._check_events()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.settings.bg_color)
            self.ship.blitme()

            # Make the most recently drawn screen visible.
            pygame.display.flip()

    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get()
        if event.type == pygame.QUIT:
            sys.exit()
```

1. We move the code that was previously in run_game to a new method, `_check_events()`.
2. We then added `self._check_events()` to the while loop to maintain the previous behavior.

### The `_update_screen()` Method; PAge 237
To further simplify `run_game()`, we'll move the code for updating the screen to a separate method called `_update_screen()`.

#### *alien_invasion.py*
```
    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            self._check_events()
            self._update_screen()

    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()

    def _update_screen(self):
        """Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)
        self.ship.blitme()

        # Make the most recently drawn screen visible.
        pygame.display.flip()
```

- Similar to the previous section, we moved the code that updates the screen to a new method and then called that method within our `run_game()` method that it was removed from.

Refactoring code in larger projects as you work along is ideal and realistic in the development process. Write code as simple as possible and then refactor as your project becomes more complex.

## Piloting the Ship; Page 238
Next, we'll give the player the ability to move the ship right and left.

### Responding to a Keypress; Page 238
Whenever the player presses a key, that keypress is registered in Pygame as an event. Each event is picked up by the `pygame.event.get()` method. We need to specify in our `_check_events()` method what kind of events we want the game to check for.

Each keypress is registered as a **KEYDOWN** event. When Pygame detects a KEYDOWN event, we need to check whether the key that was pressed is one that triggers a certain action. For example, if the player presses the right arrow key, we want to increase the ships `rect.x` value to move the ship to the right.

#### *alien_invasion.py*
```
    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RIGHT:
                    # Move the ship to the right
                    self.ship.rect.x += 1
```

- We added an elif block to the event loop to respond when Pygame detects a KEYDOWN event.
- We then check if the key pressed (`event.key`) was the right arrow key (`pygame.K_RIGHT`).
- If so, we move the ship 1 space to the right by increasing the value of `self.ship.rect.x`.

Now each press will move the ship to the right 1 pixel. However, we should look into allowing continuous movement to help improve the control.

### Allowing Continous Movement; Page 239
When the player hodls down the right arrow key, we want the ship to continue moving right until the player releases the key. We will have the game detect a `pygame.KEYUP` event so we'll know when the right arrow key is released; then we'll use the KEYDOWN and KEYUP events together with a flag called `moving_right` to implement continous motion.

The ship class controls all attributes of the ship, so we'll give it an attribute called `moving_right` and an `update()` method to check the status of the `moving_right` flag.

#### *ship.py*
```
    def __init__(self, ai_game):
        """Initialize the ship and set its starting position."""
        self.screen = ai_game.screen
        self.screen_rect = ai_game.screen.get_rect()

        # Load the ship image and get its rect.
        self.image = pygame.image.load('images/ship.bmp')
        self.rect = self.image.get_rect()

        # Start each new ship at the bottom center of the screen.
        self.rect.midbottom = self.screen_rect.midbottom

        # Movement flag
        self.moving_right = False
    
    def update(self):
        """Update the ship's position based on the movement flag."""
        if self.moving_right:
            self.rext.x += 1
```

1. We add a `self.moving_right` attribute in the `__init__()` method and set it to False initially.
2. Then we add `update()` which moves the ship right if the flag is True. This method will be called through an instance of Ship, so its not considered a helper method.

Now we need to modify `_check_events()` so that `moving_right` is set to True when the right arrow key is pressed and False when the key is released.

#### *alien_invasion.py*
```
    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RIGHT:
                    # Move the ship to the right
                    self.ship.moving_right = True
            elif event.type == pygame.KEYUP:
                if event.key == pygame.K_RIGHT:
                    self.ship.moving_right = False
```

1. Here, we just changed the result of the KEYDOWN event for the K_RIGHT key to set the moving_right value to True and False when the KEYUP event is triggered.

Next, we modify the while loop in `run_game()` so it calls the ship's `update()` method on each pass through the loop.

#### *alien_invasion.py*
```
    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            self._check_events()
            self.ship.update()
            self._update_screen()
```

The ships position will be updated after we've checked for keyboard events and before we update the screen.

### Moving Both Left and Right; Page 240

Now that the ship can move continously to the right, adding movement to the left is straightforward. Again, we will modify the Ship class and the `_check_events()` method. The one item to note is that in `update()` in the Ship class, we used an additional if statement for left movement rather than an elif so that both keys can be pressed at the same time. If we used an elif and both keys were pressed, the right key would always take priority as it was the first if statement.

### Adjusting the Ship's Speed
Currently, the ship moves one pixel per cycle through the while loop, but we can take finer control of the ship's speed by adding a ship_speed attribute to the Settings class.

#### *settings.py*
```
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (230, 230, 230)

        # Ship Settings
        self.ship_speed = 1.5
```

We are using decimals to allow for finer control as we increase the tempo of the game later on. However, rect attributes such as x store only integer values, so we need to make some modifications to Ship.

#### *ship.py*
```
def __init__(self, ai_game):
        """Initialize the ship and set its starting position."""
        self.screen = ai_game.screen
        self.settings = ai_game.settings

        # Load the ship image and get its rect.
        self.image = pygame.image.load('images/ship.bmp')
        self.rect = self.image.get_rect()
        self.screen_rect = ai_game.screen.get_rect()

        # Start each new ship at the bottom center of the screen.
        self.rect.midbottom = self.screen_rect.midbottom

        # Store a decimal value for the ship's horizontal position.
        self.x = float(self.rect.x)

        # Movement flags
        self.moving_right = False
        self.moving_left = False
    
    def update(self):
        """Update the ship's position based on the movement flag."""
        # Update the ship's x value, not the rect.
        if self.moving_right:
            self.x += self.settings.ship_speed
        if self.moving_left:
            self.x -= self.settings.ship_speed

        # Update rect object from self.x.
        self.rect.x = self.x
```

1. `self.settings = ai_game.settings` - We create a settings attribute for Ship, so we can use it in update(). Because we are adjusting the position of a ship by fractions of a pixel, we need to assign the position to a variable that can store a decimal value.
2. `self.x = float(self.rect.x)` - To keep track of the ship's position accurately, we define a new self.x attribute that can hold decimal values. We convert the value of self.rect.x to a decimal using the `float()` function.
3. `self.x += self.settings.ship_speed` - We then changed this so that the x is adjusted by the amount stored in the settings for ship_speed rather than 1.
4. `self.rect.x = self.x` - After self.x has been updated, we use the new value to update self.rect.x, which controls the position of the ship. Only the integer portion of self.x will be stored in self.rect.x, but thats fine for displaying the ship.
5. NOTE: We do not need to import Settings to Ship because ship requires us to pass an instance of AlienInvasion to it, which already has it's own settings stored.

With these changes, we can alter the ship speed as often as we see fit without causing issues.

### Limiting the Ship's Range; Page 243
At this point, the ship will disappear off either edge of the screen if you hold down an arrow long enough. We will correct this to stop at the screen edge by modifying the `update()` method in Ship.

#### *ship.py*
```
    def update(self):
        """Update the ship's position based on the movement flag."""
        # Update the ship's x value, not the rect.
        if self.moving_right and self.rect.right < self.screen_rect.right:
            self.x += self.settings.ship_speed
        if self.moving_left and self.rect.left > 0:
            self.x -= self.settings.ship_speed
```

1. These updates now check the position of the ship before changing the value of self.x. The code `self.rect.right` returns the x coordinate of the right edge of the ship's rect.
2. This also extends to the left version of this condition, except for left we can use `self.rect.left > 0` since we know 0 is the furthest left side of the screen.

### Refactoring `_check_events()`; Page 243
The `_check_events()` method will increase in length as we continue to develop the game, so let's break `_check_events()` into two more methods: one that handles KEYDOWN events and another that handles KEYUP events.

#### *alien_invasion.py*
```
    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                self._check_keydown_events(event)
            elif event.type == pygame.KEYUP:
                self._check_keyup_events(event)


    def _check_keydown_events(self, event):
        """Respond to keypresses."""
        if event.key == pygame.K_RIGHT:
            # Move the ship to the right
            self.ship.moving_right = True
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = True

    def _check_keyup_events(self, event):
        """Respond to key releases."""
        if event.key == pygame.K_RIGHT:
            self.ship.moving_right = False
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = False
```

- We make two new helper methods: _check_keydown_events() and _check_keyup_events(). 
- Each needs a self parameter and an event parameter.
- The original code is now replaced with lines calling these methods and makes this cleaner and easier to build upon.

### Pressing Q to Quit; Page 244
Now that we're responding to keypresses efficiently, we can add another way to quit the game. This will be done through a shortcut for the key 'Q'.

#### *alien_invasion.py*
```
    def _check_keydown_events(self, event):
        """Respond to keypresses."""
        if event.key == pygame.K_RIGHT:
            # Move the ship to the right
            self.ship.moving_right = True
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = True
        elif event.key == pygame.K_q:
            sys.exit()
```

- This is rather simple, we just check for the event of 'q' being pressed and then pass `sys.exit()`.

### Running the Game in Fullscreen Mode; Page 244
Pygame has a fullscreen mode that may look better than a regular window. To run the game in fullscreen mode, we make the following changes to `__init__()`.

#### *alien_invasion.py*
```
    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((0,0), pygame.FULLSCREEN)
        self.settings.screen_width = self.screen.get_rect().width
        self.settings.screen_height = self.screen.get_rect().height
        pygame.display.set_caption("Alien Invasion")

        self.ship = Ship(self)
```

1. We pass a size of (0, 0) and the parameter `pygame.FULLSCREEN` when creating a screen surface. This tells Pygame to figure out a window size that will the screen.
2. We then use the width and heigh attributes of the screen's rect to update the settings object.

**NOTE:** Before setting any games from Pygame in fullscreen mode, confirm that you can quit with a key. Pygame offers no default method to close a fullscreen window.

## A Quick Recap; Page 245
In the next section we will add the ability to shoot bullets, which involves adding a new file called *bullet.py* and making some modifications to some of the file's were already using. A brief overview of the current files for the project is as follows:

*alien_invation.py*
- Contains the `AlienInvasion` class.
    - Creates a number of important attributes used throughout the game.
- The main file to run when opening the game.

*settings.py*
- Contains the `Settings` class.
- Only has an `__init__()` method, which initializes attributes controlling the game's appearance and ship's speed.

*ship.py*
- Contains the `Ship` class.
- Manages the ships position and allows the ship to be drawn to the screen.
- Pulls the ship image from the *images* folder.

## Shooting Bullets; Page 246
Now we will add the ability to shoot bullets from the player pressing the spacebar. These will be represented by a small rectangle that will travel straight up until they disappear at the top of the screen.

### Adding the Bullet Settings; Page 247
We will need to update *settings.py* with settings for the bullet. These settings create dark gray bullets with a width of 3 pixels and height of 15 pixels. The bullets will travel slightly slower than the ship.

#### *settings.py*
```
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (230, 230, 230)

        # Ship Settings
        self.ship_speed = 1.5

        # Bullet Settings
        self.bullet_speed = 1.0
        self.bullet_width = 3
        self.bullet_height = 15
        self.bullet_color (60, 60, 60)
```

### Creating the Bullet Class; Page 247
Now we will create a new file, *bullet.py* to store our Bullet class.

#### *bullet.py*
```
import pygame
from pygame.sprite import Sprite

class Bullet(Sprite):
    """A class to manage bullets fired from the ship."""

    def __init__(self, ai_game):
        """Create a bullet object at the ship's current position."""
        super().__init__()
        self.screen = ai_game.screen
        self.settings = ai_game.settings
        self.color = self.settings.bullet_color

        # Create a bullet rect at (0, 0) and then set correct position.
        self.rect = pygame.Rect(0, 0, self.settings.bullet_width, self.settings.bullet_height)
        self.rect.midtop = ai_game.ship.rect.midtop

        # Store the bullets position as a decimal value.
        self.y = float(self.rect.y)
```

1. The `Bullet` class inherits from `Sprite` which we import from the `pygame.sprite` module. When you use sprites, you can group related elements in your game and act on all the grouped elements at once.
2. To create a Bullet instance, `__init__()` needs the current instance of `AlienInvasion`, and we call `super()` to inherit properly from `Sprite`.
3. `self.rect = pygame.Rect(0, 0, self.settings.bullet_width, self.settings.bullet_height)` - We create the bullets rect attribute. The bullet isn't based on an image, so we have to build arect from scratch using the `pygame.Rect()` class. This class requires the x- and y-coordinates of the top-left corner of the rect, and the width and height of the rect. We initialize the bullet in the top left of the screen but we'll move it to the correct location in the next line. We also pull the width and height from the attributes in settings.
4. `self.rect.midtop = ai_game.ship.rect.midtop` - We set the bullets midtop to the ships midtop to make it appear as though the bullet is coming out from the ship. We store the bullet position as a decimal value so that the speed can be adjusted.

We now need to make new methods for the Bullet class.

#### *bullet.py*
```
    def update(self):
        """Move the bullet up the screen."""
        # Update the decimal position of the bullet.
        self.y -= self.settings.bullet_speed
        # Update the rect position.
        self.rect.y = self.y

    def draw_bullet(self):
        """Draw the bullet to the screen."""
        pygame.draw.rect(self.screen, self.color, self.rect)
```

1. The `update()` method manages the bullet's position. When a bullet is fired, it moves up the screen, which corresponds to a decreasing y-coordinate value. To update the position, we subtract the amount stored in `settings.bullet_speed` from `self.y`. We then use the value of `self.y` to set the value of `self.rect.y`.
2. We call `draw_bullet()` when we want to draw a bullet. The `draw.rect()` function fills the part of the screen defined by the bullets rect with the color stored in `self.color`

### Storing Bullets in a Group; Page 248
We now can create code that fires a bullet each time a player presses space. First we need to create a group in AlienInvasion to store all the live bullets so we can manage the bullets that have already been fired. This group will be an instance of the `pygame.sprite.Group` class, which behaves like a list with some extra functionality that is helpful when building games.

#### *alien_invasion.py*
```
    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((0,0), pygame.FULLSCREEN)
        self.settings.screen_width = self.screen.get_rect().width
        self.settings.screen_height = self.screen.get_rect().height
        pygame.display.set_caption("Alien Invasion")

        self.ship = Ship(self)
        self.bullets = pygame.sprite.Group()

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            self._check_events()
            self.ship.update()
            self.bullets.update()
            self._update_screen()
```

1. `self.bullets = pygame.sprite.Group()` - Grouping the bullets.
2. `self.bullets.update()` - When this is called on a group, the group automatically calls `update()` for each sprite in the group.

### Firing Bullets; Page 249
In AlienInvasion, we need to modify `_check_keydown_events()` to fire a bullet when the player presses the spacebar. We also need to modify `_update_screen()` to make sure each bullet is drawn to the screen before we call `flip()`. We will write a new method `_fire_bullet()` as we know there will be a bit of work we need to do when we fire a bullet.

#### *alien_invasion.py*
```
    def _check_keydown_events(self, event):
        """Respond to keypresses."""
        if event.key == pygame.K_RIGHT:
            # Move the ship to the right
            self.ship.moving_right = True
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = True
        elif event.key == pygame.K_q:
            sys.exit()
        elif event.key == pygame.K_SPACE:
            self._fire_bullet()

    def _check_keyup_events(self, event):
        """Respond to key releases."""
        if event.key == pygame.K_RIGHT:
            self.ship.moving_right = False
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = False

    def _fire_bullet(self):
        """Create a new bullet and add it to the bullets group."""
        new_bullet = Bullet(self)
        self.bullets.add(new_bullet)

    def _update_screen(self):
        """Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)
        self.ship.blitme()
        for bullet in self.bullets.sprites():
            bullet.draw_bullet()

        # Make the most recently drawn screen visible.
        pygame.display.flip()
```

1. `from bullet import Bullet` - We first import our Bullet class. (Not shown in Code Snippet)
2. `elif event.key == pygame.K_SPACE`... - We then add a new if statement for our keydown event for the Space key. This calls the new `_fire_bullet()` method. We do not need to add anything for key up events as the bullet should only fire once when the key is pressed.
3. `new_bullet = Bullet(self)` - We create an instance of Bullet and call it `new_bullet`.
4. `self.bullets.add(new_bullet)` - We then add the new instance of Bullet to the group of bullets using the `add()` method. This method is similar to `append()`, but it's a method that's written specifically for Pygame groups.
5. `for bullet in self.bullets.sprites():`... - The `bullets.sprites()` method returns a list of all sprites in the group `bullets`. To draw all fired bullets to the screen, we loop through the sprites in bullets and call `draw_bullet` on each one.

At this point, the ship can now fire bullets and move left-right as needed. Bullets travel straight up the screen until they disappear upon reaching the top. Ship speed, bullet speed, and other attributes can be adjusted in the settings file as needed.

### Deleting Old Bullets; Page 250
At the moment, the bullets disappear when they reach the top, but only because Pygame can't draw them above the top of the screen. The bullets actually continue to exist; their y-coordiante values just grow increasingly negative. This is a problem, because they continue to consume memory and processing power.
We need to get rid of these old bullets as they will eventually cause the game to slow down. To do this, we need to detect when the bottom value of a bullet's rect has a value of 0, which indicates the bullet has passed off the top of the screen.

#### *alien_invasion.py*
```
    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            self._check_events()
            self.ship.update()
            self.bullets.update()

            # Get rid of bullets that have disappeared.
            for bullet in self.bullets.copy():
                if bullet.rect.bottom <= 0:
                    self.bullets.remove(bullet)
            print(len(self.bullets))

            self._update_screen()
```

1. When using a for loop with a list (or a group in Pygame), Python expacts that the list will stay the same length as long as the loop is running. Because we can't remove items from a list or group within a for loop, we have to loop over a copy of the group.
2. `for bullet in self.bullets.copy():` - We use the `copy` method to set up the for loop, which enables us to modify the bullets inside the loop.
3. `if bullet.rect.bottom <= 0:` - Check each bullet to see if it has disappeared at the top of the screen.
4. `self.bullets.remove(bullet)` - If it has passed the top of the screen, we remove it from bullets.
5. `print(len(self.bullets))` - Show how many bullets currently exist in the game and verify that they're being deleted when they reach the top of the screen. Once this was tested, this line was removed from the file.

If this program works correctly, we can monitor the terminal output while firing bullets to validate the number is increasing and returning to 0. 

### Limiting the Number of Bullets; Page 251
Many shooting games limit the number of bullets a player can have on the screen at one time; doing so encourages accuracy. We will add this feature to Alien Invasion.

#### *settings.py*
```
        # Bullet Settings
        self.bullet_speed = 1.0
        self.bullet_width = 3
        self.bullet_height = 15
        self.bullet_color = (60, 60, 60)
        self.bullets_allowed = 3
```

#### *alien_invasion.py*
```
    def _fire_bullet(self):
        """Create a new bullet and add it to the bullets group."""
        if len(self.bullets) < self.settings.bullets_allowed:
            new_bullet = Bullet(self)
            self.bullets.add(new_bullet)
```

1. First we set the bullets_allowed variable in the settings file.
2. We then modify `_fire_bullets()` to check the length of our bullets group. We will only fire a bullet if the length of the group is less than our set value of bullets_allowed.

### Creating the `_update_bullets()` Method; Page 252
We want to keep the AlienInvasion class reasonably well organized, so now that we've written and checked the bullet management code, we can move it to a separate method. We will do this through a new method.

#### *alien_invasion.py*
```
    def _update_bullets(self):
        """Update position of bullets and get rid of old bullets."""
        # Update bullet positions.
        self.bullets.update()

        # Get rid of bullets that have disappeared.
        for bullet in self.bullets.copy():
            if bullet.rect.bottom <= 0:
                self.bullets.remove(bullet)
```

1. The code from `run_game()` was cut and placed into a new method.
2. We then call this method in `run_game()` to maintain the expected behavior.
3. This results in our main loop containing minimcal code, so we can quickly read the method names and understand what's happening in the game.

## Summary; Page 253
In this chapter we learned:
1. How to make a game plan for a game and learned the basic structure of a game written in Pygame.
2. How to set a background color and store settings in a separate class where they can be adjusted more easily.
3. How to draw an image to the screen and give the player control over the movements of game elements.
4. How to create elements that move on their own, like bullets flying up a screen.
5. Deleting objects that are no longer needed.
6. To refactor code in a project on a regular basiss to facilitate ongoing development.
7. We also learned the use cases and benefits of using helper methods.

### End of Chapter Files

#### *alien_invasion.py*
```
import sys

import pygame

from settings import Settings
from ship import Ship
from bullet import Bullet

class AlienInvasion:
    """Overall class to manage game assets and behavior."""

    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((0,0), pygame.FULLSCREEN)
        self.settings.screen_width = self.screen.get_rect().width
        self.settings.screen_height = self.screen.get_rect().height
        pygame.display.set_caption("Alien Invasion")

        self.ship = Ship(self)
        self.bullets = pygame.sprite.Group()

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            # Watch for keyboard and mouse events.
            self._check_events()
            self.ship.update()
            self._update_bullets()
            self._update_screen()

    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                self._check_keydown_events(event)
            elif event.type == pygame.KEYUP:
                self._check_keyup_events(event)


    def _check_keydown_events(self, event):
        """Respond to keypresses."""
        if event.key == pygame.K_RIGHT:
            # Move the ship to the right
            self.ship.moving_right = True
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = True
        elif event.key == pygame.K_q:
            sys.exit()
        elif event.key == pygame.K_SPACE:
            self._fire_bullet()

    def _check_keyup_events(self, event):
        """Respond to key releases."""
        if event.key == pygame.K_RIGHT:
            self.ship.moving_right = False
        elif event.key == pygame.K_LEFT:
            self.ship.moving_left = False

    def _fire_bullet(self):
        """Create a new bullet and add it to the bullets group."""
        if len(self.bullets) < self.settings.bullets_allowed:
            new_bullet = Bullet(self)
            self.bullets.add(new_bullet)

    def _update_bullets(self):
        """Update position of bullets and get rid of old bullets."""
        # Update bullet positions.
        self.bullets.update()

        # Get rid of bullets that have disappeared.
        for bullet in self.bullets.copy():
            if bullet.rect.bottom <= 0:
                self.bullets.remove(bullet)



    def _update_screen(self):
        """Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)
        self.ship.blitme()
        for bullet in self.bullets.sprites():
            bullet.draw_bullet()

        # Make the most recently drawn screen visible.
        pygame.display.flip()

if __name__ == '__main__':
    # Make a new game instance, and run the game.
    ai = AlienInvasion()
    ai.run_game()
```

#### *ship.py*
```
import pygame

class Ship:
    """A class to manage the ship."""

    def __init__(self, ai_game):
        """Initialize the ship and set its starting position."""
        self.screen = ai_game.screen
        self.settings = ai_game.settings

        # Load the ship image and get its rect.
        self.image = pygame.image.load('images/ship.bmp')
        self.rect = self.image.get_rect()
        self.screen_rect = ai_game.screen.get_rect()

        # Start each new ship at the bottom center of the screen.
        self.rect.midbottom = self.screen_rect.midbottom

        # Store a decimal value for the ship's horizontal position.
        self.x = float(self.rect.x)

        # Movement flags
        self.moving_right = False
        self.moving_left = False
    
    def update(self):
        """Update the ship's position based on the movement flag."""
        # Update the ship's x value, not the rect.
        if self.moving_right and self.rect.right < self.screen_rect.right:
            self.x += self.settings.ship_speed
        if self.moving_left and self.rect.left > 0:
            self.x -= self.settings.ship_speed

        # Update rect object from self.x.
        self.rect.x = self.x

    def blitme(self):
        """Draw the ship at its current location."""
        self.screen.blit(self.image, self.rect)
```

#### *bullet.py*
```
import pygame
from pygame.sprite import Sprite

class Bullet(Sprite):
    """A class to manage bullets fired from the ship."""

    def __init__(self, ai_game):
        """Create a bullet object at the ship's current position."""
        super().__init__()
        self.screen = ai_game.screen
        self.settings = ai_game.settings
        self.color = self.settings.bullet_color

        # Create a bullet rect at (0, 0) and then set correct position.
        self.rect = pygame.Rect(0, 0, self.settings.bullet_width, self.settings.bullet_height)
        self.rect.midtop = ai_game.ship.rect.midtop

        # Store the bullets position as a decimal value.
        self.y = float(self.rect.y)

    def update(self):
        """Move the bullet up the screen."""
        # Update the decimal position of the bullet.
        self.y -= self.settings.bullet_speed
        # Update the rect position.
        self.rect.y = self.y

    def draw_bullet(self):
        """Draw the bullet to the screen."""
        pygame.draw.rect(self.screen, self.color, self.rect)
```

#### *settings.py*
```
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        # self.screen_width = 1200
        # self.screen_height = 800
        self.bg_color = (230, 230, 230)

        # Ship Settings
        self.ship_speed = 1.5

        # Bullet Settings
        self.bullet_speed = 1.0
        self.bullet_width = 3
        self.bullet_height = 15
        self.bullet_color = (60, 60, 60)
        self.bullets_allowed = 3
```

# Chapter 12 Exercises

## 12-1: Blue Sky; Page 238
Make a Pygame window with a blue background.

#### *blue_sky.py*
```
import sys

import pygame

class ExerciseGame:
    """A model of a game for this exercise."""

    def __init__(self):
        """Initialize the Exercise Game"""
        pygame.init()

        self.screen = pygame.display.set_mode((1200, 800))
        pygame.display.set_caption("Exercise Game")

        # Set BG color
        self.bg_color = (135, 206, 235)

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            self.screen.fill(self.bg_color)

            pygame.display.flip()

if __name__ == '__main__':
    # Make a new game instance, and run the game.
    eg = ExerciseGame()
    eg.run_game()
```

## 12-2: Game Character; Page 238
Find a bitmap image of a game character you like or convert an image to a bitmap. Make a class that draws the character at the center of the screen and match the background color of the image to the background color of the screen, or vice versa.

#### *game_character.py*
```
import sys

import pygame

from character import Character

class ExerciseGame:
    """A model of a game for this exercise."""

    def __init__(self):
        """Initialize the Exercise Game"""
        pygame.init()

        self.screen = pygame.display.set_mode((1200, 800))
        pygame.display.set_caption("Exercise Game")

        # Set BG color
        self.bg_color = (135, 206, 235)

        # Create Character
        self.character = Character(self)

    def run_game(self):
        """Start the main loop for the game."""
        while True:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            self.screen.fill(self.bg_color)
            self.character.blitme()

            pygame.display.flip()

if __name__ == '__main__':
    # Make a new game instance, and run the game.
    eg = ExerciseGame()
    eg.run_game()
```

#### *character.py*
```
import pygame

class Character:
    """A class to manage the character."""

    def __init__(self, eg_game):
        """Initialize the character and set their starting position."""
        self.screen = eg_game.screen
        self.screen_rect = eg_game.screen.get_rect()

        # Load the character image and get its rect.
        self.image = pygame.image.load('chapter12_exercises/images/game_character.bmp')
        self.rect = self.image.get_rect()

        # Start character at the center of the screen.
        self.rect.center = self.screen_rect.center

    def blitme(self):
        """Draw the character at its current location."""
        self.screen.blit(self.image, self.rect)
```

## 12-3 Pygame Documentation; Page 246
We're far enough into the game that you might want to look at some of the Pygame documetation. The Pygame home page is at *https://www.pygame.org/*, and the home page for the documentation is at *https://www.pygame.org/docs/*. Just skim the documentation for now. You won't need it to complete this project, but it will help if you want to modify Alien Invasion or make your own game afterward.

## 12-4 Rocket; Page 246
Make a game that begins with a rocket in the center of the screen. Allow the player to move the rocket up, down, left or right using the four arrow keys. Make sure the rocket never moves beyond any edge of the screen.

#### *rocket_game.py*
```
import sys

import pygame

from rocket_settings import Settings
from rocket import Rocket

class RocketGame:
    """Overall class for the RocketGame"""

    def __init__(self):
        """Initialize the game."""
        pygame.init()

        self.settings = Settings()

        self.screen = pygame.display.set_mode((0,0), pygame.FULLSCREEN)
        self.settings.screen_width = self.screen.get_rect().width
        self.settings.screen_height = self.screen.get_rect().height
        pygame.display.set_caption("Rocket Game")

        self.rocket = Rocket(self)

    def run_game(self):
        """Run the game."""
        while True:
            """Check events within the game."""
            self._check_events()
            self.rocket.update()
            self._update_screen()

    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                self._check_keydown_events(event)
            elif event.type == pygame.KEYUP:
                self._check_keyup_events(event)

    def _check_keydown_events(self, event):
        """Respond to keypresses."""
        if event.key == pygame.K_RIGHT:
            self.rocket.moving_right = True
        elif event.key == pygame.K_LEFT:
            self.rocket.moving_left = True
        elif event.key == pygame.K_UP:
            self.rocket.moving_up = True
        elif event.key == pygame.K_DOWN:
            self.rocket.moving_down = True
        elif event.key == pygame.K_q:
            sys.exit()

    def _check_keyup_events(self, event):
        """Respond to key releases."""
        if event.key == pygame.K_RIGHT:
            self.rocket.moving_right = False
        elif event.key == pygame.K_LEFT:
            self.rocket.moving_left = False
        elif event.key == pygame.K_UP:
            self.rocket.moving_up = False
        elif event.key == pygame.K_DOWN:
            self.rocket.moving_down = False

    def _update_screen(self):
        """Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)
        self.rocket.blitme()

        # Make the most recently drawn screen visible.
        pygame.display.flip()


if __name__ == '__main__':
    # Make a new game instance, and run the game.
    rg = RocketGame()
    rg.run_game()
```

#### *rocket.py*
```
import pygame

class Rocket:
    """Class representing the Rocket for RocketGame"""

    def __init__(self, rg_game):
        self.screen = rg_game.screen
        self.settings = rg_game.settings

        # Load the rocket image and get its rect.
        self.image = pygame.image.load('chapter12_exercises/images/rocket.bmp')
        self.rect = self.image.get_rect()
        self.screen_rect = rg_game.screen.get_rect()

        # Start each new rocket at the center of the screen.
        self.rect.center = self.screen_rect.center

        # Store a decimal value for the ship's horizontal position.
        self.x = float(self.rect.x)
        self.y = float(self.rect.y)

        # Movement flags
        self.moving_right = False
        self.moving_left = False
        self.moving_up = False
        self.moving_down = False

    def update(self):
        """Update the ship's position based on the movement flag."""
        # Update the ship's x value, not the rect.
        if self.moving_right and self.rect.right < self.screen_rect.right:
            self.x += self.settings.rocket_speed
        if self.moving_left and self.rect.left > 0:
            self.x -= self.settings.rocket_speed
        if self.moving_up and self.rect.top > 0:
            self.y -= self.settings.rocket_speed
        if self.moving_down and self.rect.bottom < self.screen_rect.bottom:
            self.y += self.settings.rocket_speed

        # Update rect object from self.x.
        self.rect.x = self.x
        self.rect.y = self.y

    def blitme(self):
        """Draw the ship at its current location."""
        self.screen.blit(self.image, self.rect)
```

#### *rocket_settings.py*
```
class Settings:
    """A class to store all settings for Rocket Game."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (135, 206, 235)

        # Ship Settings
        self.rocket_speed = 1.5
```

***NOTE:*** Something to take away from this exercise is how you visualize the axis. Since (0, 0) is the top left corner, going up is actually decreasing the Y and vice versa for going down. In addition to this, there are a number of lines that need to change or be added when working on an additional axis.

## 12-5 Keys; Page 246
Make a Pygame file that creates an empty screen. In the event loop, print the event.key attribute whenever a pygame.KEYDOWN event is detected. Run the program and press various keys to see how Pygame responds.

#### *keys.py*
```
import sys

import pygame

from keys_settings import Settings

class KeysGame:
    """A class representing the Keys Game."""
    
    def __init__(self):
        """Initialize the Keys Game."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((self.settings.screen_width, self.settings.screen_height))
        pygame.display.set_caption("Keys Game")

    def run_game(self):
        """Run the game."""
        while True:
            """Check events within the game."""
            self._check_events()
            self._update_screen()

    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                self._check_keydown_events(event)

    def _check_keydown_events(self, event):
        """Respond to keypresses."""
        if event.key == pygame.K_ESCAPE:
            sys.exit()
        elif event.key == pygame.K_SPACE:
            print(event.key)
            print(f"The event key for space is {event.key} while the value of pygame.K_SPACE is {pygame.K_SPACE}.")
        else:
            print(event.key)

    def _update_screen(self):
        """Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)

        # Make the most recently drawn screen visible.
        pygame.display.flip()


if __name__ == '__main__':
    # Make a new game instance, and run the game.
    kg = KeysGame()
    kg.run_game()
```

#### *keys_settings.py*
```
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (135, 206, 235)
```

***NOTE:*** The `event.key` attribute maps to what appears to be a numerical ID. For example, space is 32. The actual keys such as `pygame.K_SPACE` also map to a numerical value that is identical to the `event.key`. This explains how `event.key == pygame.K_SPACE` would work. This is demonstrated in this exercise by pressing space and viewing the output.

## 12-6 Sideways Shooter; Page 253
Write a game that places a ship on the left side of the screen and allows the player to move the ship up and down. Make the ship fire a bullet that travels right across the screen when the player presses the space-bar. Make sure bullets are deleted once they disappear off the screen.

#### *sideways_shooter.py*
```
import sys

import pygame
from ss_settings import Settings
from jet import Jet
from missile import Missile

class SidewaysShooter:
    """Main class for the Sideways Shooter game"""

    def __init__(self):
        """Initialze the game."""
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode((0,0), pygame.FULLSCREEN)
        self.settings.screen_width = self.screen.get_rect().width
        self.settings.screen_height = self.screen.get_rect().height
        pygame.display.set_caption("Sideways Shooter")

        self.jet = Jet(self)
        self.missiles = pygame.sprite.Group()

    def run_game(self):
        """Main Loop to Run the Game."""
        while True:
            self._check_events()
            self.jet.update()
            self._update_missiles()
            self._update_screen()

    def _check_events(self):
        """Respond to keypresses and mouse events."""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                self._check_keydown_events(event)
            elif event.type == pygame.KEYUP:
                self._check_keyup_events(event)

    def _check_keydown_events(self, event):
        """Respond to key presses."""
        if event.key == pygame.K_UP:
            self.jet.moving_up = True
        elif event.key == pygame.K_DOWN:
            self.jet.moving_down = True
        elif event.key == pygame.K_ESCAPE:
            sys.exit()
        elif event.key == pygame.K_SPACE:
            self._fire_missile()

    def _check_keyup_events(self, event):
        if event.key == pygame.K_UP:
            self.jet.moving_up = False
        elif event.key == pygame.K_DOWN:
            self.jet.moving_down = False

    def _fire_missile(self):
        """Create a new missile and add it to the missiles group."""
        if len(self.missiles) < self.settings.missiles_allowed:
            new_missile = Missile(self)
            self.missiles.add(new_missile)

    def _update_missiles(self):
        """Update position of missiles and get rid of old missiles."""
        # Update missile positions.
        self.missiles.update()

        # Get rid of missiles that have disappeared.
        for missile in self.missiles.copy():
            if missile.rect.left >= self.screen.get_rect().right:
                self.missiles.remove(missile)

    def _update_screen(self):
        """"Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)
        self.jet.blitme()
        for missile in self.missiles.sprites():
            missile.draw_missile()

        # Make the most recently drawn screen visible
        pygame.display.flip()
            


if __name__ == '__main__':
    # Make a new game instance, and run the game.
    ss = SidewaysShooter()
    ss.run_game()
```

#### *jet.py*
```
import pygame

class Jet:
    """A class to manage the jet."""

    def __init__(self, ss_game):
        """Initialze the jet and set it's starting position."""
        self.screen = ss_game.screen
        self.settings = ss_game.settings

        # Load the Jet Image and get its rect
        self.image = pygame.image.load('chapter12_exercises/images/jet.bmp')
        self.rect = self.image.get_rect()
        self.screen_rect = ss_game.screen.get_rect()

        # Start each new Jet at the left center of the screen
        self.rect.midleft = self.screen_rect.midleft

        # Store a decimal value for the jet's vertical position.
        self.y = float(self.rect.y)

        # Movement flags
        self.moving_up = False
        self.moving_down = False

    def update(self):
        """Update the Jets position based on the movement flag."""
        # Update the jets y value, not the rect.
        if self.moving_up and self.rect.top > 0:
            self.y -= self.settings.jet_speed
        if self.moving_down and self.rect.bottom < self.screen_rect.bottom:
            self.y += self.settings.jet_speed

        # Update the rect object from self.y.
        self.rect.y = self.y

    def blitme(self):
        """Draw the ship at its current location."""
        self.screen.blit(self.image, self.rect)
```

#### *missile.py*
```
import pygame
from pygame.sprite import Sprite

class Missile(Sprite):
    """A class to manage missiles fired from the jet."""
    
    def __init__(self, ss_game):
        super().__init__()
        self.screen = ss_game.screen
        self.settings = ss_game.settings
        self.color = self.settings.missile_color

        # Create a missile rect at (0, 0) and then set correct position.
        self.rect = pygame.Rect(0, 0, self.settings.missile_width, self.settings.missile_height)
        self.rect.midright = ss_game.jet.rect.midright

        # Store the missiles position as a decimal value.
        self.x = float(self.rect.x)

    def update(self):
        """Move the missiles to the right on the screen."""
        # Update the decimal position of the missile.
        self.x += self.settings.missile_speed
        # Update the rect position
        self.rect.x = self.x

    def draw_missile(self):
        """Draw the bullet to the screen."""
        pygame.draw.rect(self.screen, self.color, self.rect)
```

#### *ss_settings.py*
```
class Settings:
    """A class to store all settings for Sideways Shooter"""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen Settings
        self.bg_color = (230, 230, 230)

        # Jet Settings
        self.jet_speed = 1.5

        # Missile Settings
        self.missile_speed = 1.0
        self.missile_width = 15
        self.missile_height = 3
        self.missile_color = (60, 60, 60)
        self.missiles_allowed = 3
```