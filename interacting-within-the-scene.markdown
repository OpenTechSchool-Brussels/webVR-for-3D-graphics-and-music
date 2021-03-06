---
layout: default
title:  "Virtual interactions"
num: 2

---

We saw that the graphic part of the world is paramount to immersion. Afterall, this is the sense that is most activated in our real world (for better and worse). But that's not the whole story and we'll be learning another part of it in this log, one you already had a peak of: you already felt the immersion effect of being able to change the direction of your gaze by simply moving your head: natural interactions open up another dimension.

The question of how to interact when having a VR head set doesn't really have a definite answer yet. A lot of research is being done, old and new devices are being tested (Leap Motion, BackSpin, PSMoce, kinect...). How you will interact will shape your VR experience, this should reflect in your coding and interaction design. In our case, we'll use the good old keyboard and mouse for proof of concept. For ease of use, we'll use bluetooth ones that you would pair with your smartphone.
 
## a) Press the button

The first step is to catch an interaction event. In our case it'll be a key pressed, but you can already think of any other possibilities (especially if you have them at hand!). Before we coded our landscape, now let's sculpt it: pressing a button shall add a cube to the virtual space.

```javascript
function onKey(event) {
  if(event.keyCode == 32) { // Pressed on the space key
    var geometrie = new THREE.BoxGeometry( Math.random()*0.2+0.01, Math.random()*0.2+0.01, Math.random()*0.2+0.01 );
    var mesh = new THREE.Mesh(geometrie, new THREE.MeshLambertMaterial( ));
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
    // We put boxes everywhere inside (size of side is 5)
    mesh.position.set(Math.random()*5-2.5, Math.random()*5-2.5, Math.random()*5-2.5 );
    // We give them a random rotation
    mesh.rotation.set(Math.random()*Math.PI*2, Math.random()*Math.PI*2, Math.random()*Math.PI*2 );
    // 'cause random colors are fun
    scene.add(mesh);
    meshArray.push(mesh);
  }
};
window.addEventListener('keydown', onKey, true);
```

Funky. You know already other kind of possible modification of the space, don't heitate to link those with other keys. The value linked with the key is its [ASCII code](https://en.wikipedia.org/wiki/ASCII#ASCII_printable_code_chart). Ideally, you'd create the box in front of you, as a sculptor of space. Try to think how to do it. If you're a bit lost, wait till section c) where you'll have a similar code.

This is indeed interacting with the world but the interaction (while totally awesome) is not really reinforcing our immersion, which is our aim right now. For that, we want more natural interaction, such that populates our world and use our presence. For instance, interacting by moving around.

## b) Moving around, but not everywhere.

Yep, moving. Classic interaction that one would expect to be able to experience in a virtual world. You're actually almost done: you already know how to apply a transformation to an object in the scene, now you just need to apply that to the camera position. While moving your position along the absolute X, Y and Z axis would work (*camera.position.x += 0.1;*), this won't feel natural. You should rather move around following the relative coordinates of your own camera. Lucky you, there are functions for that:

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

One person's freedom ends where another's begins. Or when you put boundaries. It's all fun to move freely until you end up in the Void just because you can go through walls. For this purpose, you can just translate back the camera inside your boundaries when you're stepping out of them. The following code works for the X axis, we'll let to you to implement the other two.

```javascript
  // Put the code in the onKey function

  if(camera.position.x < -2.3) {
    camera.position.x = -2.3;
  }
  if(camera.position.x >  2.3) {
    camera.position.x =  2.3;
  }
```

Here, we want to retrict the movement to the skybox. What we could do to is actually forbid some movement. If we force our camera to always stay at a z equal to 1 for instance, there won't be flying no more.

## c) Activating Objects
Ok, what we said about natural interactions are true, they help with immersion. Buuuuut, supernatural interactions can also help when they are not felt too much as a switch or an interface. What if you could trigger an event by your proximity to an object? You would interact with object around just by moving in your virtual world. For that to happen, you need to check regularly (**i.e.** in your animate function) your distance to the object that matters.

```javascript
  // In your animate function
  var dist = 0.8;
  if( camera.position.distanceTo(mesh.position) < dist) {
    mesh.material.color.setRGB( Math.random(), Math.random(), Math.random() );
  }
```  

While you have here a simple interaction, you can know trigger any piece of code and behaviour you like just by proximity to an object. Unfortunately, the objects have no physicality: you can go through them. To change that, we'll prohibit the user to enter a distance too close to the object. While we could make real collision detection, this is out of the scope of the workshop. Any idea how to do so? You can have a working example by mixing the two previous code (camera position restriction & objet distance to camera). Below is a proposed version based on .... 3D geometry! yay!

```javascript
  // Put the code in the animate function
  for(var i = 0; i < meshArray.length; i++) {
    // We measure if, and how much the camera is inside the cube
    var distN = 0.8 - camera.position.distanceTo(meshArray[i].position);
    if( distN > 0) {
      // We find the unitary vector defining the direction
      // from the mesh to the camera
      var vecUnitaire = new THREE.Vector3( 0, 0, 0 );
      vecUnitaire.add(camera.position);
      vecUnitaire.sub(meshArray[i].position);
      vecUnitaire.normalize();
      // Then we push the camera of distN along this direction 
      camera.position.addScaledVector(vecUnitaire, distN);
    }
  }
  
```

Neat. Test it around, now you feel more in not just a rendered world but also in a physical world. Our immersion is getting better by the minute! In this new physical world and with this new kind of interaction, you have a full toolbox to already create stories, games or experiences (a disco floor that reacts when you move on it? [Billy Jean](https://youtu.be/Zi_XLOBDo_Y?t=18s) for the win.). You could imagine a labyrinth for instance, or a puzzle game where the aim is to activate objects in a particular order in order to go to the next level. VR -as any other medium- is not just about the skills you get. They are the basics, but the point is how you use them. Try to think of a little story or game that would rely mostly on what you learned up until now, and see if you can make it happen.

Seriously, you can already do a lot. You'll be able to do even more with what comes up next but don't underestimate what you can already do. Don't hesitate to take a breath, explore already the potential and express your creativity.

## d) Gaze
One the one hand, we try to deepen the immersion by making VR look like real reality. On the other one, VR has a lot of potential on its own, would that be graphic wise or interaction wise. Another possibility for the later is to base your interaction on the gaze of the user. You can imagine it as a pointing device, the direction of your sight as a straight line going from the camer position to an infinite target. When you look at an object this straight line intersects with it. This is similar to what we saw before by comparing the position of the camera with the position of an object. The main different is that we only compare the position of the object with the line. Based on this information, you can have behaviors triggered either when you gaze over an object, or when you push a button while gazing it.

Luckily for us ThreeJS has again all the tools in hand using [Raycaster](http://threejs.org/docs/#Reference/Core/Raycaster) and its *intersectObjects()* method. As it can be a bit tedious still to handle several object we will rely on the [vreticle](https://github.com/neuman/vreticle) library for convenience. Don't forget to import this library too!

```javascript
  // Create your reticle after having created the camera
var reticle = vreticle.Reticle(camera);
```
  
```javascript
  // For all current meshes (don't forget to add new ones on the fly), do :
  for(var i = 0; i < meshArray.length; i++) {
    // Add them to the reticle
    reticle.add_collider(meshArray[i]);
    
    // Code what a long gaze will trigger
    meshArray[i].ongazelong = function(){
      this.material.color.setRGB( Math.random(), Math.random(), Math.random() );
    }
  }
```

```javascript
  // Last, don't forget to update your reticle in your animate loop
  reticle.reticle_loop();
```

You have also access to *ongazeover()* and *ongazeout()* which are working in a similar manner. While as always we give prototypical examples, feel free to explore the new possibilities of such interactions.
 
## e) Moving Objects

So, while it's nice to discover new kind of interaction, it's as important to see what one can do with it: the creative aspect. Now, we're going to reuse the gaze interaction in order to allow the user to move object. For that, he will be able to lock an object he's gazing at, keep it in sight while moving around, and then release it.

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
