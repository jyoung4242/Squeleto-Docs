# Demo 1

## Pizza Legends

![Alt](/demo1.png "Demo 1")

## Breakdown

The first Demo is a top down 2d rpg style game demo tutorial. This demo, style AND assets borrows heavily from the Pizza Legends tutorial on YouTube made popular by Drew Conley.

This demo has two scenes, intro scene and its gameloop scene. There are several game objects, as this demonstrates how they can be deployed in different maps.

There are two different maps, which highlights some different aspects of the Map management feature. Each map has its collision walls and map trigger points which trigger cutscenes, which are automated asynchronous event loops.

This also demonstrates the Y-sorting which happens in the Renderer. The higher something is earlier its rendered so it can sit behind other items.

This demo also shows off custom plug-ins if the need for a custom game system needs created, it can be added. This demo's dialog system is a custom plug-in for this small game.

the Bulk of the logic for this demo is in 2 places, the game.ts Scene, and the Player.ts GameObject,... that is where to start

Demo Features:

- [Rendering](./Renderer.md)
- [Sprites](./Sprite.md) and [SpriteSeheets](./SpriteSheet.md)
- [Game Objects](./GameObject.md)
- Input Handling (keyboard and gamepad)
- Custom PlugIns
- Map Management (changing maps, walls, triggers)
- Story Flags
- [Events](./Events.md)
