---
layout: default
title:  "Music is my life, and now both are in 3D."
num: 3

---

We saw that senses and actions in the graphic world are pretty powerful to create an immersive experience. While you can always upgrade the visuals and better the interaction, there are other roads to follow to create a fuller experience. Big surprise, in this case it'll be about sound. Let's make our experience multimodal with spatialised sound!

## a) Playing sound
Ok, let's start first with some simple stuff. Let's see how to trigger a sound. Pretty basic:

```javascript
  // First charge the sound you want to use
  var audio = document.createElement('audio');
  var source = document.createElement('source');
  source.src = 'test.mp3';
  audio.appendChild(source);
```

```javascript
  // Then use it whenever you want (like when you're close to an object)
  audio.play();
```

Well, be sure to launch the sound when you got close to an object, and not launching it continuously whenever you're close enough.

Now that you can trigger sound, you can create you're own virtual instrument. Imagine a set of cubes along a circle, each associated with a different instrument or riff. A pretty cool (if messy) way to play music! Try to instead associated those cubes to files you have in your music library, and you have the most insane music launcher.

## b) Playing a 3D sound
Well, we have music. But not yet in 3D. How can we simulate the 3D? Well, first of all with the amplitude of the sound. In short: if you talk to me close by, I'll hear it more (and will feel we have a pretty deep connection); if you to talk to me from afar, I'll hear it less (and feel we're losing touch...). Romantic relation apart, let's just associate the volume with the distance between the location of the sound and the camera.

First add a sound as we did before, and launch it continuously. And then, to control its volume:

```javascript
  // In the animate function

  // if you linked the position of your sound with that of your cube:
  var radiusOfSound = 5;
  var distance = cube.position.distanceTo( camera.position );
  if ( distance <= radiusOfSound ) {
    audio.volume = volume * ( 1 - distance / radiusOfSound );
  } else {
    audio.volume = 0;
 }
```

Now you can create a whole 3D experience with sounds that grows louder as you get closer. If for some it's time to release your inner musician, for other it's time to create a game where the user needs to find one cube among many just based on the sound. And once you do it, let us know, we want to play it!

## c) Stereo in 3D
So, now you're having the idea of proximity. It's good, but not enough. If we're having two ears, it's to be able to gauge direction too and not just proximity. Let's try to hear in stereo and see how much that changes about our virtual world. The best for that is to actually link the sound with a moving object, like the one you had moving around the user.

<img src="3Daudi.png" width="100%">


For that...

```javascript

```


## d) Binaurial sound
Okkkkaayyyyy, well, Binaural sound is pretty cool. It tries to actually simulate your real hearings, based on many things. For instance how much you hear of the sound depending on the orientation of each ear, the difference of time or arrival of sound for each of your ears related with their positions, or even the morphology of your ear (using convolutions and lovely mathematical stuff of the same genre).

In short: this is super duper cool, but waaaay out of the scope of this workshop!
