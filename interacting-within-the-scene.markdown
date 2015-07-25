---
layout: default
title:  "Virtual interactions"
num: 2

---

We saw that the graphic part of the world is paramount to immersion. Afterall, this is the sense that is most activated in our real world (for better and worse). But while that's true, graphics aren't all. We saw the power to change the direction of your gaze by moving your head alone: natural interactions in this new world strengthen the immersion.

The question of how to interact when having a VR head set doesn't really have a definite answer yet. Let's see what we can already do with a simple bluetooth controller (keyboard, mouse, joystick...). Note that there are lots of alternatives, from the basic Google Cardboard single action to the imperfect but futuristic looking LeapMotion. Some people suggest in fact that ways to interact are so radically shaping the VR experience you should code **for** a way to interact rather than for a (now nearly homogenous) way to display VR worlds. That being said, for the moment, we are simply exploring: your laptop's keyboard and mouse, and simple and cheap Bluetooth controllers will do the trick.

## o) Press the button


The first step is to catch an interaction event, here a specific keypress but you can imagine any other event (a LeapMotion sign, a smartphone tap, etc).

Once we catch a keypress we check which key it is and modify an object in the scene accordingly.

```javascript
function onKey(event) {
  if(event.keyCode == 13) { // Pressed on the space key
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
};
window.addEventListener('keydown', onKey, true);
```

## a) Moving around

You already know how to apply a transformation to an object in the scene. It means you can rotate, scale, change colors and much more. Moving around a scene is exactly the same except that instead of translating an object your are looking, you translate the camera itself!

```javascript
  // Put the code in the onKey function (well, if you want it to work)
  switch(event.keyCode) {
  case 37: camera.position.x -= 10; break; // left
  case 38: camera.position.y += 10; break; // up
  case 39: camera.position.x += 10; break; // right
  case 40: camera.position.y -= 10; break; // down
  case 33: camera.position.z += 10; break; // page Up
  case 34: camera.position.z -= 10; break; // page Down
  }
```  

Voila, instead of change the color or position of an object you translate the camera representing the first person view. In this example you only move in one direction so feel free to extend it to all 4 directions, add a jump (going up another axis then falling down), etc.


Quickly you will notice, especially if you only go left, that you have nothing interesting left to see. It is because without limits, without boundaries you reach the end of your world without stopping. A simple way to avoid this problem is to define boundaries. They can be absolute as in the following example or relative to an object, for example a floor you defined before.

```javascript
// left
  if(keyCode == 37){
    if (camera.position.x < 100) && (camera.position.x > -100){
      camera.position.x -= 10;
    }
  }
```  

// Rez: the above...


## b) Activating Objects
So you can now trigger an action with key presses. You can change your position and the colour of an object. How can you activate an object by moving close to it?
 
```javascript
  range = 10;
  if (abs(mesh.position.x - camera.position.x) < range) && (abs(mesh.position.y - camera.position.y) < range){
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
```  

Note that for the moment you are only checking on 2 dimensions. If you want to check in 3 dimensions you extend the tests to add the missing one. Overall this solution is not optimal so if you want to dwelve deeper in the topic check at collision detection. It's an interesting topic as you have to consider several objects, avoiding detection when objects are obviously too far, etc. This goes well beyong this introduction but at least now you can easilly interact with objects.

If you have in mind a little game, you can also check if the player has in his possession a key. Imagine he goes nearby a door, does his inventory includes the right key? Then turn the door open, it doesn't? Leave it closed.

Creative suggestion : Billy Jean disco floor.

<!--
Rez: white "Cubes" as tiles on the ground with no "boundaries", when you're close to them they take a color

Labyrinth
-->

## c) Moving Objects
So far we played around a very natural way to interact with object, meaning getting close to them. That's nice because it forces us move around, explore the virtual world and it is something that we do all the time. The thing is... virtual reality can be a lot more fun than the real world! Yes, in the computer you can be a magician, you can make table levitate by just looking at them. Heck you can move mountains by looking at them!

How? Well think about the camera, it has a position and a direction. The direction is basically a straight line going from the current position to an infinite target. When you look at an object this straight line intersects with it. This is similar to what we saw before by comparing the position of the camera with the position of an object. The main different is that we only compare the position of the object with the line. Luckily for us ThreeJS has again all the tools in hand using [Raycaster](http://threejs.org/docs/#Reference/Core/Raycaster) and its intersectObjects() method. As it can be a bit tedious still to handle several object we will rely on the [vreticle](https://github.com/neuman/vreticle) library for convenience.

```javascript
  // after setting up the library you can very easilly add gaze possibilities to each object
  mesh.ongazelong = function(){
    this.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
```  

That's it, assuming you included the library, attached the reticle to your camera, added the mesh to the collider list and making sure to check (as suggested in the [doc](https://github.com/neuman/vreticle)) then you can play the magician!

Here are some suggestions : Locking an object, Keeping object in front of you, Releasing it (I would say with no fall => a more Sci Fi kind of look)

Creative suggestion : Lock & Release objects (3D)

Bonus: When released, object move around your body in a circular fashion. When released, the object stay in position, but moves as you do (follow you)
