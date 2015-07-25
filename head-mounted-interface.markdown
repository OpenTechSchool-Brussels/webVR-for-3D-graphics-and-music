---
layout: default
title:  "Head Mounted Interface"
num: 4

---

This workshop is still in beta, and you're getting to the bleeding edge part of it! Here are more ideas to explore than actual code to run. Feel free to explore the possibilities and if you have any questions or issues, be sure to refer to the coaches :) Enjoy the ride, even if now a bit more bumpy! And if you have any comments, please tell us, this part ( and the next) is a work in progress and is meant to be released for the next workshop.

## a) Passive Display
The idea here is to have a simple display, not one that you can interact with. You can think of a few icons or text that updates you on your situation, the kind of stuff you often see in games but ... this time you're living the game!

Ideas:

* visuals that follows the head
* Display a Map on top (location of cubes), and other stuff on the sides.

// Rez: Number of cube activated / Number of cube. Position in Space .... + map + "gun sight" for the style.

## b) Active Interface
An experimentation. What about if you had this menu in front of you, static, and that you could active its element with the *onlonggaze()* function we saw previously? Might make you dizzy, but there is a price for super power! We'll see together if it's a working choice.

Ideas:

* A menu appears at a press of a button (circular dock in the center of sight), fixed (if you move, it stays in place, rotation wise, not translation wise), and you can select by staying focus (or by pressing the menu button when selecting a stuff)
* Try a few different menus. Active menu appears in center. Active menu that can still display passive state stays in the corners.
* Among others, button for action "create cube", and another for states "colors"

## c) Located interface
In previous sections, the interface was link with our camera. Such interface or simple displays can also be locked on an object, giving us information over it (how much energy an enemy has) or new possibilities of interaction (like in an RPG game for instance).

Ideas:

* add information on top of other objects (ex: description of a cube and the sound, inspiration from biology drawing)
* add a menu to alter the object's propriety
