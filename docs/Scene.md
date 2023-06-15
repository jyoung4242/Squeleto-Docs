# Scenes

Scenes are the only required piece of content to do anything in Squeleto.
If there is no scene to run, there's no application running.
In short, there has to be a least one scene defined, and the CLI tool scaffolds one out on a blank project startup.

## Scenes and SceneManager

At its core the Scene systems and SceneManager is a state machine. There is entry code and exit code for each state (scene), which is leveraged to setup each scene initially.

Also, the exit code is used to teardown the scene prior to moving on to the next scene.

## TMI

The Peasy-lib package is the engine that drives much of the backend magic of Squeleto, peasy-UI is a UI binding library which takes in a HTML template and a data store object. In the HTML template you can create bindings, and UI monitors the data store for changes, and renders updates to the bindings in the HTML.

Each scene creates its own UIView for Peasy, which 'means' that the class properties of each scene IS its own data store. We will see this below.

## Using Scenes

It was mentioned earlier there's two 'types' of scenes, and in essences that's not entirely accurate. The difference between a non-game scene and a game scene is the use of the Renderer. The Renderer has the camera and gameloops native to it, and that makes something a 'game'.

Not importing and using the renderer just makes a scene a static webpage essentially, and this is useful for title screns, login screens, help screens, etc...

This is the first scene in Demo 1, super simple.

```ts
import { Scene } from "../../_Squeleto/SceneManager";

export class Login extends Scene {
  name: string = "Login";

  start = () => {
    this.setScene(1);
  };

  public template = `
  <scene-layer class="scene" style="width: 100%; height: 100%; position: absolute; top: 0; left:0; color: white;">
    <div style="position:absolute; top:50%; left: 50%; transform: translate(-50%,-50%); font-size: xx-large;">
      <a href="#" \${click@=>start}>Start Demo</a>
    </div>
  </scene-layer>`;

  //any wanted incoming code codes here, in init()
  public init() {}
}
```

You can see that we name the scene 'login', and that we are doing two things here.

First thing to point out is the template. This is the template string fed into Peasy-UI so that it knows what to render inside the viewport for this scene. Nothing really complicated here, its a couple of nested divs and an anchor tag.

The other item to point out is the binding in the template in conjunction with its associated callback.

`\${click@=>start}`

```ts
start = () => {
  this.setScene(1);
};
```

So the click event of the anchor tag will run the start method defined, which in THIS case will execute the setScene() method.

So, in the template string, we are binding a click event, to THIS SCENES state, which is the `start` callback associated with this scene. That setScene method which is being called is native to the Scene class, and it triggers a change in states. This method will call the leave() method of the scene (not shown), and it will destroy the UIView that peasy-ui is rendering, then swithc to the next scene and execute its init() method.

## State Data

The scene class itself is passed as the data model for each scene into peasy-UI. This allows for ANY class property created to be used as a binding data value in the template.

This could be handy for creating a HUD or UI overlay for your game, which will be demonstrated in Demo 2.

## Using the Renderer

To implement a gameloop and camera in this scene, one simply has to do this.

```ts
import { GameRenderer, RenderState, RendererConfig } from "../../_Squeleto/Renderer";
renderer = GameRenderer;
renderState = RenderState;

public template = `
<scene-layer class="scene" style="width: 100%; height: 100%; position: absolute; top: 0; left:0; color: white;">
    ${this.renderer.template}
</scene-layer>`;

//Initialize Renderer
const renderConfig: RendererConfig = {
  state: this.renderState,
  storyFlags: this.sm,
  viewportDims: { width: 400, aspectratio: 3 / 2 },
  objectRenderOrder: 2,
  physicsFPS: 30,
  renderingFPS: 60,
};
this.renderer.initialize(renderConfig);
```

This code establishes the renderer UI state data, the viewport details, storyflags, and sets up the engine loops.

Also, in the template it passes the Render string template into the scene template.

The [Renderer](/Renderer.md) will be covered more in its own page.

## Properties and Methods of Scenes

### Properties

- template: string literal, used by peasy-ui to render data to viewport
- storyFlags: {}, key/boolean pairs passed to each scene to manage quest systems or other milestones in a game
- setScene: any, this is set to the setScene from the viewport to give access to the method to change scenes

### Methods

- init()
  This is called when entering a Scene, all startup code and initialization should go here
- exit()
  This is called when exiting a Scene, any cleanup or teardown code should go here
