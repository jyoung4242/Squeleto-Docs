# Sprites

## Sprites and their usage

Sprites have their own class for reason, and it how its setup in the gameobjects as a sprite layer. This is due to the fact that on Game Objects, sprites are layered within the object itself, and they get rendered in order.

Sprites have these properties:

- width
- height
- zIndex
- src
- animationBinding (for static sprite, this is ignored)

A good example of sprite layering is in Demo1, and the NPC and Player characters have two sprites for each GameObject. 1) the actual sprite of the character, and 2) the shadow sprite that sits below them.

![Alt](/sprite.png "Sprite Layers")

### Usage

The sprite class is namely used to setup game objects with static sprites that do not change. In this example below, the gameobject 'Counter' only has one sprite associated with it, and it doesn't change.

`new Sprite(asset)` where asset is the Peasy-Assets which are loaded from the scene.

```ts
let config: GameObjectConfig = {
  name: "Counter",
  startingMap: "kitchen",
  initX: 112,
  initY: 96,
  width: 32,
  height: 32,
  sprites: [new Sprite(assets.image("counter").src)],
  collisionBody: {
    width: 30,
    height: 24,
    offsetX: 0,
    offsetY: 6,
    color: "cyan",
  },
};
```
