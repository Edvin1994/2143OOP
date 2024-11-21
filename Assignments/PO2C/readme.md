#include <cstdlib>
#include <ctime>
#include <iostream>
#include <ncurses.h>

// Dice class definition
class Dice {
public:
  Dice() : current_value(0) {}

  int roll() {
    current_value = rand() % 6 + 1;
    return current_value;
  }

private:
  int current_value;
};

// Grid class definition
class Grid {
public:
  Grid() {
    // Initialize grid to 0 (empty cells)
    for (int i = 0; i < 3; ++i) {
      for (int j = 0; j < 3; ++j) {
        cells[i][j] = 0;
      }
    }
  }

  bool place_dice(int col, int value) {
    // Check from bottom to top
    for (int row = 2; row >= 0; --row) {
      if (cells[row][col] == 0) {
        cells[row][col] = value;
        return true;
      }
    }
    return false;
  }

  int get_cell_value(int row, int col) const { return cells[row][col]; }

  void display_grid(WINDOW *win) {
    // Use Ncurses to display grid on the console
    for (int i = 0; i < 3; ++i) {
      for (int j = 0; j < 3; ++j) {
        mvwprintw(win, i + 2, j * 4 + 1, "%d", cells[i][j]);
      }
    }
  }

private:
  // This is a 3x3 grid
  int cells[3][3];
};

// Player class definition
class Player {
public:
  Player(std::string name) : name(name), score(0) {}

  void take_turn(class Game &game);
  void add_to_score(int points) { score += points; }

  std::string get_name() const { return name; }
  int get_score() const { return score; }

private:
  std::string name;
  int score;
};

// Game class definition
class Game {
public:
  Game() : current_player(0) {
    srand(time(0)); // Initialize random seed
    dice = Dice();
    players[0] = Player("Player 1");
    players[1] = Player("Player 2");
    grid = Grid();
  }

  void start() {
      // Initialize Ncurses
    initscr();     
       // Don't display typed characters
    noecho();      

       // Hide cursor
    curs_set(0);  

      // Enable color
    start_color(); 

    init_pair(1, COLOR_WHITE, COLOR_BLACK); // Define color pair 1
    init_pair(2, COLOR_GREEN, COLOR_BLACK); // Define color pair 2

    while (!check_win()) {
      take_turn();
    }

    end_game();
    endwin(); // Close Ncurses
  }

  void take_turn() {
    clear(); // Clear the screen for each turn

    // Display grid and scores
    grid.display_grid(stdscr);

    mvprintw(0, 0, "%s's Turn", players[current_player].get_name().c_str());
    mvprintw(0, 20, "Score: %d", players[current_player].get_score());

    // Roll dice and prompt player for input
    int rolled_value = dice.roll();
    mvprintw(1, 0, "Rolled: %d", rolled_value);

    // Wait for input (column selection)
    mvprintw(2, 0, "Choose a column (1-3): ");
    int col =
        get_input(); // Get column from user (using Ncurses input handling)

    // Place dice and apply rules
    if (grid.place_dice(col, rolled_value)) {
      // Check for any opponent dice removal and calculate score (implement as
      // needed)
    }

    players[current_player].add_to_score(rolled_value); // Add score

    switch_turn();
    refresh();
  }

  bool check_win() {
    // Check if all cells are filled
    int filled_cells = 0;
    for (int i = 0; i < 3; ++i) {
      for (int j = 0; j < 3; ++j) {
        if (grid.get_cell_value(i, j) != 0) {
          ++filled_cells;
        }
      }
    }

    // Game ends if all cells are filled
    return filled_cells == 9;
  }

  void end_game() {
    mvprintw(10, 0, "Game Over!");
    mvprintw(11, 0, "%s wins!",
             players[0].get_score() > players[1].get_score()
                 ? players[0].get_name().c_str()
                 : players[1].get_name().c_str());
    getch();
  }

private:
  Dice dice;
  Grid grid;
  Player players[2];
  int current_player;

  void switch_turn() {
      // This switch players
    current_player = (current_player + 1) % 2; // Switch players
  }

  int get_input() {
      // Gets player input
    int ch = getch(); 
    if (ch >= '1' && ch <= '3') {
      return ch - '1'; 
    }
    return -1; 
  }
};

// Main function to run the game
int main() {
  Game game;
    // This start the game
  game.start(); 
  return 0;
}

