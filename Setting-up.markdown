---
layout: default
title:  "Setting up"
num: 0

---

Before diving into coding, let's review a few points and make sure that our environment is safely set up.


## 0) What is Virtual Reality?
Virtual reality is a digital world that feels real. Since it's digital, it can cover a huge span of simulated experiences; from the most mundane act of daily life -walking in the street of a city-, to the most exotic and excite scene -enjoying a roller coaster between planets of our solar system-. While it's often associated with leisure, VR has also a strong potential with education, rehabilitation and work environment.

While VR was seen as SciFi at best and a non working gadget at worse, with the rise of video games, personal hand held devices and 3D, VR is getting mature enough to be a real player in the digital age. Not only is it mature, but it's getting affordable. Relatively cheap solution are now possible to enjoy VR and it created an enthusiasm for it that is reflected in the updated and more comfortable ways to create/code for VR.

## a) Discover WebGL
All and all, WebGL is OpenGL for the web. OpenGL is a graphic library, a set of tools that helps you draw over your screen, would that be in 2D or in 3D. Since it's working on multiple platform, it became soon a reference for 3D graphics. WebGL extends the HTML5 canvas element (used for rendering) to provide 3D graphics. With it, you're allowed to draw in 3D in your web browser. This is pretty cool because it allows you to show your work to nearly all devices without asking to install anything.

Indeed, *nearly* all devices. Let's check if it works on yours. Can you see the rotating cube below? (Be sure to check on your computer and your smartphone). If so, then great, you're already nearly all set! If not, update your drivers and web browser or chose one that is compatible in the extrem case yours is not.

<iframe width="100%" height="650px" src="https://get.webgl.org/" frameborder="0" allowfullscreen></iframe>

Since WebGL is not a programming language but a library, it relies on some other programming language. Being on the web and working on the client side, it's JavaScript. While you don't need to be a pro of JavaScript to follow this workshop, the material won't be about learning JavaScript. If you're unsure about your level, you can always check [this cheat sheet](https://media.pearsoncmg.com/ph/esm/ecs_snyder_fluency_6/javascript_refererence.pdf) or this [reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference).

While we could code directly with JavaScript and WebGL, this can be sometimes pretty bothersome since WebGL can be pretty complicated at a time (because you need to be very precise in what you do). Instead, one can use another Javascript library that would abstract calls to WebGL and make it easier to draw over your screen. If WebGL allow you to draw on your screen in 3D, such a library would make it easier. In our case, we chose Three.js as the library. It provides primitives, handles textures, offers easily specific virtual reality functions (e.g. stereoscopy, basic head motion tracking). While you do not need Three.js to make VR on the web, it makes it easier.

## b) Running your first program in threeJS
Now we know that WebGL is working, let's push it forward and get a running example of Three.js working on your computer and smartphone. For that, let's use an online HTML editor. Have a go at [http://mrdoob.com/projects/htmleditor/](http://mrdoob.com/projects/htmleditor/). As you can see, it just works, just simple HTML code and a JavaScript library imported. There is nothing to install, assuming you have a modern browser that passed the previous test.

While that's already pretty fun, you might want to explore a bit already and change the code (for instance at the end of line 32 change the line width from 2 to 9), the result of which are seen on the fly. Change some code, break it, heal it, but don't spend too much time here, the rest of the workshop is waiting for you!

## c) Putting your code on a server
This was a pretty neat way to discover Three.js and to get straight to coding but you will pretty soon want to be able to save your code, organise it and put it online on a server so that you can access it from different device, or even simply open it up to the world. That last point is important because while we will be coding from our computer, we will need our result to be displayed over our smartphone.

For that, we need to host our website on the net, which can sometimes be an annoying process when you're discovering it. As far as we thought, we have three main options (beside getting a server right now!). First, you might already have a server (your own computer or other) which makes it a non problem if you're OK with using it. If you're familiar with [github](http://github.com), you might want to use them as a web hosting service (with [github pages](https://pages.github.com/). And the simplest solution: use a free web hosting platform. Among others, you can use [neocities](https://neocities.org/) which gives you 100 Mb to host simple and static webpages. If you're not sure, this would be the solution we recommend.

## d) Virtual Reality Vs classic 3D rendering
* What you played with so far is 3D. That's nice but that feels a bit distant, you are watching a world rather than being in the world. There is a very limited immersive feeling. Everything around you is still there, the 3D feels flat and remote. Virtual reality and the recent popularity wave started with Oculus Rift. Basically sensors in the phones got a lot cheaper, screens got a lot better, speed of sensors improved dramatically (probably because of car safety) and now, in 2015, your phone with some simple lenses and a piece of carton can offer you a whole set of crazy worlds!
* From 3D rendering to virtual reality we need few minimal steps that we will explore together. 
* The first will be to duplicate the view, we have 2 eyes with a slight seapration, we need to account for it. 
* Also when we move our head to shift our gaze, our eyes move, we need to also update our view accordingly. 
* Finally when we turn our head our ears move, the perception of sound is thus modified allowing us to locate sources of sound and vice-versa know our position in space.
* All this and a lot more makes for virtual reality a real exciting technical challenge!
* ![HMD with smartphone](https://mdn.mozillademos.org/files/11085/mobileBasedVRSetup.png) Explore more basics concepts with the Mozilla WebVR API page https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API/WebVR_concepts

## e) One possibilty for VR: webVR
* By now you should convinced that webVR via ThreeJS is one of the easiest entry point to discover and play with virtual reality with existing and widly available hardware, that means your phones, your friends phone and some basically lenses.
* Time to upload your toy page on our server (and small tuto on how to do it on your own when you're back home)
