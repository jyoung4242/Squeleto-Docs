# Demo RPG

## Pizza Legends

![Alt](/demo1.png "Pizza Legends")

## Breakdown

The first Demo is a top down 2d rpg style game demo tutorial. This demo, style AND assets borrows heavily from the Pizza Legends
tutorial on YouTube made popular by Drew Conley.

This demo has two scenes, intro scene and its gameloop scene. There are several [Entities](./ECS?id=entities), as this demonstrates how
they can be deployed in different maps.

There are two different maps, which highlights some different aspects of the map change event that's written for this demo. Each map
has its collision walls and map trigger points which trigger cutscenes, which are automated asynchronous event loops.

This also demonstrates the Y-sorting which happens in the Rendering [System](./ECS?id=systems) that was authored for this demo. The
higher something is the lower its zindex will be.

The last tech that's featured with this demo is [Signals](./signals.md), which is the built in publish/subscribe tech that let's one
game module fire 'signals' or custom events to any other module that's subscribed to that broadcast.

So, in this demo, the player can interact with the bookcase AND npc, which utilizes the interactions [System](./ECS?id=systems). You
know when you can interact with an entity, as it glows white when the player is facing it (and press Space Bar), this is defined by the
interactions [component](./ECS?id=components). When that occurs a Dialog modal pops up with corresponding messages tied to those
interactions, which are defined on the [Entities](./ECS?id=entities).

The player can walk around (arrow keys -> Keyboard System) and collide with objects and walls, thanks to the collision
[System](./ECS?id=systems). When the player walks around, the map and camera track with the player due to the CameraFollow system.

Also, the maps that are provided with the Demo have trigger spots or tiles on them that trigger actions, 1.) in the top right of the
kitchen in the bathroom, and 2.) in the bottom doorway of the kitchen. The bathroom triggers a custom cutscene dialog. The bottom door
triggers a mapchange. When the player and NPC walk around, they are animated walking, thanks to the sprite animation system, and the
NPC, when not being interacted with, will endlessly walk in a loop, thanks to the Events system and the defined behavior loop
[component](./ECS?id=components) on the NPC entity.

The Entities in this demo are the player, NPC, bookcase, counter, planter, and pizzasign. Due to the Rendering system mentioned above,
the player can walk around the objects and the y-sorting is handled properly.

The StoryFlag system is briefly displayed when you first talk to the NPC, he directs you to the bookcase, where you can interact with
it. After you interact with the bookcase, the NPC's dialog message changes.

### Demo Features:

- [Scenes](./ECS?id=scenes)
- [Systems](./ECS?id=systems)
- [Components](./ECS?id=components)
- [Signals](./signals.md)

### Components for this demo

Please refer to the demo source code for details on the components

- CameraFollow
- Collider
- Map
- Events
- Interactions
- Keyboard
- Name
- Position
- Size
- Velocity
- Opacity
- Render
- Sprite and Spritesheet
- zindex

### Systems in this demo

Please refer to the demo source code for details on the systems

- Rendering
- Movement
- Collision
- Interactions
- Dialog Modal
- Event Management
- Keyboard Input
- StoryFlags
- Chiptune
- Animated Sprites
