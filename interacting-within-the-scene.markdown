---
layout: default
title:  "Virtual interactions"
num: 2

---

The question of how to interact when having a VR head set doesn't really have a definite answer yet. Let's see what we can already do with a simple bluetooth controller.

## a) Moving around
* triggering event by key pressed
* moving the camera (speed)
* scene boundaries (size)
* feel: Exploring the scene you created before
* Err: no resistane with the cube (we won't be using a 3D phyisic engine... will we?)
* Err: I would say no physic, and no collision.
* Err: isn't the z buffer dealt with by threeJS ?!
* Err: tweening ?

## b) Activating Objects
* triggering event by proximity
* changing objects (size, colour, position...)
* Billy Jean: disco ground
* Err: no interaction as of now of "looking for a long time", isn't that a good idea to actually avoid thid BS?

## c) Moving Objects
* triggering event by "ray casting" (proximity + direction)
* Locking an object
* Keeping object in front of you
* Releasing it (I would say with no fall => a more Sci Fi kind of look)
