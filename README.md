# Hop-paal Verification

A reusable UPPAAL case study demonstrating formal modeling, timed automata design, symbolic verification, and statistical model checking through a Frogger-inspired game.

## Overview

This project was developed for the *Formal Methods for Software Engineering* course at Politecnico di Milano (A.Y. 2025–2026).

The goal is to model a simplified version of the classic Frogger game and formally verify its correctness using:

- **Network of Timed Automata (NTA)**
- **TCTL (Timed Computation Tree Logic)**
- **Statistical Model Checking (SMC)**

The system is implemented and analyzed using **Uppaal**.

## Highlights

- Model the game dynamics as a system of interacting timed automata
- Verify safety and reachability properties
- Analyze system behavior under both deterministic and probabilistic assumptions
- Explore trade-offs between symbolic and stochastic verification


## Project Structure
```text
├── symbolic/      # UPPAAL model (deterministic version)
├── stochastic/    # UPPAAL model (probabilistic version)
├── report/        # Final report (PDF)
└── README.md
```

This repository also contains notes describing:

- how to structure a UPPAAL project
- how to avoid state-space explosion
- when to use templates versus global functions
- symbolic vs stochastic verification
- common modeling pitfalls
  
##  Model Description

The game is modeled as a grid-based environment including:

- Frogger (player-controlled agent)
- Vehicles (deterministic movement)
- Floating objects (logs, turtles, diving turtles)
- Game rules (timing constraints, scoring, lives)

Key modeling aspects:

- Time constraints encoded via global clock synchronization
- Movement rules enforced through transitions
- Safety conditions (e.g., collisions, drowning)
- Synchronization between entities

## Model Architecture

The game is modeled as a Network of Timed Automata consisting of:

- GlobalClock
- GameManager
- RowRoad
- TruckRow
- DivingTurtle
- Frogger

The model follows a synchronized single-clock architecture in which all automata advance through a broadcast tick event. This minimizes clock dimensions while ensuring deterministic collision evaluation.

##  Verification

| Property              | Status |
| --------------------- | ------ |
| Deadlock free         | ✅      |
| Reachability          | ✅      |
| Safety                | ✅      |
| Collision correctness | ✅      |
| SMC experiments       | ✅      |

The stochastic model analysis includes:

- Probability of completing a level and game progression
- Score-based performance evaluation
- Behavioral comparison under two different strategies


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

This repository is licensed under a Custom Educational License.

The author does **not** grant permission for this repository or its contents to be used for training, fine-tuning, or evaluation of machine learning or generative AI models.
