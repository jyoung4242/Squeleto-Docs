# Demo 2

## MM clone platformer

![Alt](/demo2.png "Demo 2")

## Breakdown

The second Demo is a side scrolling, platformer style game demo tutorial. The assets borrow heavily from Mega Man, from way back in the day, but are heavily inspired by Drew Conley's Excalibur.js demo he pulled off.

This Demo has 2 scenes much the same as Demo 1, an intro scene and its gameloop scene. There are 4 game objects, as this demonstrates how one game object can spawn others. (Bullets)

Another tech that is demonstrated is the new [Level Editor](./LevelEditor.md) for Squeleto. The map, walls, and platforms are generated when the scene is loaded each time. So one can easily make adjustments and change the levels, without having to adjust any sprites necessarily. Also, in your mapping of tiles, you can designate a tile as a collision 'wall' or body, and it is automatically generated as a collision body.

The new [Chiptune](./Chiptune.md) makes its debut in this project, and this is a neat feature wrapped around the AutoTracker project. Once can simply pass the chiptune class a seed string, and it plays infinitely.

This project also demonstrates the new [Aseprite Parser](./AsepriteParser.md) module which skips the step of one having to export different types of images from Asprite, then grab the image file and pull it into your project... this just has you pull in the Aseprite File and it can parse your Aseprite project any way you like! Super easy and convenient.

The last tech that's featured with this demo is [Signals](./signals.md), which is the built in publish/subscribe tech that let's one game module fire 'signals' or custom events to any other module that's subscribed to that broadcast.

### Demo Features:

- [Rendering](./Renderer.md)
- [Scenes](./Scene.md)
- [Sprites](./Sprite.md) and [SpriteSeheets](./SpriteSheet.md)
- [Chiptune](./Chiptune.md)
- [Level Editor](./LevelEditor.md)
- [Aseprite Parser](./AsepriteParser.md)
- [Game Objects](./GameObject.md)
- [Input Handling](./Input.md) (keyboard)
- [Signals](./signals.md)
