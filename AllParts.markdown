---
layout: default
title:  "All parts"
num: 1

---
#Goal
Becoming able to participate to the webVR revolution instead of taking the VR wave in the face and solely consuming VR content.

#Supporting format
Interactive musical and visual world.

#Requirements

##Skills
Basics of programming (what is an object)

##Technical
Having a computer with a modern up to date browser that supports VR.
Having a text editor.
Having an internet connection.

#Steps with their key concepts

## 0 : Setting a 3D scene
This scene will display a 3D cube with a basic material thanks to a camera pointing at it and a light illuminating the scene. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw1.jpg)

### Key concepts
* 3D primitive
* camera
* light
* properties of a primitive

### Degrees of freedom
* size of the cube
* colour of the cube
* type of light

### Provocative problems left
* no person on the scene
* no floor to stay on
* no VR effect
* no limits to the scene

## 1 : Experiencing VR
This scene will now display the same 3D cube but rotating around the position of the camera. The target of the camera will follow the movement of the viewer (i.e. head tracking). The stereoscopic effect will be applied. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw2.jpg)

### Key concepts
* object movement
* stereoscopic effect
* head tracking
* relative vs absolute coordinates

### Degrees of freedom
* position of the cube
* speed of the movement of the cube

### Provocative problems left
* no person on the scene
* no floor to stay on
* no limits to the scene
* no possibility to walk around

## 2 : Natural interactions
This scene will now display the same 3D cube but in a corner of a plane at floor level. Pressing a button on a Bluetooth gamepad or keyboard will change the position of the camera. The user, equivalent to the camera, will be able to move around only within the boundaries of that plane. Touching the cube will activate it by changing its colour and turning on a light. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw3.jpg)

### Key concepts
* handling keypresses
* camera movement
* scene boundaries
* objects as activable

### Degrees of freedom
* speed of movement
* size of the plane
* exploring the scene in VR

### Provocative problems left
* no resistance with the cube

## 3 : Magical interactions
This scene will now display the same 3D cube in the same corner of a plane at floor level with the same ability to activate it by touching it. The 3D cube will be positioned over a coloured plane of the same surface. The camera will be pointed nearby but not at the 3D cube. On the side of the camera position a plane the size of the 3D cube will be visible. Pointing the camera at the 3D cube will not activate it. It will though change its appearance when it is looked at for more than few seconds. The cube will then come closer to the camera position slightly higher than plane level. The position of the 3D cube will follow the camera until it roughly matches the new plane. Once looked at for few seconds the 3D cube will fall on top of the plane. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw3.jpg)

### Key concepts
* do it yourself physics
* z-buffer collision
* ray-picking
* tweening

### Degrees of freedom
* moving the 3D cube back and forth

### Provocative problems left
* no gravity
* only one cube
* only two picking and dropping positions

## 4 : Multimodal scene
This scene will now display several 3D cubes and several coloured planes. Planes will be added on the border of the main plane to form walls. Touching a cube will activate as before but also start a sample played on a loop. A cube activation will create a visualization on the the walls. The sounds of a 3D cube will be localized, the closer the camera will be position to it, the louder the loop will be and vice versa. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw4.jpg)

### Key concepts
* 3D visuals is only one medium
* one medium can be associated to another

### Degrees of freedom
* activating cubes
* visualizing each loop
* moving cubes around, organizing the space cognitively and musically

### Provocative problems left
* what other medium can be used
* what is more efficient than ray-picking
* simply cubes and square planes

## 5 : Interface
This scene will display the same scene but now a small plane in the corner of the scene will follow the camera. Pressing a button on a Bluetooth gamepad or keyboard will display the plane closer to the centre of the scene and block it there. Head movement will consequently not change its positions, user movement will be impossible. A current item of the menu will be selected. Using ray-picking few-seconds on it will activate the item. The item will change the scene globally by turning on and off the light. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw5.jpg)

### Key concepts
* floating interface
* switchable modes of interactions

### Degrees of freedom
* style of the menu
* action of the items of the menu

### Provocative problems left
* can the interface change multiple objects of the scene simultaneously
* can the interface be object dependent

## 6 : Linking between scenes
This scene will display the same scene but a plane will be added as a ceiling. The ceiling will showcase as a texture a screenshot of the scene from another participant. The other participant will provide a direct link to his scene (either using a public facing web server or sharing the files of the scenes, consequently making it stateless). Looking at the plane for 10 seconds will trigger an open link action, opening the page of the scene. Participants will then form a ring of interlinked scenes. ![](http://fabien.benetou.fr/pub/home/WebVRWorkshop/illustrations/draw7.jpg)

### Key concepts
* link between scenes

### Degrees of freedom
* use a half-dome or another shape instead of a plane

### Provocative problems left
* why not stateful
* can this work across platforms, across browsers
* is it safe
