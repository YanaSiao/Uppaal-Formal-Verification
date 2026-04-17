# Hop-paal Verification

Formal modeling and verification of a Frogger-inspired game using Uppaal, including symbolic and stochastic analysis.

## Overview

This project was developed for the *Formal Methods for Software Engineering* course at Politecnico di Milano (A.Y. 2025–2026).

The goal is to model a simplified version of the classic Frogger game and formally verify its correctness using:

- **Network of Timed Automata (NTA)**
- **TCTL (Timed Computation Tree Logic)**
- **Statistical Model Checking (SMC)**

The system is implemented and analyzed using **Uppaal**.

## Objectives

- Model the game dynamics as a system of interacting timed automata
- Verify safety and reachability properties
- Analyze system behavior under both deterministic and probabilistic assumptions
- Explore trade-offs between symbolic and stochastic verification


## Project Structure
├── symbolic/ # Uppaal model (deterministic version)
├── stochastic/ # Uppaal model (probabilistic version)
├── report/ # Final report (PDF)
└── README.md


##  Model Description

The game is modeled as a grid-based environment including:

- Frogger (player-controlled agent)
- Vehicles (deterministic movement)
- Floating objects (logs, turtles, diving turtles)
- Game rules (timing constraints, scoring, lives)

Key modeling aspects:

- Time constraints encoded via clocks
- Movement rules enforced through transitions
- Safety conditions (e.g., collisions, drowning)
- Synchronization between entities


##  Verification

### Symbolic Verification

The symbolic model verifies properties such as:

- Absence of deadlocks
- Correct movement constraints of entities
- Reachability of specific game states
- Safety conditions (e.g., Frogger survival)


### Stochastic Verification

The stochastic model extends the system with:

- Probabilistic Frogger movement
- Exponential timing for selected entities

Analysis includes:

- Probability of completing a level
- Score-based performance evaluation
- Behavioral comparison under different strategies


##  Tools

- **Uppaal** – modeling and verification of timed automata  
- **Uppaal SMC** – statistical model checking  


##  How to Run

1. Open Uppaal
2. Load the `.xml` model from either:
   - `symbolic/`
   - `stochastic/`
3. Navigate to the **Verifier**
4. Run the predefined queries


## Results

Detailed results and analysis are available in the report:
(add link)

---

## References

- Uppaal Documentation: https://uppaal.org/
- Course material (Formal Methods for Software Engineering)


## Author

- Yana Siao


## Notes

This project focuses on **formal correctness and verification**, not gameplay implementation.
