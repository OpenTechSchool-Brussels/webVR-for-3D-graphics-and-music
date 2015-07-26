# webVR-ftw
 Course on webVR, made for OTS Brussels.
 
 To Do for next version of material:
 
 * Get rid of already legacy code: why starting with a Square renderer when after you're full screen?
 * We start with a far away camera, and then a super close by one. Be coherent (and be close by).
 * Use a local version of three.js (provided by us) if sometimes it's not working. (don't use the min three.js)
 * offering the possibility to use CARDBOARD DEBUG something.... to force splitting (with polyfill.js).
 * Get a working light code (try with flat shadding -> move away from MeshBasicMaterial to MeshPhongMaterial)
 * Get coherent values all along.
 * Overall go through the workshop taking code from nowhere else and see how it goes
 * Reorder the workshop (and think back on the lenght of SettingUp) so that: after 15 min : cube in 3D, after 30 min: stereovision (if possible with head control)
 * Récupérer le code "control.reset" truc muche à la pression d'espace, histoire de pas avoir de torticolis...
 * Add ways to "stop" the music, or loop it.
 * Insist on the problems of XHR and consequently forces people to host (proper httpd) their own code, either via our solution (e.g. neocities) or if they prefer their own server
 * Insist on the risk of having multiple versions of the libraries, try to stick with one! (due to multiple problems related to using an older version of threejs)
 * Make links to code repository for libraries easier to find
 * List the typical mistakes involved in 3D and VR (e.g. objects beind you, too far, too close, no relfecting material, object with only one face material, etc)
 * Clarify that require() is NOT related to client-side JS execution but rather related to node.js (means the person MUST use an httpd, not file:///home/participant/myVirtualWorld.html !)
 * Visually clarify the key structure of the boilerplate (setup() render() animate() and the several events to catch)
