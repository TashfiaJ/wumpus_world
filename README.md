Wumpus World is an interactive adventure game where the goal is to find all treasures hidden in the board. Armed with just one arrow to eliminate the Wumpus, the agent must navigate the cave's dark passages strategically to avoid the wumpus and pits. This report presents a wumpus world game built using Angular, including many services for generation of the board, move management, evaluation and path finding. Humans can also play the game. The game rules are:
If the agent shoots an arrow in a direction and the Wumpus is in that direction, the Wumpus dies. 
(Shoot ⇒ WumpusDead)
If the agent is in the same cell as the Wumpus, it gets eaten. 
(Agent(x, y) ∧ Wumpus(x, y) ⇒ AgentEaten) 
If the agent is in the same cell as the treasure, it picks up the treasure.
 (Agent(x, y) ∧ Treasure(x, y) ⇒ GrabTreasure) 
If the agent is in the same cell as pits, it will die. 
(Agent(x, y) ∧ Pit(x, y) ⇒ AgentDead)

PEAS component
The PEAS components for the Wumpus World:

Performance Measure: Maximize the agent's score (e.g., by collecting treasure, killing the Wumpus) while minimizing actions or costs. It earns +1000 for collecting treasure, but loses -1000 for falling into a pit or being eaten by the Wumpus, –1 for each move taken and –10 for using up the arrow. The game ends when the agent dies or collects all treasures.
Environment: A 10 × 10 grid of rooms. The agent always starts in the square labeled [1,10], facing to the right. The rooms can have multiple pits, Wumpuses and treasures.
Actuators: Move, shoot and pick up treasure.
Sensors: Detects breeze, smell & glitter.  

Implementation Components
boardGeneration: This particular component generates the game board, which can be done either through user input or randomly. It also adds game elements like scents around Wumpuses, and the breezes around pits.
gameEngine: This component manages the player's movement to the best-selected destination, collecting gold, shooting arrows, and updating hints (like stench and breeze) when a Wumpus is defeated or gold is collected.

#GameLogics: 
It manages the AI's decision-making process. It determines the AI's best course of action, employing various strategies for moving, shooting arrows, and collecting treasures.
pathFinder: This component calculates the optimal path for the AI from its current location to the selected destination using the A* algorithm, which involves heuristic distance estimation and finding the shortest path.
global: This component serves as a central repository for essential variables and functionalities used across multiple components

Evaluation Criteria
We used a scoring system that takes into account several significant variables to grade the game. First, the cells have risk scores, which are determined by computing the cell's wumpus, pit, and treasure probabilities. We assign scores to adjacent cells if we detect a breeze or smell. Then, if the cell has previously been visited, we add visit risk to it. This is also updated if the agent visits the cell again . To avoid loops, we also used the A* algorithm to select the best move from unvisited adjacent cells.

The predicates are: 
Adjacent(x1, y1, x2, y2): A relation indicating that cells (x1, y1) and (x2, y2) are adjacent.
Breeze(x, y): A predicate indicating that there is breeze in cell (x, y).
Pit(x, y): A predicate indicating that there is a pit in cell (x, y)
Treasure(x, y): A predicate indicating that there is a treasure in cell (x, y).
Smell(x, y): A predicate indicating that there is smell in cell (x, y)
Wumpus(x, y): A predicate indicating that there is a wumpus in cell (x, y)

The first order logics are given below:
∀x∀y (Breeze(x, y) ⇒ ∃x1∃y1 (Adjacent(x, y, x1, y1) ∧ Pit(x1, y1)))
∀x∀y (Smell(x, y) ⇒ ∃x1∃y1 (Adjacent(x, y, x1, y1) ∧ Wumpus(x1, y1)))
∀x∀y (¬Breeze(x, y) ⇒ ¬∃x1∃y1 (Adjacent(x, y, x1, y1) ∧ Pit(x1, y1)))
∀x∀y (¬Smell(x, y) ⇒ ¬∃x1∃y1 (Adjacent(x, y, x1, y1) ∧ Wumpus(x1, y1))

The Wumpus World game offers an engaging adventure where the agent aims to maximize its score by collecting treasures while avoiding hazards. The game's implementation includes various services and logic for efficient decision-making, contributing to a challenging and enjoyable gaming experience.
