---
layout: default
title:  "Your 3D scene"
num: 1

---

<script>
function HideSourceVisibility() {
	var css = document.createElement("style");
	css.type = "text/css";
	css.innerHTML = ".highlight {opacity: 0.1;}";
	document.body.appendChild(css);
}
function ShowSourceVisibility() {
	var css = document.createElement("style");
	css.type = "text/css";
	css.innerHTML = ".highlight {opacity: 1.0;}";
	document.body.appendChild(css);
}
</script>
<button type="submit" onclick="HideSourceVisibility()">Hide source code (harder but you will learn a lot more)</button> / 
<button type="submit" onclick="ShowSourceVisibility()">Ok it's too hard, just show me that damn code again!</button>

Here we go, now you're the hero.

## a) Setting up our environnement

Let's start from zero and be proud to really understand what's happening here!

This is web tech, so we'll start from an HTML file (simple one, mainly to get library and call our javascript code). Let's call it **index.html** so it's called by default at the root of your server. And we'll need some styling (getting rid of the scroll bars, padding, and a few other stuffs), we'll put it inline, be free to use a CSS file if you want it clean. Then we import our libraries. For now we only need to import three.js. All the library you will need are in [this file](./jslibs.zip). While you can (and should, at some point) get them yourself online, we made sure to have a working set of them. Last, we call a specific file in which we will put our own javascript code (here called **myVRworld.js**).

It goes something like:

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

  <script src="./jslibs/three.js"></script>

  <script src="myVRworld.js"></script>
	
</html>
```

From now, all the code shared during the workshop (unless mentioned otherwise) is meant to be in the **myVRworld.js** file.

## b) Displaying (and seeing!) a 3D cube
Our aim here is to display a cube in classic 3D and to display it on your smartphone. Half the step needed for VR on your smartphone. For that we need a few things:

* a render, in which we will draw our scene from the point of view of our camera;
* a cube (duh!);
* a light to allow us to see the cube.
* a camera, to define our point of view;
* a scene, that will host the things we want to display;

Sounds like a lot but all steps are pretty small and straight forward. And once we're done with that, we're done with pretty much all the graphic stuff!

#### b.1) The Renderer

First, let's fill the body of our webpage with a three.js renderer:

```javascript
// We create the renderer and set its size to our full screen
var renderer = new THREE.WebGLRenderer( );
renderer.setSize( window.innerWidth, window.innerHeight);

// We add it to the HTML page
document.body.appendChild(renderer.domElement);
```

<div class="doc">Documentation <a target="_blank" href="http://threejs.org/docs/#Reference/Renderers/WebGLRenderer" title="WebGL renderer displays your beautifully crafted scenes using WebGL, if your device supports it.">THREE.WebGLRenderer()</a> and with parameters clarification for <a target="_blank" title=".resize( width, height ) Resizes the output canvas to (width, height), and also sets the viewport to fit that size, starting in (0, 0)." href="http://threejs.org/docs/#Reference/Renderers/WebGLRenderer.setSize">renderer.setSize()</a>.</div>

Hmmm a blank screen... That's a start I guess.

#### b.2) The Cube

Second, let's create the cube. For that, we'll need a geometry defining the cube and a material defining how it's displayed and its color.

```javascript
// A cube is a box of same width, height and deepness
var geometryCube = new THREE.BoxGeometry( 1, 1, 1); 
// We use a MeshLambertMaterial to define the color
var materialCube = new THREE.MeshLambertMaterial( { color: 0xffaa00 } )
// We create our Cube and modify its position
var meshCube = new THREE.Mesh( geometryCube, materialCube );
meshCube.position.y = 0.5;
```

<div class="doc">Documentation
for <a target="_blank" href="http://threejs.org/docs/#Reference/Extras.Geometries/BoxGeometry" title="BoxGeometry is the quadrilateral primitive geometry class. It is typically used for creating a cube or irregular quadrilateral of the dimensions provided with the 'width', 'height', and 'depth' constructor arguments.">BoxGeometry()</a>
, for <a target="_blank" title="A material for non-shiny (Lambertian) surfaces, evaluated per vertex. with property  .color where Diffuse color of the material. Default is white." href="http://threejs.org/docs/#Reference/Materials/MeshLambertMaterial">MeshLambertMaterial()</a>
, for <a target="_blank" title="Object's local position." href="http://threejs.org/docs/#Reference/Core/Object3D.position">mesh.position</a>
and for <a target="_blank" title="Float of the vector's x value" href="http://threejs.org/docs/#Reference/Math/Vector3">position.x</a>
.</div>

#### b.3) The Light
For the light, we just define its color, and then its position.

```javascript
var light = new THREE.DirectionalLight( 0xffffff );
light.position.set( 1, 1, 1 );
```

<div class="doc">Documentation
for <a target="_blank" href="http://threejs.org/docs/#Reference/Lights/DirectionalLight" title="objects using MeshLambertMaterial or MeshPhongMaterial.  DirectionalLight(hex) -- Hex Numeric value of the RGB component of the color.">DirectionalLight()</a>
and for <a target="_blank" title=".set ( x, y, z ) Sets value of this vector." href="http://threejs.org/docs/#Reference/Math/Vector3.set">position.set</a>
.</div>

#### b.4) The Camera
For the camera, we need to define the perspective through 4 values: the field of view -FOV-, its aspect, how near & how far we can see. You can refere to the picture below to get a sense of them.

<img src="https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png" width="100%">

Then we need to define where it is, and where it's looking at.

```javascript
var camera =
	new THREE.PerspectiveCamera( 75, 0.5 * window.innerWidth / window.innerHeight, 1, 10000);
camera.position.set( 0, 1, 2 );
```
<div class="doc">Documentation
for <a target="_blank" href="http://threejs.org/docs/#Reference/Cameras/PerspectiveCamera" title="Camera with perspective projection. PerspectiveCamera( fov, aspect, near, far )">DirectionalLight()</a>
and for <a target="_blank" title=" .lookAt ( vector ) vector - A world vector to look at.  Rotates object to face point in space." href="http://threejs.org/docs/#Reference/Core/Object3D.lookAt">.lookAt()</a>
.</div>

#### b.5) The Scene and the rest
We create our scene, add the cube and light to the scene, and then we feed the render with the scene and camera.

```javascript
var scene = new THREE.Scene();
scene.add( meshCube );
scene.add( light );

// To be sure we're looking at our scene
camera.lookAt( scene.position ); // we're aiming at the center of it

renderer.render( scene, camera );
```
<div class="doc">Documentation
for <a target="_blank" href="http://threejs.org/docs/#Reference/Scenes/Scene" title="Scenes allow you to set up what and where is to be rendered by three.js. This is where you place objects, lights and cameras.">Scene()</a>
and for <a target="_blank" title="Render a scene using a camera." href="http://threejs.org/docs/#Reference/Renderers/WebGLRenderer.render">renderer.render</a>
.</div>

OK, sweet, we're getting there. We're seeing a cube (yes yes, it's a cube) in 3D, but static. If you don't see anything, check twice your code. If it's still not working, consider displaying the debugging console in your browser to spot any possible bug.

#### b.6) Animation

In order to animate it, we need two things. First we need the cube to move, so we'll rotate it on itself. Second, we need to update the rendering and not just call it once. For that, we will create a function that will update the state of the scene (rotate the cube), render what needs to be rendered and then create a self call back for when the screen to request a new frame. This means that whenever the screen ask for what to display, the function we're writing will be called.


```javascript
// In order to animate regularly, we need to create a function...
function animate() {
	// Update
	meshCube.rotation.y += 0.01;
	// Rendering
	renderer.render( scene, camera );
	// Callback
	requestAnimationFrame( animate );
}
// ... and start calling it!
animate();
```
<div class="doc">Documentation
for <a target="_blank" href="http://threejs.org/docs/#Reference/Core/Object3D.rotation" title="
Object's local rotation (Euler angles), in radians.">.rotation.y</a>
and for <a target="_blank" title="tells the browser that you wish to perform an animation and requests that the browser call a specified function to update an animation before the next repaint. The method takes as an argument a callback to be invoked before the repaint." href="https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame">requestAnimationFrame()</a>
.</div>


Oh, and if you're getting tired of the cube, try with a sphere *new THREE.SphereGeometry( 10, 12, 12 )*, the first argument defines its size, the last two defines how smooth you want it to be (vertically & horizontally).

## c) Built from the ground up
We don't want to fall, so let's have a floor! How could we do that? Well, you have already all you need to do so: you can create a box. A ground is just a very flat and wide box! Don't forget to translate it if needed for it to be at the right place. While a floor is pretty neat to add immersion, we'll have another very handy tool to realise we're in a 3D world: a skybox. This is just some box surounding the whole scene. We're inside and it helps us getting the hang of the 3D thing before our world is more inhabited.

First in order to create the grid we'll repeat a texture, for that we need to create one. Here, we use a [box.png](./box.png) image file for the texture.

```javascript
// We define the size of the box, and hence how many time the texture gets repeated
var boxWidth = 10;
// We define the texture from an image file
var texture = THREE.ImageUtils.loadTexture('img/box.png');
// We ask it to repeat, and then define how many times
texture.wrapS = THREE.RepeatWrapping;
texture.wrapT = THREE.RepeatWrapping;
texture.repeat.set(boxWidth, boxWidth);
```
<div class="doc">Documentation
for <a target="_blank" href="http://threejs.org/docs/#Reference/Extras/ImageUtils.loadTexture" title=".loadTexture (url, mapping, onLoad, onError) with url -- the url of the texture">ImageUtils.loadTexture()</a>
, for <a target="_blank" href="http://threejs.org/docs/#Reference/Textures/Texture.wrapS" title="defines how a texture behaves when it is given texture coordinates outside the range of 0 to 1.">Texture.wrapS</a>
, for <a target="_blank" href="http://threejs.org/docs/#Reference/Textures/Texture.wrapT" title="defines how a texture behaves when it is given texture coordinates outside the range of 0 to 1.">Texture.wrapT</a>
and for <a target="_blank" title="How many times the texture is repeated across the surface, in each direction U and V." href="http://threejs.org/docs/#Reference/Textures/Texture.repeat">Texture.repeat.set()</a>
.</div>


Good, we have a texture, now time to add it to a material and to a mesh:


```javascript
// A skybox is afterall a box
var geometry = new THREE.BoxGeometry(boxWidth, boxWidth, boxWidth);
// We use BasicMaterial 'cause basic is enough
var material = new THREE.MeshBasicMaterial({
  map: texture,
  color: 0x01BE00,
  side: THREE.BackSide
});

// Then we create the mesh and add it to the scene
var skybox = new THREE.Mesh(geometry, material);
scene.add(skybox);
```
<div class="doc">Documentation
for <a target="_blank" title="
A material for drawing geometries in a simple shaded (flat or wireframe) way. The default will render as flat polygons. To draw the mesh as wireframe, simply set the 'wireframe' property to true." href="http://threejs.org/docs/#Reference/Materials/MeshBasicMaterial">MeshBasicMaterial()</a>
, for <a target="_blank" href="http://threejs.org/docs/#Reference/Materials/MeshBasicMaterial.map" title="Set texture map. Default is null.">MeshBasicMaterial.map</a>
, for <a target="_blank" href="http://threejs.org/docs/#Reference/Materials/Material.side" title=" Defines which of the face sides will be rendered - front, back or both.">Material.side</a>
and for <a target="_blank" title="Change from the default THREE.FrontSide to THREE.BackSide." href="http://threejs.org/docs/#Reference/Constants/Materials">THREE.BackSide</a>
.</div>


So yeah, a skybox is just a big box where a texture has been applied to the inside. You can put a texture of space to give you a more eerie feeling if you feel like. In any case, you not only have a better setup, you now know how to apply texture! But but but .... this is not supposed to be a 3D workshop! Where's the VR? Well, VR is based on 3D so we had to go through all that. Now ....

## d) Seeing is believing
Keep your smartphone andyour little side gear (google's cardboard, dive, or any other smartphone head set) by your side and let's make it happen! **Warning!** From now on we are going to be truly in VR on the smartphone. For that you do need a mobile browser that supports the webVR API. If you don't have one, be sure to update or get one that is compatible.

<img src="https://mdn.mozillademos.org/files/11095/createStereoscopicImages.png" alt="Stereo explanations" width="100%">

Those that didn't skip the Setting Up section knows already that we'll need t separate our scene in two to be able to display what each eye need to see. We could split our renderer in two, get two cameras close to each other and display in each our renderer what each camera see. We could. But that's a lot of work, which thankfully has been taken care by other people. We will base our code on [borismus's boiler pate](https://github.com/borismus/webvr-boilerplate). For now, we'll use:

* [VREffect](http://webvr.neocities.org/boilerplate/jslibs/VREffect.js) for rendering our scene with two images from two cameras side by side.
* [WebVR polyfill](http://webvr.neocities.org/boilerplate/jslibs/webvr-polyfill.js) for little functions that are not (yet?) native.
* [WebVR manager](http://webvr.neocities.org/boilerplate/jslibs/webvr-manager.js) for WebVR easy management.

A specific version of those libraries (tested and working) can be found [here](jslibs.zip). While you are encouraged to use up to date versions, they can happen to be not compatible. We made sure to have a compatible set for you! Don't forget to import them in your HTML file as you did for the three.js library:

```html
<script src="./jslibs/VREffect.js"></script>
<script src="./jslibs/webvr-polyfill.js"></script>
<script src="./jslibs/webvr-manager.js"></script>
``` 

And now let's use them. We need to create another object that will render our scene, based on our previous renderer. Then we'll need to create a manager to help us handle all VR stuff:

```javascript
// Create another layer on top of our renderer
var effect = new THREE.VREffect(renderer);
effect.setSize(window.innerWidth, window.innerHeight);

// Create a VR manager helper to enter and exit VR mode.
var manager = new WebVRManager(renderer, effect, {hideButton: false});
```
<div class="doc">Documentation
for <a target="_blank" href="https://github.com/mrdoob/three.js/blob/master/examples/js/effects/VREffect.js" title="THREE.VREffect = function ( renderer, onError )">THREE.VREffect()</a>
, for <a target="_blank" href="https://github.com/mrdoob/three.js/blob/master/examples/js/effects/VREffect.js#L71" title="setSize = function( width, height )">setSize()</a>
and for <a target="_blank" title="Helper for getting in and out of VR mode. Here we assume VR mode == full screen mode." href="https://github.com/borismus/webvr-boilerplate/blob/master/build/webvr-manager.js">WebVRManager(renderer, effect, params)</a>
.</div>


Now that you're not using your own renderer anymore but that manager, it's the later you need to use to render in your animate loop. This means changing *renderer.render(scene, camera);* for *manager.render(scene, camera);*.

You should now have a full graphic setup that allows you already to explore a lot in VR. But for now, the experience feels more like watching a movie than really a whole world to be in.

## e) Getting your head in the game
For this world to become real, you need to feel you're inside it. At least your head. For that, we'll control your vision by your head movement. While doing so can be pretty tough, we'll rely here too on a library that coupled with the previous will allow you to interact either from your computer or straight with your VR headset. This library is VRControl.js and can be found in the same previous zip file. Don't forget first to import it in your scripts! (We promise, this is the last import for now!).


Note that it is starting to be quite a lot of files to handle, if you are unsure your file system should look close to this <img style="float: right; margin: 15px 15px 15px 15px;" src="http://vatelier.net/boilerplate_structure_preview.png" width="100px"/>

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
  meshCube.position.applyAxisAngle( new THREE.Vector3(0,1,0), 0.1);
```

This code will function only if you didn't center your cube relatively to the Y axis. And by the way, we left some work for you: we made the rotation relative to the center, not the camera itself.

## f) A bit is nice, a lot is nice too
While we have had very simple code till now, don't think you can't already do a lot with what you have. You can create primitives objects, make them move, and watch around. And from that, you can do already a lot. Don't hesitate to explore a bit what you can do with all that, and what you can express.

For instance, our solid plane is pretty simple, we might want to have a full landscape made of little cubes. For that, you might want to add after your scene creation something along the line of:

```javascript
for(var i=0; i<500; i++) {
  var geometrie =
	new THREE.BoxGeometry(
		Math.random()*0.2+0.01, Math.random()*0.2+0.01, Math.random()*0.2+0.01 );
  var mesh = new THREE.Mesh(geometrie, new THREE.MeshLambertMaterial() );
  // We put boxes everywhere inside (size of side is 5)
  mesh.position.set(Math.random()*5-2.5, -0.5, Math.random()*5-2.5 );
  // We give them a random rotation
  mesh.rotation.set(Math.random()*Math.PI*2, Math.random()*Math.PI*2, Math.random()*Math.PI*2 );
  // 'cause random colors are fun
  mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  scene.add(mesh);
}
```
<div class="doc">Documentation
for <a target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random" title="returns a floating-point, pseudo-random number in the range [0, 1) that is, from 0 (inclusive) up to but not including 1 (exclusive), which you can then scale to your desired range. The implementation selects the initial seed to the random number generation algorithm; it cannot be chosen or reset by the user.">Math.random()</a>
and for <a target="_blank" href="http://threejs.org/docs/#Reference/Math/Color.setRGB" title=" .setRGB ( r, g, b ) with r : Red channel value between 0 and 1.  g : Green channel value between 0 and 1. b : Blue channel value between 0 and 1. ">Material.color.setRGB()</a>
.</div>


Messy but cool isn't it? If it's getting a bit too slow, don't hesitate to lower the number of boxes, if not don't hesitate to up it :D. The only bad point is that you can't actually access easily those object anymore. If you want to do so for later usage, don't forget to add them in a javascript Array.

Well, now we're having a start of a full experience, but our interaction margin is still pretty small, let's see what next section has to say about that!

#### Bonus round
Ok, this is not necessary, but if you want to continue in this direction a bit more, here is an "earthquake" kind of visualisation, where you need to have a way to access previously created meshes. For creation:

```javascript
var meshArray = new Array(); 
for(var i=-2.5; i<=2.5; i+=0.5) {
 for(var j=-2.5; j<=2.5; j+=0.5) {
  var mesh = new THREE.Mesh( new THREE.BoxGeometry( 0.5, 3, 0.5), new THREE.MeshLambertMaterial());
  mesh.position.set(i, -2+Math.random()*-1, j );
  scene.add(mesh);
  meshArray.push(mesh);
 }
}
```
<div class="doc">Documentation
for <a target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array" title="global object that is used in the construction of arrays; which are high-level, list-like objects.">Array()</a>
and for <a target="_blank" title="adds one or more elements to the end of an array and returns the new length of the array." href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push">Array.push()</a>
.</div>


And in the render loop:

```javascript
  for(var i = 0; i < meshArray.length; i++)
    meshArray[i].position.y += Math.random()*0.003-0.0015;
```
<div class="doc">Documentation
for <a target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length" title="represents an unsigned, 32-bit integer that is always numerically greater than the highest index in the array.">Array().lenth</a>
.</div>


Don't hesitate to create your own version of the ground!
