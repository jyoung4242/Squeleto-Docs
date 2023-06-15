# The Renderer

## What it is and what it does

The Renderer is where much of the game magic happens behind the scenes. It creates two gameLoops, it controls the game Camera system, it controls y-sorting of the objects, and it creates the maps and objects that are passed into it.

Let's break this down a bit.

## Game Loops

There are two gameloops created, leveraing peasy-engine library, renderLoop and physicsLoop. These two loops are exposed to each object that is active in the game. This exposure shows up in the GameObject class as:

```ts
update(deltaTime: number, objects: Array<GameObject>, currentMap: GameMap, storyFlags: any): boolean {
    return true;
  }

physicsUpdate(deltaTime: number, objects: Array<GameObject>, currentMap: GameMap, storyFlags: any): boolean {
    return true;
}
```

When you are initializing the Renderer, you can set the callback frequence of eachloop by passing an fps value to the Renderer config.

## Maps

In the Demo 1 tutorial, you can see how the maps are being loaded in the scene.

```ts
import { Kitchen } from "../Maps/kitchen";
import { OutsideMap } from "../Maps/outside";

//Load Maps
this.renderer.createMap([new Kitchen(Assets), new OutsideMap(Assets)]);
this.renderer.changeMap("kitchen");
```

The createMap() method accepts an array of Map objects that are imported. After they are created, you can switch to the default map for the scene by calling the changeMap() method of the Renderer. This is also the primary way for maps to change, when needed in a game.

## Game Objects

The Renderer also creates the list of Game Objects that gets updated every gameloop cycle. Very Similar to how the Maps gets created, thus the objects do too.

```ts
import { Player } from "../Game Objects/Player";
import { Counter } from "../Game Objects/Counter";
import { Bookshelf } from "../Game Objects/Bookshelf";
import { NPC1 } from "../Game Objects/npc1";
import { Planter } from "../Game Objects/planter";
import { PizzaThingy } from "../Game Objects/pizzathingy";

//Load Objects
let objConfig = [
  new Player(Assets, this.sm, this.dm),
  new Counter(Assets, this.sm),
  new Bookshelf(Assets, this.sm, this.dm),
  new NPC1(Assets, this.sm, this.dm),
  new Planter(Assets, this.sm),
  new PizzaThingy(Assets, this.sm),
];
this.renderer.createObject(objConfig);
```

Both Maps and Objects will be covered in more depth.

## Camera

The Renderer ultimately owns the game camera, which is covered in its own page.

[Camera](./Camera.md)

## Engine

The Renderer leverages peasy-engine to manage the game loops, which are tied to the callbacks for the loops mentioned above.

You can pause and start the engines if you want to pause the game loops.

```ts
 enginePause(engine?: string) // if no string passed, pauses both, or you can pass "renderer" or "physics" to turn one or other off
 engineStart(engine?: string) // if no string passed, starts both, or you can pass "renderer" or "physics" to turn one or other on
```

## Methods

- `initialize(config: RenderConfig)`

  Sets up Engines and Cameras

- `createObject(config: Array<GameObjectConfig | GameObject>)`

  Loads current scenes GameObjects

- `destroyObject(id:string)`

  Removes listed object from entity array

- `createMap(config: Array<MapConfig | GameMap>)`

  Adds list of maps to map manager

- `destroyMap(id: string)`

  Removes map from map manager

- `changeMap(name: string)`

  Updates the current map that's rendered to viewport

- `getMapSize()`

  primariy internally used, but returns the current map dims from the map manager

- `enginePause(engine?: string) `//"phsyics" or "renderer" or ignored

  Pauses engine, can be specified or if no parameter passed, both are paused

- `engineStart(engine?: string)` //"phsyics" or "renderer" or ignored

  Starts engine, can be specified or if no parameter passed, both are paused

- `cameraFollow(who: string)`

  Targets particular game object to display relative positioning to

- `cameraFlash(duration: number)`

  White flashes camera for defined period of time

- `cameraShake(who: GameObject, direction: ShakeDirection, magnitude: number, duration: number, interval: number )`

  performs camera shake based on provided parameters, more info in [Camera](./Camera.md) section

- `showCollisionBodies(visible: boolean)`

  turns the rendered boundaries on and off in the UI
