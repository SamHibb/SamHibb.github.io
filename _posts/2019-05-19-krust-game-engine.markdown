---
layout: post
title: The Krust game engine
date: 2019-05-22T10:10:00.000Z
description: A new game engine
img: previews/krust-1.png
published: true
category: development
tags:
  - rust
  - game engine
---
## A new game engine (Krust 1/3)
<br>



As part of my degree, I was tasked with coming up with a project to work on throughout my final year. Based on my previous experience with the Kirov engine and after discussion with lecturers I decided to create a new game engine in the Rust programming language. This post details the engine and gives a brief overview of it's current features and functions.

#### UML and Engine Structure

The engine is designed around being modular making it lightweight and capable. Using Rust's cargo system you can easily important several modules into the core module to create a customizable experience.

<img src="/SamHibb.github.io/images/krust_uml.png" alt="krust uml" width="700" height="400">
<br>
An example of one of these components is the timer module.

#### Krust Timer

Krust timer is a separate module to the core engine. Simply, it allows the access of a variety of functions for keeping tracking of periodic events in a game. 

``` rust
pub fn after_x_ms(&mut self, id: &str, ms: u64) -> bool {
    let mut value = &mut Value {
        start: 0,
        limit: 99,
    }; //Placeholder data
    let mut new_flag = false;

    {
        let result = self.timers.get_mut(id);

        match result {
            Some(v) => value = v,
            None => new_flag = true,
        }
    }

    if new_flag {
        self.timers.insert(
            id.to_string(),
            Value {
                start: current_time_milliseconds(),
                limit: ms,
            },
        );
        return false;
    }

    if (current_time_milliseconds() - value.start) >= value.limit {
        self.timers.remove(id);
        return true;
    } else {
        return false;
    }
}
```

The above is designed to be repeatedly called and to use this to keep track of the elapsed time. Once it's set time has been elapsed it will return true. An example of how this is used is in the below example taken from a prototype game.

``` rust
if self.timer.after_x_ms("GROUND_UP_ATTACK", 400) {
    self.attacking = false;
    self.animations[3].reset();
}
```

The condition is inside of an update loop that is called consistently by the engine allowing it to remain synchronized and track the passage of time.



#### Modular Design

The goal for the engine's design was to create a variety of components with most being designed around certain types of games. For example, having preset camera systems designed for isometric games, side facing games and top down games. Instead of having all of these available in the core system they can be imported as needed. This creates a much more lightweight experience as there are no unused components in the engine.
