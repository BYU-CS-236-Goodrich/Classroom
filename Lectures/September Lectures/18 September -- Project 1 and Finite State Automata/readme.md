# 18 September -- Project 1 and Finite State Automata

## Before Class Reading

- Definition 3 and Example 4 Section 13.3.3 (skip "Extending the Transition Function").
- Definition 4 and Examples 5-8 Section 13.3.4
- <a href= "Project1__Guide.pdf"> Project 1 Guide</a> (and the General Project Guide if you haven't read it already)
- <a href="https://github.com/BYU-CS-236/jupyter-notebook-tutorials"> Jupyter notebook tutorial </a> on implementing FSAs in Python:
  - A video tutorial on how to download and step through Jupyter notebook tutorials is available <a href="https://www.youtube.com/watch?v=wGpAdec_Ycg"> here </a>
  - The tutorial is useful if you've never used Jupyter notebooks before.
- Relationship between regular expressions and FSAs:
  - Single symbol means a single transition from the start state to the accept state.
  - Empty set means FSA with no accept states.
  - Empty string means FSA that has a start state that is also an accept state.
  - Union means multiple transitions from the start state to one or more accept states.
  - Concatenation means sequential state transitions from the start state to the accept state.
  - Kleene star means a self-loop (or loop over sequence a la (ab)* where the last character before the * causes a transition to an accept state, and the transition to the loop is from an accept state (so that lambda is accepted)).
  - Required reject state.

## In-Class Discussion

- What is a finite state automaton, and how is it different from a finite state machine?
- Project 1 overview

## Slides

- <a href = "FiniteStateAutomata_fall_2023.pptx"> Before Class </a>
- <a href="FiniteStateAutomata_fall_2023_after_class.pptx"> After Class </a>