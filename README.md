## PROJECT AIM

Create an AI for the board game "Ticket to Ride" by using a Reinforcement Learning (RL) approach. Its blend of strategic route planning, resource management, and competitive blocking makes it an ideal environment for exploring advanced AI learning techniques through self-play.

## WHAT “IS TICKET TO RIDE”?

“Ticket to Ride” (America base game) is a board game where you have a map of major cities in America. Players draw "tickets" which give the task to connect two cities with a train route network. Players earn points based on fulfilled tickets, routes claimed, and for holding the longest continuous train route.

The game uses 110 train car cards (8 different colors and 1 joker/locomotive type). There are also 30 unique destination tickets. Players can draw 3 new tickets  on their turn and must keep at least one. Unfulfilled tickets at the end of the game count as minus points. The game ends when a player has 2 or fewer trains left.

## MY LEARNING JOURNEY

The project idea is simple yet represents a potentially steep learning curve for me. It's "simple" because its core concept isn't entirely groundbreaking, but "difficult" because I have never implemented such a complex system before. This makes it a non-typical starter project in the field, challenging me to learn and apply new concepts throughout its development.

## Latest Update (June 23, 2025)

**Completed:**
* Initial game state data representation, including all cities, routes, and card configurations.
* Basic game initialization: Setting up player dictionaries, distributing initial train car cards and destination tickets, and handling the random choice for returning initial tickets.
* Core train car card drawing logic implemented (face-up and deck draws), including specific rules for handling locomotives during the second draw.
* Initial visualization of the map created using Matplotlib, showing cities and routes.

**Next Focus:**
* Implementing the full route claiming logic, including managing the discard pile for train car cards used in claims.
* Developing end-game logic: Triggering the last round, determining the longest route, and calculating final scores.
* Exploring real-time visualization updates as agents play simulated games.

## CURRENT ROAD MAP

Phase 1: Game Engine Development
Phase 2: Basic AI making random choices
Phase 3: Reinforcement Learning Setup
Phase 4: Training and Iteration
Phase 5: Evaluation and Analysis

## DEVELOPMENT DIARY

### Data Setup

Game data is logically separated into two main dictionaries for clarity and modularity:
1.  `board_static`: Stores unchanging game data, currently including detailed configurations for all routes and cities on the map.
2.  `board_active`: Stores dynamic game state, such as player data, current player ID, turn count, available ticket deck, completed tickets, the main train card deck, and the discard pile.

Detailed components:
* `cities`: A list of dictionaries for each city, including its unique ID, full name, and coordinates (for visualization).
* `routes`: A dictionary of dictionaries, where each entry represents a route with its ID, connected city IDs, length, color, and current owner (initialized to None).
* `tickets`: A list of dictionaries detailing all 30 unique destination tickets, including their two connected cities and point value.
* `train_car_cards`: A dictionary mapping each train car card color (including locomotive) to its initial count for deck setup.

### Game Engine

* Implemented the `set_up_game` function, which takes the player count as an argument. This function handles initializing and shuffling the train car card deck, setting up player dictionaries, distributing initial train car cards and destination tickets, and managing the random choice for players returning initial tickets using `draw_train_card`, `draw_ticket_card`, and `return_ticket_card` helper functions.
* Developed the `pick_card_and_update_decks` function: This function handles the drawing of a single train car card, managing deck reshuffling, and implementing specific rules like discarding all face-up cards if more than two locomotives are present, and ensuring two cards are drawn per turn according to standard rules.
* Implemented the main player card drawing function (`draw_train_cards`) which utilizes the `pick_card_and_update_decks` helper function.

### Visualization Attempt

* Used Matplotlib to plot each city on a 22x13 grid, demonstrating the geographical layout.
* Added lines to represent all routes, with a special offset mechanism to prevent double routes from overlapping.
* Implemented logic to display claimed routes with brighter/bolder lines, dynamically showing board ownership.
    **[INSERT SCREENSHOT/GIF OF YOUR MAP VISUALIZATION HERE]**

## TECHNOLOGIES USED

* Python 3.x
* Matplotlib (for visualization)
* NumPy (for numerical operations)
* Python's built-in `random` library (for initial random choices and deck shuffling)
