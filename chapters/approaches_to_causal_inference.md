# Approaches to causal inference

- See holland1986statistics
- See also https://www.the100.ci/2024/06/26/sometimes-a-causal-effect-is-just-a-causal-effect-regardless-of-how-its-mediated-or-moderated/
- See discussion (also in comments) in this Gelman post: https://statmodeling.stat.columbia.edu/2012/09/11/advantages-of-the-instrumental-variables-or-potential-outcomes-approach-in-clarifying-causal-thinking/


## Perl

Directed Acyclic Graphs

- A *confounder* is a variable that simultaneously affects the treatment indicator and the outcome (the same as an omitted variable).
- A *collider* is a variable that is simultaneously affected by the treatment indicator and the outcome.
- A *backdoor path* is a path from the treatment indicator to the outcome via a confounder.
- There are two ways to close a backdoor path:
  - Control for the confounder if it is available
  - Have a collider on the backdoor path
- An analysis design meets the *backdoor criterion* if all backdoor paths are closed, in which case we have isolated a causal effect.

## Thought experiments

- See @heckman2023econometric

