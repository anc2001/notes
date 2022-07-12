# Program Exraction
## Slack Review 
**Abbreivated Version of Daniel's Original Post**
Rather than devising a procedure up-front for extracting programs from scenes to create the ‘optimal’ program for each object insertion task (i.e. not overly specific, not overly general), we should (a) write a naive procedure for extracting programs from scenes (For the object to be inserted, create the program which consists of all possible constraints which could apply) and (b) try training a neural network to generate these programs. It seems premature to do any kind of pattern analysis to pre-process the training data before seeing how a network behaves when trained on naive programs. 

If designed appropriately, the network should be able to generalize somewhat and produce programs which are more general than the training data. From Kai’s prior work, we know that networks can do some of this kind of generalization if they’re sufficiently regularized. So it will be interesting to see what sort of generalization behavior we observe when we switch from trying to predict a continuous distribution of possible placement locations to predicting a discrete program. Based on what behavior we see here, we may then decide we need to do some further pattern analysis as a pre-process for the training data. 

### Neural Network 
**Daniel:** I think it makes sense to to treat this as a sequence-to-sequence (or rather, a ‘set to set’) translation problem: we have a transformer encoder that computes context-aware representations of each object in the scene (where objects include wall segments, doors, etc.), and then a decoder that produces the set of constraints. We might get some inspiration from this recent work, which also learns to generate sets of constraints that describe layouts: https://arxiv.org/pdf/2011.13417.pdf

**Kai:** If I understand it correctly: the main idea we want to take from this paper is the "generating constraints" parts and, to a lesser degree, the idea that we can leave some of the work to an optimization process. For pointer networks: if you know what attention is, think about it as attention, but instead of using attention to generate embeddings, you use the dot product attention directly to figure out some discrete problems e.g. connecting edges. But connecting edges isn't necessarily the only thing that can be done here - figuring out the anchors for our relation formulation is also one way it can be used, i think.

**Daniel:** The main thing I was hoping you’d take away from this paper is “some neural network architecture ideas for how to generate a set of elements which have references to other elements.” Our programs consist of constraint/functions, and those functions have arguments that refer to objects in the current partial scene. So, you could imagine using a transformer to generate the set of functions, and then using a pointer network to generate the arguments (i.e. figuring out which objects in the partial scene each argument should point to)

### Refining the 'ground truth' programs 
**Daniel:** I had an idea for trying to find ‘the right’ program for an object insertion task, given multiple candidate programs (e.g. trying to figure out which constraints should be included out of all possible constraints, or even trying to figure out which sample from a generative model of programs is the best). We could take the program, generate a bunch of object placements according to it, and then try to assess how well those samples match what’s in our dataset. Ideally we’d pick the program such that (a) all of its samples look like they come from the same distribution as the dataset, and (b) the samples have high internal diversity (i.e. we’re not just generating the same placement over and over again). (a) and (b) can be in conflict, so there’s some choice of hyperparameter to trade off how much we care about one versus the other.

## Designing the Naive Program 
For now, only deal with the object categories 
 * `wardrobe`
 * `bed`
 * `nightstand`
 * `desk`
 * `chair`

### Available syntax and constraints
Number of bins for angular domain = 4

Syntax
 * `mask = location_constraint operator orientation_constraint`
 * `mask = mask operator mask`

Implemented location constraints 
 * `attach(Object, Directions)`
 * `reachable_by_arm(Object, Directions)`

Implemented orientation constraints 
 * `align(Object)`
 * `face(Object)`

### Naive Procedure 
For every object, remove it from the room and choose its program based on its original position in the room. 

Choose which objects to include in constraint arguments by looking in the general vicinity of the object (walls included)

Location constraints - for each object choose between `attach` and `reachable_by_arm`
 * If the object seems to be very close choose `attach`, otherwise choose `reachable_by_arm`

Orientation constraints - for each object choose between `align` and `face`
 * Do this by looking at the object categories of the object currently in the room and the object to place. 

Case of no valid constraints -> just specify any direction and orientation 

Resampling of the room -> just arbitrarily define an order? 
x
## Network Architecture 
