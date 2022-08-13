---
layout: post
title: Kirov dev diary 1
date: 2017-10-27T16:30:00.000Z
description: A follow up to my java game engine
img: previews/kirov-1.png
published: true
category: development
tags:
  - java
  - game engine
---
## Introducing kirov
<br>

Kirov is the (placeholder) name of my Java game engine project that I’ve been working on. And this dev diary will document the progress I’ve been making on it so far. These diary posts will likely be sporadic with the all other University work that I’ll be doing as a focus but when I get time I’ll be documenting and developing this game engine. 

But for now, I’ll go over what’s been done so far:

#### UML

<img src="/SamHibb.github.io/images/kirov-UML.png" alt="kirov-UML" width="700" height="400">
<br>

This diagram is a super rough interpretation of what the UML diagram will look like. My style of coding tends to be that I put together a rough diagram of what the program will look like and then start coding a block, update the UML and go from there. This way I don’t have to make massive changes to the UML mid-way through the project making it obsolete as I tend to find that I may have over looked something important in the planning phase that I found out in the development phase.

#### The map

<img src="/SamHibb.github.io/images/kirov-mapImg.png " alt="kirov-map" width="700" height="400">
<br>

This image is the map that is loaded into the game engine for rendering and below is the output that game engine gives. For the record, I’m not a good artist…

<iframe width="700" height="400" src="https://www.youtube.com/embed/xAmswB1xDow?rel=0" frameborder="0" allowfullscreen></iframe>

<br>

But as you can see it serves its purpose and shows of the map being rendered and scaled correctly. This works by reading in the pixels from the map image and drawing sprites based on what it reads in. A code excerpt that shows some of this process is given below:

```java
private char colourToTile(int colour){
    if(colour == 0xff007F0E){ //green
        fetchSprites('G');
        return 'G';
    }

    if(colour == 0xff0026FF){   //blue
        fetchSprites('W');
        return 'W';
    }

    if(colour == 0xffFFD800){   //yellow
        fetchSprites('B');
        return 'B';
    }

    fetchSprites('N');
    return 'N';
}
```

In this example, the colour of the given map pixel is passed in and a sprite with the relevant character code is “fetched” for rendering.

#### Camera

Also in the above video, the camera is being demonstrated. This works by segmenting a portion of the map equal to the size of the window and rendering only that part of the map to the screen. When the player moves the camera around the portion of the map that is rendered moves along with it and the other previous parts fall out of range and are un-rendered.

```java
public void renderMap(Map map){
        int mapWidth = map.getWidth();
        int mapHeight = map.getHeight();

	//retrieve the entire map image
        int[] mapPixels = map.getMapPixels();

        for(int y = 0; y < height; y++){
            for(int x = 0; x < width; x++){
			
		//Offset the render coordinates by the camera position
                int mapX = x + camera.getX();
                int mapY = y + camera.getY();

		//Lock the camera if it tries to move out of bounds
                if(mapY >= mapHeight) {
                    camera.yNegLimit = true;
                } else if(mapY < 0) {
                    camera.yPosLimit = true;
                } else if(mapX <= 0){
                    camera.xPosLimit = true;
                }else if(mapX >= mapWidth){
                    camera.xNegLimit = true;
                }else {
			/*
			pixels is the actual values rendered to the screen
			render only the pixels that can be seen by camera
			*/
                    	pixels[x + y * width] = mapPixels[mapX + mapY * mapWidth];
                }
            }
        }
    }
```

#### Currently developing?

At the moment I’m adding entities and mobs to the engine as well as a mouse control system befitting of an RTS game to allow for the movement of these entities.
