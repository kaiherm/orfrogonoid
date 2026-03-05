# WARNING!!

This is not a particularly good explanation right now. It lacks visual aids. You can still scroll down to the final section of this write-up and do what it says, if you want to watch its operation or puzzle it out. I do recommend running it in Golly, though; nothing here is a heavy download.

I am also self taught in all of this. The fact that my project was well conceived and succeeded has no bearing on the fact that I at times do not know what I am talking about. Sorry in advance.

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

There is one term from the summary statement at the top that I have not discussed, that being "self-replicating universal constructor". A universal constructor in a CA is a pattern which takes some other formulaic information-containing pattern as input, and can produce any pattern as output, so long as the desired output pattern has infinitely many distinct predecessors. (This is so painfully close to true that the cases where it may not be are far beyond the scope of this project). 

This project is a constructive example of a universal constructor run on an input pattern, whose output pattern consists of a copy of the original universal constructor and input pattern, with some translative offset. In addition, when it ceases use of a given universal constructor instance, it uses later-constructed universal constructor instances to "clean up", converting unused instances back into default state-0 cells.

More information on universal constructors in CA, including the oldest constructed example by John von Neumann, can be found here: https://en.wikipedia.org/wiki/Von_Neumann_universal_constructor

# how is this done? (1)

Much of the broader study of CA, especially ones similar to the one studied here, has been focused on certain types of fixed points and periodic orbits of the rule function. Somewhat whimsically, these fixed points were named "still lifes", "oscillators", and "spaceships" by Conway and other early enthusiasts of his CA: The Game of Life. A still life is a fixed point of the rule function, a pattern of cell states C where R(C) = C. An oscillator is a periodic orbit of the rule function, a pattern of cell states C where R^p(C) = C for some period p. Let T(x,y,C) be a function taking in two integers x, y, and a pattern of cell states C and returning the pattern of cell states C translated by (x,y): then a spaceship is a pattern of cell states C where R^p(C) = T(x,y,C) for some period p and integers x, y. One can view an oscillator as a degenerate case of a spaceship where x, y are zero, and one can view a still life as a degenerate case of an oscillator where p = 1.

Still lifes, oscillators and spaceships are the smallest building blocks of this project, other than individual cells themselves. At this low level, this project is composed of only four parts: (In keeping with CA tradition, these have descriptive nontechnical names).

Demonstrations of this construction will use Golly. You may view behavior of the components of this construction below by copying-and-pasting the text in quotation marks into Golly, and setting the rule to "B2ci3aery4eqr5aeiq6acn78/S01c2ei3-i4ijrtz5ainry7c8". The text in quotation marks is a run-length-encoded description of the component.

The Dot ("o!")
IMAGE TBA {img dot}
The simplest fixed point of R0( ). R0(Dot) = Dot.

The Box ("4ob$ob2ob$2obob$4ob$4bo!")
IMAGE TBA {img all phases of box}
A periodic orbit of R0( ). R0^4(Box) = Box.

The Frog ("bo$2o$bo!")
IMAGE TBA {img all phases of frog}
R0^2(Frog) = T(2,0,Frog)

The Duck ("o2b$30$bob!")
IMAGE TBA {img all phases of duck}
R0^4(Duck) = T(1,1,Duck)

Interactions between components are crucially important toward developing higher-order behavior. Below is a list of basic interactions.

"Split"
Consists of a Frog and a Dot. The Dot is static and the Frog translates by one cell after R0^2( ) is applied. This repetitive behavior is disturbed when the state-changes of cells begin to be influenced both by the presence of the Dot and the Frog, as they become closer. Non-periodic behavior occurs for several applications of R0 until periodicity resumes; once it does, we note that a Dot has formed in the exact same spot as the original Dot, while two Frog-s have formed translating in opposite directions, whose directions of translation are perpendicular to that of the first. With correct positioning, this operation can occur for arbitrarily many input Frog-s.

"Eat"
Consists of a Frog and a Box. The Box does not translate, but has a periodic orbit with p = 4 (R0^4(Box) = Box). Both have periodic behavior until they are sufficiently close together, at which point chaotic behavior occurs for (FOUR??) applications of R0, and then resumes: the Box is re-formed, having not been perturbed significantly. Meanwhile, no Frog emerges. This interaction has an important role in universal construction; if we wish to change the orientation of a Frog by 90 degrees without producing another Frog in the opposite direction, we can apply the Split interaction to a Frog, then apply the Eat interaction to one of the two Frog-s resulting from Split.

IMAGES TBA: {all pertinent phases of above interactions}

# intermission: structure
Here I'll discuss how the self-replicating universal constructor is structured.

This is composed of three components: two "constructors", and one "tape". Each constructor is composed of three pieces: an "arm", an "elbow" and a "hand". The tape is a very long (on the order of 38,500 cells) line of Frog-s, each translating in the same direction parallel to this line.

IMAGES TBA: {annotated constructor} {tape "the entire tape"} {tape portion "zoomed-in segment of the tape"}

Operation proceeds as follows. The tape is placed so that, when its constituent Frog-s approach the constructor, they undergo a Split reaction with the lower-most Dot in the constructor. Sequentially as the tape moves to the left, each Frog in the tape undergoes a Split with the lower-most Dot in the constructor. For each Frog, this results in one Frog translating upwards, toward another Dot in the constructor, and one translating downwards, toward a Box. This Box is placed so that it and the downwards-translating Frog undergo an Eat reaction. Meanwhile, the upwards-translating Frog undergoes another Split reaction with the middle Dot in the constructor. This results in one Frog translating left, toward the elbow Dot, and another traveling right, toward the second constructor, which is placed very far away.

The net effect of the tape's reaction with the constructor is as follows: the tape is duplicated, with one copy translating in the opposite direction of the input tape, and another colliding with the hand Dot. The tape's reactions with the elbow (and, tangentially, the hand Dot) are the most complex piece of the machine's operation, and will be discussed in the next section.

# how is this done? (2)
One duplicate tape will react with the elbow Dot. The astute may notice that this Dot is not placed in such a way to result in a Split operation. In order to gain a useful understanding of the tape's interactions with the elbow, we must view the tape at a higher, more cohesive level. Consider this first: were the Frog-s composing the tape spaced evenly, the tape would carry no information, and we would likely not be able to construct anything. The information in the tape is thus contained in the choice of spacings between subsequent Frog-s.

At a higher level, the tape can be viewed as a sequence of codons. Each of these codons is a sequence of Frog-s with prescribed spacings; some codons are longer or shorter than others. The smallest codon contains 1 Frog, and the largest contains 24. This construction uses 7 codons, labeled (+, -, Z, Y, X, /, and S). Each of these codons, when it collides with the elbow, causes the elbow to be transformed in one of seven important ways:

When the - codon interacts with the elbow, the elbow is duplicated; one copy (recall, just a Dot) appears translated two cells away from the direction of the onncoming Frog-s, and the other copy appears translated thirteen cells toward the oncoming Frog-s. Note that this means the elbow will not always be composed of a single Dot - sometimes, it will be composed of several Dot-s in a line. This only functions if the two closest Dot-s in the elbow are more than four cells apart, or if there is only one Dot in the elbow.

When the + codon interacts with the elbow, the closest Dot in the elbow reappears translated two cells away from the direction of the oncoming Frog-s. This only functions if the two closest Dot-s in the elbow are more than four cells apart, or if there is only one Dot in the elbow.

When the Z codon interacts with the elbow, the closest Dot in the elbow is converted to a state-0 cell (destroyed). This only functions if the two closest Dot-s in the elbow are more than four cells apart, or if there is only one Dot in the elbow.

When the Y codon interacts with the elbow, the two closest Dot-s in the elbow are converted to state-0 cells. This only functions if the two closest Dot-s in the elbow are exactly four cells apart.

When the X codon interacts with the elbow, the two closest Dot-s in the elbow are converted to state-0 cells. This only functions if the two closest Dot-s in the elbow are exactly three cells apart.

When the / codon interacts with the elbow, the closest Dot in the elbow reappears translated two cells away from the direction of the oncoming Frog-s. In addition to this, a Duck appears as a result of the reaction; its translation is toward the hand Dot. This only functions if the two closest Dot-s in the elbow are more than four cells apart, or if there is only one Dot in the elbow.

When the S codon interacts with the elbow, the closest Dot in the elbow reappears translated six cells toward the oncoming Frog-s. Note that the elbow's position is 'to one side' of the duplicated tape: this interaction also causes the elbow to appear on the opposite side. This is only used once in the tape, and is used only when there is one Dot in the elbow.

IMAGES TBA: {+,-,Z,Y,X,/,S codons}

Finding these codons was, to put it bluntly, damn near impossible. I am not an experienced nor an especially skilled programmer, and I was much less skilled when I began searching for codons to accomplish what I wanted to accomplish. My first and only search program took in five parameters: minimum and maximum spacing between subsequent Frog-s, number of Frog-s in the codon to be found, starting configuration and ending configuration. The program searched 

The elbow can, if the sequence of codons is not chosen carefully, enter undefined behavior, and become unusable. Avoiding this is difficult and must be considered in constructing the tape; the logic underlying these choices can be seen in programs/pathfind.py (TBA. I have to rewrite it for this demo. because my last computer was destroyed.)

We have demonstrated the tape's interactions with the construction elbow. How do these interactions serve the stated goal of constructing a new constructor? Recall the / codon. This causes a Duck to be produced, which translates toward the construction hand. Near the start of this project, after designing the construction arm, I designed a pattern consisting of 83 Duck-s which, when positioned correctly relative to a Dot, would build a copy of the constructor. Crucially, this happens asynchronously; the timing between interactions with subsequent Duck-s can be arbitrarily large, and the new constructor will successfully be built. We have codons (+ and -) that cause the topmost Dot in the elbow to be translated by 2 cells away and 13 cells closer to the duplicated tape, respectively. As these shifts are relatively prime, we can acheive any position for the closest Dot in the elbow, and thus, by then appending the / codon, we can acheive any Duck position relative to the construction hand.

Finally. I can give you a top-level description of what this machine does: A tape (made of codons, which are in turn made of aligned Frog-s) travels toward a constructor. This tape is turned 90 degrees and then duplicated. One duplicate travels back in the direction of the original tape, with some translative offset. The other duplicate interacts with a construction elbow, to produce a total of 83 Duck-s. One after another, these Duck-s interact with a construction hand so as to build a copy of the original constructor, at a 32-cell translative offset. The elbow is then reduced to one Dot using X, Y, and Z codons. Then, an S codon interacts with the elbow, allowing Duck-s to be produced which translate in a different direction. Seven additional Duck-s are produced, which interact with previous copies of the constructor in such a way that it is converted to state-0 cells (destroyed). Seven Z codons convert any remaining Dot-s in the elbow into state-0 cells. The tape ends and the constructor processes no more information. It will wait 32,000 applications of R0 until it is destroyed by the constructor that it built. The net effect? A massive spaceship, dubbed "Orfrogonoid" for reasons beyond the scope of this write-up, consisting of its own special kind of DNA and cellular machinery, assembling to a carbon copy of itself over the course of 69,704 rule applications, and being erased by its child. Let us call this spaceship S: then R0^69704(S) = T(0,32,S).

Thank you for reading.

# if you want to see Orfrogonoid in action
Download Golly at https://golly.sourceforge.io/ , then:

download constructions/orfrogonoid.mc and open with Golly

copy and paste constructions/orfrogonoid.txt into Golly

Or, watch the Orfrogonoid's replication cycle on Youtube at LINK TBA
