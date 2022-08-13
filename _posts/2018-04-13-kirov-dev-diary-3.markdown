---
layout: post
title: Kirov dev diary 3
date: 2018-04-13T16:00:00.000Z
description: Thinking bigger
img: previews/kirov-3.png
published: true
category: development
tags:
  - java
  - game engine
---
## Thinking Bigger
<br>

I’ve been working on an off with Kirov trying to balance my course work with working on the engine. This had given me some time to think about what I want to do with Kirov and I’ve decided that I’m going to broaden the horizons and try and make the engine capable of making a variety of games and not just and RTS engine.
With that out the way let’s look at some of Kirov’s new features:

#### Colliders
After a lot of development and reworking of systems I finally got colliders to work.
Step 1 was getting a component system working. Entities now has a list of components like that found in Unity.
Step 2 was implementing a transform component so that the box collider could share the same position and update that position at the same time as it’s attached entity.
Step 3 was finally implementing the collision code. The way I chose to design it was to use a four point system where each edge of a square drawn a round a sprite would detected if something crosses it and compares with other edges to confirm if something has collided.

``` java
//Four point collision
public boolean checkCollision(Vector2 position, Vector2 area){
    Vector2 posPlusArea = Vector2.add(position, area);

    Vector2[] fourPoints = new Vector2[4];
    fourPoints[0] = position;                               //North West
    fourPoints[1] = new Vector2(posPlusArea.x, position.y); //North East
    fourPoints[2] = new Vector2(position.x, posPlusArea.y); //South West
    fourPoints[3] = posPlusArea;                            //South East

    for (Vector2 vector : fourPoints) {
        if(Vector2.greaterThan(vector, min) && Vector2.lessThan(vector, max)) {
            collision = true;
            return true;
        }
    }

    collision = false;
    return false;
}
```
Here’s a little demo of this in action:
<img src="/SamHibb.github.io/images/kirov-collision1.gif" alt="kirov-collision-demo" width="700" height="400">
<br>

#### Scene Manager
To tidy up some of the rest of the program I put together a scene manager for the game. Now all the game logic and what needs to be rendered can be stored in game scenes. This will also allow me to debug and test things a lot easier plus I can now work on a menu scene as well.
``` java
public abstract class Scene {

    protected ArrayList<Entity> entities = new ArrayList<>();

    protected abstract void update();
    protected abstract void render();
}
```
This is what a scene looks like now. I’m going to implement an input method as well so that logic can be handled in here too. Currently all scenes will have an update and render method for handling that logic as well as an entities array for handling all sorts of things from UI elements to game objects like enemies and scenery.

As for the scene manager it is basically a simple stack system. The scenes get pushed onto a stack so that they can always be returned to. The scene manager will tell the current scene to handle all of its render and update logic until the scene tells the manger to pop the scene or push on another scene to the stack. An example for how this could be used is a pause menu.
If the player is playing their game and they want to pause, hitting escape can push a “pause scene” to the scene manger stopping the previous scene logic and running whatever pause scene logic is needed. If they player is done with the pause the pause scene is popped off and the game scene returns and continues running. Additionally, the menu scene if needed can access data from the game scene by just viewing the stack (which will likely be implemented later in development).
``` java
public class SceneManager {

    private Stack<Scene> sceneStack = new Stack<>();
    private Scene currentScene;

    public SceneManager(){
    }

    public void init() {

    }

    public void update(){
        currentScene.update();
    }

    public void render(){
        currentScene.render();
    }

    public void pushScene(Scene scene){
        if(sceneStack.empty()){
            currentScene = scene;
        }
        sceneStack.push(scene);
    }

    public void popScene(){
        sceneStack.pop();
        currentScene = sceneStack.firstElement();
    }
}
```
#### Coming Soon: Input Manager
Currently I’m working on improving the input system. With my new planned system Classes can subscribe to the input system and be notified via call-back functions whenever the player interacts somehow (via the mouse/keyboard or maybe even via gamepad in future!). Currently I’m deciding what the easiest way to do this is. My original idea was to have a simple “subscribe(CallBackFunc, EventType)” function that would allow classes to subscribe to certain data and tell the Input manger to “call them back” on certain functions but I don’t think this is possible with the limitations of java. However, I will continue to research into this; my current system looks like it’ll have to be functions implementing an interface that provides all input event data. Something like this:
``` java
public interface InputEvent {
    enum MouseState {
        LEFT_DOWN,
        LEFT_UP,
        MIDDLE_DOWN,
        MIDDLE_UP,
        RIGHT_DOWN,
        RIGHT_UP,
    }
    class SharedInputData {
        MouseState mouseState;
    }

    void input(SharedInputData data);
}
```

