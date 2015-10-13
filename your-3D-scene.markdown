---
layout: default
title:  "Your 3D scene"
num: 1

---

Here we go, now you're the hero.

## a) Displaying (and seeing!) a 3D cube
We already saw a working example in the past section, but let's be honest, that was cheating! Let's start from scratch so that we have the opportunity to really understand what's happening and build from there.

First step, let's create a simple object, display it and look at it. For that, we need some HTML page to host our code, nothing fancy here. We will render directly in the body of the HTML page, and for that we need a bit of styling (getting rid of the scroll bars, padding, and a few other stuffs). Last, we'll need to import Three.js in order to ... well, use it.

```html
<html>
<style>
body {
  background-color: #000;
  color: #fff;
  margin: 0px;
  padding: 0;
  overflow: hidden;
}
</style>
<body>
</body>

<script src="http://webvr.neocities.org/boilerplate/jslibs/three.js"></script>

<script>
// Here be dragons
</script>
	
</html>
```

From now, all the code shared during the workshop (unless mentioned otherwise) is meant to be between the script tags. Now let's have access to that *div* and create the canvas we will use to draw in: our renderer. We want it square and as big as possible.

```javascript

// We create the renderer and set its size.
var renderer = new THREE.WebGLRenderer( );
renderer.setSize( window.innerHeight, window.innerHeight);

// We add it to the HTML page
document.body.appendChild(renderer.domElement);
```

Hmmm a blank screen... That's a start. Let's introduce the three main players now: what you will see (the cube), who will see it (you, the camera), and where you will see it (the scene). A scene is a scene, and that's already enough. To create a cube, we actually create a mesh and feed it with the structure of a cube (a box of same width, height and depth), as well as display options (its color and if it's filled or just wires). For the camera, we define where we look from (where we are) and where we look at (our focus). And a bunch of other stuff that defines the perspective (the four classics: the field of view -FOV-, its aspect, how near & how far we can see).

<img src="https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png" width="100%">

Now let's see how that works with code:

```javascript
// First, creating the scene.
var scene = new THREE.Scene();

// Then, the camera.
var camera = new THREE.PerspectiveCamera( 50, 0.5 * window.innerWidth / window.innerHeight, 1, 10000);
    camera.position.set( 0, 0, 500 );
camera.lookAt( scene.position );

//  Last, the objects to see, here  simple cube.
var mesh = new THREE.Mesh( new THREE.BoxGeometry( 100, 100, 100 ),
                           new THREE.MeshBasicMaterial( { color: 0xffffff, wireframe: true } ) );


// Both last element need to be added to the scene.
scene.add( mesh );
scene.add( camera );

// Last, we render our scene from our camera point of view
renderer.render( scene, camera );
```

OK, sweet, we're getting there. We're seeing a cube (yes yes, it's a cube) in 3D, but static. 

*Warning!* This is the first time you have code that should do something. If it doesn't consider displaying the debugging console in your browser to spot any possible bug.

In order to animate it, we need two things. First we need the cube to move, so we'll rotate it on itself. Second, we need to update the rendering and not just call it once. For that, we will create a function that will update the state of the scene (rotate the cube), render what needs to be rendered and then create a self call back for when the screen to request a new frame. This means that whenever the screen ask for what to display, the function we're writing will be called.


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

Oh, and if you're getting tired of the cube, try with a sphere *new THREE.SphereGeometry( 50, 10, 10 )*, the first argument defines its size, the last two defines how smooth you want it to be (vertically & horizontally).

## b) Setting up the stage
First, let's turn the switch on and get some light. Which alas is not enough, you need to tell your meshes to be receptive to it! Yes, life it hard... You remember the display options (MeshBasicMaterial)? We don't want to see wireframes anymore, but a physical object, reacting to the light. For that, we will replace *wireframe: true* by *shading: THREE.FlatShading*, which mean that no, we don't want a wireframe, and that yes, we want some dealing with the light (using shaders. Curious about them? [who isn't](https://en.wikipedia.org/wiki/Shader).). And of course, add a light in the scene (color & position).

```javascript
var light = new THREE.DirectionalLight( 0xffffff );
light.position.set( 0, 0.5, 1 );
scene.add( light );
```

Still no lighting? It is because the basic material MeshBasicMaterial doesn't support shading. Try a most complex one MeshPhongMaterial to enjoy actual light difference.

Next, we don't want to fall, so let's have a floor! For that, two options. Either we create a grid (which won't react to light) or we define a plane. Pretty easy for the grid, we just need to define its size and the steps of the wireframe:

```javascript
var grid = new THREE.GridHelper( 500, 10 );
// can't see anything but a straight line? Trying bringing the grid down, e.g. grid.position.set( 0, -10, 0 );
scene.add(grid);
```

But yes, you might want to have a more stable ground. For that we need a plane. Not much more complicated: we rely on a mesh and feed it a *PlaneGeometry* to create our plane (as parametres: size -horizontal/vertical- and steps -horizontal/vertical):

```javascript
var plane = new THREE.Mesh( new THREE.PlaneGeometry( 300, 300, 10, 10 ),
                            new THREE.MeshLambertMaterial( { color: 0xffffff, shading: THREE.FlatShading } ) );
plane.position.set( 0, 0, 0 );
// We also need to rotate it. Why? Well, try without the next line!
plane.rotation.x -= 1.3;
scene.add(plane);
```

Ok so... that is not very pretty and you are not even convinced the light makes a difference? Just comment it out and see what happens. Rotate the plane around, shift it down, etc. Again it is your world you have to make it the way YOU want it to be!

## c) Seeing is believing 
It's pretty hard to have a strong definition of VR. What one can still agree on is that bare 3D on a screen does not really feel as reality. Virtual reality is all about immersion. For now, we'll try to better it through graphics alone. Movies theater relies on bigger screen to get a better immersion for a lot of people. On our end, we just need to feed one person so ... we'll use smaller screen and feed graphics straight to our user eyes (not as creepy as it sounds!). This way, all our user will be able to see is our world and our world alone. Way more immersive.

For that to happen, we need to simulate human vision. We can't feed the same image to both eyes, we actually need to get an idea of what each of our eyes would see of our virtual world if we were to inhabit it.

<img src="https://mdn.mozillademos.org/files/11095/createStereoscopicImages.png" alt="Stereo explanations" width="100%">

**Warning!** From now on we are going to be truly in VR. For that you do need a browser that supports the webVR API. As said earlier Chrome on Android does out of the box. For the others like Chrome on the desktop, Firefox Desktop or Firefox Mobile you need a nightly build. Please follow [our instructions](http://opentechschool-brussels.github.io/webVR-for-3D-graphics-and-music/Setting-up.html#Browser) to get the right setup quickly.

We could split our renderer in two, get two cameras close to each other and display in each our renderer what each camera see. We could. But that's a lot of work, which thankfully has been taken care by other people. We will base our code on [borismus's boiler pate](https://github.com/borismus/webvr-boilerplate). For now, we'll use:

* [VREffect](http://webvr.neocities.org/boilerplate/jslibs/VREffect.js) for rendering our scene with two images from two cameras side by side.
* [WebVR polyfill](http://webvr.neocities.org/boilerplate/jslibs/webvr-polyfill.js) for little functions that are not (yet?) native.
* [WebVR manager](http://webvr.neocities.org/boilerplate/jslibs/webvr-manager.js) for WebVR easy management.

Don't forget to download those and to import them in your HTML file. By the way, if you are using our shared neocities account you can easilly include the required libraries using http://webvr.neocities.org/boilerplate/jslibs/ :

```html
<script src="/boilerplate/jslibs/VREffect.js"></script>
<script src="/boilerplate/jslibs/webvr-polyfill.js"></script><!--Warning, if bugs feel free to remove it-->
<script src="/boilerplate/jslibs/webvr-manager.js"></script>
``` 

And now let's use them. We need to create another object that will render our scene, based on our previous renderer. Then we'll need to create a manager to help us handle all VR stuff:

```javascript
// Create another layer on top of our renderer
var effect = new THREE.VREffect(renderer);
effect.setSize(window.innerWidth, window.innerHeight);

// Create a VR manager helper to enter and exit VR mode.
var manager = new WebVRManager(renderer, effect, {hideButton: false});
```

Now that you're not using your own renderer anymore but that manager, it's the later you need to use to render in your animate loop. This means changing *renderer.render(scene, camera);* for *manager.render(scene, camera);*.

And while we're at it, let's create a little resize function so that whenever we're doing something to our window (resize, fullscreen, rotate...) everything doesn't go berserk:

```javascript
function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  effect.setSize( window.innerWidth, window.innerHeight );
}
window.addEventListener('resize', onWindowResize, false);
```

You should now have a full graphic setup that allows you already to explore a lot in VR. But for now, the experience feels more like watching a movie than really a whole world to be in.

Unfortunately it seems at this point some people seem to get stuck. We recommend that you try your hardest to figure out what is wrong with your code if you don't manage to see a VR scene yet. That being said the workshop last only so long. If after a while we can't figure it out, cheat a tiny bit by comparing your code to this simple test https://webvr.neocities.org/fabien_test_materials/test.html . Now... if really nothing works still (I know Roman will hate me forever for this, sorry Roman!) but... take the code, use it as a boiler plate and move on. It's important to figure it out but at some point you have to move on and keep the pace going :)

## d) Getting your head in the game
For this world to become real, you need to feel you're inside it. At least your head. For that, we'll control your vision by your head movement. While doing so can be pretty tough, we'll rely here too on a library that coupled with the previous will allow you to interact either from your computer or straight with your VR headset. This library is [VRControl.js](http://webvr.neocities.org/boilerplate/jslibs/VRControls.js). Don't forget first to import it in your scripts!

Then to use it, you need to add two lines of codes:

```javascript
  // Add this line after your camera is created:
var controls = new THREE.VRControls(camera);
```

```javascript
  // Add this line in the animate function:
controls.update();
```

You should now be able to see the whole world around you as you move your gaze around. Should. Not must. Let's change that and force our user to actually look around: how would you make your cube move around your user? You should be able to do it with what you learned up until now (and a bit of math). Try to think and make it happen before looking at the solution below:

```javascript
  // Add this line in the animate function:
  mesh.position.applyAxisAngle( new THREE.Vector3(0,1,0), 0.1);
```

This code will function only if you didn't center your cube relatively to the Y axis. And by the way, we left some work for you: we made the rotation relative to the center, not the camera itself.


## d) A bit is nice, a lot is nice too
While we have had very simple code till now, don't think you can't already do a lot with what you have. You can create primitives objects, make them move, and watch around. And from that, you can do already a lot. Don't hesitate to explore a bit what you can do with all that, and what you can express.

For instance, our solid plane is pretty simple, we might want to have a full landscape made of little cubes. For that, you might want to add after your scene creation something along the line of:

```javascript
var material = new THREE.MeshBasicMaterial( { color: 0xffffff, shading: THREE.FlatShading } );

for(var i=0; i<100; i++) {
  var mesh = new THREE.Mesh( new THREE.BoxGeometry( Math.random()*10, Math.random()*10, Math.random()*10 ), material);
  mesh.position.set( Math.random()*100-50, Math.random()*100-50, 0 );
  scene.add(mesh);
}
```

Messy but cool (isn't it?). The only bad point is that you can't actually access easily those object anymore. If you want to do so for later usage, don't forget to add them in a javascript Array.

Well, now we're having a start of a full experience, but our interaction margin is still pretty small, let's see what next section has to say about that!
