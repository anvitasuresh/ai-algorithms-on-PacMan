# AI Algorithms on Ms. Pac-Man

Implementing and comparing classical AI search algorithms — Greedy, Minimax with alpha-beta pruning, and Monte Carlo — on the Ms. Pac-Man OpenAI Gym environment. The project evaluates each algorithm's performance in pellets collected and survival time, and reflects honestly on the trade-offs and limitations that arise when classical game-tree algorithms meet a continuous pixel-based state space.

**Course:** COGS 188 — AI Algorithms, UC San Diego

---

## Algorithms Compared

### Greedy Search
Makes locally optimal decisions at each step — always moves toward the nearest pellet without looking ahead. Fast and simple, but cannot anticipate ghost positions beyond the immediate frame.

### Minimax with Alpha-Beta Pruning
Models the game as an adversarial two-player tree: Pac-Man maximizes score, ghosts minimize it. Alpha-beta pruning eliminates branches that cannot affect the final decision, reducing the search space. Alternates turns between Pac-Man and a single representative ghost.

### Hybrid Greedy-Minimax
Switches dynamically between strategies based on a pixel-value threshold as a proxy for game state (danger vs. safe). Uses Greedy when the environment appears safe and Minimax when it detects threat proximity.

### Monte Carlo Simulation
Runs multiple episode rollouts with an epsilon-greedy policy and averages outcomes to estimate action value. Provides a sampling-based alternative to full tree search.

---

## Environment

**OpenAI Gym `MsPacman-v4`**
- State: RGB pixel array (210 × 160 × 3)
- Actions: 5 discrete (NONE, UP, RIGHT, LEFT, DOWN)
- Reward: number of pellets collected
- Terminal: all pellets cleared OR ghost collision

---

## Results

| Algorithm | Avg Pellets | Win Rate | Avg Time |
|---|---|---|---|
| Greedy | ~240 | 0% | 0.12s |
| Minimax | ~232 | 0% | 0.13s |
| Hybrid Greedy-Minimax | ~28 | 0% | 0.12s |
| Monte Carlo (10 episodes) | ~229 (range: 130–270) | 0% | — |

No algorithm achieved a game win. Greedy and Minimax performed comparably on pellets collected; the Hybrid approach underperformed due to the unreliability of pixel-sum thresholds as a state signal.

---

## The Core Challenge

Classical search algorithms assume access to a **structured game state** — grid positions of Pac-Man, ghosts, and pellets. The OpenAI Gym environment exposes only raw RGB pixel values, which means:

- Ghost and Pac-Man positions must be inferred from pixel patterns rather than read directly
- Converting pixel state to a navigable grid proved intractable without a dedicated object detector
- Minimax's turn-alternation model is a poor fit for the simultaneous movement of Pac-Man and ghosts

This is the central lesson of the project: **algorithm-environment mismatch limits performance more than algorithm quality**. A well-implemented Greedy search on raw pixels outperformed a theoretically superior Minimax precisely because neither could access the structured state they were designed for. The project includes `.gif` recordings of each agent playing to illustrate the behavioral differences.

---

## Project Structure

```
AI-Algorithms-on-PacMan/
├── FinalProject_YourGroupNameHere.ipynb        # full implementation and evaluation
├── Proposal_Project_YourGroupNameHere.ipynb    # project proposal
├── Proposal_SpecialTopics_YourGroupNameHere.ipynb
├── pacman_gameplay_greedy.gif                  # Greedy agent gameplay recording
├── pacman_gameplay_minimax.gif                 # Minimax agent gameplay recording
└── ms_pacman_monte_hybrid.gif                  # Monte Carlo / Hybrid recording
```

---

## Tech Stack

Python · OpenAI Gym (`MsPacman-v4`) · NumPy · Matplotlib
