# Number Warmer — Specifications

## Overview
Number Warmer is a two-player number guessing game (1-100). Players alternate
guessing a mystery number. After each guess, the game responds with WARMER
(closer to the answer than the previous guess) or COLDER (farther away).

The game is server-based, played in a browser, designed mobile-first for
iPhone (390x640 CSS px) with responsive scaling to tablets and desktops.

Expected domain: **numberWarmer.plutonic9.com**

---

## Game Modes
There is ONE game mode: two players, alternating turns. The second player
is either a human on another device or an AI (ChatGPT or Claude API).
This keeps the game loop identical regardless of opponent type.

---

## Screens (4 total)

### 1. LOBBY (lobby.html)
The entry point. Simplified and focused.

**Layout (top to bottom):**
- Title: "Number Warmer" with decorative question marks (yellow left, blue right)
- One-liner: "Guess the number between 1 and 100!"
- Name input: "My name:" with text field (8 characters max, saved/loaded via localStorage)
- Three game-mode buttons with clear action labels:
  - "Start a new game" (invite a friend) → goes to HUMANHOST-SETUP
  - "Join a friend's game" (I have a game code) → reveals gameID input field
  - "Play vs AI" (solo game) → goes to AI-SETUP
- GameID input: only visible when "Join a friend's game" is selected
- START GAME button

**Design decisions:**
- Removed the long welcome/instruction paragraph from original mockup. The button
  labels are self-explanatory and reduce reading time.
- Moved "tricky number" toggle OUT of the lobby. It's a game option, not a setup
  step. Relocated to the HUMANHOST-SETUP screen (host decides game rules).
- Button labels describe WHAT HAPPENS, not the player's ROLE. New players don't
  think "I am the host" — they think "I want to start a new game."
- Added a brief instruction line for human games: "To play a friend: one of you
  starts a new game and shares the code. The other joins with that code." This
  one sentence explains the entire two-device flow.

**Button shapes (from original mockup):**
- "Start a new game" — green circle
- "Join a friend's game" — green circle
- "Play vs AI" — red diamond shape
- Selected button gets yellow highlight treatment

### 2. HUMANHOST-SETUP (humanhost-setup.html)
Where the host generates a game code and prepares to start.

**Layout:**
- Title header (same as lobby)
- "I will be the HOST, and my gameID will be"
- Generate button (circular, with refresh icon) → produces random gameID (111-999)
- GameID display (large, green, NOT an input field — read only)
- Instruction text: "Give this number to the other player so they can join the
  game. You need to START the game first, before they join!"
- Tricky number toggle (YES / NO, defaults to NO) — moved here from lobby
- Back to LOBBY button (red outline)
- START GAME button (green hatched)

### 3. AI-SETUP (ai-setup.html)
Where the player picks their AI opponent.

**Layout:**
- Title header (same as lobby)
- "Choose your adversary!!"
- Two AI options:
  - "chatty" — blue diamond shape (ChatGPT)
  - "claude" — green circle (Claude API)
- Selected option gets yellow highlight
- Back to LOBBY button (red outline)
- START GAME button (green hatched)

### 4. GAMEPLAY (gameplay.html)
The main game screen. Heart of the app.

**Layout (top to bottom):**
- Title header (compact): "Number Warmer" + "Guess the number between 1 and 100!"
- Info bar: gameID display, tricky number indicator
- Current player display: "It is [name]'s turn!"
- Guess input area:
  - "Your GUESS?" label
  - Number display (large)
  - Up/down arrows for tens digit (left side, range 0-9)
  - Up/down arrows for ones digit (right side, range 0-9)
  - Player can also type directly
  - "Guess" button (with arrow indicator)
- **Center Display Box** (the multi-mode message area — see below)
- Bottom buttons: NEW GAME | Back to LOBBY

---

## The Center Display Box (multi-mode message area)

The largest div on the GAMEPLAY screen. Normally shows the guess history table,
but can be blanked and switched to display other content.

**Modes:**
1. **guess-history** (default) — scrollable table of all guesses
   - Columns: Player, Turn, Guess, Result (WARMER/COLDER/First Guess)
   - Shows at least 6 entries, scrollable for more
   - WARMER rows highlighted warm color, COLDER rows highlighted cool color
2. **first-turn** — coin flip / random selection of who guesses first
   - Shown at the start of every game (including NEW GAME)
   - Solves the "who goes first" question without adding a 5th screen
   - After selection, transitions to guess-history mode
3. **ai-thinking** — summary of AI reasoning between turns (AI games only)
   - Fun banter for single-player experience
   - Could show abbreviated thought process
4. **celebration** — victory screen when mystery number is guessed
   - Big congratulations message
   - Winner name and number of turns
5. **help/info** — rules or help text if needed

**Implementation:** Simple className swap on the div, each mode has its own CSS
styling. Contents replaced via innerHTML. Modes built incrementally.

---

## Visual Design Notes

- **Font:** Bold, playful style (mockups show heavy weight, rounded feel)
- **Color palette:**
  - Yellow question marks (left), blue question marks (right) as decorations
  - Green: positive actions (START GAME, gameID display, "claude" button)
  - Red/pink: back/cancel actions (Back to LOBBY)
  - Blue: "chatty" AI option
  - Yellow highlight: selected state for toggle buttons
  - WARMER results: warm color (yellow/orange)
  - COLDER results: cool color (blue)
- **Layout basis:** 390x640 CSS px logical design stage (see canonical_browser_ui_layout_spec_DETAIL.md)
- **Responsive:** uniform scaling, no document scrolling, mobile-first
- **Minimum tap targets:** 48x48 CSS px
- **Primary text:** >= 16px, secondary >= 14px

---

## Game Rules

- Mystery number is between 1 and 100 (inclusive)
- Two players alternate guessing
- First guess of the game always shows "First Guess" (no WARMER/COLDER since
  there's no previous guess to compare against)
- Subsequent guesses compared to the PREVIOUS guess (not the player's own
  previous guess — the last guess by either player)
- If new guess is closer to mystery number than previous guess → WARMER
- If new guess is farther from mystery number than previous guess → COLDER
- If new guess equals the mystery number → WIN, celebration mode
- "Tricky number" option: (to be defined — gameplay variant)
- Client validates guess (1-100) before sending to server
- Server also validates as the authority (no cheating)
