# Tic-Tac-Toe LLD Design Notes

## Problem Understanding

Tic-Tac-Toe is fundamentally a board game where:

* Two players play alternately.
* Each move updates the board state.
* A player may win after a move.
* The game ends either when someone wins or the board is full.
* Undo/Redo functionality should be supported.

The goal of the design is to keep responsibilities clearly separated and avoid storing duplicate state.

---

# Domain Model

## Player

```java
class Player {
    String name;
    char symbol;
}
```

### Why?

A player is simply a participant in the game.

The player does not own the board and does not perform moves directly.

A player only provides:

* Identity (name)
* Symbol (X/O)

Keeping Player simple makes the model easier to understand and extend.

---

## Move

```java
class Move {
    int row;
    int col;
    Player player;
}
```

### Why?

A move represents a single action:

> Player P placed a symbol at (row, col).

Initially, `isGameDone` was considered inside Move, but later removed.

Reason:

* A move should represent an action.
* Game completion is game state, not move state.
* Storing game state inside every move introduces unnecessary coupling.

Therefore Move only stores information required to replay or undo that move.

---

## Board

```java
class Board {
    char[][] cells;
}
```

### Responsibilities

Board owns:

* Current board state
* Cell validation
* Cell updates
* Winner detection

Board does NOT own:

* Players
* Turn management
* History
* Undo/Redo

### Why?

Board is simply the playing surface.

It should focus only on board-related operations.

---

## TicTacToe (Game)

```java
class TicTacToe {
    Board board;
    Player player1;
    Player player2;
    List<Move> history;
}
```

### Responsibilities

Game owns:

* Players
* Turn management
* History
* Undo/Redo
* Game flow

The Game acts as the orchestrator.

---

# Turn Management

## Initial Design

Initially:

```java
makeMove(row, col, Player p)
```

was used.

This required:

```java
isValidPlayer()
```

to verify whether the correct player was making the move.

---

## Problem

The client could perform:

```java
game.makeMove(..., player1);
game.makeMove(..., player1);
```

which violates game rules.

The Game then had to validate the caller's behavior.

---

## Final Design

The Game owns turn management.

```java
makeMove(row, col)
```

Only the Game decides whose turn it is.

This removed the need for:

```java
isValidPlayer()
```

entirely.

---

# Current Player Design

## Initial Design

```java
Player currPlayer;
```

was stored as mutable state.

---

## Problem

Undo and Redo constantly required updating:

```java
currPlayer
```

which created synchronization complexity.

---

## Final Design

Current player is derived from:

```java
currIndex
```

Example:

```java
currIndex % 2 == 0 ? player1 : player2
```

### Benefit

No extra state to maintain.

Undo and Redo automatically remain correct.

---

# History Design

## Important Insight

History should represent actions.

History should not represent current game state.

Therefore:

```java
List<Move> history;
```

was chosen.

instead of:

```java
List<GameState>
```

---

# Cursor-Based History

A cursor model is used.

```java
currIndex
```

represents:

> Next executable position.

Example:

```text
[M1, M2, M3]
             ^
         currIndex = 3
```

All three moves are currently applied.

---

# Undo

Undo operation:

```text
Move cursor backward
Reverse the move
```

Example:

```text
[M1, M2, M3]
         ^
currIndex = 2
```

Only M1 and M2 remain active.

---

# Redo

Redo operation:

```text
Replay move at currIndex
Increment cursor
```

No new history is created.

Redo only replays existing history.

---

# New Move After Undo

Example:

```text
History:
M1 M2 M3 M4

Undo twice:

M1 M2 | M3 M4
```

If user creates a new move:

```text
M5
```

Then:

```text
M3 M4
```

must be deleted.

Final history becomes:

```text
M1 M2 M5
```

This matches behavior found in:

* IDEs
* Text editors
* Browser navigation
* Drawing applications

---

# Game Completion

## Initial Design

```java
boolean isGameDone;
```

was maintained.

---

## Problem

Every operation had to keep it synchronized:

* makeMove()
* undo()
* redo()

This creates opportunities for bugs.

---

## Final Design

Game completion is derived.

```java
isBoardFilled() || isAnyPlayerWins()
```

### Benefit

No duplicate state.

The system always calculates the answer from the current board state.

---

# Essence of the Design

The most important lesson from this design exercise is:

> Every class should own only the information it is naturally responsible for.

### Player

Knows who is playing.

### Move

Knows what action happened.

### Board

Knows the board state.

### Game

Knows how the game progresses.

By assigning responsibilities carefully, many unnecessary validations and state variables disappeared naturally.

The final design is simpler, easier to reason about, and easier to extend in the future.

---

### Final Design Summary

Player
 ├─ name
 └─ symbol

Move
 ├─ row
 ├─ col
 └─ player

Board
 ├─ cells
 ├─ validation
 ├─ winner detection
 └─ board modifications

TicTacToe
 ├─ players
 ├─ history
 ├─ currIndex (cursor)
 ├─ undo/redo
 ├─ turn management
 └─ game orchestration

 ```java
 import java.util.ArrayList;
import java.util.List;

class Player {
    private final String name;
    private final char symbol;

    Player(String name, char symbol) {
        this.name = name;
        this.symbol = symbol;
    }

    public String getName() {
        return name;
    }

    public char getSymbol() {
        return symbol;
    }
}

class Move {
    final int row;
    final int col;
    final Player player;

    Move(int row, int col, Player player) {
        this.row = row;
        this.col = col;
        this.player = player;
    }
}

class Board {

    private final char[][] cells;
    private final int rows;
    private final int cols;

    Board(int rows, int cols) {
        this.rows = rows;
        this.cols = cols;

        cells = new char[rows][cols];

        for (int i = 0; i < rows; i++) {
            java.util.Arrays.fill(cells[i], '*');
        }
    }

    boolean placeSymbol(int row, int col, char symbol) {

        if (!isValidPosition(row, col)) {
            return false;
        }

        cells[row][col] = symbol;
        return true;
    }

    boolean clearPosition(int row, int col) {

        if (row < 0 || row >= rows || col < 0 || col >= cols) {
            return false;
        }

        cells[row][col] = '*';
        return true;
    }

    boolean isValidPosition(int row, int col) {

        return row >= 0
                && row < rows
                && col >= 0
                && col < cols
                && cells[row][col] == '*';
    }

    boolean hasWinner() {

        // Winner checking logic

        return false;
    }

    int totalCells() {
        return rows * cols;
    }
}

public class TicTacToe {

    private final Board board;

    private final Player player1;
    private final Player player2;

    /*
     * Cursor points to next executable position
     */
    private int currIndex;

    private final List<Move> history;

    TicTacToe(Board board,
              Player player1,
              Player player2) {

        this.board = board;
        this.player1 = player1;
        this.player2 = player2;

        this.currIndex = 0;
        this.history = new ArrayList<>();
    }

    public void makeMove(int row, int col) {

        if (isGameCompleted()) {
            System.out.println("Game already completed");
            return;
        }

        Player currentPlayer = getCurrentPlayer();

        if (!board.placeSymbol(row, col, currentPlayer.getSymbol())) {
            return;
        }

        /*
         * User made new move after undo.
         * Delete future history.
         */
        if (currIndex < history.size()) {
            history.subList(currIndex, history.size()).clear();
        }

        history.add(
                new Move(
                        row,
                        col,
                        currentPlayer
                )
        );

        currIndex++;
    }

    public void undo() {

        if (currIndex == 0) {
            return;
        }

        currIndex--;

        Move moveToUndo = history.get(currIndex);

        board.clearPosition(
                moveToUndo.row,
                moveToUndo.col
        );
    }

    public void redo() {

        if (currIndex >= history.size()) {
            return;
        }

        Move moveToRedo = history.get(currIndex);

        board.placeSymbol(
                moveToRedo.row,
                moveToRedo.col,
                moveToRedo.player.getSymbol()
        );

        currIndex++;
    }

    private Player getCurrentPlayer() {

        return currIndex % 2 == 0
                ? player1
                : player2;
    }

    private boolean isBoardFilled() {

        return currIndex == board.totalCells();
    }

    private boolean isGameCompleted() {

        return isBoardFilled()
                || board.hasWinner();
    }
}
 ```