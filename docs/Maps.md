# Maps

## Using Maps in the Renderer

In the Renderer, maps are used simply by:

```ts
//Load Maps
import { Kitchen } from "../Maps/kitchen";
import { OutsideMap } from "../Maps/outside";

this.renderer.createMap([new Kitchen(Assets), new OutsideMap(Assets)]);
this.renderer.changeMap("kitchen");
```

But each Map is its own content piece.

Let's look at the Kitchen example:

```ts
import { GameObject } from "../../_Squeleto/GameObject";
import { GameMap, MapConfig } from "../../_Squeleto/MapManager";
import { ChangeMap } from "../../_Squeleto/Renderer";

export class Kitchen extends GameMap {

  constructor(assets: any) {
    let config: MapConfig = {
      name: "kitchen",
      width: 192,
      height: 192,
      layers: [assets.image("lower").src, assets.image("DemoUpper").src],
      walls: [
        {
          x: 10,
          y: 64,
          w: 5,
          h: 96,
          color: "red",
        },
        ...
      ],
      triggers: [
        {
          x: 112,
          y: 48,
          w: 14,
          h: 5,
          color: "yellow",
        },
        ...
      ],
    };
    super(config);
  }
}
```

There are a few elements to point out.

- Layers
  these are the asset strings that are used to render to the sprite layers in the Map object. You can pass many layers and they'll stack on top of each other, which is handy for managing 'canopies' in your levels for walking under and what not...

For Walls and Triggers we export

```ts
export type collisionBody = {
  x: number;
  y: number;
  w: number;
  h: number;
  actions?: any[];
  color?: string;
};
```

This is a standard object configuration for both walls and triggers

- Walls

  walls exist to be ran into, and when the object is reated, the collision manage will detect an intersection with that wall... what you do with that is up to you

- triggers

  triggers not only have the above details as walls, but utilize the actions list, which is an array of Events that can be triggered when a collision occurs

## Properties

    - id, assigned unique identifier for a map
    - name, string provided at configuration for reference
    - width/height: numbers, pixel sizes of map areas
    - layers = array of MapLayer objects that are used to be drawn, multiple available for layered effects for a level
    - walls: array of collision bodies representing static entities for collision detection
    - triggers: array of collision bodies representing static entities for triggering cutscene actions

## Methods

- getMapSize()
  returns width and height of current map
