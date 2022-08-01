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

Orders by model super category, and then orders within each super category 

# Constraint Extraction Ordered 
## Bed
### All Constraints 
```
bed attach to bed's bottom side | occurrence: 0.05
bed attach to bed's left side | occurrence: 0.25
bed attach to bed's top side | occurrence: 0.15
bed attach to bed's  side | occurrence: 0.45

bed reachable by arm from bed's top side | occurrence: 0.25
bed reachable by arm from bed's left side | occurrence: 0.45
bed reachable by arm from bed's bottom side | occurrence: 0.05
bed reachable by arm from bed's right side | occurrence: 0.05
bed reachable by arm from bed's  side | occurrence: 0.8

bed aligned with and attached to wall | occurrence: 0.905455443100469

bed aligned with bed | occurrence: 0.1

bed face bed | occurrence: 0.2
```

### Removed
```
bed attach to bed's bottom side | occurrence: 0.05

bed reachable by arm from bed's right side | occurrence: 0.05
bed reachable by arm from bed's bottom side | occurrence: 0.05

bed aligned with bed | occurrence: 0.1

bed face bed | occurrence: 0.2
```

### Final 
```
bed attach to bed's left side | occurrence: 0.25
bed attach to bed's top side | occurrence: 0.15
bed attach to bed's  side | occurrence: 0.45

bed reachable by arm from bed's top side | occurrence: 0.25
bed reachable by arm from bed's left side | occurrence: 0.45
bed reachable by arm from bed's  side | occurrence: 0.8

bed aligned with and attached to wall | occurrence: 0.905455443100469
```

## Wardrobe 
### All Constraints 
```
wardrobe attach to wardrobe's left side | occurrence: 0.33287671232876714
wardrobe attach to wardrobe's right side | occurrence: 0.33287671232876714
wardrobe attach to wardrobe's top side | occurrence: 0.038356164383561646
wardrobe attach to wardrobe's bottom side | occurrence: 0.021917808219178082
wardrobe attach to wardrobe's  side | occurrence: 0.726027397260274

wardrobe attach to bed's bottom side | occurrence: 0.005855161787365178
wardrobe attach to bed's left side | occurrence: 0.04129429892141757
wardrobe attach to bed's right side | occurrence: 0.04375963020030817
wardrobe attach to bed's top side | occurrence: 0.01325115562403698
wardrobe attach to bed's  side | occurrence: 0.10416024653312789

wardrobe reachable by arm from bed's left side | occurrence: 0.23020030816640985
wardrobe reachable by arm from bed's right side | occurrence: 0.22742681047765795
wardrobe reachable by arm from bed's bottom side | occurrence: 0.012326656394453005
wardrobe reachable by arm from bed's top side | occurrence: 0.04930662557781202
wardrobe reachable by arm from bed's  side | occurrence: 0.5192604006163328

wardrobe aligned with and attached to wall | occurrence: 0.9395807644882861

wardrobe aligned with wardrobe | occurrence: 0.6684931506849315

wardrobe aligned with bed | occurrence: 0.028043143297380585

wardrobe face bed | occurrence: 0.048998459167950696
```

### Removed
```
wardrobe attach to wardrobe's top side | occurrence: 0.038356164383561646
wardrobe attach to wardrobe's bottom side | occurrence: 0.021917808219178082

wardrobe attach to bed's bottom side | occurrence: 0.005855161787365178
wardrobe attach to bed's left side | occurrence: 0.04129429892141757
wardrobe attach to bed's right side | occurrence: 0.04375963020030817
wardrobe attach to bed's top side | occurrence: 0.01325115562403698
wardrobe attach to bed's  side | occurrence: 0.10416024653312789

wardrobe reachable by arm from bed's top side | occurrence: 0.04930662557781202
wardrobe reachable by arm from bed's bottom side | occurrence: 0.012326656394453005

wardrobe aligned with bed | occurrence: 0.028043143297380585

wardrobe face bed | occurrence: 0.048998459167950696
```

### Final
```
wardrobe attach to wardrobe's left side | occurrence: 0.33287671232876714
wardrobe attach to wardrobe's right side | occurrence: 0.33287671232876714
wardrobe attach to wardrobe's  side | occurrence: 0.726027397260274

wardrobe reachable by arm from bed's left side | occurrence: 0.23020030816640985
wardrobe reachable by arm from bed's right side | occurrence: 0.22742681047765795
wardrobe reachable by arm from bed's  side | occurrence: 0.5192604006163328

wardrobe aligned with and attached to wall | occurrence: 0.9395807644882861

wardrobe aligned with wardrobe | occurrence: 0.6684931506849315
```

## Nighstand 
### All Constraints 
```
nightstand attach to bed's bottom side | occurrence: 0.06755280407865986
nightstand attach to bed's right side | occurrence: 0.29916241806263655
nightstand attach to bed's left side | occurrence: 0.29806991988346687
nightstand attach to bed's top side | occurrence: 0.0049162418062636565
nightstand attach to bed's  side | occurrence: 0.669701383831027

nightstand attach to wardrobe's top side | occurrence: 0.07155756207674943
nightstand attach to wardrobe's right side | occurrence: 0.023250564334085778
nightstand attach to wardrobe's left side | occurrence: 0.020993227990970656
nightstand attach to wardrobe's bottom side | occurrence: 0.002708803611738149
nightstand attach to wardrobe's  side | occurrence: 0.11851015801354402

nightstand attach to nightstand's right side | occurrence: 0.0006613756613756613
nightstand attach to nightstand's left side | occurrence: 0.0008818342151675485
nightstand attach to nightstand's top side | occurrence: 0.0002204585537918871
nightstand attach to nightstand's  side | occurrence: 0.001763668430335097

nightstand reachable by arm from bed's bottom side | occurrence: 0.06973780043699927
nightstand reachable by arm from bed's right side | occurrence: 0.45721048798252
nightstand reachable by arm from bed's left side | occurrence: 0.46285506190823017
nightstand reachable by arm from bed's top side | occurrence: 0.0065549890750182084
nightstand reachable by arm from bed's  side | occurrence: 0.9963583394027676

nightstand aligned with and attached to wall | occurrence: 0.6955729641100382

nightstand aligned with bed | occurrence: 0.9388201019664967

nightstand aligned with wardrobe | occurrence: 0.005191873589164785

nightstand aligned with nightstand | occurrence: 0.0013227513227513227

nightstand face bed | occurrence: 0.014020393299344501

nightstand face wardrobe | occurrence: 0.0013544018058690745
```

### Removed
```
nightstand attach to bed's top side | occurrence: 0.0049162418062636565
nightstand attach to bed's bottom side | occurrence: 0.06755280407865986

nightstand attach to wardrobe's top side | occurrence: 0.07155756207674943
nightstand attach to wardrobe's right side | occurrence: 0.023250564334085778
nightstand attach to wardrobe's left side | occurrence: 0.020993227990970656
nightstand attach to wardrobe's bottom side | occurrence: 0.002708803611738149
nightstand attach to wardrobe's  side | occurrence: 0.11851015801354402

nightstand attach to nightstand's right side | occurrence: 0.0006613756613756613
nightstand attach to nightstand's left side | occurrence: 0.0008818342151675485
nightstand attach to nightstand's top side | occurrence: 0.0002204585537918871
nightstand attach to nightstand's  side | occurrence: 0.001763668430335097

nightstand reachable by arm from bed's top side | occurrence: 0.0065549890750182084
nightstand reachable by arm from bed's bottom side | occurrence: 0.06973780043699927

nightstand aligned with wardrobe | occurrence: 0.005191873589164785

nightstand aligned with nightstand | occurrence: 0.0013227513227513227

nightstand face bed | occurrence: 0.014020393299344501

nightstand face wardrobe | occurrence: 0.0013544018058690745
```

### Final
```
nightstand attach to bed's right side | occurrence: 0.29916241806263655
nightstand attach to bed's left side | occurrence: 0.29806991988346687
nightstand attach to bed's  side | occurrence: 0.669701383831027

nightstand reachable by arm from bed's right side | occurrence: 0.45721048798252
nightstand reachable by arm from bed's left side | occurrence: 0.46285506190823017
nightstand reachable by arm from bed's  side | occurrence: 0.9963583394027676

nightstand aligned with and attached to wall | occurrence: 0.6955729641100382

nightstand aligned with bed | occurrence: 0.9388201019664967
```

## Desk
### All Constraints 
```
desk attach to wardrobe's left side | occurrence: 0.042735042735042736
desk attach to wardrobe's right side | occurrence: 0.05982905982905983
desk attach to wardrobe's top side | occurrence: 0.05555555555555555
desk attach to wardrobe's bottom side | occurrence: 0.008547008547008548
desk attach to wardrobe's  side | occurrence: 0.16666666666666666

desk attach to bed's bottom side | occurrence: 0.10778443113772455
desk attach to bed's top side | occurrence: 0.11676646706586827
desk attach to bed's left side | occurrence: 0.15269461077844312
desk attach to bed's right side | occurrence: 0.1377245508982036
desk attach to bed's  side | occurrence: 0.5149700598802395

desk attach to nightstand's left side | occurrence: 0.021505376344086023
desk attach to nightstand's bottom side | occurrence: 0.014336917562724014
desk attach to nightstand's right side | occurrence: 0.03225806451612903
desk attach to nightstand's top side | occurrence: 0.007168458781362007
desk attach to nightstand's  side | occurrence: 0.07526881720430108

desk attach to desk's right side | occurrence: 0.2
desk attach to desk's top side | occurrence: 0.3
desk attach to desk's left side | occurrence: 0.1
desk attach to desk's  side | occurrence: 0.6

desk reachable by arm from bed's bottom side | occurrence: 0.10778443113772455
desk reachable by arm from bed's top side | occurrence: 0.2245508982035928
desk reachable by arm from bed's left side | occurrence: 0.20359281437125748
desk reachable by arm from bed's right side | occurrence: 0.20359281437125748
desk reachable by arm from bed's  side | occurrence: 0.7395209580838323

desk aligned with and attached to wall | occurrence: 0.9397590361445783

desk aligned with wardrobe | occurrence: 0.042735042735042736

desk aligned with bed | occurrence: 0.38622754491017963

desk aligned with nightstand | occurrence: 0.06093189964157706

desk face bed | occurrence: 0.18862275449101795

desk face nightstand | occurrence: 0.010752688172043012

desk face desk | occurrence: 0.4

desk face wardrobe | occurrence: 0.03418803418803419
```

### Removed
```
desk attach to wardrobe's left side | occurrence: 0.042735042735042736
desk attach to wardrobe's right side | occurrence: 0.05982905982905983
desk attach to wardrobe's top side | occurrence: 0.05555555555555555
desk attach to wardrobe's bottom side | occurrence: 0.008547008547008548
desk attach to wardrobe's  side | occurrence: 0.16666666666666666

desk attach to nightstand's left side | occurrence: 0.021505376344086023
desk attach to nightstand's bottom side | occurrence: 0.014336917562724014
desk attach to nightstand's right side | occurrence: 0.03225806451612903
desk attach to nightstand's top side | occurrence: 0.007168458781362007
desk attach to nightstand's  side | occurrence: 0.07526881720430108

desk attach to desk's left side | occurrence: 0.1

desk reachable by arm from bed's bottom side | occurrence: 0.10778443113772455

desk aligned with wardrobe | occurrence: 0.042735042735042736

desk aligned with bed | occurrence: 0.38622754491017963

desk aligned with nightstand | occurrence: 0.06093189964157706

desk face bed | occurrence: 0.18862275449101795

desk face nightstand | occurrence: 0.010752688172043012

desk face wardrobe | occurrence: 0.03418803418803419
```

### Final
```
desk attach to bed's bottom side | occurrence: 0.10778443113772455
desk attach to bed's top side | occurrence: 0.11676646706586827
desk attach to bed's left side | occurrence: 0.15269461077844312
desk attach to bed's right side | occurrence: 0.1377245508982036
desk attach to bed's  side | occurrence: 0.5149700598802395

desk attach to desk's right side | occurrence: 0.2
desk attach to desk's top side | occurrence: 0.3
desk attach to desk's  side | occurrence: 0.6

desk reachable by arm from bed's top side | occurrence: 0.2245508982035928
desk reachable by arm from bed's left side | occurrence: 0.20359281437125748
desk reachable by arm from bed's right side | occurrence: 0.20359281437125748
desk reachable by arm from bed's  side | occurrence: 0.7395209580838323

desk aligned with and attached to wall | occurrence: 0.9397590361445783

desk face desk | occurrence: 0.4
```

## Chair 
### All Constraints 
```
chair attach to desk's top side | occurrence: 0.5253456221198156
chair attach to desk's left side | occurrence: 0.03686635944700461
chair attach to desk's bottom side | occurrence: 0.07373271889400922
chair attach to desk's right side | occurrence: 0.013824884792626729
chair attach to desk's  side | occurrence: 0.6497695852534562

chair attach to bed's left side | occurrence: 0.05752961082910321
chair attach to bed's right side | occurrence: 0.06429780033840947
chair attach to bed's top side | occurrence: 0.05922165820642978
chair attach to bed's bottom side | occurrence: 0.00676818950930626
chair attach to bed's  side | occurrence: 0.18781725888324874

chair attach to wardrobe's right side | occurrence: 0.006787330316742082
chair attach to wardrobe's top side | occurrence: 0.038461538461538464
chair attach to wardrobe's bottom side | occurrence: 0.00904977375565611
chair attach to wardrobe's left side | occurrence: 0.0022624434389140274
chair attach to wardrobe's  side | occurrence: 0.05656108597285068

chair attach to nightstand's right side | occurrence: 0.010769230769230769
chair attach to nightstand's left side | occurrence: 0.003076923076923077
chair attach to nightstand's bottom side | occurrence: 0.004615384615384616
chair attach to nightstand's top side | occurrence: 0.006153846153846154
chair attach to nightstand's  side | occurrence: 0.024615384615384615

chair attach to chair's left side | occurrence: 0.016129032258064516
chair attach to chair's top side | occurrence: 0.016129032258064516
chair attach to chair's  side | occurrence: 0.03225806451612903

chair reachable by arm from bed's right side | occurrence: 0.25888324873096447
chair reachable by arm from bed's bottom side | occurrence: 0.011844331641285956
chair reachable by arm from bed's top side | occurrence: 0.23181049069373943
chair reachable by arm from bed's left side | occurrence: 0.233502538071066
chair reachable by arm from bed's  side | occurrence: 0.7360406091370558

chair reachable by arm from chair's left side | occurrence: 0.12903225806451613
chair reachable by arm from chair's top side | occurrence: 0.12903225806451613
chair reachable by arm from chair's right side | occurrence: 0.11290322580645161
chair reachable by arm from chair's bottom side | occurrence: 0.08064516129032258
chair reachable by arm from chair's  side | occurrence: 0.45161290322580644

chair aligned with and attached to wall | occurrence: 0.1606837606837607

chair aligned with bed | occurrence: 0.25042301184433163

chair aligned with chair | occurrence: 0.16129032258064516

chair aligned with nightstand | occurrence: 0.009230769230769232

chair aligned with desk | occurrence: 0.0967741935483871

chair face bed | occurrence: 0.25042301184433163

chair face desk | occurrence: 0.4976958525345622

chair face wardrobe | occurrence: 0.03619909502262444

chair face nightstand | occurrence: 0.007692307692307693

chair face chair | occurrence: 0.03225806451612903
```

### Removed
```
chair attach to desk's right side | occurrence: 0.013824884792626729
chair attach to desk's left side | occurrence: 0.03686635944700461
chair attach to desk's bottom side | occurrence: 0.07373271889400922

chair attach to bed's left side | occurrence: 0.05752961082910321
chair attach to bed's right side | occurrence: 0.06429780033840947
chair attach to bed's top side | occurrence: 0.05922165820642978
chair attach to bed's bottom side | occurrence: 0.00676818950930626
chair attach to bed's  side | occurrence: 0.18781725888324874

chair attach to wardrobe's right side | occurrence: 0.006787330316742082
chair attach to wardrobe's top side | occurrence: 0.038461538461538464
chair attach to wardrobe's bottom side | occurrence: 0.00904977375565611
chair attach to wardrobe's left side | occurrence: 0.0022624434389140274
chair attach to wardrobe's  side | occurrence: 0.05656108597285068

chair attach to nightstand's right side | occurrence: 0.010769230769230769
chair attach to nightstand's left side | occurrence: 0.003076923076923077
chair attach to nightstand's bottom side | occurrence: 0.004615384615384616
chair attach to nightstand's top side | occurrence: 0.006153846153846154
chair attach to nightstand's  side | occurrence: 0.024615384615384615

chair attach to chair's left side | occurrence: 0.016129032258064516
chair attach to chair's top side | occurrence: 0.016129032258064516
chair attach to chair's  side | occurrence: 0.03225806451612903

chair reachable by arm from bed's bottom side | occurrence: 0.011844331641285956

chair reachable by arm from chair's bottom side | occurrence: 0.08064516129032258

chair aligned with and attached to wall | occurrence: 0.1606837606837607

chair aligned with bed | occurrence: 0.25042301184433163

chair aligned with chair | occurrence: 0.16129032258064516

chair aligned with nightstand | occurrence: 0.009230769230769232

chair aligned with desk | occurrence: 0.0967741935483871

chair face bed | occurrence: 0.25042301184433163

chair face wardrobe | occurrence: 0.03619909502262444

chair face nightstand | occurrence: 0.007692307692307693

chair face chair | occurrence: 0.03225806451612903
```

### Final
```
chair attach to desk's top side | occurrence: 0.5253456221198156
chair attach to desk's  side | occurrence: 0.6497695852534562

chair reachable by arm from bed's right side | occurrence: 0.25888324873096447
chair reachable by arm from bed's top side | occurrence: 0.23181049069373943
chair reachable by arm from bed's left side | occurrence: 0.233502538071066
chair reachable by arm from bed's  side | occurrence: 0.7360406091370558

chair reachable by arm from chair's left side | occurrence: 0.12903225806451613
chair reachable by arm from chair's top side | occurrence: 0.12903225806451613
chair reachable by arm from chair's right side | occurrence: 0.11290322580645161
chair reachable by arm from chair's  side | occurrence: 0.45161290322580644

chair face desk | occurrence: 0.4976958525345622
```

