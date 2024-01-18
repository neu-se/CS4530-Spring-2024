---
layout: assignment
title: "Individual Project 1"
permalink: /assignments/ip1
parent: Assignments
nav_order: 1
due_date: "Wednesday January 24, 11:00am EST"
submission_notes: Submit via Autograder.io at neu.autograder.io
---

Welcome aboard to the Covey.Town team! We're glad that you're here and ready to join our development team as a new software engineer.
We're building an open source virtual meeting application, and are very happy to see that we have so many new developers who can help make this application a reality.
By the end of the semester, you'll be able to propose, design, implement and test a new feature for our project.
We understand that some of you may have some web development experience, but don't expect that most of you do, and hence, have created an individual project to help you get up to speed with our existing codebase and development environment.

Covey.Town is a web application that consists of some code that runs in each client's web browser, and also code that runs on a server.
Users join the application in a "town": a 2D arcade-style map with different rooms to explore.
Each town is also a video call: when two players get close to each other, they can see and hear each other; there is also a text chat available within the town.
In Winter of 2021, our lead software engineer, Avery, developed a prototype for Covey.Town, and since then, hundreds of students in this class have built on that codebase.
The most recent class-wide effort, in Fall 2023, added a concept called [Game Areas](https://neu-se.github.io/CS4530-Fall-2023/assignments/ip1), allowing players to play games within a special part of the town. 
Students implemented a single game, [Tic-Tac-Toe](https://en.wikipedia.org/wiki/Tic-tac-toe), as a proof-of-concept for this feature.

The objective for this semester's individual project is to extend this new `GameArea` abstraction, implementing [Connect Four](https://en.wikipedia.org/wiki/Connect_Four) as a new game that can be played within a `GameArea`.
This implementation effort will be split across two deliverables. In this first deliverable, you will implement and test the core backend components for this feature, and in the second deliverable, you will implement and test the frontend components. 

## Change Log
* 1/9/2024: Fixed a typo in handout (`status` in the `ConnectFourGame` constructor should be `WAITING_FOR_PLAYERS`, not `WAITING_TO_START`), removed reference to `shared` directory in `tsconfig.json`.
* 1/12/2024: Added a detail in the spec of `startGame` method for determining the `firstPlayer` given a prior game to remove any ambiguity (The first player of the game will be the other color of the last game, if at least one player from the previous game joins the game and they get the same color that they were in the previous game).
* 1/17/2024: Add clarification to `_leave` method
* 1/18/2024: Add clarification to `handleCommand` method

## Objectives of this assignment
The objectives of this assignment are to:
*  Get you familiar with the basics of TypeScript, VSCode, and the project codebase
*  Learn how to read and write code in TypeScript
*  Translate high-level requirements into code
*  Learn how to write unit tests with Jest

## Getting started with this assignment

Before you begin, be sure to check that you have NodeJS 18.x installed, along with VSCode. We have provided a [tutorial on setting up a development environment for this class]({{site.baseurl}}{% link tutorials/week1-getting-started.md %}) 
Start by [downloading the starter code]({{site.baseurl}}{% link /Assignments/ip1/ip1-handout.zip %}). Extract the archive and run `npm ci` to fetch the dependencies. **Please extract the entire archive** instead of copying files over.

You may see the following warnings:

![image]({{site.baseurl}}{% link /Assignments/ip1/npm-warnings.jpg %})

You can safely ignore these warnings: the assignment works with npm 10, and the vulnerabilities are not relevant for this assignment. (Do **not** run `npm audit fix` or `npm audit fix --force` as this may break your copy of the handout code. If this happens to you, please restart with a fresh copy of the handout.)

Avery has provided you with some very basic sanity tests that you can extend for testing your implementation as you go. You can run these tests with the command `npm test ConnectFour` (note that many tests are *expected* to fail until you have begun to implement the assignment).

## Submission Instructions & Grading
You will submit your assignment to the instance of Autograder.io running at [neu.autograder.io](https://neu.autograder.io).
Navigate to [neu.autograder.io](https://neu.autograder.io) in your web browser, click the "Sign in" button, and log in with your Northeastern account.
You should then see the course listed on the neu.autograder.io home page.
*The autograder will be open by Wed., Jan 10. We will publish an announcement indicating when it is open.*

All students who were enrolled in the class on 1/7 were added to the roster on Autograder.io. 
If you do not see the course listed on Autograder.io, please contact the instructors immediately via email or Piazza post so that we can add you.
If you haven't been added to the course roster yet, you can access the assignment submission page at [this direct link](https://neu.autograder.io/web/project/12).

To submit your assignment: run the command `npm run zip` in the top-level directory of the handout. This will produce a file called `covey-town-townService.zip`. Submit that zip file on Autograder.io.

This submission will be scored out of 100 points, 90 of which will be automatically awarded by the grading script, with the remaining 10 manually awarded by the course staff.

Your code will automatically be evaluated for linter errors and warnings. Submissions that have *any* linter errors will automatically receive a grade of 0. **Do not wait to run the linter until the last minute**. To check for linter errors, run the command `npm run lint` from the terminal. The handout contains the same eslint configuration that is used by our grading script.

Your code will be automatically evaluated for functional correctness by a test suite that expands on the core tests that are distributed in the handout. 
Your tests will be automatically evaluated for functional correctness by a process that will inject bugs into our reference solution. 
This process proceeds as follows:
1. Your tests will be run against our (correct) reference solution. If any of your tests fail (i.e., a "false positive"), you will receive __no credit__ for your test cases.
2. Your tests will be run against a sequence of injected bugs, and you will be awarded points for each bug your tests detect. Note: Each submission will be graded against the same set of injected bugs (repeated submissions will not receive new/different injected bugs).

You will receive limited feedback on which injected bugs you do or do not find (e.g., "Your tests detected 5 bugs"). 
You will additionally be able to request up to 2 automatic hints per task (e.g., tests for joining the game, tests for leaving the game) per submission.
These hints will provide additional guidance on what to test based on which bugs your tests did not detect.
Please take a moment to provide feedback on these hints through the autograder's interface.

The autograding script will impose a strict rate limit of 5 submissions per 24 hours.
This submission limit resets at 11am EST.
Submissions that contain linter errors and warnings will not count towards that limit.
This limit exists to encourage you to start early on this assignment: students generally report that assignments like this take between 3-20 hours.
We strongly encourage you to lint and test your submission on your local development machine, and *not* rely on Autograder.io for providing grading feedback - relying on Autograder.io is a very slow feedback loop.
If you start early, you will be able to take full advantage of the resources that we provide to help you succeed: office hours, discussion on Piazza --- and the ability to have a greater total number of submission attempts and hints.

Your code will be manually evaluated for conformance to our course [style guide]({{ site.baseurl }}{% link style.md %}). This manual evaluation will account for 10% of your total grade on this assignment. We will manually evaluate your code for style on the following rubric:

To receive all 10 points:
* All new names (e.g. for local variables, methods, and properties) follow the naming conventions defined in our style guide
* There are no unused local variables
* All public properties and methods (other than getters, setters, and constructors) are documented with JSDoc-style comments that describes what the property/method does, as defined in our style guide
* The code and tests that you write generally follows the design principles discussed in week one. In particular, your design does not have duplicated code that could have been refactored into a shared method.

We will review your code and note each violation of this rubric. We will deduct two points for each violation, up to a maximum of deducting all 10 style points.

## Overview of Relevant Classes

<script src="{{site.baseurl}}/assets/js/mermaid.min.js"></script>
<div class="mermaid">
%%{init: { 'theme':'forest', } }%%
classDiagram
    class Game {
      +GameState state
      +GameInstanceID id
      ~Player[] _players
      ~GameResult _result
    + join(player: Player)
    + leave(player: Player)
    ~ _join(player: Player)
    ~ _leave(player: Player)
    + applyMove(move: GameMove)

    }
    class  GameArea {
        ~Game _game
        ~GameResult _history
    }
    class ConnectFourGame {

    }
    class ConnectFourGameArea {
    }
    class GameResult {
        +GameInstanceID gameID
        +Map scores
    }
    GameArea o-- GameResult
    ConnectFourGame ..|> Game
    ConnectFourGameArea ..|> GameArea
    ConnectFourGameArea o-- ConnectFourGame
    GameArea o-- Game
</div>

State diagram for game status:
<div class="mermaid">
%%{init: { 'theme':'forest', } }%%
flowchart TD

    I([New Game])--> A
    A[WAITING_FOR_PLAYERS] --> |1 player joins| A
    A --> |2nd player joins| B
    B --> |A player leaves| A
    B[WAITING_TO_START] --> |1 player clicks ready|B
    B --> |Other player clicks ready| C
    C[IN_PROGRESS] --> |Game Ends or a player leaves| D
    D[OVER] --> |1 player clicks 'new game'| B
</div>

## Implementation Tasks
This deliverable has four parts; each part will be graded on its own rubric. You should complete the assignment one part at a time, in the order presented here.

**General Requirements**: Implement your code *only* in the files specified: `src/town/games/ConnectFourGame.ts`, `src/town/games/ConnectFourGame.test.ts` and `src/town/games/ConnectFourGameArea.ts`. You should not install any additional dependencies. The autograder will ignore any other files that you modify, and will not install any dependencies that you add to the project.

### Task 1: Joining and Leaving the ConnectFourGame (10 points)
The `ConnectFourGame` class extends the base `Game` class, implementing the semantics of the game Connect Four. Avery has provided a definition for the types that will be used to represent the state of a `ConnectFourGame` - `ConnectFourGameState`. That type definition is reproduced below:

{% highlight typescript %}
/**
 * Type for the state of a ConnectFour game.
 * The state of the game is represented as a list of moves, and the playerIDs of the players (red and yellow)
 */
export interface ConnectFourGameState extends WinnableGameState {
  // The moves in this game
  moves: ReadonlyArray<ConnectFourMove>;
  // The playerID of the red player, if any
  red?: PlayerID;
  // The playerID of the yellow player, if any
  yellow?: PlayerID;
  // Whether the red player is ready to start the game
  redReady?: boolean;
  // Whether the yellow player is ready to start the game
  yellowReady?: boolean;
  // The color of the player who will make the first move
  firstPlayer: ConnectFourColor;
}

/**
 * Type for a move in ConnectFour
 * Columns are numbered 0-6, with 0 being the leftmost column
 * Rows are numbered 0-5, with 0 being the top row
 */
export interface ConnectFourMove {
  gamePiece: ConnectFourColor;
  col: ConnectFourColIndex;
  row: ConnectFourRowIndex;
}

/**
 * Row indices in ConnectFour start at the top of the board and go down
 */
export type ConnectFourRowIndex = 0 | 1 | 2 | 3 | 4 | 5;
/**
 * Column indices in ConnectFour start at the left of the board and go right
 */
export type ConnectFourColIndex = 0 | 1 | 2 | 3 | 4 | 5 | 6;

export type ConnectFourColor = 'Red' | 'Yellow';
{% endhighlight %}

Your first task is to implement the `_join`, `startGame` and `_leave` methods of `ConnectFourGame`. To implement these methods, you should not need to read any other parts of the codebase besides `Game.ts` and `ConnectFourGame.ts`. You might find it useful or necessary to modify the constructor of `ConnectFourGame` to initialize the state of the game, and to add private instance variables and/or helper methods.

> **Clarification** 
> 
> `_leave` method: If the game is in progress, the game will be over after a player leaves. 
> A winning player is decided according to the game rules of Connect Four. 
> Leaving a game that leaves the game status to "OVER" would be considered a "forfeit" by the player who left the game.
> A new game must be initialized after the game is over and a game object is not "re-used" once it is "IN_PROGRESS".
> _Think about a ConnectFourGame object or instance as a single game._
> 
> About tests: If you are not sure about the specification of a method, you can always use tests as a method to validate your assumptions.

{::options parse_block_html="true" /}
<details><summary markdown="span">View the specification for these methods</summary>
{% highlight typescript %}
  /**
   * Joins a player to the game.
   * - Assigns the player to a color (red or yellow). If the player was in the prior game, then attempts
   * to reuse the same color if it is not in use. Otherwise, assigns the player to the first
   * available color (red, then yellow).
   * - If both players are now assigned, updates the game status to WAITING_TO_START.
   *
   * @throws InvalidParametersError if the player is already in the game (PLAYER_ALREADY_IN_GAME_MESSAGE)
   * @throws InvalidParametersError if the game is full (GAME_FULL_MESSAGE)
   *
   * @param player the player to join the game
   */
  protected _join(player: Player): void

  /**
   * Indicates that a player is ready to start the game.
   *
   * Updates the game state to indicate that the player is ready to start the game.
   *
   * If both players are ready, the game will start.
   *
   * The first player (red or yellow) is determined as follows:
   *   - If neither player was in the last game in this area (or there was no prior game), the first player is red.
   *   - If at least one player was in the last game in this area and they get the same color that they were in the last game, then the first player will be the other color from last game.
   *   - If a player from the last game *left* the game and then joined this one, they will be treated as a new player (not given the same color by preference).
   *
   * @throws InvalidParametersError if the player is not in the game (PLAYER_NOT_IN_GAME_MESSAGE)
   * @throws InvalidParametersError if the game is not in the WAITING_TO_START state (GAME_NOT_STARTABLE_MESSAGE)
   *
   * @param player The player who is ready to start the game
   */
  public startGame(player: Player): void 

  /**
   * Removes a player from the game.
   * Updates the game's state to reflect the player leaving.
   *
   * If the game state is currently "IN_PROGRESS", updates the game's status to OVER and sets the winner to the other player.
   *
   * If the game state is currently "WAITING_TO_START", updates the game's status to WAITING_FOR_PLAYERS.
   *
   * If the game state is currently "WAITING_FOR_PLAYERS" or "OVER", the game state is unchanged.
   *
   * @param player The player to remove from the game
   * @throws InvalidParametersError if the player is not in the game (PLAYER_NOT_IN_GAME_MESSAGE)
   */
  protected _leave(player: Player): void

{% endhighlight %}
</details>

*Testing*: Avery has provided you with some very simple (and incomplete) tests for `_join`. You can run these tests by running the command `npx jest --watch ConnectFourGame.test`, which will automatically re-run the tests as you update the file (note that tests for `applyMove` will also run - but you can ignore those at this point). As you read and understand the specification, you should extend the existing test suite, adding tests to cover the entire specification. Please implement these additional tests in the file `src/town/games/ConnectFourGame.test.ts`.

Grading for implementation tasks:
* `_join`: 2 points
* `startGame`: 1 points
* `_leave`: 1 points

Grading for testing tasks:
* `_join`: 2 points
* `startGame`: 2 points
* `_leave`: 2 points 

### Task 2: Connect Four Game Semantics (70 points total)
The next (and largest) task for this deliverable is to implement the method `ConnectFourGame.applyMove`, which applies a player's move to the game. This method is responsible for validating the move, and updating the game state to reflect the move. Given the complexity of this method, you should anticipate creating (at least one) private, helper method to implement its logic.

<details><summary markdown="span">View the specification for this method</summary>
{% highlight typescript %}
  /**
   * Applies a move to the game.
   * Uses the player's ID to determine which color they are playing as (ignores move.gamePiece).
   *
   * Validates the move, and if it is valid, applies it to the game state.
   *
   * If the move ends the game, updates the game state to reflect the end of the game,
   * setting the status to "OVER" and the winner to the player who won (or "undefined" if it was a tie)
   *
   * @param move The move to attempt to apply
   *
   * @throws InvalidParametersError if the game is not in progress (GAME_NOT_IN_PROGRESS_MESSAGE)
   * @throws InvalidParametersError if the player is not in the game (PLAYER_NOT_IN_GAME_MESSAGE)
   * @throws INvalidParametersError if the move is not the player's turn (MOVE_NOT_YOUR_TURN_MESSAGE)
   * @throws InvalidParametersError if the move is invalid per the rules of Connect Four (BOARD_POSITION_NOT_VALID_MESSAGE)
   *
   */
  public applyMove(move: GameMove<ConnectFourMove>): void
{% endhighlight %}
</details>

Grading for implementation tasks:
* Correctly validating who is the first player: 5 points
* Applying moves: 7 points
* Checking for invalid moves: 12 points
* Checking for game-ending moves: 10 points

Grading for testing tasks:
* Correctly validating who is the first player: 5 points
* Permitting valid moves: 8 points
* Checking for invalid moves: 13 points
* Handling game-ending moves: 10 points

### Task 3: Implement the ConnectFourGameArea (10 points total)
The `ConnectFourGameArea` receives `InteractableCommand`s from players who enter the area on their client. The main responsibility of this class is to interpet those commands, dispatching them as appropriate to the `ConnectFourGame` instance that it manages. Your final task is to implement the `handleCommand` method of `ConnectFourGameArea`.

There are four types of commands that the `ConnectFourGameArea` will receive, which map directly to the three methods of `ConnectFourGame` that you implemented in the previous task. 

Avery has provided a complete test suite for `handleCommand` - you do not need to write any additional tests.

> **Clarification**
> 
> Invalid commands regarding when a "game is not in progress" refers to the game instance itself (Does the game exist?).
> It does _**not**_ refer to the game status (state) being "IN_PROGRESS".
> 
> Hint: Read through the `GameArea` class to understand the difference between a game instance and a game state (`ConnectFourGame`).

<details><summary markdown="span">View the specification for this methods</summary>
{% highlight typescript %}
  /**
   * Handle a command from a player in this game area.
   * Supported commands:
   * - JoinGame (joins the game `this._game`, or creates a new one if none is in progress)
   * - StartGame (indicates that the player is ready to start the game)
   * - GameMove (applies a move to the game)
   * - LeaveGame (leaves the game)
   *
   * If the command ended the game, records the outcome in this._history
   * If the command is successful (does not throw an error), calls this._emitAreaChanged (necessary
   * to notify any listeners of a state update, including any change to history)
   * If the command is unsuccessful (throws an error), the error is propagated to the caller
   *
   * @see InteractableCommand
   *
   * @param command command to handle
   * @param player player making the request
   * @returns response to the command, @see InteractableCommandResponse
   * @throws InvalidParametersError if the command is not supported or is invalid.
   * Invalid commands:
   * - GameMove, StartGame and LeaveGame: if the game is not in progress (GAME_NOT_IN_PROGRESS_MESSAGE) or if the game ID does not match the game in progress (GAME_ID_MISSMATCH_MESSAGE)
   * - Any command besides JoinGame, GameMove, StartGame and LeaveGame: INVALID_COMMAND_MESSAGE
   */
  public handleCommand<CommandType extends InteractableCommand>(
    command: CommandType,
    player: Player,
  ): InteractableCommandReturnType<CommandType>
{% endhighlight %}
</details>

Grading for implementation tasks:
* Handling JoinGame: 2 points
* Handling GameMove: 2 points
* Handling LeaveGame: 2 points
* Handling StartGame: 2 points
* Handling invalid commands: 2 point
