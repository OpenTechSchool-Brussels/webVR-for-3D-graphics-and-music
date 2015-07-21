---
layout: default
title:  "Your 3D scene"
num: 1

---


## a) Displaying (and seeing!) a 3D cube
Le us start from the beginning, by displaying a simple object and looking at it.
* 3D primitive : THREE.[BoxGeometry](http://threejs.org/docs/api/extras/geometries/BoxGeometry.html)( edgelenth, otheredgelenth, lastedgelenth );  <iframe width="200" height="200" src="http://threejs.org/examples/webgl_geometry_cube.html" frameborder="0" allowfullscreen></iframe>
* Camera : THREE:[PerspectiveCamera](http://threejs.org/docs/#Reference/Cameras/PerspectiveCamera)( fov, aspect, near, far ) ![Camera details](https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png)
* moving the primitive (rot & trans?)
// Rez: Seeing a 3D cube

## b) Setting up the stage
* floor
* light (type of light)
* ![Spotlight](https://sites.google.com/site/threejstuts/_/rsrc/1427678925804/home/spotlight-shadowmap/spot1.jpg?height=181&width=200)
* properties of a primitive (size, colour)
* type of primitive
// Rez: Whole stage (all the above)

## c) Baby step in Virtual Reality
* stereoscopic effect
// Rez: two renderers of the same scene
* ![Stereo explanations](https://mdn.mozillademos.org/files/11095/createStereoscopicImages.png  )
* camera follow head movement
** relative Vs absolute coordiantes
** Object moving (various speed) (forcing the player to look around)
// Rez: head movement...  

## d) A bit is nice, a lot is nice too
* Many cubes (with or without movement, different size)
* Creating a landscape from it (can't see anymore the floor with all the cubes on it)
// Rez: Random cubes Landscape.

Until now, you could only look aroud and act as a sensor, let's change that!
