# Coding Challenge

## Summary

The coding challenge is to create a single/one page website that includes a THREE.JS scene with a few 3D objects, some form input elements to manipulate the scene, and output text (see below for more details)

## Setup

-  Set up a new repository using either Vue 2 or Vue 3 
-  Include THREE.JS as a dependency


## Webpage layout

- The ThreeJS scene inside a div at the top of the page 
- The form input fields, and the output text underneath


## Milestone 1: 3D Objects

Have a ThreeJS Scene at the top of the page featuring these objects: 

1. Rectangular Cuboid (black)
2. Line Segment (green)
3. Sphere (blue)

Try to make these 3D objects’ dimensions/positioning/colors roughly match the illustration above. The styling of the rest of the page is up to you.
Please add some opacity/transparency to all objects so they are somewhat visible even if they are behind each other

## Milestone 2: Inputs
- Allow the User to move the Cuboid by typing in the X,Y,Z coordinates that represent the center of the Cuboid. So when any of these 3 values are edited, the whole Cuboid relocates in the 3D scene
- Allow the User to move the Line Segment by typing in the Start and End point locations of the Line Segment (Start X,Y,Z and End X,Y,Z). Again, editing any of these fields should move the line in the 3D scene

## Milestone 3: Output
- Determine if there is at least one intersection point between the Line Segment and any of the 6 faces of the Cuboid
- If not, hide the Sphere
- If there is at least one intersection, pick the intersection closest to the center of the Cuboid, and move the Sphere over the intersection point (and show it) – The center of the Sphere should be at the intersection point
- Show the X/Y/Z coordinates of the intersection (the location of the Sphere) in the Output text when there is an intersection
