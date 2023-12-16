[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/bukotsunikki.svg?style=social&label=Follow%20%40jyoung424242)](https://twitter.com/jyoung424242)

# 👋 Introducing `Scenes`

## Approach

Scenes are the atomic unit of Squeleto. Meaning... all game content is encapsulated within a scene. So you have to have at least one
scene for your app to run.

A good use case for scenes maybe:

Login page, which jumps to a Lobby Scene, then joins into the main game loop.

The login page is a scene, the lobby screen is a scene, the game is a scene.... etc etc...

also, you can split scenes by level, like level 1 is a scene, level 2 is a scene...

## API

Scenes are controlled by SceneManager

The SceneManager at its core is a finite state machine, and it manages the switching in and out of different scenes.

The SceneManager also carries the Viewport instance with it so that its shared among all the scenes, so viewport layers can be
manipulated.

### Viewport Management

Example

```ts
// Setting up Viewport
SceneManager.viewport = Viewport.create({ size: { x: VIEWPORT_WIDTH, y: VIEWPORT_HEIGHT } });
```

Then in my scenes I can use the [Viewport](./Viewport.md) API to control layers

### Loading Scenes

Scenes are usually external modules, using the Scene Template that extends the States class

You can import your scenes into your main file and register them with the SceneManager

```ts
// Scenes
import { Login } from "./Scenes/login";
import { Game } from "./Scenes/game";

let sceneMgr = new SceneManager();
sceneMgr.register(Login, Game);
```

### Changing Scenes

SceneManagers have a .set() method that takes the target scene as a parameter and automatically unloads current scene and loads new
scene

```ts
sceneMgr.set("Login");
```

### Scene Template

Starting a new scene can start with the template boilerplate:

```ts
import { State, States } from "@peasy-lib/peasy-states";
import { UI, UIView } from "@peasy-lib/peasy-ui";
import { System } from "./system";
import { Viewport } from "@peasy-lib/peasy-viewport";

export class SceneManager extends States {
  public static viewport: Viewport;
}

export class Scene extends State {
  public view: UIView | undefined = undefined;
  public template: string = "";
  public stateData: any;
  public storyFlags = {};
  public setScene: any;
  public params: Array<any> = [];
  systems: Array<System> = [];

  public async enter(previous: State | null, ...params: any[]) {}

  public leave() {
    this.view?.destroy();
    SceneManager.viewport.removeLayers();
  }

  public initialize() {}
}
```

### Enter and Exit

When the .set() method is called, if you're in an existing scene, the scene runs clean up methods before switching...

you can have your exit() method used for clean up, as well as the leave() method is called to autoclean Viewport layers

your scene initializations are call in the enter() method that you can setup
