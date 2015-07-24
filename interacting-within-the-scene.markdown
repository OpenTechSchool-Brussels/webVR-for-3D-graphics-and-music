---
layout: default
title:  "Virtual interactions"
num: 2

---

The question of how to interact when having a VR head set doesn't really have a definite answer yet. Let's see what we can already do with a simple bluetooth controller seen as a keyboard. Note that tere are lots of alternatives, from the basic Google Cardboard single action to the imperfect but futuristic looking LeapMotion. Some people suggest in fact that ways to interact are so radically shapping the VR experience you should code FOR a way to interact rather than a now nearly homogenous way to display VR worlds. That being said for the moment we are simply exploring, a keyboard available on all laptops and through cheap ($5) controllers is a good start.

## a) Moving around
* The first step is to catch an interaction event, here a specific keypress but you can imagine any other event (a LeapMotion sign, a smartphone tap, etc).
* Once we catch a keypress we check which key it is and modify an object in the scene accordingly.

```javascript
function onDocumentKeyDown(event){
  // Get the key code of the pressed key
  var keyCode = event.which;
  // space
    if(keyCode == 13){
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
}
```

* You already know how to apply a transformation to an object in the scene. It means you can rotate, scale, change colours and much more. Moving around a scene is exactly the same except that instead of translating an object your are looking, you translate the camera itself!

```javascript
// left
  if(keyCode == 37){
    camera.position.x -= 10;
  }
```  

* Voila, instead of change the colour or position of an object you translate the camera representing the first person view. In this example you only move in one direction so feel free to extend it to all 4 directions, add a jump (going up another axis then falling down), etc.

// Rez: moving in all four directions

* feel: Exploring the scene you created before
* Quickly you will notice, especially if you only go left, that you have nothing interesting left to see. It is because without limits, without boundaries you reach the end of your world without stopping. A simple way to avoid this problem is to define boundaries. They can be absolute as in the following example or relative to an object, for example a floor you defined before.

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
* So you can now trigger an action with key presses. You can change your position and the colour of an object. How can you activate an object by moving close to it?
 
```javascript
  range = 10;
  if ((mesh.position.x - camera.position.x) < range) && ((mesh.position.x - camera.position.x) < range){
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
```  

* changing objects (size, colour, position...)

// Rez: When close to an object (still block) change colour

* Billy Jean: disco ground

// Rez: white "Cubes" as tiles on the ground with no "boundaries", when you're close to them they take a color
// Labyrinth

## c) Moving Objects
* triggering event by "ray casting" (proximity + direction) e.g. via https://github.com/neuman/vreticle
* Locking an object
* Keeping object in front of you (what about rotation?)
* Releasing it (I would say with no fall => a more Sci Fi kind of look)
// Rez: Lock & Release objects (3D)

* Bonus: When released, object move around your body in a circular fashion. When released, the object stay in position, but moves as you do (follow you)
