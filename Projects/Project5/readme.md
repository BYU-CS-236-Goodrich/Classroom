# Dependency Graph

## General Project Requirements
1. Projects are to be completed by each student individually (not by groups of students)
2. Projects must be passed-off through Github Classroom.
3. Even though we provide the test cases, the definition of your project working is that it passes all tests on Github Classroom. The TAs will only grade what is on Github Classroom.

## Resources
1. No Jupyter Notebook tutorial for this project.
2. Starter Code: https://classroom.github.com/a/tPcF9k7q  
3. Please watch the [TA help video](https://www.youtube.com/watch?v=koMOU66uszQ) and review the [Ta help slides](https://docs.google.com/presentation/d/103Tm7hog1ANWJhKZ0imYT1AxLz8RVCUuFlHPSBnmEDk/edit#slide=id.g24b20288035_0_0)
4. Project <a href="diagrams/diagram.png" target="_blank">Diagrams</a>

__NOTE__: Do Project 5a, the written portion, _first_ (see the Schedule) before you start to code.

__Build the dependency graph for the rules in a Datalog program and use depth-first search on the graph to improve the evaluation of the rules.__

## Example Input

```
Schemes:
Parent(p,c)
Sibling(a,b)
Ancestor(x,y)

Facts:
Parent('bob','ned').
Parent('jim','bob').
Sibling('sue','ned').

Rules:
Sibling(x,y):-Sibling(y,x).
Ancestor(x,y):-Ancestor(x,z),Parent(z,y).
Ancestor(x,y):-Parent(x,y).

Queries:
Ancestor(x,'ned')?
Sibling('ned','sue')?
```

## Example Output
```
Dependency Graph
R0:R0
R1:R1,R2
R2:

Rule Evaluation
SCC: R2
Ancestor(x,y) :- Parent(x,y).
  x='bob', y='ned'
  x='jim', y='bob'
1 passes: R2
SCC: R1
Ancestor(x,y) :- Ancestor(x,z),Parent(z,y).
  x='jim', y='ned'
Ancestor(x,y) :- Ancestor(x,z),Parent(z,y).
2 passes: R1
SCC: R0
Sibling(x,y) :- Sibling(y,x).
  a='ned', b='sue'
Sibling(x,y) :- Sibling(y,x).
2 passes: R0

Query Evaluation
Ancestor(x,'ned')? Yes(2)
  x='bob'
  x='jim'
Sibling('ned','sue')? Yes(1)
```
## Datalog Interpreter
For this project, the major steps of the interpreter are:

1. Process the schemes (same as the last project)
2. Process the facts (same as the last project)
3. Evaluate the rules (add more code as described below)
4. Evaluate the queries (same as the last project)


## Evaluating Rules
The following steps are used to evaluate the rules:

1. Build the dependency graph and the reverse dependency graph.
2. Run DFS-Forest on the reverse dependency graph to get the post-order.
3. Use the post-order for a DFS-Forest on the forward dependency graph to find the strongly connected components (SCCs).
4. Evaluate the rules in each component.

## Dependency Graph
Build the dependency graph for the rules in the Datalog program.

Assign a numeric identifier to each rule starting with zero. Assign the identifiers to the rules in the order the rules appear in the input. Use 0 for the first rule, 1 for the second rule, etc.

When outputting rule identifiers always precede the rule identifier with the letter R. For example, output the first rule as R0, the second rule as R1, etc. The preceding R is for decoration only and is not part of the identifier.

Make a node in the graph for every rule identifier. Don't add the same node to the graph more than once.

Make an edge in the graph for each rule dependency. Add an edge to the graph from node x to node y if the evaluation of x depends on the result of y. Rule A depends on rule B if any of the predicate names in the body of rule A is the same as the predicate name of the head of rule B. Don't add the same edge to the graph more than once.

Note that more than one rule may have the same predicate name in the head. This means that a single name in the body of a rule may cause a dependency on more than one rule.

Note also that a rule may depend on itself.

Consider the following rules as an example:
```
A(X,Y) :- B(X,Y), C(X,Y). # R0
B(X,Y) :- A(X,Y), D(X,Y). # R1
B(X,Y) :- B(Y,X).         # R2
E(X,Y) :- F(X,Y), G(X,Y). # R3
E(X,Y) :- E(X,Y), F(X,Y). # R4
```
The dependency graph as an adjacency list is:
```
R0: R1 R2
R1: R0
R2: R1 R2
R3:
R4: R3 R4
```

## Reverse Dependency Graph

Finding the strongly connected components (SCCs) of the dependency graph allows the rules to be grouped and ordered for improved evaluation. An algorithm for computing the SCCs is found in section 3.4.2 of [Algorithms](https://people.eecs.berkeley.edu/~vazirani/algorithms/chap3.pdf) by Dasgupta, C. H. Papadimitriou, and U. V. Vazirani.

The first step of the algorithm is to build the reverse dependency graph. The reverse graph has the same nodes as the original dependency graph. The edges in the reverse graph are the same as the edges in the original graph except the direction of each edge is reversed.

The reverse dependency graph for the example from the previous section as an adjacency list is:
```
R0: R1
R1: R0 R2
R2: R0 R2
R3: R4
R4: R4
```

## DFS Forest
The next step of the SCC algorithm is to run DFS-Forest on the reverse dependency graph to obtain post-order numbers.

Run DFS-Forest on the reverse dependency graph. Create a post-order of node identifiers. When there is a choice of which node to visit next in the DFS-Forest, choose the node identifier that comes first numerically.

The post-order for the example from the previous section is:
```
R2, R1, R0, R4, R3
```

## Finding the SCCs
The post-order from the DFS-Forest on the reverse graph gives the correct order to use for searching for SCCs in the original dependency graph. The order is used in reverse order.

The first SCC is found by running a depth-first search on the original dependency graph starting from the node last in the post-order. Any node visited during the DFS is part of the SCC.

For the example from the previous section, the search starts at node R3 and visits only node R3. The first SCC contains only node R3.

The process is repeated with the other nodes from the end of the post-order to the beginning.

The search for the second SCC starts at node R4 and visits only node R4. The second SCC contains only node R4.

The search for the next SCC starts at node R0 and visits nodes R0, R1, and R2. The SCC contains nodes R0, R1, and R2.

The process attempts to find additional SCCs starting with node R1 and then node R2. Since these nodes have already been visited, additional SCCs are not found.

Note that the visit markers used in the DFS-Forest must not be reset between the searches for each SCC.

## SCC Evaluation
For each SCC found in the previous step, run the rule evaluation process from the previous project on the subset of rules found in the SCC.

Evaluate the SCCs in the order they are found.

If an SCC contains only one rule and that rule does not depend on itself, the rule is only evaluated once.

If an SCC contains more than one rule, or a single rule that depends on itself, repeat the evaluation of the rules in the SCC until the evaluation reaches a fixed point.

When an SCC contains more than one rule, evaluate the rules in the SCC in the numeric order of the rule identifiers.

## Output Format
The output for this project is composed of the following parts:

1. Dependency graph output
2. Rule evaluation output
3. Query evaluation output (same as the last project)

## Dependency Graph Output
Output a line with the title 'Dependency Graph' followed by the dependency graph followed by a blank line. Print the graph using an adjacency list representation. Output each node in the graph on a separate line. For each node x, print the identifier for x followed by a colon and a comma-separated list of the identifiers for each node y such that there is an edge from node x to node y. Sort each list of successor nodes by identifier. Sort the output lines by node identifier.

## Rule Evaluation Output
Output a line with the title 'Rule Evaluation' followed by the output from evaluating each SCC, followed by a blank line. For each SCC (in the order they are found by the algorithm),

1. Output a line with the prefix 'SCC: ' and a comma-separated list of the nodes contained in the SCC.
2. For each rule that is evaluated, output the rule followed by the tuples generated by the rule in the same form as in the previous project.
3. Output a line with the prefix 'n passes: ' (where 'n' is replaced with the number of passes required to evaluate the SCC), and a comma-separated list of the nodes contained in the SCC.

## Implementation Requirements
1. Don't create duplicate nodes or duplicate edges in the graph.
2. Print node identifiers in numeric order.
3. When there is a choice of which node to visit next in the depth-first search, choose the node identifier that comes first numerically.
4. The program must run to completion with a normal exit status for any input. Do not terminate with a non-zero exit status for any input, including inputs that have errors.

## Implementation Suggestions
1. In the Graph class, use a map from an int to set(int) to store the graph edges. (The int is the node identifier and the set gives the neighbor nodes.)
2. In the Graph class, use a map from an int to bool to indicate if each node is visited (or use a Node class with a member variable for visited).
3. In the Graph class, use a list of ints to save the post-order of node identifiers.
4. SCCs should be represented by sets of integers.

## FAQ

1. __How are rule identifiers assigned to rules?__

    Rule IDs are assigned following program order starting from zero. Rule IDs are integers and not strings. The proceeding R in the output is just a decoration.

2. __What order should be followed when printing output or starting depth-first searches?__

    Always follow node order from least to greatest according to the rule IDs. Also, since the R is a decoration on the output, node 2 is ordered before node10. When printing the dependency graph as an adjacency list, edges should also follow numerical order. In a depth-first search, when there is a choice of two edges to follow, for example node R0 can go to either node R2 or node R4, again follow numerical order and visit R2 first.

3. __Why is the code taking a long time to run?__

    Are you running your code in Visual Studio? Visual Studio usually runs code in 'debug' mode. Debug mode may slow down the code significantly. Try running the code outside of Visual Studio (from the command line), or configure Visual Studio to run in release mode. (Microsoft explains how to configure release mode at this link: https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-set-debug-and-release-configurations ). CLion also has a release configuration that can be set up in Settings > Build, Execution, Deployment > CMake.

    Other things that may impact running time include:

    1. The computer on which the code is running. (Run the code on the lab computers for the best approximation of how it will run on the TA computers.)

    2. Using a list to store the tuples in a relation instead of a set. (Sorting lists and searching lists for duplicates are relatively slow operations.)