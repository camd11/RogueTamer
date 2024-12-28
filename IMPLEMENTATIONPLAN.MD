Implementation Plan (Text-Only to Start)
Phase 1: Project Setup & Basic Text-Rendering

    Initialize Repository & React Project
        Create a new repository (GitHub or GitLab, etc.).
        Use Vite or Create React App to scaffold a React project.
        Add a .gitignore for Node projects (exclude node_modules, build artifacts, etc.).

    Folder Structure
        Maintain a similar structure but focus on text-based components in src/components/.
        Example:

    src/
      data/
        creatures.json
        moves.json
      game/
        battleSystem.js
        dungeonGeneration.js
        ...
      components/
        MainMenu.jsx
        Dungeon.jsx
        Battle.jsx
        GameOver.jsx
      ...

Basic React Setup

    Render simple textual content in your App.js or index.js to ensure everything works:

        function App() {
          return <div>Rogue Tamer (Text-Only Prototype)</div>;
        }
        export default App;

        Confirm the local dev server runs.

Outcome

    A basic React project that displays a static text placeholder.
    Repository and folder structure ready for more sophisticated logic.

Phase 2: Data Modeling & State Management

    Define Creature, Move, and Item Data
        Create JSON or JS data files (e.g., creatures.json, moves.json).
        Keep them minimal but sufficient for testing:

        // creatures.json (example)
        [
          { "id": 1, "name": "Flameling", "hp": 20, "attack": 5, "defense": 2, "type": "Fire" },
          { "id": 2, "name": "Leafbug", "hp": 18, "attack": 4, "defense": 3, "type": "Grass" }
        ]

    Set Up a Store / Global State
        Use Redux Toolkit or React Context to handle:
            Current party (array of creatures)
            Current floor and dungeon layout
            Items / inventory
            Meta-progression (if any, e.g. unlockedStarters)
        Provide this store at the root of your React app.

    Text-Only Rendering of Data
        In a simple <MainMenu />, display text like “Choose a starter: [1] Flameling, [2] Leafbug” to confirm data is loading.
        This sets up the environment for eventual AI or unit testing, since all output is textual.

Outcome

    A functional data layer containing creatures, moves, and items.
    A simple global state solution to store/modify game data.
    Basic text-based UI to confirm data loading.

Phase 3: Dungeon (Text-Based) & Navigation

    Routing or Screen Management
        Decide on React Router or a simple screen variable in state (e.g., currentScreen = 'MAIN_MENU' | 'DUNGEON' | 'BATTLE' | 'GAME_OVER').
        For easier text-based transitions, a single-page state machine approach might be simplest. For example:

    switch (currentScreen) {
      case 'MAIN_MENU':
        return <MainMenu />;
      case 'DUNGEON':
        return <Dungeon />;
      ...
    }

Dungeon Generation (Text Version)

    In dungeonGeneration.js, create a function generateDungeon(floorNumber) that returns a simple representation:

{
  rooms: [
    { id: 'Room1', enemies: [1, 2], items: [101], description: 'A dark room with 2 creatures' },
    { id: 'Room2', ... }
  ],
  boss: { id: 999, name: 'BossMonster', ... }
  // etc.
}

In <Dungeon />, display text for the current room, e.g.:

        You are in Room1. You see 2 wild Flameling(s).
        What would you like to do?
        [1] Fight
        [2] Move to the next room

    Text-Only Interactions
        Present the user with typed or button-based choices.
        For AI testing, ensure your code returns well-structured text or a JSON-based representation of available actions.

Outcome

    A text-based “Dungeon” screen that references a generated layout.
    Clear text prompts for movement, encounter triggers, and item pickups.

Phase 4: Battle System (Text-Only)

    Battle Logic
        In battleSystem.js, implement pure functions for damage, turn order, etc.
        Example:

    export function calculateDamage(attacker, defender, move) {
      // use attacker.attack, defender.defense, move.power, etc.
      return finalDamage;
    }
    export function processTurn(state, action) {
      // 'ATTACK', 'USE_ITEM', or 'ATTEMPT_CAPTURE'
      // Return updated battle state (HP changes, status changes, logs)
      return { ...newState };
    }

    Keep these “headless” so they can be tested in isolation without UI.

Text-Based Battle Flow

    In <Battle />, display something like:

        Enemy: Flameling (HP: 15/20)
        Your Creature: Leafbug (HP: 10/18)

        Choose an action:
          [1] Attack with Razor Leaf
          [2] Use Potion
          [3] Attempt Capture

        When the player (or AI test) selects an action, run processTurn() and re-render the updated text state.

    Capture Mechanic (Textual)
        Prompt: “You threw a capture device. Rolling for success... [Success/Failure]”
        If success, add creature to the party. If party is full, ask which to release or skip.

    Win/Lose State
        If the enemy’s HP reaches 0, display “You won the battle!”
        If all party creatures faint, trigger a “Game Over.”

Outcome

    A fully operational text-based battle system.
    Pure JavaScript logic for turn resolution, easily unit-tested.
    Clear textual feedback on every step, facilitating AI-driven or automated testing.

Phase 5: Automated Testing & AI Validation

    Unit Tests
        Use Jest (or similar) to test battleSystem.js, dungeonGeneration.js, etc.
        Example test snippet:

    test('calculateDamage should apply type advantage correctly', () => {
      const attacker = { type: 'Fire', attack: 5 };
      const defender = { type: 'Grass', defense: 2 };
      const move = { power: 4 };
      const damage = calculateDamage(attacker, defender, move);
      expect(damage).toBeGreaterThan(10); // Example assertion
    });

Integration Tests (React Testing Library)

    Render <Dungeon /> or <Battle /> in a test environment.
    Simulate user actions or AI actions and check textual output:

        test('Battle flow: user attacks and wins', () => {
          render(<Battle initialEnemy={...} />);
          // Check text for "Choose an action"
          // Click or call the function for "Attack"
          // Expect text updated to "You dealt X damage"
        });

    AI-Driven Testing
        If your AI coder can parse textual outputs, it can sequentially feed commands and verify the updated text states.
        Ensure your text is consistent, e.g., “Enemy HP: X/Y” or “Party: [Leafbug(HP:10), Flameling(HP:0)]”.

Outcome

    A robust suite of tests that confirm your game logic holds up under various scenarios.
    A text interface consistent enough for automated or AI-driven validation.

Phase 6: Permadeath & Meta-Progression (Still Text-Based)

    Roguelike Loop
        If the party faints, transition to a text-based “Game Over” screen.
        Reset or clear the current run data in your state management.

    Meta-Progression Storage
        Use localStorage or a simple object for unlocked starters, achievements, etc.
        For example:

        const profile = JSON.parse(localStorage.getItem('rogueTamerProfile')) || {};
        profile.unlockedStarters = profile.unlockedStarters || [];
        // Update after the run
        localStorage.setItem('rogueTamerProfile', JSON.stringify(profile));

    Text Display of Meta-Progression
        In <MainMenu />, show “Unlocked Starters: Flameling, Leafbug, etc.”
        If a new starter is unlocked, display “New Starter Unlocked!” for the next run.

Outcome

    Full roguelike loop in text form, including death resets and persistent unlocks.
    Consistent textual displays for achievements/unlocks, still AI-friendly for automated checks.

Phase 7: Refinements & Polish (Still Text-Only)

    Balancing
        Iterate on move power, creature stats, item drop rates for a balanced experience.
        Adjust text prompts to be more user-friendly.

    Error Handling & Edge Cases
        E.g., what if you try to capture with no capture items?
        Make sure your text output handles unexpected states gracefully.

    Testing Coverage
        Expand test scenarios (e.g., capturing multiple creatures, running out of potions, using status effects).
        Aim for high coverage on your game logic files.

Outcome

    A stable, thoroughly tested text-based roguelike with near-complete coverage of major features.
    Clear user (or AI) text prompts that confirm all edge cases.

Phase 8: Optional Visual & Audio Enhancements

(Only after the text-based logic is well-tested and stable.)

    Introducing Graphics
        Replace or augment text displays with images or sprites.
        Possibly layer on a canvas or Phaser/Pixi approach, but keep the underlying logic intact.
        Maintain text-based data for continued automated testing, or maintain a “headless” mode for tests.

    Adding Sound
        Integrate minimal BGM or SFX triggers on events.
        For test environments, you can disable audio or keep it behind a flag (e.g., SOUND_ENABLED=false).

    Coexisting with Text Mode
        If you keep “Text Mode” as a toggle, you can preserve your ability to do automated testing.
        For example, process.env.TEST_MODE === 'textOnly' could swap out the UI layer with text-based outputs.

Outcome

    A transition from purely text-based to a modern, visual/audio experience—without breaking your proven, testable core logic.
    The best of both worlds: robust testing + enhanced player experience.