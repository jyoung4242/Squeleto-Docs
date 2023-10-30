# Viewport

## Approach

Squeleto now uses peasy-viewport to manage Camera and Viewport functionality

[Link to Peasy-Viewport GitHub](https://github.com/peasy-lib/peasy-lib/tree/main/packages/peasy-viewport)

## Usage

Once the viewport is created, you will add layers to it to be displayed.

```ts
// IN MAIN.ts
import { Viewport } from "@peasy-lib/peasy-viewport";

// Setting up Viewport
export const VIEWPORT_WIDTH = 400;
const ASPECT_RATIO = 16 / 9;
export const VIEWPORT_HEIGHT = VIEWPORT_WIDTH / ASPECT_RATIO;
SceneManager.viewport = Viewport.create({ size: { x: VIEWPORT_WIDTH, y: VIEWPORT_HEIGHT } });
```

```ts
// in you're scene.ts
// *************************************
// Setup Viewport Layers
// this uses the peasy-ui UI.create() method
// *************************************

SceneManager.viewport.addLayers([
  {
    name: "maplower",
    parallax: 0,
    image: Assets.image(Kitchen.lower).src,
    size: { x: 192, y: 192 },
    position: { x: 192 / 2, y: 192 / 2 },
  },
  { name: "game", parallax: 0, size: { x: 0, y: 0 } },
  {
    name: "mapupper",
    parallax: 0,
    image: Assets.image(Kitchen.upper).src,
    size: { x: 192, y: 192 },
    position: { x: 192 / 2, y: 192 / 2 },
  },

  {
    name: "dialog",
    size: { x: VIEWPORT_WIDTH, y: VIEWPORT_HEIGHT },
  },
]);
let layers = SceneManager.viewport.layers;

const game = layers.find(lyr => lyr.name == "game");
if (game) this.view = UI.create(game.element as HTMLElement, this, this.template);
if (this.view) await this.view.attached;

const dialog = layers.find(lyr => lyr.name == "dialog");
if (dialog) UI.create(dialog.element, new Dialogue(), Dialogue.template);
```

## Adding/Removing Layers

You can manipulate the layers, for example, if you want to add a transition effect between map changes, you can add a 'transition'
layer to hide the game layer.

For changing maps, one approach is to remove the old map layer and insert a new map layer using `viewport.addLayer()`

After all you're changes are complete, you can `viewport.removeLayer('layername')`

## Changing Images and Size

on each Layer object, there is an element property that gives you access to the DOM element for each layer the background image, width,
and height in-line styles can be manipulated to change size and images.

## Styling

The viewport will have the 'viewport' class attached to the element, so you can reference this for styling purposes.

```css
.viewport {
  image-rendering: pixelated;
  border: 1px whitesmoke solid;
  border-radius: 3px;
  background-color: #222222;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(var(--pixel-size));
  box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
}
```
