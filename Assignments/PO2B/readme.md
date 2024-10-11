Class Organization
Dice Class
•	Data:
o	sides: Integer (number of sides on the die)
o	number of dice
o	current_value: Integer (current value showing on the die)
•	Actions:
o	roll(): Randomly generates and sets current_value.
o	get_value(): Returns the current_value of the die.
•	Relationships:
o	Used by the Player class to roll dice during the game (composition).
Player Class
•	Data:
o	name: String (name of the player)
o	score: Integer (total score of the player)
o	dice_set: Array of Dice objects (the dice the player can roll)
o	player_stats: Dictionary (contains additional stats like rounds played, wins, etc.)
•	Actions:
o	roll_dice(): Rolls one die from dice_set and updates score based on placement.
o	get_score(): Returns the current score of the player.
o	update_score(points): Updates the player’s score by adding points.
o	place_die(column): Places a rolled die in a specified column on the grid.
o	remove_matching_die(column): Removes an opponent's matching die from the grid.
•	Relationships:
o	A Player HAS-A Dice (composition).
o	A Player IS-A participant in the Game (inheritance from a general Participant class, if needed).
Game Class
•	Data:
o	players: Array of Player objects (the participants in the game)
o	teams: how many players are allowed on a team. 
o	rules: String (the rules of the game)
o	current_round: Integer (tracks the current round)
•	Actions:
o	start_game(): Initializes game state and starts the game.
o	end_game(): Finalizes the game state and determines the winner.
o	get_winner(): Calculates and returns the player with the highest score.
•	Relationships:
o	The Game class HAS-A Player (composition).
Knucklebones Class
•	Data:
o	board: 2D array (3x3 grid for each player)
o	knucklebones_specific_rules: Dictionary (specific rules for scoring or placement)
•	Actions:
o	start_round(): Begins a new round of play.
o	end_round(): Ends the current round and updates scores.
o	calculate_winner or winners (): Determines the winner at the end of the game.
•	Relationships:
o	Inherits from Game, as Knucklebones IS-A Game.
Inheritance vs. Composition
•	Inheritance:
o	Knucklebones IS-A Game: Knucklebones inherits from the Game class. 
•	Composition:
o	Player HAS-A Dice: Players have a collection of dice they can roll.
o	Game HAS-A Player: The game manages a list of players participating in it


