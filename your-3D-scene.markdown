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

Hmmm a blank screen... That's a start. Let's introduce the three main players now: what you will see (the cube), who will see it (you, the camera), and where you will see it (the scene). A scene is a scene, and that's already enough. To create a cube, we actually create a mesh and feed it with the structure of a cube (a box of same width, height and depth), as well as display options (its color and if it's filled or just wires). For the camera, we define where we look from (where we are) and where we look at (our focus). And a bunch of other stuff that defines the perpective (the four classics: the field of view -fov-, its aspect, how near & how far we can see).

<img src="https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png" width="100%">

Now let's see how that works with code:

```javascript

	// First, creating the scene.
	var scene = new THREE.Scene();

	//  Second, creating the object.
	var mesh = new THREE.Mesh( new THREE.BoxGeometry( 100, 100, 100 ),
	                       new THREE.MeshBasicMaterial( { color: 0xffffff, wireframe: true } ) );

	// Third, creating the camera.
	var camera = new THREE.PerspectiveCamera( 50, 0.5 * window.innerWidth / window.innerHeight, 1, 10000 );
    	camera.position.set( 0, 0, 500 );
	camera.lookAt( scene.position );
	
	// Both last element need to be added to the scene.
	scene.add( mesh );
	scene.add( camera );
	
	// Last, we render our scene from our camera point of view
	renderer.render( scene, camera );

```

OK, sweet, we're getting there. We're seeing a cube (yes yes, it's a cube) in 3D, but static. In order to animate it, we need two things. First we need the cube to move, so we'll rotate it on itself. Second, we need to update the rendering and not just call it once. For that, we will create a fonction that will update the state of the scene (rotate the cube), render what needs to be rendered and then create a self call back for when the screen to request a new frame. This means that whenever the screen ask for what to display, the fonction we're writing will be called.


```javascript
	// In order to animate regularly, we need to create a fonction...
	function animate() {
		// Update
		mesh.rotation.y += 0.01;		
		// Rendering
		renderer.render( scene, camera );
		// Callback
		requestAnimationFrame( animate );
	}
	// ... and start calling it!
	animate();
```

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
<img src="https://mdn.mozillademos.org/files/11095/createStereoscopicImages.png" alt="Stereo explanations" width="100%">
* camera follow head movement
** relative Vs absolute coordiantes
** Object moving (various speed) (forcing the player to look around)
// Rez: head movement...  

## d) A bit is nice, a lot is nice too
* Many cubes (with or without movement, different size)
* Creating a landscape from it (can't see anymore the floor with all the cubes on it)
// Rez: Random cubes Landscape.

Until now, you could only look aroud and act as a sensor, let's change that!
