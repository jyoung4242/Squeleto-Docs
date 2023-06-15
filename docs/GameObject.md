# Game Objects

Game Objects are entities that get sprite layers, collision bodies, rendering update loops and physics update loops.

The constructor of a game object is its intialization code.

This is the included logic that will be needed for setting up the object.

If there is any InputHandling, animation states, events or cutscenes, this is all setup at this time.

So there are some basic elements to a standard GameObject.

- The properties definition, like any other class.
- The constructor, which all object initialization happens
- The rendering loop, `update()`
- The physics loop, `physicsUpdate()`

## Properties

This is standard JavaScript or Typescript class properties, but there are some key considerations here.

This is essentially your state for that object, so any key data that needs tracked needs to be represensted here.

For non-static objects, you need to track velocity. Maybe your using a custom plug-in like a dialog system that you create, but it needs to be pulled in here. If this object uses the StoryFlag system, they need to be referenced here as well.

Default properties in the GameObject class:

```ts
xPos = 0;
yPos = 0;
isPlayable: boolean = false;
isColliding: boolean = false;
collisionDirections: Array<direction> = [];
velocity = { x: 0, y: 0 };
id: string;
name: string;
zIndex: number;
width: number;
height: number;
class = "gameObject";
spriteLayers: spriteLayer = [];
collisionLayers: Array<collisionBody> = [];
isCollisionLayersVisible = false;
currentMap = "";
interactionEvents: Array<interaction> = [];
SM: StoryFlagManager | undefined;
```

Let's briefly break these down. A game object has a position, xPos and yPos. This is standard web technology positioning, origin is top/left of DOM or parent object. X moves right, Y moves down.

isPlayable is a flag that you can set and use to filter out NPCs. isColliding can be used in your physics loop and if your implementing the Collision manager library. Velocity is a 2d vector that can be used in phyics loop to adjust positioning.
collision layers is just that, its an array of collision bodies.

## Constructor and GameObjectConfig

```ts
let config: GameObjectConfig = {
  name: "Bookshelf",
  startingMap: "kitchen",
  initX: 48,
  initY: 48,
  width: 32,
  height: 26,
  sprites: [new Sprite(assets.image("bookshelf").src)],
  collisionBody: {
    width: 30,
    height: 10,
    offsetX: 0,
    offsetY: 16,
    color: "cyan",
  },
};
```

Let's break down the config object. We give each object a name, which should be a unique value. Also we need to tell the library what starting map this object belongs to. We provide size and position with init X & Y, and width & height. (In pixels). Then we pass our Array of sprite objects in the config. This way we can layer different sprites for each object. Finally, we specify the collision body for this object, including the color of its rectangle when it appears on screen. The Collision Body currently is a rectangle shape, and has an offset positioning from the gameobject and a size (width & height).

## Loops

```ts
  update(deltaTime: number, objects: Array<GameObject>, currentMap: GameMap, storyFlags: any): boolean {
    return true;
  }
  physicsUpdate(deltaTime: number, objects: Array<GameObject>, currentMap: GameMap, storyFlags: any): boolean {
    return true;
  }
```

There are two loops available for each game object. The update loop is tied to the rendering game loop established when you setup the renderer in your scene.

The physics loop is also called from the renderer, but it setup as a separate loop. They essentially can be used however you see fit, they are just independantly called and can be at different FPS rates.

Both loops return a boolean, and out of convenience both loops get access to the list of gameobjects, the current map, and storyflags.

## Behaviors

Behaviors are asynchronus list of events. This is tied into the cutscene system and there are several ways that behaviors can be initiated.

- Maps can have trigger spaces that when collided with trigger cutscenes for a player
- GameObjects can have interaction behaviors that are passed to the player when requested. This is done with NPC and the Player GameObjects in Demo 1
- GameObjects can have autonomouse idle behaviors that are executed if nothing else is happening

To learn more about this, check out the [Events](./Events.md) section.
