# webVR-ftw
 Course on webVR, made for OTS Brussels.

STRUCTURE
 * (F) Create a working HTML file with associeated with library files, all that in a zip file.
 * (F) Visually clarify the key structure of the boilerplate (setup() render() animate() and the several events to catch)
 * (F) Voir si on peut faire une skybox en simple (si possible sans texture) (si super simple, ok dans boiler plate)
 * (R+F) Reorder the workshop (and think back on the lenght of SettingUp) so that: after 15 min : cube in 3D, after 30 min: stereovision (if possible with head control)

LEGACY CODE
 * (R) Legacy code: why starting with a Square renderer when after you're full screen?
 * (R) Legacy code: We start with a far away camera, and then a super close by one. Be coherent (and be close by).
 * (R) Legacy code:  Get coherent values all along.

GRAPHICS
 * (R) Propose better graphic outputs (nicer stuff to look at, not just one cube, or random stuff) -> cube of cube, a better random landscape of many cube, a ground made of cube, "rectangles" cubes on the ground (à la earthquake effect in some RGP), ...
 * (R) Check back how to simplify color, lights ....(try with flat shadding -> move away from MeshBasicMaterial to MeshPhongMaterial) => See if you can accelerate going to VR
 * (R) ref graphic viz cube : https://github.com/jiin/dancing-cube

VRAC
 * (R) Apparemment pas necessaire d'ajouter la camera avec .add(camera). Car on affiche une scene et une camera. A checker.
 * ref for multiplayer stuff: https://blog.pusher.com/building-3d-multiplayer-game-pusher/
 * offering the possibility to use CARDBOARD DEBUG something.... to force splitting (with polyfill.js).
 * Récupérer le code "control.reset" truc muche à la pression d'espace, histoire de pas avoir de torticolis... reset camera, probablement. A REVERIFIER QUAND ON AURA LE TEMPS
 * (R) Add ways to "stop" the music, or loop it.
 * (R+F) List the typical mistakes involved in 3D and VR (e.g. objects beind you, too far, too close, no relfecting material, object with only one face material, etc)
 * AVancer sur WEBVR
 * Voir si on peut s'en faire un truc tout les deux (projet etc etc)
 * Voir comment rajouter les modules de cours de dessus (kinect etc etc)

