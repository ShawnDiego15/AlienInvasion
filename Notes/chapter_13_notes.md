# Alien Invasion

# Chapter 13; Aliens!; Page 255
In this chapter we will add aliens near the top of the screen and then generate a whole fleet of aliens. We will have the fleet advance sideways and down, and we'll get rid of any aliens hit by a bullet. Finally, we will limite the number of ships a player has and end the game when the player runs out of ships.

## Reviewing the Project; PAge 256
When beginnining a new phase of development on a large project, it's always a good idea to revisit your plan and clarify what you want to accomplish with the code you're about to write. Our plan for this chapter is as follows:
- Examine out code and determine if we need to refactor before implementing new features.
- Add a single alien to the top-left corner of the screen with appropriate spacing around it.
- Use the spacing and overall screen size to determine how many aliens can fit on the screen.
- Make the fleet move sideways and down until the entire fleet is shot down.
- Destroy the ship if hit.
- Limit the number of ships the player can use and end the game when the player has used up the alloted number of ships.

This plan will be refined as we implement features. Existing code should be reviewed at intervals to clean up and refactor before moving forward on new features.

## Creating the First Alien; Page 256
Placing one alien on the screen will follow the same path as placing a ship on the screen. We will also control the alien's behavior through a class. A new image was uploaded to represent the alien.

### Creating the Alien Class; Page 257
We will start by creating the Alien class.

#### *alien.py*
```
import pygame
from pygame.sprite import Sprite

class Alien(Sprite):
    """A class to represent a single alien in the fleet."""

    def __init__(self, ai_game):
        """Initialize the alien and set its starting position."""
        super().__init__()
        self.screen = ai_game.screen

        # Load the alien image and set its rect attribute.
        self.image = pygame.image.load('images/alien.bmp')
        self.rect = self.image.get_rect()

        # Start each new alien near the top left of the screen.
        self.rect.x = self.rect.width
        self.rect.y = self.rect.height

        # Store the alien's exact horiziontal position.
        self.x = float(self.rect.x)
```

1. `self.rect.x = self.rect.width` - Most of this class is like the Ship class except for the aliens' placement on the screen. We initially place each alien near the top-left corner of the screen; we add a space to the left of it that's equal to the alien's width and a space above it equal to its height so its easy to see.
2. `self.x = float(self.rect.x)` - We are mainly concerned with the aliens horiziontal speed, so we'll track the position of each alien precisely.
3. This Alien class doesn't need a method for drawing it to the screen; instead, we will use a Pygame group method that automatically draws all the elements of a group to the screen.

### Creating an Instance of Alien; Page 258
We want to create an instance of Alien so we can see the first alien on the screen. We will eventually make a fleet of aliens and will be creating a new helper method to avoid extra work. The order of methods in a class does not matter, as long as theres some consistency to how they're placed.

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
        self.aliens = pygame.sprite.Group()

        self._create_fleet()
    
    # ---- Snip ----

    def _create_fleet(self):
        """Create the fleet of aliens."""
        # Make an alien.
        alien = Alien(self)
        self.aliens.add(alien)

    def _update_screen(self):
        """Update the images on the screen, and flip to the new screen."""
        # Redraw the screen during each pass through the loop.
        self.screen.fill(self.settings.bg_color)
        self.ship.blitme()
        for bullet in self.bullets.sprites():
            bullet.draw_bullet()
        self.aliens.draw(self.screen)

        # Make the most recently drawn screen visible.
        pygame.display.flip()
```

1. We first import the Alien class (not shown)
2. Then, we will update the `__init__()` method.
3. Next, we create the new `_create_fleet()` helper method.
4. Lastly, to make the alien appear, we then need to call the group's `draw()` method in `_update_screen()`.

When you call `draw()` on a group, Pygame draws each element in the group at the position defined by its `rect` attribute. The `draw()` method requires on argument: a surface on which to draw the elements from the group. At this point, a single alien appears in the top-left corner of our screen.

## Building the Alien Fleet; Page 259
To draw a fleet, we need to figure out how many aliens can fit across the screen and how many rows of aliens can fit down the screen. We first figure out the horizontal spacing between aliens and create a row; then we'll determine the vertical spacing and create an entire fleet.

### Determining How Many Aliens Fit in a Row; Page 260
The screen width is stored in `settings.screen_width` but we need an empty margin on either side of the screen. We'll make this margin the width of one alien. Because we have two margins, the available space is the screen width minus two alien widths:

`available_space_x = settings.screen_width - (2 * alien_width)`

We also need to set the spacing between aliens, which we will make the width of one alien. The space needed to display one alien is twice its width: one width for the alien and one width for the empty space to its right. To find the number of aliens that fit across the screen, we divide the available space by two times teh width of an alien. We use ***floor division (//)***, which divides two numbers and drops any remainder, so we'll get an integer number of aliens.

`number_aliens_x = available_space_x // (2 * alien_width)`

In programming you can always adjust your calculations as needed based on the results. These will be used to create our fleet.

### Creating a Row of Aliens; Page 260
We are now ready to generate a full row of aliens. We know that our code for a single alien works in `_create_fleet()` so we will rewrite this method for a fleet.

#### *alien_invasion.py*
```
    def _create_fleet(self):
        """Create the fleet of aliens."""
        # Create an alien and find the number of aliens in a row.
        # Spacing between each alien is equal to one alien width.
        alien = Alien(self)
        alien_width = alien.rect.width
        available_space_x = self.settings.screen_width - (2 * alien_width)
        number_aliens_x = available_space_x // (2 * alien_width)

        # Create the first row of aliens.
        for alien_number in range(number_aliens_x):
            # Create an alien and place it in the row.
            alien = Alien(self)
            alien.x = alien_width + 2 * alien_width * alien_number
            alien.rect.x = alien.x
            self.aliens.add(alien)
```

1. `alien = Alien(self)` - We first create an alien to grab its width and height to place aliens.
2. `alien_width = alien.rect.width` - We get the aliens width from its rect attribute and store this value in alien_width so we don't have to keep working through this attribute.
3. `available_space_x = self.settings.screen_width - (2 * alien_width)` - We calculate the horizontal space available for aliens and the number of aliens that can fit into that space.
4. `for alien_number in range(number_aliens_x):` - A loop is used that counts from 0 to the number of aliens we need to make.
5. `alien.x = alien_width + 2 * alien_width * alien_number` - We create a new alien and then set its x-coordinate value to place it in the row. Each alien is pushed to the right one alien width from the left margin. Next, we multiply the alien width by 2 t oaccount for the space each alien takes up, including the empty space to its right. We then multiply this amount by the alien's position in the row. We use the alien's x attribute to set the position of its rect, then add each new alien to the group aliens.

When running the program at this point, we should have a full top row of aliens. Aliens are offset to the left, which is preferred.

### Refactoring `_create_fleet()`; Page 262
To clean up this method, we will create a new helper method and call it from `_create_fleet()`.

#### *alien_invasion.py*
```
    def _create_fleet(self):
        """Create the fleet of aliens."""
        # Create an alien and find the number of aliens in a row.
        # Spacing between each alien is equal to one alien width.
        alien = Alien(self)
        alien_width = alien.rect.width
        available_space_x = self.settings.screen_width - (2 * alien_width)
        number_aliens_x = available_space_x // (2 * alien_width)

        # Create the first row of aliens.
        for alien_number in range(number_aliens_x):
            # Create an alien and place it in the row.
            self._create_alien(alien_number)

    def _create_alien(self, alien_number):
        """Create an alien and place it in the row."""
        alien = Alien(self)
        alien_width = alien.rect.width
        alien.x = alien_width + 2 * alien_width * alien_number
        alien.rect.x = alien.x
        self.aliens.add(alien)
```

- This new method requires an additional parameter to self, the alien number being created.
- We also get the width of the alien inside the method instead of passing it as an argument.

This method will make it easier to add new rows and create an entire fleet.

### Adding Rows; Page 262
To finish the fleet, we will determine the number of rows that fit on the screen and then repeat the loop for creating aliens in one row until we have the correct number of rows. To determine the number of rows, we find the available vertical space by subtracting the alien height from the top, the ship height from the bottom, and two alien heights from the bottom of the screen.

`available_space_y = settings.screen_height - (3 * alien_height) - ship_height`

For the space available below a row, we will make it equal to the height of one alien. We will use floor division again since we can only have an integer number of rows.

`number_rows = available_space_y // (2 * alien height)`

Using these calculations, we can now repeat the code for creating a row.

#### *alien_invasion.py*
```
    def _create_fleet(self):
        """Create the fleet of aliens."""
        # Create an alien and find the number of aliens in a row.
        # Spacing between each alien is equal to one alien width.
        alien = Alien(self)
        alien_width, alien_height = alien.rect.size
        available_space_x = self.settings.screen_width - (2 * alien_width)
        number_aliens_x = available_space_x // (2 * alien_width)

        # Determine the number of rows of aliens that fit on the screen.
        ship_height = self.ship.rect.height
        available_space_y = (self.settings.screen_height - (3 * alien_height) - ship_height)
        number_rows = available_space_y // (2 * alien_height) 

        # Create the full fleet of aliens.
        for row_number in range(number_rows):
            # Create an alien and place it in the row.
            for alien_number in range(number_aliens_x):
                self._create_alien(alien_number, row_number)

    def _create_alien(self, alien_number, row_number):
        """Create an alien and place it in the row."""
        alien = Alien(self)
        alien_width, alien_height = alien.rect.size
        alien.x = alien_width + 2 * alien_width * alien_number
        alien.rect.x = alien.x
        alien.rect.y = alien.rect.height + 2 * alien.rect.height * row_number
        self.aliens.add(alien)
```

1. `alien_width, alien_height = alien.rect.size` - We use the attribute `size`, which contains a tuple with the width and height of the rect object to get the width and height of an alien.
2. `available_space_y = (self.settings.screen_height - (3 * alien_height) - ship_height)` - We use the calculation we made to calculate the number of rows we can fit on the screen.
3. `for row_number in range(number_rows):` - We use two nested loops to create multiple rows. The inner loop creates the aliens in one row while the outer loop counts the number of rows we want.
4. We then need to pass the row number into the `_create_alien()` method.
5. `alien.rect.y = alien.rect.height + 2 * alien.rect.height * row_number` - We change the alien's y-coordinate value when it's not in the first row by starting with one alien's height to create empty space at the top of the screen.

At this point in the program, a full fleet should appear.

## Making the Fleet Move; Page 265

# Exercises

## 13-1 Stars; Page 264
Find an image of a star. Make a grid of stars appear on the screen.

## 13-2 Better Stars; Page 264
You can make a more realistic star pattern by introducing randomness when you place each star. Recall that you can get a random number like this:

```
from random import randint
random_number = randint(-10, 10)
```

This code returns a random integer between -10 and 10. Using your code in Exercise 13-1, adjust each star's position by a random amount.