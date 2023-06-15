# Assets

## Peasy-Assets

It was decided to not create a layer of abstraction around peasy for this, and just to straight up use peasy-assets module to manage this.

[Link to Peasy-Assets GitHub](https://github.com/peasy-lib/peasy-lib/tree/main/packages/peasy-assets)

## Usage

Example from game.ts, in Demo 1

```ts
import { Assets } from "@peasy-lib/peasy-assets";
Assets.initialize({ src: "../src/Assets/" });
await Assets.load([
  "lower.png",
  "DemoUpper.png",
  "hero.png",
  "shadow.png",
  "counter.png",
  "bookshelf.png",
  "npc2.png",
  "outsideUpper.png",
  "outsidemod.png",
  "planter.png",
  "pizzazone.png",
]);

//Load Maps
this.renderer.createMap([new Kitchen(Assets), new OutsideMap(Assets)]);

//Load Objects
let objConfig = [
  new Player(Assets, this.sm, this.dm),
  new Counter(Assets, this.sm),
  new Bookshelf(Assets, this.sm, this.dm),
  new NPC1(Assets, this.sm, this.dm),
  new Planter(Assets, this.sm),
  new PizzaThingy(Assets, this.sm),
];
```

This is the core of the asset management, the intentions of peasy-Assets is to provide up front assets caching in the browser and making it easy to access those assets (sounds and images) when needed.

Pass the reference to the cached assets to Maps and GameObjects to exposed the cached content to them.

## Methods

- `initialize({src: string})`

the src parameter sets the directory for where to load the assets from

- `load([strings])`
