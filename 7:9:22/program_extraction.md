# Program Exraction 
## Available syntax and constraints
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

## Naive procedure for bedrooms
### Object categories and their semantic fronts 
Every possible super category (lighting not included) and the numer of semantic fronts each of them will be assigned 
 * `'sofa'` : 1, long side
 * `'table'` : 4
 * `'pier/stool'` : 4
 * `'cabinet/shelf/desk'` : elaborated below 
 * `'chair'` : 1, any side
  * `'bed'` : 1, short side

The super category `'cabinet/shelf/desk'` is a little too broad to definitively say how many semantic fronts the object has, so each possible category is labelled. 
 * `'nightstand'` : 1, any side
 * `'drawer chest/corner cabinet'` : 1
 * `'tv stand'` : 2, both long sides
 * `'children cabinet'` : 1, long side 
 * `'round end table'` : 4
 * `'coffee table'` : 4 
 * `'bookcase/jewelry armoire'` : 1, long side 
    * I have never heard of a jewelry armoire lol 
 * `'sideboard/side cabinet/console table'` : 1, long side
    * Kind of unsure about this one 
 * `'wardrobe'` : 1
 * `'corner/side table'` : 2, two sides next to each other 
    * Really there needs to be 8 possible angles to properly label this 
 * `'shelf'` : 2, both long sides 

Objects that hold humans 
 * `{'sofa', 'pier/stool', 'chair', 'bed'}`

Objects that hold objects 
 * `{'table', 'cabinet/shelf/desk'}`

### Naive Procedure 

## Network Architecture 
