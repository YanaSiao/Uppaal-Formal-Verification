# Recommendations for Future Formal Methods Projects

This document summarizes practical lessons learned during the development of a UPPAAL case study for the *Formal Methods for Software Engineering* course. While every project differs, the following recommendations can help design models that are easier to implement, verify, and extend.

# 1. Plan Before Modeling

A significant amount of development time can be saved by carefully planning the model before creating any automata. Many design decisions become difficult to change once the model grows.

Before opening UPPAAL, identify:

- the entities in the system;
- which entities should be modeled as timed automata;
- which entities can be represented as variables instead;
- shared resources;
- synchronization points;
- the temporal and logical properties that will eventually be verified.

A useful guideline is:

> **Create an automaton only for entities that exhibit independent behavior.** Static information or data that never performs actions independently is usually better represented as variables.

For example:

| Automata | Variables |
|----------|-----------|
| Frogger | Score |
| Cars | Lives |
| Logs | Goal flags |
| Turtles | Game status |

Planning the verification properties at this stage also prevents modeling unnecessary behaviors that never contribute to the final analysis.

# 2. Designing the Timed Automata

After identifying the system components, design each automaton by defining:

- locations;
- transitions;
- guards;
- updates;
- synchronization channels.

Keep automata as simple as possible. Every additional location or transition increases the complexity of the model.

## Committed Locations

Committed locations should be used when multiple transitions must execute atomically without allowing time to pass or other automata to interleave.

Typical use cases include:

- updating several shared variables;
- completing synchronization sequences;
- avoiding inconsistent intermediate states.

### Advantages

- preserves atomic execution;
- simplifies synchronization logic;
- prevents invalid intermediate configurations.

### Disadvantages

- excessive use can make models more difficult to understand;
- increases synchronization constraints between automata.


## Urgent Locations

Urgent locations prevent time from passing but still allow other enabled automata to execute.

They are useful when:

- actions must occur immediately;
- atomic execution is **not** required.

### Difference Between Committed and Urgent

- **Committed** locations prevent both time progression and interleaving by other automata.
- **Urgent** locations prevent only time progression while still allowing interleaving.

Choosing the correct location type improves both correctness and model readability.

# 3. Global Clock vs. Independent Clocks

One of the earliest architectural decisions is whether to synchronize the entire model using a single global clock or allow each automaton to maintain its own timing.

## Global Clock

A global clock typically broadcasts periodic synchronization events (e.g., `tick!`) that every automaton receives.

### Advantages

- deterministic progression of time;
- simpler synchronization;
- easier debugging;
- easier symbolic verification;
- consistent timing across all components.

### Disadvantages

- introduces additional synchronization events;
- may not accurately represent asynchronous systems;
- can create unnecessary transitions.


## Independent Clocks

Each automaton maintains its own local clock and evolves independently.

### Advantages

- more natural representation of asynchronous systems;
- greater modeling flexibility;
- components evolve independently.

### Disadvantages

- significantly larger state space;
- more difficult synchronization;
- more challenging debugging;
- increased verification complexity.

For educational projects and game-like systems, a single broadcast clock often provides a good balance between simplicity and expressiveness.


# 4. Controlling State Space Explosion

State space explosion is one of the primary challenges in formal verification. Understanding what contributes to the growth of the state space helps create more scalable models.

## Components That Increase the State Space

| Component | Impact | Explanation |
|-----------|--------|-------------|
| Additional automata | Very High | Every automaton contributes to the Cartesian product of locations. |
| Multiple clocks | Very High | Clock zones become significantly more complex. |
| Multiple template instances | Very High | Often one of the largest contributors to state space growth. |
| Additional locations | High | More locations increase the number of reachable configurations. |
| Large integer domains | High | Every additional value increases possible states. |
| Non-deterministic choices | High | Every choice creates additional execution branches. |
| Large arrays | Medium–High | Depends on array size and usage. |
| Synchronization channels | Medium | Increase possible interleavings between automata. |


## Reducing the State Space

After creating an initial working model, review every component critically.

Consider the following questions:

- Can two locations be merged?
- Is an intermediate state actually necessary?
- Can a state be replaced by an update?
- Can an integer become a Boolean or enumeration?
- Is this state referenced by any verification property?
- Can several transitions be combined?
- Can symmetry be exploited?

A useful principle is:

> **Only model behavior that influences the verification properties.**

If a state is never referenced by any property and has no observable effect, it may be unnecessary.


# 5. Build the Symbolic Model First

Developing the symbolic model before introducing stochastic behavior significantly simplifies debugging and verification.

Initially:

- simplify movement where possible;
- minimize non-determinism;
- avoid probabilities;
- verify correctness first.

Only after the symbolic model satisfies all required properties should stochastic behavior be introduced.

This incremental approach makes locating modeling errors much easier.


# 6. Designing for Future Stochastic Extensions

Even when the initial goal is only symbolic verification, it is beneficial to design the model with future extensions in mind.

Some useful design principles include:

- keep templates modular;
- separate deterministic logic from probabilistic behavior;
- avoid embedding random behavior throughout the model;
- reuse synchronization mechanisms whenever possible.

A well-structured symbolic model usually requires only minimal modifications to become a stochastic model.


# 7. Symbolic vs. Stochastic Models

Although both approaches use timed automata, they serve different purposes.

| Symbolic Model | Stochastic Model |
|---------------|------------------|
| Exhaustive verification | Statistical verification |
| Timed automata | Stochastic timed automata |
| Reachability and safety | Probability estimation |
| Deterministic timing | Exponential delays |
| Uses `A[]`, `E<>` | Uses `Pr[]`, `simulate`, expected values |

## Understanding Rates

A **rate** does **not** specify a fixed delay.

Instead, the expected waiting time is

```
Expected delay = 1 / rate
```

For example,

```
rate = 5
```

does **not** mean waiting five time units.

Instead, the average waiting time is

```
1 / 5 = 0.2 time units
```

Understanding this distinction is essential when constructing stochastic models.


# 8. Verification Strategy

Verification should be performed continuously throughout development rather than only after the model is complete.

A recommended order is:

1. syntax validation;
2. deadlock checking;
3. reachability properties;
4. safety properties;
5. liveness properties;
6. performance analysis;
7. stochastic verification.

Frequent verification makes modeling errors easier to locate and reduces debugging time.


# 9. Common Mistakes

Some of the most common modeling mistakes include:

- creating an automaton for every object in the system;
- using unnecessarily large integer ranges;
- introducing clocks that are never required;
- misunderstanding committed and urgent locations;
- postponing verification until the end of development;
- introducing stochastic behavior before validating the symbolic model;
- modeling implementation details that do not affect any verification property;
- writing verification queries only after completing the model.

Most verification problems become easier once the model is simplified.


# 10. Choosing the Appropriate Level of Abstraction

Formal models are abstractions of real systems rather than exact implementations.

A common mistake is modeling every implementation detail, even when those details have no influence on the properties being verified.

Instead, include only behavior that affects correctness.

Whenever possible:

- abstract away unnecessary details;
- simplify repetitive behavior;
- represent static information as variables instead of automata;
- avoid creating locations that exist solely for implementation convenience.

Appropriate abstraction often provides greater performance improvements than algorithmic optimizations.


# 11. Final Advice

When designing formal models, remember the following principles:

- Plan the architecture before modeling.
- Model only behavior relevant to the verification goals.
- Keep automata as small and simple as possible.
- Verify incrementally after every major modification.
- Reduce the model before attempting to increase computational resources.
- Document every abstraction and modeling decision.
- Design the symbolic model so that it can later be extended into a stochastic model with minimal changes.

A well-designed model is not necessarily the most detailed one, but the one that captures the essential behavior required to prove the desired properties while remaining understandable, maintainable, and verifiable.
