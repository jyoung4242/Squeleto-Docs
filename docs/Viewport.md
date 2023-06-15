# Viewport

## Approach

The Viewport is the highest level parent object in the DOM for your game.
All other elements are children of the Viewport.

Teh Viewport specifically owns the SceneManager, and ultimately controls which scene is being displayed, and controls the 'transition' between the scenes.

The primary task is setting up your viewport is to nail down the size and aspect ratio, which are passed in to the viewport in main.ts.

    The CLI tool will boilerplate up an instance of your viewport, so in main.ts, if you want to adjust it, feel free too.

## Usage

```ts
import { Viewport } from "../_Squeleto/Viewport";

/**************************************
 * Import and Configure Scenes
 **************************************/
//import Scenes
import { Login } from "./Scenes/login";
import { Game } from "./Scenes/game";
let scenes = [Login, Game];

/**************************************
//setup game state
/**************************************/
export const datamodel = new StateManagement();

/**************************************
 * Import and configure game Viewport
 **************************************/
const viewport = Viewport;
viewport.initialize(datamodel, scenes, 400, "3.125/1.75");
const template = `${viewport.template}`;
viewport.setScene(0);
```

    Author's note on media query breakpoints... right now their hardcoded for the below limits
    Looking to abstract this away in the future so its configurable.

```css
@media (max-width: 1100px) {
  :root {
    --pixel-size: 1.5;
  }
}

@media (max-width: 675px) {
  :root {
    --pixel-size: 0.75;
  }
}
```

## Methods

### initialize

`initialize(state:any, scenes:any, width?: number, aspectRatio?: string),`

- state is the returned value from `new StateManagement()`

### setScene

`setScene(sceneIndex: number)`

- the sceneIndex is the index number of the scene which is passed into the Viewport on startup

this method is passed to each scene, so that native to each scene running there is a this.setScene() method to call that allows for easy switching from scene to scene
