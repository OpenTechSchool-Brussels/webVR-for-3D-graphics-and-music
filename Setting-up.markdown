---
layout: default
title:  "Setting up"
num: 0

---

Before diving into coding, let's review a few points and make sure that our environment is safely set up.

## 0) What is Virtual Reality?
Virtual reality is a digital world that feels real. It usually rely on either a screen or a head mounted display to show you the world around. Some allow you to navigate the worlds, interact with the world, and with potential other people inside it. 

Since it's digital, it can cover a huge span of simulated experiences; from the most mundane act of daily life -walking in the street of a city-, to the most exotic and excite scene -enjoying a roller coaster between planets of our solar system-. While it's often associated with leisure, VR has also a strong potential with education, rehabilitation and work environment.

While VR was seen as SciFi at best and a non working gadget at worse, with the rise of video games, personal hand held devices and 3D, VR is getting mature enough to be a real player in the digital age. Not only is it mature, but it's getting affordable. Relatively cheap solution are now possible to enjoy VR and it created an enthusiasm for it that is reflected in the updated and more comfortable ways to create/code for VR.

## a) Discover WebGL
All in all, WebGL is OpenGL for the web. OpenGL is a graphic specification, a set of tools that helps you draw over your screen, would that be in 2D or in 3D. Since it's working on multiple platforms, it became soon a reference for 3D graphics. WebGL extends the HTML5 canvas element (used for rendering) to provide 3D graphics. With it, you're allowed to draw in 3D in your web browser. This is pretty cool because it allows you to show your work to nearly all devices without asking to install anything. Let's check if it works on yours: can you see the rotating cube below? If not, you'll need to update or change your configuration.

<iframe width="100%" height="450px" scrolling="no" src="https://get.webgl.org/" frameborder="0" allowfullscreen></iframe>

Since WebGL is not a programming language but a spcecification, it relies on some other programming language. Being on the web and working on the client side, it's JavaScript. While you don't need to be a pro of JavaScript to follow this workshop, the material won't be about learning JavaScript. If you're unsure about your level, you can always check [this cheat sheet](https://media.pearsoncmg.com/ph/esm/ecs_snyder_fluency_6/javascript_refererence.pdf) or this [reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference).

Coding straight in WebGL can be pretty bothersome. Instead, we'll here use [Three.js](http://threejs.org/), a library that takes care of the boring part. It provides primitives, handles textures, offers easily specific virtual reality functions (e.g. stereoscopy, basic head motion tracking). While you do not need Three.js to make VR on the web, it makes it easier.

## b) Putting your code on a server
Since we're using web technology, we're going to need a sever to host our code (espcially that it'll need to be accessible by your smartphone). While you could think of creating the files on your own computer and loading it in your web browser, this create a lot of issues at some point. Don't do that.

There are many way to put your code on a server. If you have already yours, no need to change it. If you're tech savy, you might want to use your computer as a server or host your code on [github](https://pages.github.com/) and host it through [github pages](https://pages.github.com/). If you have no idea about all that then a simpler solution is to use a free web hosting platform (doing the work for you) like [neocities](https://neocities.org/) which gives you 100 Mb to host simple webpages. Just create an account and you're up and running. You don't want to create an account? Worry no more, we have one already made for you, just be aware that anybody can edit/erase your code! [Login](https://neocities.org/signin): webvr, password: webVR.

## d) Virtual Reality Vs classic 3D rendering
Up until now, what you played with is 3D. It's already a step in the right direction, but it still feels a bit distant, you are watching a world rather than being in it: a very limited immersive feeling. Not only does it feels flat and remote, but everything around you is still in sight. The recent popularity of VR started with the [Oculus Rift](https://www.oculus.com/en-us/), a full headset similar to what was done up until now, just much better and cheaper. At the same time emerged the idea to use another device with growing quality of sensors and screen: the smartphone. Now, in 2015, with your phone, some simple lenses and a piece of carton, you can experience a whole new reality! Curious about all that? You can get more reference about basic concepts of VR from the [Mozilla WebVR API page](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API/WebVR_concepts).

![HMD with smartphone](https://mdn.mozillademos.org/files/11085/mobileBasedVRSetup.png)

So, what is the difference between that 3D and VR? We already saw it was about getting your whole vision involved. For that, you can't have one big screen close but two little ones, one for each eye, that will simulate what each of your eyes would see from this world. Not very different, but feeding a slightly separation of point of view will make all the difference. Then, to reinforce your presence in this world, we will link our head shifting to our gazer. In short, if you move your head, you will move your point of view in the world accordingly. Last, we will make use of spatialised sound in order to fool one more of our senses.


## e) The next frontier of VR: webVR
One of the key concept of our journey will be webVR. What does it mean to apply the web concept to VR? It means being able to visit virtual worlds not as opaque bundled applications to install but through modern browser ([bonus arguments on why it matters](https://www.reddit.com/r/WebVR/comments/3e2mes/what_are_the_advantages_of_webvr_over_native_vr/  )). It means visiting worlds on the web. The fundamental appeal of the net was hypertext, this ability to surf the web, from pages to pages, and not to just stay on one specific resource. Right now, VR experiences are pretty isolated from one another. This is not a necessity, VR worlds could be linked together and you should be able to navigate from one to another as freely as you click on a link on a webpage. While this concept is still pretty new, we want to give you a taste of it, not just as consumers but also as creators.

<img alt="under the hood" src="http://sales-patch.com/wp-content/themes/sales-patch/images/home/icon-hood.png" height="64px" /> A neat feature of webVR is also that the code can be read. You see a nice demo and you want to understand what is going on? Just display the source! Make sure you "cheat" this way during this workshop BUT do not copy and paste mindlessly, remember what you came here for.

<a href="#browser" /></a>This is still bleeding edge stuff so some only some browsers support the webVR API. Most recent Android phone do via Chrome so if you bought your phone a year or two ago, just try <img alt="QRTtest" src="http://webvr.neocities.org/img/qrtest.png" height="64px" /> it should work out of the box! If not simply get a [Chrome](https://drive.google.com/folderview?id=0BzudLt22BqGRbW9WTHMtOWMzNjQ&usp=sharing#list) or [Firefox Desktop](http://mozvr.com/downloads/) or [Firefox Mobile](https://nightly.mozilla.org/) nightly build with webVR support and try again.

Since you will be working mostly in the browser take advatange of it. Most browsers now feature a console to debug, display values, error messages, etc. Do display it in order to spot problems early rather than just a bare blank dark cold screen. 
