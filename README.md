 #Ticket to Ride AI with Reinforcement Learning

## PROJECT AIM

Create an AI for the board game "Ticket to Ride" by using a Reinforcement Learning (RL) approach. Its blend of strategic route planning, resource management, and competitive blocking makes it an ideal environment for exploring advanced AI learning techniques through self-play.

## WHAT IS "TICKET TO RIDE”?

“Ticket to Ride” (America base game) is a board game where you have a map of major cities in America. Players draw "tickets" which give the task to connect two cities with a train route network. Players earn points based on fulfilled tickets, routes claimed, and for holding the longest continuous train route.

The game uses 110 train car cards (8 different colors and 1 joker/locomotive type). There are also 30 unique destination tickets. Players can draw 3 new tickets on their turn and must keep at least one. Unfulfilled tickets at the end of the game count as minus points. The game ends when a player has 2 or fewer trains left.

## MY LEARNING JOURNEY

The project idea is simple yet represents a potentially steep learning curve for me. It's "simple" because its core concept isn't entirely groundbreaking, but "difficult" because I have never implemented such a complex system before. This makes it a non-typical starter project in the field, challenging me to learn and apply new concepts throughout its development. I aim for this project to be a testament to my ability to tackle complex, self-directed engineering and AI challenges.

## Project Progress: Recent Updates

### Last Update: July 8, 2025

**Completed:**
* **Core Game Engine Refinement:** Implemented full route claiming mechanics, end-game triggers, last round actions, and complete end-game scoring.
* **Enhanced Game Flow:** Added dynamic start player selection and organized persistent player turn order.
* **Real-time Visualization:** Reworked map visualization to update in real-time after each turn in the console environment.
* **Modular Game Logic:** Refactored main game loop to consolidate random choices and improve overall game function modularity.
* **Pathfinding:** Integrated **Breadth-First Search (BFS)** for `check_ticket_completed` functionality.
* **Initial AI Foundations:** Began laying groundwork for the RL model by starting work on longest route logic and preliminary state representations. Also formulated an initial prediction on AI behavior
* Thorough testing of the game engine components.

**Next Focus:**
* Finalize comprehensive scoring logic, including the longest route bonus.
* Deepen understanding of fundamental RL models and approaches.
* Formally define the initial state space, action space, and reward system for the RL agent.

---

### Update: June 23, 2025

**Completed:**
* Initial game state data representation, including all cities, routes, and card configurations.
* Basic game initialization: Setting up player dictionaries, distributing initial train car cards and destination tickets, and handling the random choice for players returning initial tickets.
* Core train car card drawing logic implemented (face-up and deck draws), including specific rules for handling locomotives during the second draw.
* Initial visualization of the map created using Matplotlib, showing cities and routes.

**Next Focus:**
* Implementing the full route claiming logic, including managing the discard pile for train car cards used in claims.
* Developing end-game logic: Triggering the last round, determining the longest route, and calculating final scores.
* Exploring real-time visualization updates as agents play simulated games.

## CURRENT ROADMAP

This project will proceed in distinct phases:

* **Phase 1: Game Engine Development** 
* **Phase 2: Basic AI Development** 
* **Phase 3: Reinforcement Learning Setup** 
* **Phase 4: Training and Iteration** 
* **Phase 5: Evaluation and Analysis** 

## DEVELOPMENT DIARY

This section serves as a detailed chronological log of my development process, design decisions, challenges encountered, and lessons learned throughout the project.

### Entry: July 9, 2025

**Overall Summary for this period:**
This period focused heavily on finalizing the core game engine components, particularly around player turn management, complex rule implementations like ticket completion, and setting up the foundational state representation needed for the upcoming Reinforcement Learning phase. I also implemented dynamic visualization for better debugging.

#### Detailed Features & Implementations:

* **Game Setup Rework (`set_game_up` function):**
    * **Random Start Player:** Added dynamic functionality to choose a random starting player.
    * **Turn Order Management:** Implemented the use of a `player_list` within the `board_active` dictionary to accurately track and manage the turn order of players throughout the game.

* **Route Claiming Functionality (`claim_route`):**
    * **Core Constraints:** Implemented essential game constraints for claiming a route:
        1.  The route must not be owned by another player.
        2.  The player must have enough wagons left to build the route.
        3.  The player must possess the required train car cards and, if necessary, locomotive cards.
    * **Supporting Function `check_if_route_build_possible`:**
        * Loops through all currently unowned routes.
        * Determines if the current player has sufficient wagons and the correct combination of train car and locomotive cards to claim each route.
        * Stores all buildable routes as a tuple `(route_id, route_color, required_length)` in a `buildable_routes` list within each player's dictionary.
    * **Main `claim_route` Function:**
        * Takes a `route_to_claim_tuple` (a random choice from the `buildable_routes` list for simple AI/testing).
        * Deducts used train car cards and locomotives from the player's hand and moves them to the discard pile.
        * Updates the ownership status of the claimed route in the `board_static.routes` dictionary.
        * Adds the claimed `route_id` to the player's `claimed_routes_id` list.
        * Adds points to the player's score based on route length.
        * Deducts the number of wagons used from the player's wagon count.
    * **Future Considerations for Claiming Logic:**
        * **Secondary Routes:** Logic to prevent claiming a second route between two cities if the primary route is already claimed by the same player.
        * **Ticket-Driven Claims:** Restrict claimable routes only to those that connect cities relevant to an active ticket.
        * **Network Connectivity:** Add logic to prioritize routes that connect existing claimed segments.
        * **Longer Route Prioritization:** Implement heuristics to favor claiming longer routes.
        * **Locomotive Usage:** Integrate more nuanced logic for when to use locomotive cards.

* **Ticket Fulfillment Check (`check_ticket_fulfilled`):**
    * **Algorithm:** Determines if a given destination ticket (connecting two cities) is fulfilled using a **Breadth-First Search (BFS)** algorithm.
    * **Player Network Creation:** Creates a dynamic `player_network` (adjacency list/dictionary) based on all routes the player has currently claimed. Example: `{'STF': {'DEN', 'ELP'}, 'ELP': {'STF'}, 'DEN': {'STF'}}`.
    * **BFS Execution:** Executes the BFS on the `player_network` to find if a path exists between the ticket's start and end cities.

* **Player Action Function:**
    * **Action Choices:** Allows the current player (or AI) to choose one of three possible high-level actions:
        * Claim a route
        * Draw a destination ticket
        * Draw a train car card
    * **Integration:** This function serves as the interface between the game engine and player/AI decisions.

* **Main Game Loop Enhancements:**
    * **Real-time Visualization Integration:** Reworked the loop to utilize `IPython.display.clear_output(wait=True)` and the refactored `plot_ttr_map` (now accepting `ax` as an argument) to provide dynamic, turn-by-turn updates of the game board and player status in the Colab output.
    * **End Game & Last Round:** Implemented the logic for triggering the end game (when a player reaches 2 or fewer wagons), managing the "last round" for all other players, and transitioning to final scoring.
    * **Consolidated Randomness:** Streamlined the game loop to centralize random choices (e.g., initial player order, initial ticket returns, random player actions for baseline AI) within a higher-level structure, improving modularity for RL integration.

### Challenges Encountered & Lessons Learned:

* **BFS Network Graph Construction:** Initial difficulties arose in correctly building the `player_claimed_routes_graph` as an adjacency list/dictionary, particularly handling cities with no claimed connections. This was resolved by ensuring robust `dict.get()` usage with default empty sets.
* **Matplotlib Dynamic Updates:** Realized the necessity of passing the `ax` object to the plotting function and calling `ax.clear()` for each update. This highlighted the difference between static plotting and managing dynamic figure updates in an iterative environment like Colab.
* **State Space Design for RL:** The decision to generalize player IDs (from specific 'player_1', 'player_2' to 'self', 'opponent') for route ownership was a key insight to manage potential state space explosion, a common challenge in RL for multi-agent games. This required careful design of the state representation function.
* **Game Flow Interdependencies:** Ensuring seamless transitions between different player actions and end-game triggers required careful mapping of game states to available actions and their consequences.

### Next Steps & Focus:
* **RL Model Fundamentals:** Begin a deep dive into basic Reinforcement Learning model approaches (e.g., Q-Learning, DQN) to understand their mechanisms and suitability for Ticket to Ride.
* **Formalize RL Components:** Precisely define the full state space (how the game state is translated into an input for the RL model), the action space (all possible actions the RL agent can choose from), and the initial reward system.

---

## Update: June 23, 2025

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

## TECHNOLOGIES USED

* Python 3.x
* Matplotlib (for visualization)
* NumPy (for numerical operations)
* `collections.deque` (for efficient queue operations, notably in BFS)
* Python's built-in `random` library (for initial random choices and deck shuffling)


