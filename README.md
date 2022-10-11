# `learn-vue`

A simple Tic Tac Toe game with AI and time travel,
made with JavaScript, Vue.js, HTML, and SCSS.

## Quick start

Make sure you have a recent version of Node and `npm` installed:

```bash
node --version
npm --version
```

Install Node dependencies:

```bash
npm install
```

Then, run a `vite` server with `npx`:

```bash
npx vite
```

The website "Tic Tac Toe" should then be available at `http://http://localhost:3000/`,
hosted by the `vite` server.

## Simple explanation

The main code of the application is at `src/App.vue`, which houses the website and game logic;
the Tic Tac Toe board is rendered and interacted with `src/components/Board.vue`.

### Vue and Single-file Components (`SFC`s)

Vue is a JavaScript framework for building webpages.
It separates website code into HTML, JavaScript, and CSS parts, and groups them into a single file,
to provide comfortable coding experience for developers.
The files that Vue uses ends with the extension `.vue`.
The main game display and logic are in `src/App.vue`,
while the board is displayed with `src/components/Board.vue`.

Vue uses single-file components to group the data, logic, and style of a component together,
while separating components into different files to allow code to be easily viewed and modified.
For the style of `App.vue` and `Board.vue`, I had used SCSS, which is an extension of CSS,
to write cleaner and more dynamic CSS style code.

Due to the mild complexity of the Tic Tac Toe board compared to the rest of the app,
I had refactored it into its own SFC, `src/components/Board.vue`.
This allows the board to act like an HTML component in `src/App.vue`,
separating the board display from the game logic for easier coding and debugging.
It also allows the CSS styles of the two files to be independent of one another,
reducing the amount of `class` properties used in the HTML components,
and facilitating SCSS's ability to do complex CSS styles.

### AI

A rudimentary yet efficient AI is incorporated in the game.
For every turn the AI has to make,
It checks all 9 squares on the board and finds open squares;
for these, it checks if it can win by placing on the square,
or the player may win should the player places a piece on the square.

Through this check, it can rank the different possible moves,
and favour winning over blocking the player's winning move,
and both over not doing anything at all.
The AI then picks a random move from the set of moves it favours most;
for example, it would randomly pick 1 move out of 2 blocking moves,
should it not be able to win for the turn.

This AI will not be able to fully solve Tic Tac Toe.
Clever players will notice that this AI is designed not to solve paradoxes on their own,
and can win quickly by forcing the AI to block in two places at once,
which it will certainly miss one.
However, this AI's goal is to make the game challenging and fun for players anyway,
and I would consider these flaws to be what make this AI complete.

### Time travel

An empty board is saved at the start of the game.
For every move the player (not AI) has made, the update board will also be saved.
All these saved moves form a history.
Through a panel of buttons, the user can jump from one point of this history to the other.

This allows the player to play around with the AI,
and to test various move sequences easily.
It may help novices to rapidly explore and train their skills for Tic Tac Toe.

## Credits

The file that are completely written by me are:
- `src/App.vue`
- `src/components/Board.vue`
- `README.md`, which is this file

This app project is initiated with Vue's `create` feature,
which uses a template to automatically initialize a project.
I had based my application on this template, and modified it for a bit.
Nonetheless, credits of files such as `vite.config.js` goes to the Vue and `vite` developers.
