# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Snake Game** — a single-file HTML implementation with embedded CSS and JavaScript. It's a practice project with no build process, testing framework, or external dependencies.

**File:** `snake.html`

## Running the Game

To play the game, open `snake.html` directly in a web browser:
- Double-click the file, or
- Right-click → Open with → Browser, or
- Drag into a browser window

The game starts in an idle state. Press **Enter** to begin gameplay.

## Game Architecture

### Single-File Structure
All code (HTML, CSS, JavaScript) is contained in `snake.html`:
- **HTML** (lines 1–137): Canvas element, info panel (Score, High Score, Level), game over overlay, exit button
- **CSS** (lines 7–119): Dark theme styling, game board, UI components, button styling
- **JavaScript** (lines 139–390): Game logic, state management, event handling

### Key Game Systems

**Canvas & Rendering:**
- 600×600px canvas with 20×20 grid (30px cells)
- Grid drawn every frame; snake and food rendered as colored squares
- Colors: snake=#4ade80 (green), food=#ff6b6b (red), background=#0a0a0a

**Game State:**
- `snake`: array of head and body segments {x, y}
- `food`: current food position {x, y}
- `score`, `level`, `currentSpeed`: tracked throughout gameplay
- `gameRunning`, `gameOver`: state flags

**Game Loop:**
- `update()` runs at interval `currentSpeed` (150ms initial, speeds up with levels)
- Direction changes buffered via `nextDirection` to prevent input queue issues
- Food collision triggers score increase, level check, and interval restart if speed changes
- Wall and self-collision end the game

**Level & Speed System:**
- Level = `Math.floor(score / 50) + 1` (one level per 50 points / 5 food items)
- Speed = `Math.max(50, 150 - (level - 1) * 15)` ms per update
- On level up: game loop interval is cleared and restarted with new speed
- Minimum speed: 50ms (maximum difficulty)

### Key Functions
- `init()`: Reset snake, score, level, speed; prepare board
- `startGame()`: Transition from idle to active; start game loop
- `update()`: Main game loop; handle movement, collision, food, scoring, level-up
- `draw()`: Render grid, snake, food
- `endGame()`: Stop game, update high score, show game over screen

## Controls

| Action | Keys |
|--------|------|
| Start/Restart | Enter |
| Move Up | ↑ or W |
| Move Down | ↓ or S |
| Move Left | ← or A |
| Move Right | → or D |
| Exit to Start | Click "Exit Game" button |

## Common Tasks

**Test a game mechanic:** Open `snake.html` in browser → play through and verify behavior. No automated tests exist; manual testing is the standard approach.

**Add a feature:** Modify the relevant section:
- New UI element? Update HTML and CSS.
- New game mechanic? Update `update()` function and state variables.
- New visual style? Update the COLORS object and CSS.

Example: To add difficulty modes, introduce a `difficulty` state variable and scale `currentSpeed` calculations accordingly.

**Modify appearance:** Adjust the COLORS object (lines 155–161) or CSS (lines 7–119) for theme changes.

## Notes for Future Work

- The game is self-contained and requires no build or deployment step.
- High score is stored in-memory only (resets on page reload). To persist, implement localStorage.
- Arrow keys are prevented from scrolling the page. This is intentional.
- The game loop is stopped explicitly on game over and restarted on each play — be careful when modifying `clearInterval()` and `setInterval()` calls to avoid orphaned timers.
