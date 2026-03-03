# background on cellular automata
This is a constructive demonstration of a self-replicating universal constructor in an isotropic non-totalistic 2-state Moore neighborhood cellular automaton.

A cellular automaton (CA) is a 0-player game. In the most general sense this game is played on an infinite undirected graph. each node of this graph is labeled using one of an alphabet A of symbols. We call the label of a node the 'state' of that node. As the cellular automaton studied here has 2 states, our alphabet is {0, 1}. 

Much of the past research in CA has been focused on CA played on graphs that can be represented as a grid of tesselating square 'cells', such that for each node in the graph, the corresponding cell's physical neighbors correspond to neighboring nodes in the graph. The cellular automaton studied here is played on a grid where a cell's 'neighbors' are those cells with which it shares an edge or corner. This is denoted as the Moore neighborhood.

The CA studied here is a function taking in, for each node, its label and the label of its neighboring nodes. Based on a rule specified below, this rule returns a single value from our alphabet of symbols. Simulating this CA consists of synchronously labeling each node with the output of the aforementioned function.

Past this point we will be treating this CA as a function operating on a discrete grid of square cells rather than as an undirected graph, as it aids visualization. 

This CA is isotropic; the function returning the next state taken by a cell will not output a different value if a cell and its neighborhood are rotated by a multiple of 90 degrees, or mirrored, or translated. However, it is not 'totalistic': the function's output does not solely rely on the sum of the states of neighboring cells.

The CA is specified by the rule string "B2ci3aery4eqr5aeiq6acn78/S01c2ei3-i4ijrtz5ainry7c8". What a rule string is, and how exactly it specifies a 2-state isotropic non-totalistic CA, will not be discussed here in depth. Further information can be found here { https://conwaylife.com/wiki/Isotropic_non-totalistic_rule }. In broad strokes, the characters between "B" and "/" denote conditions for a state-0 cell to become state-1, and the characters after "S" denote conditions for a state-1 cell to remain state-1.

In the following section, the simultaneous application of the function discussed above to our infinite grid of cells will be referred to as the "rule function", or as "R0( )". When discussing the general concept of representing a CA as a rule function, R( ) will be used.

# what is this? and, JvN

There is one term from the summary statement at the top that I have not discussed, that being "self-replicating universal constructor". A universal constructor in a CA is a pattern which takes some other formulaic information-containing pattern as input, and can produce any pattern as output, so long as the desired output pattern has infinitely many distinct predecessors. (This is likely not true; EXPLAIN). 

This project is a constructive example of a universal constructor run on an input pattern, whose output pattern consists of a copy of the original universal constructor and input pattern, with some translative offset. In addition, when it ceases use of a given universal constructor instance, it uses later-constructed universal constructor instances to "clean up", converting unused instances back into default state-0 cells.

More information on universal constructors in CA, including the oldest constructed example by John von Neumann, can be found here {https://en.wikipedia.org/wiki/Von_Neumann_universal_constructor}

# how is this done? (1)

Much of the broader study of CA, especially ones similar to the one studied here, has been focused on certain types of fixed points and periodic orbits of the rule function. Somewhat whimsically, these fixed points were named "still lifes", "oscillators", and "spaceships" by Conway and other early enthusiasts of his CA: The Game of Life. A still life is a fixed point of the rule function, a pattern of cell states C where R(C) = C. An oscillator is a periodic orbit of the rule function, a pattern of cell states C where R^p(C) = C for some period p. Let T(x,y,C) be a function taking in two integers x, y, and a pattern of cell states C and returning the pattern of cell states C translated by (x,y): then a spaceship is a pattern of cell states C where R^p(C) = T(x,y,C) for some period p and integers x, y. One can view an oscillator as a degenerate case of a spaceship where x, y are zero, and one can view a still life as a degenerate case of an oscillator where p = 1.

Still lifes, oscillators and spaceships are the smallest building blocks of this project, other than individual cells themselves. At this low level, this project is composed of only four parts: (In keeping with CA tradition, these have descriptive nontechnical names).

-discuss run-length encoding-

The "Dot" ("o!")
{img dot}
The simplest fixed point of R0( ). R0(Dot) = Dot.

The "Box" ("4ob$ob2ob$2obob$4ob$4bo!")
{img box}
A periodic orbit of R0( ). R0^4(Box) = Box.

The "Frog" ("bo$2o$bo!")
{img frog}
R0^2(Frog) = T(2,0,Frog)

The "Duck" ("o2b$30$bob!")
{img duck}
R0^4(Duck) = T(1,1,Duck)

Interactions between components are crucially important toward developing higher-order behavior. Below is a list of basic interactions.

"Split"
Consists of a Frog and a Dot. The Dot is static and the Frog translates by one cell after R0^2( ) is applied. This repetitive behavior is disturbed when the state-changes of cells begin to be influenced both by the presence of the Dot and the Frog, as they become closer. Non-periodic behavior occurs for several applications of R0 until periodicity resumes; once it does, we note that a Dot has formed in the exact same spot as the original Dot, while two Frog-s have formed translating in opposite directions, whose directions of translation are perpendicular to that of the first. With correct positioning, this operation can occur for arbitrarily many input Frog-s.

"Eat"
Consists of a Frog and a Box. The Box does not translate, but has a periodic orbit with p = 4 (R0^4(Box) = Box). Both have periodic behavior until they are sufficiently close together, at which point chaotic behavior occurs for (FOUR??) applications of R0, and then resumes: the Box is re-formed, having not been perturbed significantly. Meanwhile, no Frog emerges. This interaction has an important role in universal construction; if we wish to change the orientation of a Frog by 90 degrees without producing another Frog in the opposite direction, we can apply the Split interaction to a Frog, then apply the Eat interaction to one of the two Frog-s resulting from Split.

# intermission: structure
Here I'll discuss how the self-replicating universal constructor is structured.
