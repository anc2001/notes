# 7/7/22 Code Update
Shown is the implementation of the language redesign and the new datastructure. The examples shown largely follow the handdrawn diagrams of the last [udpate](../7%3A1%3A22/orientation_update.md). 

All images are 256 x 256
## Reading diagrams
**Note:** Apologies on how small the encoding display is in the visualization 

I've decided to present everything in a horizontal format. Now each column represents a different bin of angular histogram (if 5 columns are present, the leftmost is the full room encoding). Left to right is in increasing value of the angle. Rows represent each different mask. 

For room encodings that display the current objects in the room, the color coding is as follows. 
 * `walls - black`
 * `bed - blue`
 * `cabinet/shelf/desk - red`
 * `light - yellow`
 * `semantic front - green`

# Examples
## Attach wardrobe to wall
Room layout and encoding (thin black line is the wall)

![lorem ipsum](diagrams/attach_wall_1/layout.png)

Wardrobe semantic front pointing in direction `[0,0,1]`

Program 
```
attach(wall) && align(wall)
```

Constraint solve

Possible object orientation = `0`

![lorem ipsum](diagrams/attach_wall_1/solve_0.png)

Possible object orientation = `pi/2`

![lorem ipsum](diagrams/attach_wall_1/solve_1.png)

Possible object orientation = `pi`

![lorem ipsum](diagrams/attach_wall_1/solve_2.png)

Possible object orientation = `3pi/2`

![lorem ipsum](diagrams/attach_wall_1/solve_3.png)

Final mask

![lorem ipsum](diagrams/attach_wall_1/final.png)

Some samples with semantic front shown in green

![lorem ipsum](diagrams/attach_wall_1/1.png)
![lorem ipsum](diagrams/attach_wall_1/2.png)
![lorem ipsum](diagrams/attach_wall_1/3.png)
![lorem ipsum](diagrams/attach_wall_1/4.png)
![lorem ipsum](diagrams/attach_wall_1/5.png)
![lorem ipsum](diagrams/attach_wall_1/6.png)