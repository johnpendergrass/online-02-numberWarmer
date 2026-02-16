# Number Warmer — Session Summary
**Date:** 2026-02-15
**Session:** First session — project setup and design discussions

---

## What Was Done This Session

### 1. Project Setup
- Initialized git repo in `D:\Dev\Projects\online-02-numberWarmer`
- Created folder structure: `client/css`, `client/js`, `client/assets`, `server/`
- Added `.gitignore`
- Initial commit with design docs and folder structure
- Connected to GitHub remote (`johnpendergrass/online-02-numberWarmer`)
- Pushed to GitHub (merged with remote README)

### 2. Design Review and Decisions
Reviewed all design mockups (LOBBY, AI-SETUP, HUMANHOST-SETUP, GAMEPLAY) and
the gameplay markdown doc. Made the following design decisions through discussion:

**Game mode simplification:**
- ONE game mode only: two players, alternating turns
- AI fills the second seat for solo play (not a separate "solo mode")
- This keeps the game loop identical regardless of opponent type

**LOBBY screen simplification:**
- Removed the long welcome/instruction paragraph
- Renamed buttons to describe actions, not roles:
  - "Start a new game" (invite a friend)
  - "Join a friend's game" (I have a game code)
  - "Play vs AI" (solo game)
- Moved "tricky number" toggle to HUMANHOST-SETUP (it's a game option, not setup)
- Added brief instruction line about the human game flow
- Result: cleaner, less reading, faster to action

**Center Display Box (GAMEPLAY screen):**
- The large center div is a multi-mode message area, not just a guess table
- Five modes: guess-history (default), first-turn, ai-thinking, celebration, help/info
- Solves the "who goes first" problem without adding a 5th screen
- Modes built incrementally — start with guess-history only
- Updated `numberWarmer_gameplay.md` with this design

**Screen architecture:**
- Four separate HTML files (lobby.html, humanhost-setup.html, ai-setup.html, gameplay.html)
- Each with own CSS and JS files, plus shared.css and shared.js
- Between-screen data via localStorage (browser's built-in key-value store)
- Client-server data via JSON over network
- Same gameState object shape used for both transports

### 3. Documentation Created
- `specifications.MD` — full design spec (screens, layout, visual notes, game rules)
- `specifications-technical.MD` — architecture, file structure, gameState object,
  localStorage code examples, client-server flow, data transport patterns

---

## Current Project State
- **Git:** initialized, connected to GitHub, one commit pushed
- **Code:** NONE yet — no HTML, CSS, or JS files written
- **Design:** complete enough to start building
- **Documentation:** specifications and technical specs written

---

## What To Do Next (Immediate)
1. **Build the LOBBY screen** — first HTML/CSS/JS, get the layout right on 390x640
   - Name input with localStorage save/load
   - Three game-mode buttons with selection highlighting
   - Conditional gameID input for "Join a friend's game"
   - START GAME button that saves state to localStorage and navigates
2. **Build shared.css and shared.js** — common header, button styles, localStorage
   helper functions, viewport scaling logic

## What To Do Next (After LOBBY)
3. Build HUMANHOST-SETUP screen
4. Build AI-SETUP screen
5. Build GAMEPLAY screen (guess-history mode first)
6. Wire up server (technology TBD — likely Node.js)
7. Connect client to server for actual gameplay
8. Add remaining Center Display Box modes (first-turn, celebration, etc.)
9. Add AI opponent API integration

---

## Open Questions
- **Tricky number:** What does this gameplay variant actually do? Not yet defined.
- **Server technology:** Node.js + Express is likely, but not decided. WebSockets
  vs HTTP polling for real-time two-player games needs discussion.
- **AI API details:** How to format prompts, handle responses, manage API keys.
- **Deployment:** QNAP NAS setup, domain configuration for numberWarmer.plutonic9.com.
