# Demo 1

## Pizza Legends

![Alt](/demo1.png "Demo 1")

## Breakdown

The first Demo is a top down 2d rpg style game demo tutorial. This demo, style AND assets borrows heavily from the Pizza Legends tutorial on YouTube made popular by Drew Conley.

This demo has two scenes, intro scene and its gameloop scene. There are several [game objects](./GameObject.md), as this demonstrates how they can be deployed in different maps.

There are two different maps, which highlights some different aspects of the [Map management](./Maps.md) feature. Each map has its collision walls and map trigger points which trigger cutscenes, which are automated asynchronous event loops.

This also demonstrates the Y-sorting which happens in the [Renderer](./Renderer.md). The higher something is earlier its rendered so it can sit behind other items.

This demo also shows off custom [Plug-ins](./plugins.md) if the need for a custom game system needs created, it can be added. This demo's dialog system is a custom plug-in for this small game.

the Bulk of the logic for this demo is in 2 places, the game.ts [Scene](./Scene.md), and the Player.ts GameObject,... that is where to start

One important feature is the [Event Manager](./Events.md) which drives a lot of the asynchronous activities that occur in this demo.

The new [Chiptune](./Chiptune.md) makes its debut in this project, and this is a neat feature wrapped around the AutoTracker project. Once can simply pass the chiptune class a seed string, and it plays infinitely.

The last tech that's featured with this demo is [Signals](./signals.md), which is the built in publish/subscribe tech that let's one game module fire 'signals' or custom events to any other module that's subscribed to that broadcast.

### Demo Features:

- [Rendering](./Renderer.md)
- [Scenes](./Scene.md)
- [Sprites](./Sprite.md) and [SpriteSeheets](./SpriteSheet.md)
- [Game Objects](./GameObject.md)
- [Input Handling](./Input.md) (keyboard and gamepad)
- [Custom PlugIns](./plugin.md)
- [Map Management](./Maps.md) (changing maps, walls, triggers)
- [Story Flags](./Storyflags.md)
- [Events](./Events.md)
- [Signals](./signals.md)
- [Chiptune](./Chiptune.md)
