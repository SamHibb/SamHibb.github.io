---
layout: post
title: Kirov dev diary 2
date: 2018-01-06T14:00:00.000Z
description: New stuff!
img: previews/kirov-2.png
published: true
category: development
tags:
  - java
  - game engine
---
## New Stuff!
<br>

Kirov has had quite a few changes since the last update. Starting with:

#### Entities

<img src="/SamHibb.github.io/images/kirov-entity1.gif" alt="kirov-entity-demo" width="700" height="400">
<br>

Entities have been implemented into Kirov. In the above demo, you can see an (unfinished) Tank sprite that can be clicked and moved about in a typical RTS style. Currently there is a basic way of moving the entity:

```java
public void update(){
        if(position.x < target.x) {
            position.x++;
        }
        else if (position.x > target.x){
            position.x--;
        }

        if(position.y < target.y) {
            position.y++;
        } else if(position.y > target.y){
            position.y--;
        }
}
```

All this snippet does is move along the axis till it reaches the correct point. This is a placeholder until I implement an A* movement system. 

#### Map and tile transition changes

Also updated was the map and sprite set. Now there is a system to handle sprite transitions on the map. It works by using a generated file which details the exact transitions between sprites and what sprite to use in that case. The engine uses custom files for its map. Currently it uses an image file as before to load the actual tiles and a “.kirov” file with all the map config data. An example for this file:
```
TRANSITION_TABLE {
	WATER_GRASS_UL = WATER_GRASS_UL
	WATER_GRASS_U = WATER_GRASS_U
	WATER_GRASS_UR = WATER_GRASS_UR
	WATER_GRASS_L = WATER_GRASS_L
	WATER_GRASS_R = WATER_GRASS_R
	WATER_GRASS_DL = WATER_GRASS_DL
	WATER_GRASS_D = WATER_GRASS_D
	WATER_GRASS_DR = WATER_GRASS_DR

	WATER_GRASS_/L = WATER_GRASS_/L
	WATER_GRASS_/R = WATER_GRASS_/R
	WATER_GRASS_\L = WATER_GRASS_\L
	WATER_GRASS_\R = WATER_GRASS_\R
	
	BEACH_GRASS_UL = BEACH_GRASS_UL
	BEACH_GRASS_U = BEACH_GRASS_U
	BEACH_GRASS_UR = BEACH_GRASS_UR
	BEACH_GRASS_L = BEACH_GRASS_L
	BEACH_GRASS_R = BEACH_GRASS_R
	BEACH_GRASS_DL = BEACH_GRASS_DL
	BEACH_GRASS_D = BEACH_GRASS_D
	BEACH_GRASS_DR = BEACH_GRASS_DR
	
}

TILE_CODES {
	GRASS = 0xFF007F0E
	BEACH = 0xFFFFD800
	WATER = 0xFF0026FF
}
```
Using this the engine can tell what sprite needs to be loaded for example; a transition between Grass and a water tile above it would be: Water -> Grass Upper, which is WATER_GRASS_U, this is what Kirov knows the sprite as. Then the engine looks for the corresponding user’s name for the sprite (in this case it is the exact same name) and uses that to load the sprite.

<img src="/SamHibb.github.io/images/kirov-map2.png" alt="kirov-map" width="700" height="400">
<br>

<img src="/SamHibb.github.io/images/kirov-map3.png" alt=" kirov-map " width="700" height="400">
<br>

<img src="/SamHibb.github.io/images/kirov-map4.png" alt=" kirov-map " width="700" height="400">
<br>
(In this last example image the first water tile doesn’t have a sprite for a transition on all side so it uses the default)

#### UML

There have been some major changes to the UML diagram with new classes and changes to old classes:
<img src="/SamHibb.github.io/images/kirov-UML2.png" alt=" kirov-UML " width="700" height="400">
<br>
This diagram only shows the basic class structure at the moment as this is easier to work with for me but later on in development I will put together a full detailed diagram.

#### Selection Box System

Finally, I’d like to show off the multiple unit selection system:
<img src="/SamHibb.github.io/images/kirov-entity2.gif" alt=" kirov-demo " width="700" height="400">
<br>
As you can see the selection works like traditional RTS games allowing you to select multiple entities and issue them the same order. Collision is currently missing, which would solve the overlap issue.

#### What to expect next?

Next, I’ll be adding collision to the engine and then start work on implementing A* and additionally fixing some of the main bugs such as the rendering issues at the sides of the map and the selection box orders sometimes pulling in other entities.


