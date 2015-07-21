---
layout: default
title:  "Your 3D scene"
num: 1

---

Here we go, now you're the hero.

## a) Displaying (and seeing!) a 3D cube
We already saw a working example in the past section, but let's be honest, that was cheating! Let's start from scratch so that we have the opportunity to really understand what's happening and build from there.

First step, let's create a simple object, display it and look at it. For that, we need some HTML page to host our code, nothing fancy here. We create a *div* in which we will render our stuff, we import our library, and get a place where we will write our code.

```html
<html>
<body>
	<div id="displayVR"></div>
	</script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r71/three.min.js"></script>
	
	<script>
	// Here be dragons
	</script>
	
</body>
</html>
```

From now, all the code shared during the workshop (unless mentioned otherwise) is meant to be between the script tags. Now let's have access to that *div* and create the canvas we will use to draw in: our renderer. We want it square and as big as possible.

```javascript
	// We get an anchor to the div element
	var container = document.getElementById('displayVR');

	// We create the renderer, set its size and add it to the div element.
	var renderer = new THREE.WebGLRenderer( );
	renderer.setSize( window.innerHeight, window.innerHeight);
	container.appendChild( renderer.domElement );
```

	
Le us start from the beginning, by displaying a simple object and looking at it. 
* 3D primitive : THREE.[BoxGeometry](http://threejs.org/docs/api/extras/geometries/BoxGeometry.html)( edgelenth, otheredgelenth, lastedgelenth ); 
* Camera : THREE.[PerspectiveCamera](http://threejs.org/docs/#Reference/Cameras/PerspectiveCamera)( fov, aspect, near, far ) ![Camera details](https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png)
* moving the primitive (rot & trans?)
Result <iframe width="200" height="200" src="http://threejs.org/examples/webgl_geometry_cube.html" frameborder="0" allowfullscreen></iframe>

## b) Setting up the stage
* We don't wnat to fall, give us a floor! : THREE.[PlaneGeometry](threejs.org/docs/#Reference/Extras.Geometries/PlaneGeometry)(width, height, widthSegments)
* Let there be light : THREE.[SpotLight](http://threejs.org/docs/#Reference/Lights/SpotLight)
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
