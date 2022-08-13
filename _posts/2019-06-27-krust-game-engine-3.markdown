---
layout: post
title: Krust's Games
date: 2019-06-27T14:00:00.000Z
description: video games!
img: previews/krust-3.png
published: true
category: development
tags:
  - rust
  - game engine
---
## Games (Krust 3/3)

While building the engine part of my focus went into making small games with simple core loops to test out the different capabilities of the engine.  These games were crucial in development as they allowed me to find flaws in the engine.

### Not Galaga

The first game I made was a Galaga clone. I chose Galaga because the core loop was very simple, the sprites were very simple and the camera didn't move. At the time Krust's camera system wasn't yet implemented so it made a perfect candidate.

<iframe width="700" height="400" src="https://www.youtube.com/embed/6SAuSdzHktQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This game would lead me to start on the camera system as well as fixing issues with z-ordering and scene management. In the game the intro scene isn't a separate scene to the game scene and instead just works like the game scene. This would be fixed in a later version of the engine when a proper scene system was implemented.

#### Tower

The second game was more of a test for the new camera system and asset toolkit more than anything. Due to the time constraints near the end of my degree progress couldn't be made on the game's systems so it basically became a very simple tech demo.

<iframe width="700" height="400" src="https://www.youtube.com/embed/CcmvC1ae56k" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

It still proved useful to test out new systems and was instrumental in testing and implementing the modular design. A lot of the systems used in Not Galaga were used in Tower. This was made is with the engine's modular design.

#### The Future of Krust

The engine isn't something I plan to abandon but I will be taking a break from it for a while. For now I've started work on a new portfolio piece. Something more game-oriented to pair with job applications. I will come back to this engine in time and give it a proper editor and finish it.
