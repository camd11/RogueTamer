Rogue Tamer

Design Document
1. High-Level Overview

Rogue Tamer is a monster-collecting roguelike that blends turn-based creature battles with procedural dungeon generation, permadeath, and run-based progression. Each “run” challenges the player to traverse randomized floors, capture new creatures along the way, and survive increasingly tough encounters until reaching the final floor—or being defeated.
1.1 Core Pillars

    Roguelike Structure
        Random Dungeons: Each attempt (run) features a unique layout of rooms, wild creatures, trainers, items, and boss fights.
        Permadeath: If the player’s entire party faints, the run ends, resetting progress (with some possible meta-progression).
        High Replay Value: Randomized routes, items, and creatures each run encourage varied strategies.

    1v1 Turn-Based Battles
        Classic-Style Combat: Strictly 1v1 battles with turn-based moves, type matchups, status conditions, and item usage.
        Capturing & Training: Players can catch wild creatures, train them, and manage their party.
        Progression: Creatures level up and evolve quickly during each run, encouraging fast-paced progression.

    Accessible Yet Challenging
        Shorter Sessions: Each run might last 30–60 minutes.
        Steep Difficulty Curve: Floors get progressively tougher, forcing strategic choices about party management and resource usage.

2. Game Loop

    Start / Main Menu
        Player begins a run, choosing a starter creature (or multiple) if unlocked via meta-progression.
        May configure difficulty or special run modifiers (e.g., “Hardcore Mode”).

    Exploration Phase
        Player enters a randomized dungeon/floor.
        Navigates rooms or paths, encountering wild creatures, trainers, or item chests.
        May discover healing spots or shops (depending on design).

    Battle Phase (1v1 Only)
        Triggered by encountering wild creatures or trainers.
        Player can fight, use items, or attempt to catch wild creatures.
        Victory yields experience and items; defeat ends the run (permadeath).

    Progression to Next Floor
        Upon clearing the floor (often by defeating a boss or meeting certain conditions), the player proceeds to a deeper floor with tougher challenges.
        Rewards or opportunities to swap creatures or heal may be provided.

    Run End
        Victory: Player completes the final floor/boss.
        Defeat: All creatures faint.
        Post-Run: Displays stats (creatures caught, floors cleared) and possibly unlocks new starters/items for future runs.

3. Systems & Mechanics
3.1 Procedural Generation

    Floor Generation
        A DungeonGenerator script can create random layouts of rooms or corridors.
        Could be node-based, grid-based, or a prefab-based approach to ensure replay variety.
    Enemy & Item Distribution
        Random tables scale with floor depth for increasing difficulty.
        Weighted probabilities for different creatures ensure variety in encounters.

3.2 Creature Data

    Data Storage
        Use Unity ScriptableObjects or JSON/CSV for creature stats—name, type(s), base stats, moves, and evolution data.
    Moves & Types
        Maintain a list of moves (power, accuracy, type, special effects).
        Calculate damage using a formula factoring Attack vs. Defense, type advantage, and more.
    Capturing & Party Management
        Implement capture mechanics (HP ratio, item type, status).
        Limit the party to 6 creatures. If full, choose which one to swap out.

3.3 Battle System (1v1)

    Turn Order
        Player and opponent each choose an action (attack, item, switch, or attempt capture if wild).
        Actions resolve based on Speed and priority.
    Damage Calculation
        Uses a classic formula combining Attack vs. Defense, type effectiveness, critical hits, etc.
    Status Effects
        Track conditions like paralysis, sleep, burn, poison, etc.
        Each status modifies stats or turn availability.
    Fainting & Post-Battle
        A creature with zero HP faints. If all faint, the run ends.
        Winners gain experience; leveling up can lead to evolutions.

3.4 Progression & Permadeath

    Run Reset
        If all party creatures faint, the run is wiped, returning to the main menu.
    Meta-Progression (Optional)
        Earn tokens or points each run (even on failure) to unlock new starters, items, or difficulty modes.
        Encourages replay and experimentation.

4. Technical Breakdown (Unity)
4.1 Folder/Project Structure

Assets/
  Scenes/
    MainMenu
    Overworld
    Battle
    GameOver
  Scripts/
    GameManager.cs       // Overarching run logic
    DungeonGenerator.cs  // Procedural generation
    BattleSystem.cs      // Handles turn-based battles (1v1)
    CreatureData/MoveData // ScriptableObjects or classes for creature/move stats
    UI/                  // HUD, menus, inventory UI
  Prefabs/
    Enemies, items, environment objects
  Art/
    (Stable Diffusion outputs for creatures, dungeons, etc.)
  Audio/
    Music, SFX
ProjectSettings/

4.2 Singletons & Managers

    GameManager (DontDestroyOnLoad)
        Manages global references: current party, floor level, meta-progress, and unlocking logic.
        Coordinates scene transitions.
    AudioManager (optional)
        Handles background music transitions and sound effects.

4.3 Scenes

    MainMenu Scene
        Title screen with start options, difficulty selection, and any meta-progression unlocks.
    Dungeon/Overworld Scene
        Procedurally generated map.
        Player explores, picks up items, triggers battles.
    Battle Scene (1v1)
        Turn-based battle UI.
        Return to Overworld on victory or display game over if party is wiped.
    GameOver/RunSummary Scene
        Displays stats, floors cleared, and unlocked items/creatures for future runs.

5. User Interface (UI) & Experience

    Overworld HUD
        Displays party status (HP, conditions).
        Floor/Depth indicator.
        Items/currency (if applicable).
    Battle Screen (1v1)
        Options: Fight / Creatures / Bag / Run (wild battles only).
        Battle log for moves and status changes.
        HP bars for the player’s active creature and the enemy.
    Party/Inventory Menus
        Check each creature’s stats, moves, reorder, or switch active creature.
        Bag for potions, capture items, status cures, etc.
        Key/special items for roguelike-specific systems.

6. Balancing Considerations

    Difficulty Ramp
        Enemy levels and stats scale per floor.
        Stronger or rarer creatures appear deeper in the run.
    Resource Scarcity
        Limit healing items and/or capture items for strategic challenge.
        Include rare healing spots or controlled item drops.
    Catch Mechanics
        Reward players who capture new creatures with strong synergy or coverage.
        Control capture rates or the number of capture items to maintain tension.

7. Meta-Progression & Replay Value

    Unlockable Starters
        Each successful run (or certain floors reached) unlocks new creature starters.
    Collectibles / Achievements
        Track total creatures caught, highest floor reached, time spent, etc.
    Alternate Modes
        Hardcore Mode: Fewer items, stronger enemies.
        Draft Mode: Choose from a limited pool of creatures each floor.

8. Implementation Roadmap

    Prototype Phase
        Simple overworld movement in a test scene.
        Basic procedural generation (small grid or node-based).
        Core 1v1 battle system with a few placeholder creatures and moves.
    Core Systems Build
        Expand creature data (stats, moves, evolution).
        Capture mechanics, XP leveling.
        Basic UI for battles and overworld (HP bars, item usage).
    Roguelike Integration
        Multiple floors with incremental difficulty.
        Permadeath and run reset logic.
        Meta-progression (unlock new starters and items).
    Balancing & Polish
        Fine-tune encounter rates, item drops, leveling speed.
        Add UI polish, audio, and animations.
        Conduct playtests for difficulty and pacing.
    Finalization & Ongoing Updates
        Release alpha/beta for feedback.
        Iterate on bug fixes, gameplay adjustments, and new features.
        Potential expansions (more creatures, new floor biomes, additional bosses).

9. Conclusion

Rogue Tamer blends procedural generation, 1v1 creature battles, and run-based progression for a fresh, replayable experience. By focusing on a small set of creatures, refining the battle mechanics, and implementing robust procedural generation, you can deliver a tightly scoped, addictive roguelike adventure. Your Stable Diffusion-generated art will give the world and creatures a distinct style, tying it all together visually.

Key Takeaways:

    Keep Scope Manageable: Start with a handful of creatures and expand over time.
    Focus on Core Systems First: Polish the 1v1 combat, dungeon generation, and meta-progression.
    Meaningful Unlocks: Motivate players to keep trying new runs.
    Consistent Art Pipeline: Use the same Stable Diffusion process and post-editing for cohesive visuals.

Good luck building Rogue Tamer—and have fun creating unique creatures and dungeons!