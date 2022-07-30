# Constraint Generation 

![temp](diagrams/neural_network.png)

## Object Representation and Object Conversion Model 
Objects are represented by their bounding box as a sequence of tokens `(category, holds_humans, size, position, rotation)`. The Object Conversion Model should use learnable (or non learnable) embeddings for each of the tokens to create per object context vectors. Each of the token embeddings will be concatenated together to generate object context embeddings. 

Notes/Questions about getting the per object context embeddings 
 * The `Structure Encoder` in [ATISS](https://nv-tlabs.github.io/ATISS/) has a positional encoding function for continuous values (orientation, size, and position). In [SceneFormer](https://arxiv.org/abs/2012.09793) these values are discretized and correspond with learnable embeddings. 
 * **(For Later)** Not sure how to represent walls since size, position, and rotation are not really applicable -> perhaps a layout encoder to get a vector representation? [SceneFormer](https://arxiv.org/abs/2012.09793) does mention that walls are not included as objects because walls are assumed to be at the edges of the layout image 

## Constraint Sequence and Argument Selection Models 
Constraints are represented as tuples `(constraint type, object index, direction)`. These constraints are generated autoregressively with transformer decoders using cross attention from the encoder above. Each of these tokens are generated until the `stop` token is reached (only the constraint model can output a `stop` token). The input to these models takes the form of multiple input sequences where each column represents a single constraint and each row represents a constraint type. Start and stop tokens are added to the beginning and end. 

For example the list 
```
(attach, bed, right) (attach, bed, left) (align, bed, e)
```

Would take the form 
```
start attach attach align stop
start bed bed bed stop
start right left e stop 
```

 Both the Constraint Selection Model and the Direction Selection Model will have a final linear layer of `N` logits where `N` is the number of valid tokens for that type, since the number of valid tokens is fixed in both cases. In the case of orientation constraints, the direction selection model is skipped and the special token `e` (empty) is added to the final sequence. The Object Selection Model outputs a pointer embedding which will be compared to all of the valid context aware object embeddings with a dot product and softmax to choose the object index for that constraint. Both constraint and direction tokens use learnable embeddings. The object selection tokens use the context aware object embeddings. 

The sequence representation follows the same format as [SceneFormer](https://arxiv.org/abs/2012.09793) where there are multiple input sequences to the model corresponding to each of the token types being generated, and the corresponding embeddings for each sequence is added together. After each token generation, the corresponding sequence of that token type is shifted to the left so that the next model can condition on that token. For example, after the generation of token `C_2`, the constraint sequence is shifted to the left for the object selection model. 

I also considered just a single input sequence where each token generated is concatenated to the same sequence. 

**Constraint sequence representation**
1. Single input sequence 
```
start attach, bed, right attach, bed, left align, bed, e top 
```
During training tokens generated are just concatenated 

2. SceneFormer style sequence generation described above 

3. Single embedding per constraint, but instead of shifting each input sequence to the left to condition on the current constraint, just have empty tokens during training for unfilled arguments 

## Constraint Sequence Ordering 
The list of constraints will be ordered `attach`, `reachable_by_arm`, `align`, and `face`. For constraints of the same type they will be ordered by object according to (a) the canonical ordering according to average size and frequency in the dataset (b) the size of the object (biggest to largest) if two objects of the same category exist for that constraint type. For location constraints of both the same type and object, constraints are ordered by direction going `right`, `top`, `left`, `bottom`. 

For toy dataset, ordering follows: `bed -> wardrobe -> nightstand -> desk -> chair`


