# Number Warmer

- a simple two player game where players alternate trying to guess a 'mysteryNumber' between 1-100
- there are three actors in the game:
  1. the COMPUTER, who generates the 'mysteryNumber' and enforces rules
  2. the CLIENTS, one or two devices and the HTML/CSS/JS softare that runs on them
  3. the HUMAN-HOST, who is the 1st player
  4. and a 2ND-PLAYER, who is either a human or an AI API call.
## The Role of the COMPUTER
- this is a online, server based game, played in a browser. It is generally designed to be played on mobile (iPhone), tablets (iPad) and PC desktop browsers.
- the design is responsive, and the UI are layed out to look good on a modern iPhone resoltion
- the default domain expected for this game is: **numberWarmer.plutonic9.com**
- the server generates the 'mysteryNumber' that is the target to be guessed
- the server takes in a json state, updates it, and sends it to each client
  - sends to one client in an AI game
  - sends to both clients in a two player HUMAN game
- the server evaluates the guesses, generates the WARMER, COLDER responses, and the winning condition.
## The Role of the CLIENTS
- either mobile, tablet or desktop browsers
- run HTML/CSS/JS that connects to the COMPUTER via **numberWarmer.plutonic9.com**
- sends messages to the COMPUTER (number guessed, boxes checked, buttons clicked, names typed, etc)
- receives JSON from the COMPUTER to update the game state, then renders that new game state
- can quit the game at any point - the computer should recognize this an provide an orderly retreat
## LOBBY landing page and SETUP
- the HUMAN-HOST starts in a LOBBY, where they
  - enter their name (or it is retrived from cookie storage)
  - they select the 'tricky number' option if desired
  - they select which type of game they are playing:
    1. they are the host playing another human (on a different device) (see HUMANHOST-SETUP below)
    2. they are the 2nd player playing another human (they need a gameID) (see HUMAN-2ND-PLAYER below)
       - to continue as the 2nd HUMAN player they need a gameID number, which must be obtained from the HUMAN-HOST via text, phone call, etc.  There is no 'find game' funtionality.
    3. they want to play an AI opponent (see AI-SETUP below)
  - they then click 'START GAME' which takes the player to the next screen
## HUMANHOST-SETUP landing page
- if the player selected 'play another HUMAN as the host' then they are sent to a new screen called 'HUMANHOST-SETUP'.
- the player must click a button to generate a random new gameID (1-100).
- the result is shown on the screen. That is the number that must be given to the 2nd human player. It will allow them to join this game.
- The player then can click:
  - BACK TO LOBBY - which takes them back to the original LOBBY screen
  - START GAME - which leads to the GAMEPLAY screen
## HUMAN-2ND-PLAYER joining the game
- in the LOBBY, if the player selects 'play another HUMAN, I am the 2nd player!' they will have the option to enter the gameID code.
- when that option is selected, and the gameID code is entered, the player can press the START GAME button.
- if the gameID code is valid they are taken to the 'GAMEPLAY' screen
- if the gameID code is not valid, they are given a message and can try again.
- they are already in the LOBBY, so if they want they can just close the game on their browser.
## AI-SETUP landing page
- if the player selects an AI opponent, they are taken to new page
- options are expected to be 'chatty' (chatGPT) or 'claude' (claudeAPI)
- player can either go back to the lobby if they change their mind
- player can click START GAME, which takes them to the GAMEPLAY screen.
## GAMEPLAY landing page
- this is the heart of the game
### This loop is basically the same for either VS HUMAN, or VS AI games.  The only real difference is in handling the API calls, sending state to the AI and receiving it back.  This is beyond the scope of this document for now.
- one thing need to figure out - how to select starting player?  (ie. who gets to make the first guess?)
  - is should be explicit - maybe that center div that holds the list of guesses could also serve as a generic messaging area?  it could allow the players to either select who goes first, or have a button to randomly select who goes first?
  - or should that functionality be in the LOBBY?  Not really, because if the player's play a 2nd game (ie. NEW GAME) then they would not leave the lobby, and there wouldn't be a way to select the starting player?
  - I do not want to introduce another screen for that.  What do do?
- Game loop is pretty simple:
  1. COMPUTER redisplays current status
  2. player enters new guess (ie. '47') and either hits ENTER, or clicks 'Guess' button
  3. guess is sent to COMPUTER
  4. COMPUTER evaluates new guess...
     - is it valid (1-100)?  or maybe the CLIENT should evaluate that before sending it?
     - compares the new guess to the 'mysteryNumber' and the last guess.
     - is the number equal to the 'mysteryNumber'?  if yes, celebrate.  wait for NEW GAME?
     - if new guess is nearer to the 'mysteryNumber' than the last guess, then add to list as 'Player Name, Turn#, new guess, and WARMER.
     - if new guess is farther away from the 'mysteryNumber' than the last guess, then add to the list as 'Player Name, Turn#, new guess, and COLDER.
  5. CLIENT packages the current state up and sends it back to the COMPUTER.
  6. COMPUTER evaluates and updates state, changes current player, and sends the new state to CLIENT(s).
  7. if either player clicks Back To LOBBY button then that is sent to the COMPUTER, who processes it, resets the state to a brand new game, and sends that new state to CLIENT(s).
