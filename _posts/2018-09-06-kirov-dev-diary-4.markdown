---
layout: post
title: Kirov dev diary 4
date: 2018-09-06T16:00:00.000Z
description: New additions
img: previews/kirov-4.png
published: true
category: development
tags:
  - java
  - game engine
---
## New Additions
<br>


It’s been a while since the last dev diary. A lot of new additions have been made to the game engine. Starting with:

#### Input Manager
The input management system (and entire manager system) was reworked and reimplemented to be easier to use and more concise. The system now uses custom listener to send out messages to all subscribed classes when an input is detected. Using java’s inbuilt input detection listeners, the Input Manger converts the data into a ShardInputData instance which is sent to all subscribed classes, decoded into game actions.

``` java
private static final char VK_TOTAL_KEYS = 0xFFFF;

public enum MouseState {
    LEFT_DOWN,
    LEFT_UP,
    MIDDLE_DOWN,
    MIDDLE_UP,
    RIGHT_DOWN,
    RIGHT_UP,
    DRAGGED,
    NONE,
}

//Mouse
public MouseState mouseState = MouseState.NONE;
public int clickedX, actualX, releasedX;
public int clickedY, actualY, releasedY;

//Keyboard
public boolean keys[] = new boolean[VK_TOTAL_KEYS];

public void reset(){
    mouseState = MouseState.NONE;
    actualX = 0;
}
```
Above is the data contained within ShardInputData. For example;
 a button could decode the data to detect where the mouse is, if the mouse is on top the button checks to see if it was left clicked. If it was left clicked perform button related actions. 
This way at each interval the program checks if it needs to continue decoding the information which should help with efficiency when there are many more buttons and listeners.

#### Vectors
The engine now implements much better vector logic allowing from static operations and instance-based operations. For example:

``` java
vec3 = Vector2.add(vec1, vec2);
vec3.multiply(vec4);
if(Vector2.compare(vec3, vec2)){
	vec3.multiply(Vector2.zero())
}
```
Are now all acceptable.

#### Sound and Audio
Kirov now has an audio system that uses java’s inbuilt ‘Clip’ functionality to allow for looping sounds (music) and single audio clips. The audio managers allow for music and sound effect individual volumes as well as individual clip audio.

#### Kirov Splash Screen
There is now a splash screen when booting up a Kirov engine game.
<img src="/SamHibb.github.io/images/kirov-splash.png" alt="kirov-splash " width="700" height="400">
<br>

#### GUI System (Coming soon)
Currently being developed is a new GUI system designed to be as simple and intuitive as possible. It will be made to be easily customizable and easy to implement. The plan is to make a variety of different GUI elements that can be added into a scene easily. Elements such as buttons, tooltips and drop-down menus all with pre-programmed logic.
<img src="/SamHibb.github.io/images/kirov-ui-1.png" alt="kirov-ui " width="700" height="400">
<br>

