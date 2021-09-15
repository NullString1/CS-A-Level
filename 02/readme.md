# Startup.py
Uses sys library to get local path and append subdirectory components. This makes all scripts in ‘/components’ accessible on the PATH. The setup script from ‘/components/setup.py’ is then imported and the function `ScreenSetUp` is run.

# setup.py

## imports

`sys, pygame, os, pygame.font, socket, getpass`

## ScreenSetUp()

- Sets `size` variable to a tuple (700px, 500px)
- Sets pygame display size to `size`
- Sets pygame title to `Chancellor's School Interactive Learning Environment`
- Refreshes contents of screen with `pygame.display.flip`
- Calls `MainMenu` passing pygame `screen`

## MainMenu(screen)

- Calls pygame function `pygame.font.init`
- Loads background image from `menuImages/Menu.jpg`
- Uses `screen.blit` to draw image onto display window at coords `(0,0)`
- Loads font `Calibri.tff` with size `20`
- Stores username from return of `getpass.getuser()`
- Renders username and uses `screen.blit` to draw onto window
- Refreshes contents of screen with `pygame.display.flip`
- Loop:
	- Sets up a basic event handler
	- Calls `Exit` if event is `pygame.QUIT`
	- Checks if mouse is clicked (LCLICK)
	- Compares click pos to button areas and calls relevant handler functions
	- `Punctuation`
	- `Spelling`
	- `Recognition`
	- `Grammer`
	- `Exit`

## Punctuation(screen) / Spelling(screen) / Recognition(screen) / Grammar(screen)


- Imports local library `(Punctuation/Spelling/Recognition/Grammar)Load`
- Calls `(Punctuation/Spelling/Recognition/Grammar)Load.ScreenSetUp`
- Calls `Exit` to close current display window




	
	
# (Punctuation/Spelling/Recognition/Grammar)Load.py

Scripts (Punctuation/Spelling/Recognition/Grammar) contain very similar functions, with the only difference being the loaded background image (eg. `loadImages/grammar.jpg`), and the imported play library (eg. `GrammarPlay`) and called function from said library.

## ScreenSetUp
- See `ScreenSetUp` from `setup.py` as they are identical

## Exit(screen)
- See `Exit` from `setup.py` as they are identical

## MainMenu(screen)
- Very similar to `MainMenu` from `setup.py`, differences being the loaded background image and the available ‘buttons’.

## Back(screen)
- Imports local library `setup` from `setup.py`
- Calls `ScreenSetUp`, this loads original main menu UI

## Play(screen)
- Imports local library `(Punctuation/Spelling/Recognition/Grammar)Play`
- Calls `Question1` from previously imported library

# GrammarPlay.py

`GrammarPlay` is unlike the other Play scripts. It is essentially an empty challenge, just displaying a locked screen

## Question1
- Defines global `points`
- Sets `points` to 0 
- Loads background image `grammarImages/locked.jpg`
- Draws image to screen with `screen.blit`
- Refreshes contents of screen with `pygame.display.flip`
- Sleeps 2 seconds
- Calls `Back`

## Back(screen)
- Imports local library `setup` from `setup.py`
- Calls `ScreenSetUp`, this loads original main menu UI

# SpellingPlay.py

Contains the functions necessary for the spelling part of the game

## get_key()

Helper function which waits for a keypress and returns the key

## display_box(screen, message)

Helper function which displays a message in a box 

- Defines `fontobject` from font `Calibri.tff` with size `48`
- Draws a rectangle with colour `(255,255,255)` with at `(30, 435)` with size `(630, 40)`
- Refreshes contents of screen with `pygame.display.flip`

## ask(screen, question)

- Initiates `pygame.font`
- Defines `current_string` list to hold user entered text
- Calls `display_box`, passing `screen` object and `message` as `question` joined with `current_string`
- Loop:
	- Calls `get_key` and stores return in `inkey`
	- If `inkey` is:
		- `K_BACKSPACE` - Remove last character from `current_string`
		- `K_RETURN` 	- Break from loop
		- `K_MINUS` 	- Append `_` (underscore character) to `current_string`
		- `<= 127`		- Append letter from character code to `current_string`
	- Calls `display_box`, passing `screen` object and `message` as `question` joined with `current_string`
- Returns `current_string` concatenated into a single string


## Question`[i]`()

Question functions are very similar, differing in the background image, asked question and required answer

- Defines global `points`
- Loads background image `SpellingImages/[i].jpg`
- Draws image to screen with `screen.blit`
- Defines global `runningpointsfont`
- Sets `strpoints` to `points` as a string
- Draws text `'Points: ' + strpoints` with antialias `2`, colour `(30,144,255)`, at position `(555, 30)`
- Refreshes contents of screen with `pygame.display.flip`
- Calls `pygame.mixer.init`
- Calls `pygame.mixer.music.load` to load `"SpellingImages/[i].wav"` 
- Calls `pygame.mixer.music.play()` to play loaded music
- Compares return value of `ask`, passing `screen` object and question `Answer`, to `(Answer of [i])`
	- If correct answer given
		- `points` increased by `1`
		- Calls `Question[i+1]` (next question function)
		- (ONLY `Question10`) Calls `Uploadpoints`
	- If incorrect answer given
		- Calls `Question[i+1]` (next question function)
		- (ONLY `Question10`) Calls `Uploadpoints`

## Uploadpoints()

- Calls `Results` with argument `screen`

## Results(screen)

- Defines global `points`
- Sets `strpoints` to `points` as a string
- Sets `pointsfont` to font object returned by `pygame.font.SysFont` with font `"Calibri.tff"`, and size `160`
- Sets `resultfont` to font object returned by `pygame.font.SysFont` with font `"Calibri.tff"`, and size `136`
- Sets `userfont` to font object returned by `pygame.font.SysFont` with font `"Calibri.tff"`, and size `20`
- Loads background image `SpellingImages/results.jpg`
- Draws image to screen with `screen.blit`
- Stores username from return of `getpass.getuser()` into `user`
- Stores `user` right justified to `10` characters into `userline`
- Draws text `userline` with antialias `2`, colour `(30,144,255)`, at position `(600, 50)`
- Draws text `strpoints` with antialias `2`, colour `(30,144,255)`, at position `(420, 130)`
- If `points` >= 7
	- Draws text `'pass'` with antialias `2`, colour `(30,144,255)`, at position `(410, 270)`
- Else
	- Draws text `'fail'` with antialias `2`, colour `(30,144,255)`, at position `(445, 275)`
- Refreshes contents of screen with `pygame.display.flip`
- Loop
	- For each `event` returned from `pygame.event.get`
		- Set `mouseButtons` to returned tuple from `pygame.mouse.get_pressed`
		- If `mouseButtons` == `(1,0,0)` (Left click)
			- Set `mouseX` and `mouseY` to values from returned tuple from `pygame.mouse.get_pos`
			- If `mouseX` between `25` and `675` && `mouseY` between `430` and `480`
				- Call `Back`, passing `screen` object


## Back(screen) - used in multiple files and is identical
- Imports local library `setup` from `setup.py`
- Calls `ScreenSetUp`, this loads original main menu UI
