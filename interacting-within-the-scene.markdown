---
layout: default
title:  "Virtual interactions"
num: 2

---

The question of how to interact when having a VR head set doesn't really have a definite answer yet. Let's see what we can already do with a simple bluetooth controller.

## a) Moving around
* triggering event by key pressed
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
* moving the camera (speed)
```javascript
// left
  if(keyCode == 37){
    camera.position.x -= 10;
  }
```  
// Rez: moving in all four directions
* feel: Exploring the scene you created before
* Boundaries (Scene borders) &  (discs around cubes)
```javascript
// left
  if(keyCode == 37){
    if (camera.position.x < 100) and (camera.position.x > -100){
      camera.position.x -= 10;
    }
  }
```  
// Rez: the above...


## b) Activating Objects
* (tweening ?)
* triggering event by proximity
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
