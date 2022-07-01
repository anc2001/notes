# 7/1/22 - Data Structure Update
[Conventions and definitions](../6%3A30%3A22/orientation.md) are the same, but here are a few more definitions that build upon the previous ones, and a new procedure for evaluation. Shown in this document are the same examples except with the new procedure and definitions 

The only instantiated variables in the language are `masks`. Masks are represented as a 3D array of `shape = (num_angles, dimension, dimension)`. 

Syntax 
 * `mask = constraint operator constraint`
    * Details below
 * `mask = mask operator mask`
    * Simple CSG 
 * `mask = macro(mask)`
    * For methods that involve non CSG operations (ensuring no placement generates collision, ensuring walkability in the room)

 ## Mask generation
`constraint operator constraint`

Each 2D location slice of `shape = (dimension, dimension)` corresponds to a possible orientation of the object. For each possible orientation `a_o`, evaluate the two constraints and the operator to produce a single 2D slice. Evaluation of the constraints involves solving the constraint given the object orientation `a_o` for each possible direction objects face in the room. The masks for each constraint is then combined with the operator and finally unioned together to produce the final output slice. 

**Note:** If we impose another restriction on the syntax of mask generation stating that one constraint must be a location constraint and one must be an orientation constraint and have the operator always be an intersection, then we can do Kai's little optimization trick of not even computing the mask for invalid orientations. 

## Reading Diagrams
### Left side
The left side shows the process for generating the 2D location slice for every possible orientation of the object. Each row labelled with angle `a_r` corresponds with which faces of objects in the room have normals at an angle `a_r` from `[1, 0]` in the room. 

Column 1 shows the room encoding. Column 2 shows solving for the first constraint, and Column 3 solving for second constraint. Column 4 shows result of applying the operator row wise to each of the 2D slices resulting from the constraint solving. 

### Right side 
The right side shows the generated mask. Each row corresponds to the angle of rotation of the object itself and the 2D slices in the 3D mask. 

# Revisiting previous situations with new method
## Attach wardrobe to wall
### Room and object
Wardrobe with semantic front in direction `[1, 0]`

![lorem ipsum](diagrams/wardrobe_room_setting.png)
### Masks

![lorem ipsum](diagrams/wardrobe_1.png)

![lorem ipsum](diagrams/wardrobe_2.png)

![lorem ipsum](diagrams/wardrobe_3.png)

![lorem ipsum](diagrams/wardrobe_4.png)

### Room and object
Wardrobe with semantic front in direction `[0, -1]`

![lorem ipsum](diagrams/wardrobe_rot_setting.png)
### Masks
![lorem ipsum](diagrams/wardrobe_rot_1.png)

![lorem ipsum](diagrams/wardrobe_rot_2.png)

![lorem ipsum](diagrams/wardrobe_rot_3.png)

![lorem ipsum](diagrams/wardrobe_rot_4.png)
## Place chair at table

## Place nightstand by bed

## Place chair in living room context 
 


