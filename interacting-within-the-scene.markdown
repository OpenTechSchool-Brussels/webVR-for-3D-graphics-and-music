---
layout: default
title:  "Virtual interactions"
num: 2

---

We saw that the graphic part of the world is paramount to immersion. Afterall, this is the sense that is most activated in our real world (for better and worse). But while that's true, graphics aren't all. We saw the power to change the direction of your gaze by moving your head alone: natural interactions in this new world strengthen the immersion.

The question of how to interact when having a VR head set doesn't really have a definite answer yet. Let's see what we can already do with a simple bluetooth controller (keyboard, mouse, joystick...). Note that there are lots of alternatives, from the basic Google Cardboard single action to the imperfect but futuristic looking LeapMotion. Some people suggest in fact that ways to interact are so radically shaping the VR experience you should code **for** a way to interact rather than for a (now nearly homogenous) way to display VR worlds. That being said, for the moment, we are simply exploring: your laptop's keyboard and mouse, and simple and cheap Bluetooth controllers will do the trick.

## o) Press the button

The first step is to catch an interaction event. In our case it'll be a key pressed, but you can already think of any other possibilities (especially if you have them at hand!). For now, we'll just use a simple action: changing the color of a mesh. We'll refer for the to the mesh you already created, but don't hesitate to go wild and modify all you might have done.

```javascript
function onKey(event) {
  if(event.keyCode == 13) { // Pressed on the space key
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
};
window.addEventListener('keydown', onKey, true);
```

If you're epileptic, or just want another kind of fun, you could actually add objects with the press of a button. How? Well, you know how to check for a press of a button, and you know how to create object. Time to link both of them. Don't be lazy and check already the answer below, I promise it'll still be here after you tried it a bit by yourself!

```javascript
  // To put in onKey function
  var geometry = new THREE.BoxGeometry( Math.random(), Math.random(), Math.random());
  var cube = new THREE.Mesh( geometry,  new THREE.MeshBasicMaterial( { color: 0xffffff, shading: THREE.FlatShading } ));
  cube.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  cube.position.set( Math.random()*6-3, Math.random()*6-3, Math.random()*6-3);
  scene.add(cube);
```

Funky. This is indeed interacting with the world, but the interaction (while totally awesome) is not really reinforcing our immersion, which is our aim right now. It's because it's not a coherent one, one that the user feel natural. You could say it's a digital one and not an analogical one somehow. Let's see what other interaction is closer to what one would want.

## a) Moving around

Yep, moving. Classic interaction that one would expect to be able to experience in a virtual world. You're actually almost done: you already know how to apply a transformation to an object in the scene, now you just need to apply that to your own camera position. While moving your position along the absolute X, Y and Z axis would work (*camera.position.x += 0.1;*), this won't feel natural. You should rather move around following the relative coordinates of your own camera. Lucky you, there is a function for that (along Y this would be: *camera.translateY(0.1);*

```javascript
  // Put the code in the onKey function (well, if you want it to work)
  switch(event.keyCode) {
  case 73: camera.translateZ(-0.2); break; // i
  case 74: camera.translateX(-0.2); break; // j
  case 75: camera.translateZ(0.2);  break; // k
  case 76: camera.translateX(0.2);  break; // l
  case 33: camera.translateY(0.2);  break; // page Up
  case 34: camera.translateY(-0.2); break; // page Down
  }
```  

Voila, instead of change the color or position of an object you translate the camera representing the first person view. In this example you only translate. Try to imagine how you could simulate steps, or even a jump (going up then falling back)!

One person's freedom ends where another's begins. Or when you put boundaries. It's all fun to move freely, but it means you can get out of your own world when you're going to far. Let's put boundaries and forbid that. For instance, you could translate back inside boundaries when you're going too far. Along the X axis that would be something like:

```javascript
  // Put the code in the onKey function

  if(camera.position.x < -10) {
    camera.position.x = -10;
  }

  if(camera.position.x > 10) {
      camera.position.x = 10;
  }
```  

By the way, you can also forbid some movement (like flying along the Z axis), especially if you added the possibility to jump! Further more, while we put arbitrary value for the boundaries, you can put some that makes sense: you could put relative boundaries linked with an object you display, such as the floor you defined for instance.

## b) Activating Objects
Ok, what we said about natural interactions are true, they help with immersion. Buuuuut, supernatural interactions can also help when they are not felt too much as a switch or an interface. Previously we changed the color of a mesh by pressing a button. Felt really like a classic interface. What if you could trigger an event by your proximity to an object? You would interact with object around just by moving in your virtual world.
 
For that to happen, you need to check regularly (**i.e.** in your animate function) your distance to the object that matters. On a not so related note, if you're epileptic don't change the color in the following random manner...
 
```javascript
  // In your animate function
  var dist = 0.8;
  if( camera.position.distanceTo(mesh.position) < dist) {
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
```  

We mentioned boundaries earlier. Now that you can check the distance to an object, you can easily add a simply collision detection and create boundaries that will forbid you to get too close to an object.

So, what about this natural interaction? Actually a lot. With this interaction, you have a full toolbox to already create simple experiences, stories or even games. You could imagine a labyrinth for instance, or a puzzle game where the aim is to activate objects in a particular order in order to go to the next level. VR -as any other medium- is not just about the skills you get. They are the basics, but the point is how you use them. Try to think of a little story or game that would rely mostly on what you learned up until now, and see if you can make it happen.

Or you could do a disco floor that reacts when you move on it. [Billy Jean](https://youtu.be/Zi_XLOBDo_Y?t=18s) for the win.

## c) Gaze

So far we played around a very natural way to interact with object, meaning getting close to them. That's nice because it forces us move around, explore the virtual world and it is something that we do all the time. The thing is... virtual reality can be a lot more fun than the real world! Yes, in the computer you can be a magician, you can make table levitate by just looking at them. Heck you can move mountains by looking at them!

How? Well think about the camera, it has a position and a direction. The direction is basically a straight line going from the current position to an infinite target. When you look at an object this straight line intersects with it. This is similar to what we saw before by comparing the position of the camera with the position of an object. The main different is that we only compare the position of the object with the line.

Luckily for us ThreeJS has again all the tools in hand using [Raycaster](http://threejs.org/docs/#Reference/Core/Raycaster) and its *intersectObjects()* method. As it can be a bit tedious still to handle several object we will rely on the [vreticle](https://github.com/neuman/vreticle) library for convenience. Don't forget to import this library too!

```javascript
  // Create your reticle after having created the camera
var reticle = vreticle.Reticle(camera);
```
  
```javascript
  // Once your mesh is defined, add it to the check list, and add functions
  reticle.add_collider(mesh);

  mesh.ongazelong = function(){
    this.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
```

```javascript
  // Last, don't forget to update your reticle in your animate loop
  reticle.reticle_loop();
```

You have also access to *ongazeover()* and *ongazeout()* which are working in a similar manner. While as always we give prototypical examples, feel free to explore the new possibilities of such interactions.
 
## d) Moving Objects

Here are some suggestions : Locking an object, Keeping object in front of you, Releasing it.

In order to move an object around you can rely on the previous sections. You already know how to do handle a user action, e.g. pressing a key, and since the previous section you know when you look at an object. Now the only problem left is to make the object follow the gaze. This requires to make a vector that represents where the user is looking (not just the camera position!) then add it to the object you look at.

```javascript
//in the animate loop as this has to be constantly updated
  if(picked && gazed){
    lookAtVector = new THREE.Vector3(0,0, -1);
    lookAtVector.applyQuaternion(camera.quaternion);
    newPos.copy(lookAtVector);
    newPos.add(camera.position);
    cube.position.copy(newPos);
  }
```

As you may have guessed this also needs 2 booleans to keep track of the intent of the user, does he want to move the object or not, and also the gaze.

The user intent is managed like any other event :

```javascript
  if (event.keyCode == 13) { // enter
    picked = !picked;
  }
```

The gaze is managed by vreticle useful methods :

```javascript
cube.ongazeover = function(){
  gazed = true;
}

cube.ongazeout = function(){
  gazed = false;
}
```

That's it, try and see your object being moved around "magically"! In order to better get the idea you can try to duplicate objects, e.g. instead of moving an object you make a new one when you press enter then move it. This allows to get a better sense of moving by keeping track of the initial position.

Creative suggestion : Lock & Release objects (3D), launching object. little physique added, and stop on contact with the floor.
