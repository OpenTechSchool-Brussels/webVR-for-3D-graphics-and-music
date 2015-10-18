---
layout: default
title:  "Your 3D scene"
num: 1

---

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

First, let's fill the body of our webpage with a three.hs renderer:

```javascript
// We create the renderer and set its size to our full screen
var renderer = new THREE.WebGLRenderer( );
renderer.setSize( window.innerHeight, window.innerHeight);

// We add it to the HTML page
document.body.appendChild(renderer.domElement);
```

Hmmm a blank screen... That's a start I guess.

#### b.2) The Cube

Second, let's create the cube. For that, we'll need a geometry defining the cube and a material defining how it's displayed (using, among other stuff, sharders. Curious about them? [who isn't.](https://en.wikipedia.org/wiki/Shader)).

```javascript
// We use a BoxGeomoetry of similar size along all three axises to get a cube shape
var geometryCube = new THREE.BoxGeometry( 1, 1, 1); 
// We use a MeshLambertMaterial to define the color and the shading.
var materialCube = new THREE.MeshLambertMaterial( { color: 0xffaa00, shading: THREE.FlatShading } )
// We create our Cube and modify its position
var meshCube = new THREE.Mesh( geometryCube, materialCube );
mesh.position.y = 0.5;
```

#### b.3) The Light
For the light, we just define its color, and then its position.

```javascript
var light = new THREE.DirectionalLight( 0xffffff );
light.position.set( 0, 0.5, 1 );
```

#### b.4) The Camera
For the camera, we need to define the perspective through 4 values: the field of view -FOV-, its aspect, how near & how far we can see. You can refere to the picture below to get a sense of them.

<img src="https://mdn.mozillademos.org/files/11091/FOVrelatedProperties.png" width="100%">

Then we need to define where it is, and where it's looking at.

```javascript
var camera = new THREE.PerspectiveCamera( 50, 0.5 * window.innerWidth / window.innerHeight, 1, 10000);
camera.position.set( 0, 3, 6 );
camera.lookAt( scene.position ); // we're aiming the center of the scene.
```

#### b.5) The Scene and the rest
We create our scene, add the cube and light to the scene, and then we feed the render with the scene and camera.

```javascript
var scene = new THREE.Scene();
scene.add( meshCube );
scene.add( light );

renderer.render( scene, camera );
```

OK, sweet, we're getting there. We're seeing a cube (yes yes, it's a cube) in 3D, but static. If you don't see anything, check twice your code. If it's still not working, consider displaying the debugging console in your browser to spot any possible bug.

#### b.6) Animation

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

So yeah, a skybox is just a big box where a texture has been applied to the inside. You can put a texture of space to give you a more eerie feeling if you feel like. In any case, you not only have a better setup, you now know how to apply texture! But but but .... this is not supposed to be a 3D workshop! Where's the VR? Well, VR is based on 3D so we had to go through all that. Now ....

## d) Seeing is believing 
Those that didn't skip the Setting Up section knows already that we'll need t separate our scene in two to be able to display what each eye need to see.

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

## e) Getting your head in the game
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


## f) A bit is nice, a lot is nice too
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
