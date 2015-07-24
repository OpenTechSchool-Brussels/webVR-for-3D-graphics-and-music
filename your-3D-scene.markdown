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

Hmmm a blank screen... That's a start. Let's introduce the three main players now: what you will see (the cube), who will see it (you, the camera), and where you will see it (the scene). A scene is a scene, and that's already enough. To create a cube, we actually create a mesh and feed it with the structure of a cube (a box of same width, height and depth), as well as display options (its color and if it's filled or just wires). For the camera, we define where we look from (where we are) and where we look at (our focus). And a bunch of other stuff that defines the perspective (the four classics: the field of view -FOV-, its aspect, how near & how far we can see).

<img src="https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png" width="100%">

Now let's see how that works with code:

```javascript
// First, creating the scene.
var scene = new THREE.Scene();

//  Second, creating the object.
var mesh = new THREE.Mesh( new THREE.BoxGeometry( 100, 100, 100 ),
                           new THREE.MeshBasicMaterial( { color: 0xffffff, wireframe: true } ) );

// Third, creating the camera.
var camera = new THREE.PerspectiveCamera( 50, 0.5 * window.innerWidth / window.innerHeight, 1, 10000);
    camera.position.set( 0, 0, 500 );
camera.lookAt( scene.position );

// Both last element need to be added to the scene.
scene.add( mesh );
scene.add( camera );

// Last, we render our scene from our camera point of view
renderer.render( scene, camera );
```

OK, sweet, we're getting there. We're seeing a cube (yes yes, it's a cube) in 3D, but static. In order to animate it, we need two things. First we need the cube to move, so we'll rotate it on itself. Second, we need to update the rendering and not just call it once. For that, we will create a function that will update the state of the scene (rotate the cube), render what needs to be rendered and then create a self call back for when the screen to request a new frame. This means that whenever the screen ask for what to display, the function we're writing will be called.


```javascript
// In order to animate regularly, we need to create a function...
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

By the way, if you prefer to use vector, that's possible too, you can use directly *mesh.rotation.add(0.1, 0.5, 0.8)* to add a vector to the rotation. Oh, and if you're getting tired of the cube, try with a sphere *new THREE.SphereGeometry( 50, 10, 10 )*, the first argument defines its size, the last two defines how smooth you want it to be (vertically & horizontally).

## b) Setting up the stage
First, let's turn the switch on and get some light. Which alas is not enough, you need to tell your meshes to be receptive to it! Yes, life it hard... You remember the display options (MeshBasicMaterial)? We don't want to see wireframes anymore, but a physical object, reacting to the light. For that, we will replace *wireframe: true* by *shading: THREE.FlatShading*, which mean that no, we don't want a wireframe, and that yes, we want some dealing with the light (using shaders. Curious about them? [who isn't](https://en.wikipedia.org/wiki/Shader).). And of course, add a light in the scene (color & position).

```javascript
var light = new THREE.DirectionalLight( 0xffffff );
light.position.set( 0, 0.5, 1 );
scene.add( light );
```

Next, we don't want to fall, so let's have a floor! For that, two options. Either we create a grid (which won't react to light) or we define a plane. Pretty easy for the grid, we just need to define its size and the steps of the wireframe:

```javascript
var grid = new THREE.GridHelper( 500, 10 );
scene.add(grid);
```

But yes, you might want to have a more stable ground. For that we need a plane. Not much more complicated: we rely on a mesh and feed it a *PLaneGeometry* to create our plane (as parametres: size -horizontal/vertical- and steps -horizontal/vertical):

```javascript
var plane = new THREE.Mesh( new THREE.PlaneGeometry( 300, 300, 10, 10 ),
                            new THREE.MeshLambertMaterial( { color: 0xffffff, shading: THREE.FlatShading } ) );
plane.position.set( 0, 0, 0 );
// We also need to rotate it. Why? Well, try without the next line!
plane.rotation.x -= 1.3;
```


## c) Baby step in Virtual Reality
* stereoscopic effect cf VREffect.js from https://github.com/borismus/webvr-boilerplate 

```javascript
var effect = new THREE.VREffect(renderer);
var mgr = new WebVRManager(effect);
```

<img src="https://mdn.mozillademos.org/files/11095/createStereoscopicImages.png" alt="Stereo explanations" width="100%">

* camera follow head movement cf VRControls.js from https://github.com/borismus/webvr-boilerplate 

```javascript
var controls = new THREE.VRControls(camera);
```

// Rez: head movement...  

## d) A bit is nice, a lot is nice too
* Many cubes (with or without movement, different size)
* Creating a landscape from it (can't see anymore the floor with all the cubes on it)


// Rez: Random cubes Landscape.

Until now, you could only look aroud and act as a sensor, let's change that!
