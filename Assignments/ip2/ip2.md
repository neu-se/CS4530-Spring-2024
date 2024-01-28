---
layout: assignment
title: "Individual Project 2"
permalink: /assignments/ip2
parent: Assignments
nav_order: 2
due_date: "Wednesday February 21, 11:00am EDT"
submission_notes: Submit via Autograder.io at neu.autograder.io
---

The objective for this semester's individual project is to extend this new `GameArea` abstraction, implementing [Connect Four](https://en.wikipedia.org/wiki/Connect_Four) as a new game that can be played within a `GameArea`.
This implementation effort will be split across two deliverables. In this second deliverable, you will implement and the core frontend components. 

## Objectives of this assignment
The objectives of this assignment are to:
* Investigate and understand a large, existing codebase
* Write new TypeScript code that uses asynchronous operations
* Write test cases that utilize mocks and spies
* Write React components and hooks that make use of state


## Getting started with this assignment
Start by [downloading the starter code]({{site.baseurl}}{% link /Assignments/ip1/ip1-handout.zip %}). Extract the archive and run `npm install` to fetch the dependencies.  **TODO add IP2 handout to GitHub after Thurs 11am**

**Configuring Jest and VSCode**: If you would like to use the built-in Jest test runner for VSCode (where it shows the tests and their status in the sidebar), the easiest way to accomplish this for this project is to open *just* the "frontend" directory or just the "townService" directory in VSCode - not the top-level "ip2-handout" directory. If you have a quick-fix to make it work with the whole project at once, please feel free to share on Piazza and we will incorportate that here.

**NPM install failures**: The libraries used for React require some native binaries to be installed -- code written and compiled for your computer (not JavaScript). If you run into issues with `npm install` not succeeding, please try installing the following libraries using either [Homebrew (if on Mac)](https://brew.sh), apt-get, or your favorite other package manager: `pixman`, `cairo`, `pkgconfig` and `pango`. For example, run `brew install pixman cairo pkgconfig pango`. If you are on a newer Mac with an M1 or M2 chip, you may need to use `arch -arm64 brew install pixman cairo pango`. On Windows: Students have reported seeing the failure `error /bin/bash: node: command not found` upon `npm install` in the `frontend` directory. If you encounter this error, please try to delete the `node_modules` directory and re-run `npm install` in the `frontend` directory from a bash shell instead of a windows command prompt.

**Running the app**: We strongly encourage you to interactively experiment as you develop by running the entire application in your development environment. See the instructions in README.md for how to run the app.

## Grading
This submission will be scored out of 200 points, 180 of which will be automatically awarded by the grading script, with the remaining 20 manually awarded by the course staff.

Your code will automatically be evaluated for linter errors and warnings. Submissions that have *any* linter errors will automatically receive a grade of 0. **Do not wait to run the linter until the last minute**. To check for linter errors, run the command `npm run lint` from the terminal. The handout contains the same eslint configuration that is used by our grading script.

Your code will be automatically evaluated for functional correctness by a test suite that expands on the core tests that are distributed in the handout. 
Your tests will be automatically evaluated for functional correctness by a process that will inject bugs into our reference solution: to receive full marks your tests must detect a minimum number of injected bugs. 
Each submission will be graded against the same set of injected bugs (repeated submissions will not receive new/different injected bugs).
You will __not__ receive detailed feedback on which injected bugs you do or do not find.

The autograding script will impose a strict rate limit of 5 submissions per 24 hours.
Submissions that fail to grade will not count against the quota.
This limit exists to encourage you to start early on this assignment: students generally report that assignments like this take between 3-20 hours.
If you start early, you will be able to take full advantage of the resources that we provide to help you succeed: office hours, discussion on Piazza --- and the ability to have a greater total number of submission attempts.

Your code will be manually evaluated for conformance to our course [style guide]({{ site.baseurl }}{% link style.md %}). This manual evaluation will account for 10% of your total grade on this assignment. We will manually evaluate your code for style on the following rubric:

To receive all 20 points:
* All new names (e.g. for local variables, methods, and properties) follow the naming conventions defined in our style guide
* There are no unused local variables
* All public properties and methods (other than getters, setters, and constructors) are documented with JSDoc-style comments that describes what the property/method does, as defined in our style guide
* The code and tests that you write generally follows the design principles discussed in week one. In particular, your design does not have duplicated code that could have been refactored into a shared method.

We will review your code and note each violation of this rubric. We will deduct four points for each violation, up to a maximum of deducting all 20 style points.

## Implementation Tasks
This deliverable has four parts; each part will be graded on its own rubric. You should complete the assignment one part at a time, in the order presented here.

**General Requirements**: Implement your code *only* in the files specified:
**TODO** - update this list
* Task 1: `frontend/src/classes/interactable/TicTacToeAreaController.ts`
* Task 1: `frontend/src/classes/interactable/TicTacToeAreaController.test.ts`
* Task 2: `frontend/src/components/Town/interactables/TicTacToe/TicTacToeArea.tsx`
* Task 3: `frontend/src/components/Town/interactables/TicTacToe/TicTacToeBoard.tsx`
* Task 4: `frontend/src/components/Town/interactables/Leaderboard.tsx`

You should not install any additional dependencies. The autograder will ignore any other files that you modify, and will not install any dependencies that you add to the project.

### Task 1: Implement the ConnectFourAreaController (60 points)
The `ConnectFourAreaController` is a class that is responsible for managing the state of a single ConnectFour game. It is responsible for communicating with the TownService backend. Frontend components will interact with the `ConnectFourAreaController` to get the current state of the game, and to send commands to the backend to update the game state. The `ConnectFourAreaController` also will emit events when the state of the game changes, so that frontend components can update their state accordingly.

`ConnectFourAreaController` extends the base `GameAreaController` class.
The base class tracks the game model (`this._model`), the set of players in the game (`this.players`), and the set of observers in the game (`this.observers`). It also provides methods for joining and leaving the game, as well as a base implementation of `_updateFrom`, which is responsible for updating the game state when the backend notifies the frontend of a change.

Your first task is to implement each of the properties and methods of the `ConnectFourAreaController` class (`frontend/src/classes/interactable/ConnectFourAreaController.ts`). The specification for these properties and methods appears below:

{::options parse_block_html="true" /}
<details><summary markdown="span">View the specification for these methods</summary>
{% highlight typescript %}
 /**
   * Returns the current state of the board.
   *
   * The board is a 6x7 array of ConnectFourCell, which is either 'Red', 'Yellow', or undefined.
   *
   * The 2-dimensional array is indexed by row and then column, so board[0][0] is the top-left cell,
   */
  get board(): ConnectFourCell[][] 

  /**
   * Returns the player with the 'Red' game piece, if there is one, or undefined otherwise
   */
  get red(): PlayerController | undefined 

  /**
   * Returns the player with the 'Yellow' game piece, if there is one, or undefined otherwise
   */
  get yellow(): PlayerController | undefined 

  /**
   * Returns the player who won the game, if there is one, or undefined otherwise
   */
  get winner(): PlayerController | undefined 

  /**
   * Returns the number of moves that have been made in the game
   */
  get moveCount(): number 

  /**
   * Returns true if it is our turn to make a move, false otherwise
   */
  get isOurTurn(): boolean

  /**
   * Returns true if the current player is in the game, false otherwise
   */
  get isPlayer(): boolean 

  /**
   * Returns the color of the current player's game piece
   * @throws an error with message PLAYER_NOT_IN_GAME_ERROR if the current player is not in the game
   */
  get gamePiece(): ConnectFourColor 

  /**
   * Returns the status of the game
   * If there is no game, returns 'WAITING_FOR_PLAYERS'
   */
  get status(): GameStatus 

  /**
   * Returns the player whose turn it is, if the game is in progress
   * Returns undefined if the game is not in progress
   *
   * Follows the same logic as the backend, respecting the firstPlayer field of the gameState
   */
  get whoseTurn(): PlayerController | undefined 

  /**
   * Returns true if the game is empty - no players AND no occupants in the area
   *
   */
  isEmpty(): boolean 

  /**
   * Returns true if the game is not empty and the game is not waiting for players
   */
  public isActive(): boolean 

  /**
   * Updates the internal state of this ConnectFourAreaController based on the new model.
   *
   * Calls super._updateFrom, which updates the occupants of this game area and other
   * common properties (including this._model)
   *
   * If the board has changed, emits a boardChanged event with the new board.
   * If the board has not changed, does not emit a boardChanged event.
   *
   * If the turn has changed, emits a turnChanged event with the new turn (true if our turn, false otherwise)
   * If the turn has not changed, does not emit a turnChanged event.
   */
  protected _updateFrom(newModel: GameArea<ConnectFourGameState>): void 

  /**
   * Sends a request to the server to start the game.
   *
   * If the game is not in the WAITING_TO_START state, throws an error.
   *
   * @throws an error with message NO_GAME_STARTABLE if there is no game waiting to start
   */
  public async startGame(): Promise<void> 

  /**
   * Sends a request to the server to place the current player's game piece in the given column.
   * Calculates the row to place the game piece in based on the current state of the board.
   * Does not check if the move is valid.
   *
   * @throws an error with message NO_GAME_IN_PROGRESS_ERROR if there is no game in progress
   * @throws an error with message COLUMN_FULL_MESSAGE if the column is full
   *
   * @param col Column to place the game piece in
   */
  public async makeMove(col: ConnectFourColIndex): Promise<void>
{% endhighlight %}
</details>

*Testing*: Avery has provided you with tests for everything in `ConnectFourAreaController`. You can run the tests by running the command `npx jest ConnectFourAreaController` in the `frontend` directory (for convenience, you may want to use `npx jest --watch` ...).

The grading script will assign full marks for each implementation task if all of the tests for that task pass. There is no partial credit.

Grading for implementation tasks:
* All properties and methods besides `_updateFrom`, `startGame` and `makeMove`: 10 points
* `_updateFrom`: 10 points
* `startGame`: 10 points
* `makeMove`: 20 points

### Task 2: GamesArea Component (20 points total)
In last semester's individual project, students implemented the TicTacToeArea component. This component was responsible for rendering information about the area, including the list of players observing the game, a leaderboard, and the game itself. Avery decided that rather than copy-pasting this component, it would be better to refactor it into a more general `GamesArea` component, which could be used to render any game area. This component is located in the file `frontend/src/components/Town/interactables/GamesArea.tsx`.

As you work on this component, you might find it helpful to view the code of the [old TicTacToeArea component](https://github.com/neu-se/covey.town/blob/cf69be63f2d43ec3327dfab04a8a045c642dc741/frontend/src/components/Town/interactables/TicTacToe/TicTacToeArea.tsx), which included the functionality for rendering the leaderboard and list of observers. 

<details><summary markdown="span">View the specification for this component</summary>
{% highlight typescript %}
/**
 * A generic component that renders a game area.
 *
 * It uses Chakra-UI components (does not use other GUI widgets)
 *
 * It uses the GameAreaController corresponding to the provided interactableID to get the current state of the game. (@see useInteractableAreaController)
 *
 * It renders the following:
 *  - A leaderboard of the game results
 *  - A list of observers' usernames (in a list with the aria-label 'list of observers in the game')
 *  - The game area component (either ConnectFourArea or TicTacToeArea). If the game area is NOT a ConnectFourArea or TicTacToeArea, then the message INVALID_GAME_AREA_TYPE_MESSAGE appears within the component
 *  - A chat channel for the game area (@see ChatChannel.tsx), with the property interactableID set to the interactableID of the game area
 *
 */
function GameArea({ interactableID }: { interactableID: InteractableID }): JSX.Element
{% endhighlight %}
</details>

Grading:
* Correctly register the listeners: 10 points
* Displaying the leaderboard component: 2 points
* List of observers watching game: 2 points
* Render the game area component: 2 points
* Render the chat channel: 2 points


### Task 3: Connect Four Area (50 points total)
The next task is to implement the React component that will render the Connect Four game area. This component will show information about the game area, and the current state of the game. It displays the `ConnectFourBoard`` (which you'll implement in the next task).

This component is located in the file `frontend/src/components/Town/interactables/TicTacToe/TicTacToeArea.tsx` - you should implement component class in this file.

<details><summary markdown="span">View the specification for this component</summary>
{% highlight typescript %}
/**
 * The ConnectFourArea component renders the Connect Four game area.
 * It renders the current state of the area, optionally allowing the player to join the game.
 *
 * It uses Chakra-UI components (does not use other GUI widgets)
 *
 * It uses the ConnectFourAreaController to get the current state of the game.
 * It listens for the 'gameUpdated' and 'gameEnd' events on the controller, and re-renders accordingly.
 * It subscribes to these events when the component mounts, and unsubscribes when the component unmounts. It also unsubscribes when the gameAreaController changes.
 *
 * It renders the following:
 * - A list of players' usernames (in a list with the aria-label 'list of players in the game', one item for red and one for yellow)
 *    - If there is no player in the game, the username is '(No player yet!)'
 *    - List the players as (exactly) `Red: ${username}` and `Yellow: ${username}`
 * - A message indicating the current game status:
 *    - If the game is in progress, the message is 'Game in progress, {moveCount} moves in, currently {whoseTurn}'s turn'. If it is currently our player's turn, the message is 'Game in progress, {moveCount} moves in, currently your turn'
 *    - If the game is in status WAITING_FOR_PLAYERS, the message is 'Waiting for players to join'
 *    - If the game is in status WAITING_TO_START, the message is 'Waiting for players to press start'
 *    - If the game is in status OVER, the message is 'Game over'
 * - If the game is in status WAITING_FOR_PLAYERS or OVER, a button to join the game is displayed, with the text 'Join New Game'
 *    - Clicking the button calls the joinGame method on the gameAreaController
 *    - Before calling joinGame method, the button is disabled and has the property isLoading set to true, and is re-enabled when the method call completes
 *    - If the method call fails, a toast is displayed with the error message as the description of the toast (and status 'error')
 *    - Once the player joins the game, the button dissapears
 * - If the game is in status WAITING_TO_START, a button to start the game is displayed, with the text 'Start Game'
 *    - Clicking the button calls the startGame method on the gameAreaController
 *    - Before calling startGame method, the button is disabled and has the property isLoading set to true, and is re-enabled when the method call completes
 *    - If the method call fails, a toast is displayed with the error message as the description of the toast (and status 'error')
 *    - Once the game starts, the button dissapears
 * - The ConnectFourBoard component, which is passed the current gameAreaController as a prop (@see ConnectFourBoard.tsx)
 *
 * - When the game ends, a toast is displayed with the result of the game:
 *    - Tie: description 'Game ended in a tie'
 *    - Our player won: description 'You won!'
 *    - Our player lost: description 'You lost :('
 *
 */
export default function ConnectFourArea({
  interactableID,
}: {
  interactableID: InteractableID;
}): JSX.Element 
{% endhighlight %}
</details>

You can begin to implement these tasks in whatever order you see fit, but we would suggest completing them in the order shown in the specification.

There is significant ambiguity in the specification when it comes to exactly how this looks. We will not evaluate your submission on the basis of how closely it looks like our referencence implementation: it need only be functionally correct (as defined by the included test cases).

Grading for implementation tasks:
* Correctly registering the listeners: 10 points
* Join game button: 15 points
* Start game button: 15 points
* List of players in game: 5 points
* Display the game status: 5 points

All of the tests are provided in the handout. Run the tests for this task by running the command `npx jest ConnectFourArea.test` in the `frontend` directory (for convenience, you may want to use `npx jest --watch` ...). You will also likely find it convenient to run the app in your browser while you work on this task for interactive debugging.

The grading script will assign full marks for each implementation task if all of the tests for that task pass.  There is no partial credit. 

### Task 4: Implement the ConnectFourBoard (20 points total)
This task is to implement the `ConnectFourBoard` component, which will render the actual (interactive) board. It is located in the file `frontend/src/components/Town/interactables/ConnectFour/ConnectFourBoard.tsx`.

<details><summary markdown="span">View the specification for this component</summary>
{% highlight typescript %}
/**
 * A component that renders the ConnectFour board
 *
 * Renders the ConnectFour board as a "StyledConnectFourBoard", which consists of "StyledConnectFourSquare"s
 * (one for each cell in the board, starting from the top left and going left to right, top to bottom).
 *
 * Each StyledConnectFourSquare has an aria-label property that describes the cell's position in the board,
 * formatted as `Cell ${rowIndex},${colIndex} (Red|Yellow|Empty)`.
 *
 * The board is re-rendered whenever the board changes, and each cell is re-rendered whenever the value
 * of that cell changes.
 *
 * If the current player is in the game, then each StyledConnectFourSquare is clickable, and clicking
 * on it will make a move in that column. If there is an error making the move, then a toast will be
 * displayed with the error message as the description of the toast. If it is not the current player's
 * turn, then the StyledConnectFourSquare will be disabled.
 *
 * @param gameAreaController the controller for the ConnectFour game
 */
export default function ConnectFourBoard({
  gameAreaController,
}: ConnectFourGameProps): JSX.Element
{% endhighlight %}
</details>

Again, there is significant ambiguity in the specification when it comes to exactly how this looks. We will not evaluate your submission on the basis of how closely it looks like our referencence implementation: it need only be functionally correct (as defined by the included test cases).

Grading for implementation tasks:
* Drawing the board for observers: 10 points
* Drawing the interactive board for players: 10 points

All of the tests are provided in the handout. Run the tests for this task by running the command `npx jest ConnectFourBoard` in the `frontend` directory (for convenience, you may want to use `npx jest --watch` ...). You will also likely find it convenient to run the app in your browser while you work on this task for interactive debugging.

The grading script will assign full marks for each implementation task if all of the tests for that task pass.  There is no partial credit. 

### Task 5: Extend the SocialSidebar (30 points total)

On the right hand side of the Covey.Town interface, there is a sidebar that displays information about the current town, listing the players in the town and the active ConversationAreas. This sidebar is implemented in the `SocialSidebar` component, located in the file `frontend/src/components/SocialSidebar/SocialSidebar.tsx`. 

Given the growth of the Covey.Town codebase, Avery has decided that instead of just listing the active Conversation Areas, it would be better to list all of the active interactables in the town. This will allow the sidebar to list the active Conversation Areas, as well as any active games, viewing areas, or future interactable areas not yet conceived.

Your specific task is to implement the `InteractableAreaList` component, which will display a list of all active interactable areas in the town. This component is located in the file `frontend/src/components/SocialSidebar/InteractableAreaList.tsx`. This component should make use of two React hooks defined in `InteractableController.ts` - `useInteractableAreaOccupants` and `useInteractableAreaFriendlyName`. These hooks are responsible for fetching the list of occupants of an interactable area, and the friendly name of the interactable area, respectively. You will need to implement the `useInteractableAreaFriendlyName` hook, but will find the other is already completed.

You might find it useful to view the code of the [old ConversationAreasList component](https://github.com/neu-se/covey.town/blob/cf69be63f2d43ec3327dfab04a8a045c642dc741/frontend/src/components/SocialSidebar/ConversationAreasList.tsx).

<details><summary markdown="span">View the specification for this component and these hooks</summary>
{% highlight typescript %}

/**
 * A react component that displays a list of all active interactable areas in the town.
 * The list is grouped by type of interactable area, with those groups sorted alphabetically
 * by the type name. Within each group, the areas are sorted first by the number of occupants
 * in the area, and then by the name of the area (alphanumerically).
 *
 * The list of interactable areas is represented as an ordered list, with each list item
 * containing the name of the area (in an H4 heading), and then a list of the occupants of the area, where
 * each occupant is shown as a PlayerName component.
 *
 * @returns A list of all active interactable areas in the town as per above spec
 */
export default function InteractableAreasList(): JSX.Element

/**
 * A react hook to retrieve the friendly name of an InteractableAreaController, returning a string.
 *
 * This hook will re-render any components that use it when the friendly name changes.
 *
 */
export function useInteractableAreaFriendlyName(area: GenericInteractableAreaController): string
{% endhighlight %}
</details>

Grading for implementation tasks:
* Implement the useInteractableAreaFriendlyName hook: 10 points
* Implement the InteractableAreaList component: 20 points

All of the tests are provided in the handout. Run the tests for this task by running the command `npx jest InteractableAreasList hooks` in the `frontend` directory (for convenience, you may want to use `npx jest --watch` ...). You will also likely find it convenient to run the app in your browser while you work on this task for interactive debugging.

The grading script will assign full marks for each implementation task if all of the tests for that task pass.  There is no partial credit. 

## Submission Instructions
Submit your assignment to the instance of Autograder.io running at [neu.autograder.io](https://neu.autograder.io).
Navigate to [neu.autograder.io](https://neu.autograder.io) in your web browser, click the "Sign in" button, and log in with your Northeastern account.
You should then see the course listed on the neu.autograder.io home page.
Please contact the instructors immediately if you have difficulty accessing the course on Autograder.io.
If you haven't been added to the course roster yet, you can access the assignment page at [this direct link](https://neu.autograder.io/web/project/12).

To submit your assignment: run the command `npm run zip` in the top-level directory of the handout. This will produce a file called `covey-town.zip`. Submit that zip file on Autograder.io.

Autograder.io will provide you with feedback on your submission, but note that it will *not* include any marks that will be assigned after we manually grade your submission for code style (these marks will remain hidden until it is graded). It may take several minutes for the grading script to complete.

Autograder.io is configured to only provide feedback on at most 5 submissions per-24-hours per-student (submissions that fail to run or receive a grade of 0 are not counted in that limit). We strongly encourage you to lint and test your submission on your local development machine, and *not* rely on Autograder.io for providing grading feedback - relying on Autograder.io is a very slow feedback loop.

To check for linter errors, run the command `npm run lint` from the terminal. The handout contains the same eslint configuration that is used by our grading script.
Submission limit resets at 11am EST.