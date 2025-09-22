## PROJECT AIM

Create an AI for the board game "Ticket to Ride" by using a Reinforcement Learning (RL) approach. Its blend of strategic route planning, resource management, and competitive blocking makes it an ideal environment for exploring advanced AI learning techniques through self-play.

## WHAT IS "TICKET TO RIDE”?

“Ticket to Ride” (America base game) is a board game where you have a map of major cities in America. Players draw "tickets" which give the task to connect two cities with a train route network. Players earn points based on fulfilled tickets, routes claimed, and for holding the longest continuous train route.

The game uses 110 train car cards (8 different colors and 1 joker/locomotive type). There are also 30 unique destination tickets. Players can draw 3 new tickets on their turn and must keep at least one. Unfulfilled tickets at the end of the game count as minus points. The game ends when a player has 2 or fewer trains left.

## MY LEARNING JOURNEY

The project idea is simple yet represents a potentially steep learning curve for me. It's "simple" because its core concept isn't entirely groundbreaking, but "difficult" because I have never implemented such a complex system before. This makes it a non-typical starter project in the field, challenging me to learn and apply new concepts throughout its development. I aim for this project to be a testament to my ability to tackle complex, self-directed engineering and AI challenges.

## Project Progress: Recent Updates

### Last Update: September 17, 2025 
 
**Completed:**
* **Reinforcement Learning Setup:** Implemented a Q-Learning Agent to control the AI player.
* **State Space Discretization:** Developed a custom _encode_state function to map the game's observation space (train cards, route ownership, ticket status) to a single integer index.
* **Training and Learning Logic:** Integrated the agent's core select_action (using an epsilon-greedy strategy) and learn (using the Bellman equation) functions into a complete training loop.
* **Evaluation:** Added a final testing phase to evaluate the trained agent's performance with no exploration.
* **Analysis:** Analyze the reward plot, agent behaviour and decision-making.
* **Next Focus:** Develop a RL agent with a neural network approach that can handle the entire scope of the game and not a highly simplified version.

### Update: July 8, 2025

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

* **Phase 1: Game Engine Development (Completed)** 
* **Phase 2: RL Setup with Q table Approach (Completed)** 
* **Phase 3: RL Setup with neural network approach (In Progress)** 
* **Phase 4: Training and Iteration (In Progress)**  

## DEVELOPMENT DIARY

This section serves as a detailed chronological log of my development process, design decisions, challenges encountered, and lessons learned throughout the project.

### Entry: September 17, 2025

**Overall Summary for this period:**
This period was dedicated to building and integrating the Q-learning agent. I focused on translating game states into a format the model could understand and then setting up a robust training loop.

The Q-table approach meant that it was not possible to train the agent on the entire TtR map. The size of the action space and observation space would have caused a memory error if used with the Q-table approach. Therefore, I decided to use a tiny version of the game, with three cities, three connection routes, only two color cards (grey and red), and a single ticket that is in both players' hands from the start of the game.

Analyzing the results of the current RL agent with a Q-table approach: after training for 10,000 episodes, it's safe to say the agent has discovered a valid strategy for the tiny map setup, with a clear stabilization of the average reward after the 1,500th episode (see phase 2 qt able leaning results.JPG). The Q-table approach is a success and an encouraging sign that, with the right models and approaches, the challenge of training an agent on the full map will also be achievable.

#### Detailed Features & Implementations:

* **Object-Oriented Design (OOP) Principles:** The project is structured with a clear separation of concerns, which is a fundamental principle of OOP. The game logic is contained within the TicketToRideEnv class, while the learning and decision-making capabilities are encapsulated in the QLearningAgent class. This modular approach allows for clean, maintainable code where each class has a single, well-defined responsibility.

* **The TicketToRideEnv Environment Class:**
This class is the heart of the simulation and is built to be compatible with the gymnasium library, a standard for RL environments.

    * **__init__:** Initializes all aspects of the game, including players, the board state, train cards, and tickets. It also defines the observation space (what the agent can see) and the action space (what the agent can do).
    * **reset:** This function is called at the beginning of each training episode. It resets the game to its initial state, shuffles decks, deals cards, and returns the initial observation to the agent.
    * **step:** The most critical function. It takes an action from the agent and advances the game one step. It processes the action (e.g., claims a route, draws a card), updates the game state, calculates the reward for that action, and checks if the game is terminated or truncated (e.g., the game ends). Finally, it returns the new observation, the reward, and the termination status to the agent.

* **The Hard-Coded AI Opponent:**
The opposing player is not a learning agent but rather a hard-coded AI with a simple rule-based strategy. Its purpose is to provide a consistent environment for our Q-learning agent to train against. Its actions are deterministic: it checks for a valid route to claim and, if one exists, it claims it. Otherwise, it defaults to drawing a card. This allows us to evaluate our learning agent's performance without the added complexity of a constantly adapting opponent.

* **The Main Training Loop:**
This is the primary function that orchestrates the entire learning process.

    * **Initialization:** It starts by creating an instance of the TicketToRideEnv and QLearningAgent classes. It also initializes a list to store the rewards for each episode.
    * **Episode Iteration:** The loop iterates for a predefined number of num_episodes. In each episode.
		*  The environment is reset.
    	*  A while loop continues as long as the game has not ended.
    	*  The agent's select_action function is called to choose an action based on the current observation.
		*  The chosen action is passed to the environment's step function, which moves the game forward.
		*  The environment's step returns the reward and the new_observation.
		*  The agent's learn function is called to update its Q-table using the reward and new_observation.
		*  The loop updates the total reward for the episode and sets the current observation to the new_observation for the next turn.
	* **Post-Episode:** After each episode, the agent's decay_epsilon function is called, gradually reducing its exploration rate.

* **Q-Learning Agent Implementation:**
This is the primary function that orchestrates the entire learning process.

    * **__init__:** The agent is initialized with hyperparameters like learning rate (alpha), discount factor (gamma), and epsilon for the exploration-exploitation trade-off. It creates the Q-table, a NumPy array where each row represents a unique game state and each column represents a possible action.
    * **_encode_state** This crucial function takes the game's observation array (a multi-dimensional NumPy array) and converts it into a single, unique integer. This is necessary because the Q-table requires a discrete index for each state. The logic uses a base-N system to map each dimension of the observation space to a unique row in the Q-table, efficiently handling the large number of potential states.
    * **select_action:** This function implements the epsilon-greedy strategy. With a probability of epsilon, the agent chooses a random action (exploration). Otherwise, it chooses the action with the highest Q-value from the Q-table for the current state (exploitation).
    * **learn:** The heart of the algorithm. This function updates the Q-table after each action. It uses the Bellman equation to update the Q-value for the previous state-action pair based on the reward received and the maximum possible Q-value of the new state. This is how the agent learns from experience.
    * **decay_epsilon:** After each full game episode, the epsilon value is decayed. This means the agent gradually shifts from a phase of high exploration (random actions) to a phase of high exploitation (choosing the best-known actions).

### Analysing the current performance of the q table approach:
This is the primary function that orchestrates the entire learning process.

* **1. The Initial Exploration Phase (first 100 episodes):** the agent's performance is low and highly volatile, with rewards often fluctuating. This is exactly what we would expect. The agent's epsilon-greedy strategy is heavily weighted towards exploration, meaning it is taking many random actions to populate its Q-table with reward data.
* **2. The Learning and Improvement Phase (until episode 1,500):** You can see a noticeable increase in the average reward during this period. The total rewards are consistently more positive than in the initial phase, with spikes reaching near to 20 or more. This indicates that the agent is now beginning to exploit the positive knowledge it has gained from its exploration.
* **3. The Convergence Phase (Episodes 1,500-10,000):** After the 1,500 the agent's performance has largely stabilized. The total rewards are consistently positive, and the range of fluctuation has significantly narrowed especially in the negative direction, spikes reaching 25 or more are stil frequently visible.

### Next Steps & Focus:
* **Advanced Reinforcement Learning:** Transitioning from the Q-table to a more advanced model capable of handling the full game's state space. This will involve implementing a Deep Q-Network (DQN) which uses a neural network to approximate Q-values.

---

### Update: July 9, 2025

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
* Gymnasium (for the RL environment)
* NumPy (for numerical operations)
* collections.deque (for efficient queue operations, notably in BFS)
* Python's built-in "random" library (for initial random choices and deck shuffling)


