---
layout: default
title:  "Setting up"
num: 0

---

Welcome to our webVR for 3D graphics and music workshop. During the next hours you will start to build a virtual reality environment. We will remain available to help you, answer any question and find the solution with you. The goal is to provide a fun learning process. There is NO ideal world to show in the end, there is no target except learning while enjoying the day. If at the end of the day you have something gorgeous you want to show to the world, great, if in the end you only have ideas and a better understand on HOW you will be able to make it work during the next few weeks, it's also great!

Now for this to begin we have to share some basic principle and tools :


## 0) What is Virtual Reality?
* Virtual reality is a digital world that feels real. Being digital it can simulate the most mundane act of daily life, walking in the street of a city, to the most exotic and excite scene, enjoying a roller coaster between planets of our solar system. 

## a) Discover WebGL
* WebGL is basically OpenGL for the web. OpenGL is basically a graphic library that works on multiple plateforms. It is THE reference for 3D Graphics. WebGL extends the HTML5 canvas to provide 3D graphics in the browser. That means allowing nearly all devices to have 3D animations without having to install anything. (GL on the web, using Javascript, if you're unsure ..... you can check cheatsheet here, or tutorial here).
* ![WerbGL stack](http://www.ibm.com/developerworks/library/wa-webgl1/figure3.png)
* Now how does all this look like? Do you see the cube on your computer AND your phone? Then you are nearly all set!
* <iframe width="100%" height="500" src="https://get.webgl.org/" frameborder="0" allowfullscreen></iframe>
* Why threeJS? ThreeJS is a relatively new Javascript library that helps to build 3D applications on top of WebGL. It provides primitives, handles textures, offers easilly specific virtual reality functions (e.g. stereoscopy, basic head motion tracking). You do not need ThreeJS to make a webVR application but it helps.

## b) Running your first program in threeJS
* After all this blabla what about a 3D working example on your computer, phone or laptop? Have a go at http://mrdoob.com/projects/htmleditor/ , as you can see there is nothing to install, assuming you have a modern browser that passed the previous test it "just works". 
* That's fun but we are not here to just enjoy what exists, we are here to build, to make! Now change a bit the code. For example at the end of line 32 change the line width from 2 to 9. Instantly you can see the result being updated. Feel free to explore more, change untils it breaks and then simply refresh the page to get back to the start. 
* You can also see that this is JUST basic HTML with a Javascript library imported, nothing more.

## c) Putting your code on a server
* Now that was a convenient way to discover. In order to go further IN the virtual reality world you will build you need virtual reality glasses. Your page thus need to be accessible from your phone.
* The easy way is to use our group Github account. Please ask us the login and password and you can start to edit your world!

## d) Virtual Reality Vs classic 3D rendering
* What you played with so far is 3D. That's nice but that feels a bit distant, you are watching a world rather than being in the world. There is a very limited immersive feeling. Everything around you is still there, the 3D feels flat and remote. Virtual reality and the recent popularity wave started with Oculus Rift. Basically sensors in the phones got a lot cheaper, screens got a lot better, speed of sensors improved dramatically (probably because of car safety) and now, in 2015, your phone with some simple lenses and a piece of carton can offer you a whole set of crazy worlds!
* From 3D rendering to virtual reality we need few minimal steps that we will explore together. 
* The first will be to duplicate the view, we have 2 eyes with a slight separation, we need to account for it. 
* Also when we move our head to shift our gaze, our eyes move, we need to also update our view accordingly. 
* Finally when we turn our head our ears move, the perception of sound is thus modified allowing us to locate sources of sound and vice-versa know our position in space.
* All this and a lot more makes for virtual reality a real exciting technical challenge!
* ![HMD with smartphone](https://mdn.mozillademos.org/files/11085/mobileBasedVRSetup.png) Explore more basics concepts with the Mozilla WebVR API page https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API/WebVR_concepts

## e) One possibilty for VR: webVR
* By now you should convinced that webVR via ThreeJS is one of the easiest entry point to discover and play with virtual reality with existing and widly available hardware, that means your phones, your friends phone and some basically lenses.
* Time to upload your toy page on our server (and small tuto on how to do it on your own when you're back home)
