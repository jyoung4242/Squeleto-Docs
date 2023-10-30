# Introducing Squeleto ECS

## What is an ECS?

(from Wikipedea) "... Entity component system (ECS) is a software architectural pattern mostly used in video game development for the
representation of game world objects. An ECS comprises entities composed from components of data, with systems which operate on
entities' components.

ECS follows the principle of composition over inheritance, meaning that every entity is defined not by a type hierarchy, but by the
components that are associated with it. Systems act globally over all entities which have the required components. ... "

So, in essence, a game is comprised of entities, and these entities have properties (components), and these properties are modified by
systems.

## Getting Started with Squeleto-ECS

From you current working directory, open up a command line prompt.

run `npx squeleto` in your prompt.

To create a new ECS project, select that option, or you can use the Demo tutorial to get started.

## Scenes

Squeleto-ECS runs off of scenes.

```typescript
import { SceneManager } from "../_Squeleto/Scene";

/// Scenes
import { Login } from "./Scenes/login";
import { Game } from "./Scenes/game";

//Load Scenes
let sceneMgr = new SceneManager();
sceneMgr.register(Login, Game);
sceneMgr.set("Login");
```

You will import the scenemanager and import your scenes. Then you register your scenes with the scenemanager, and set the initial scene

### Scene Template

```ts
// Library
import { Scene, SceneManager } from "../../_Squeleto/Scene";
import { Vector } from "../../_Squeleto/Vector";
import { Entity } from "../../_Squeleto/entity";
import { System } from "../../_Squeleto/system";
import { Signal } from "../../_Squeleto/Signals";

// Entities
import { TemplateEntity } from "./Entities/Template.ts";

// Systems
import { TemplateSystem } from "./Systems/Template.ts";

export class Test extends Scene {
  name: string = "test";

  entities: Entity[] = [];
  Systems: System[] = [];

  public template = `
  <scene-layer class="scene" style="width: 100%; height: 100%; position: relative; top: 0; left:0; color: white;">
    < \${ System === } \${ System <=* Systems }>
  </scene-layer>`;

  public async enter(previous: State | null, ...params: any[]): Promise<void> {
    // Setups Assets Loading
    Assets.initialize({ src: "../src/Assets/" });
    await Assets.load(["test.png"]);

    // Setup Viewport
    SceneManager.viewport.addLayers([
      {
        name: "game",
        parallax: 0,
        image: Assets.image("test.png"),
        size: { x: 100, y: 100 },
        position: { x: 50, y: 50 },
      },
    ]);

    const game = layers.find(lyr => lyr.name == "game");
    if (game) this.view = UI.create(game.element as HTMLElement, this, this.template);
    if (this.view) await this.view.attached;

    // Load Entites
    this.entities.push(TemplateEntity.create());

    // Load Systems
    this.Systems.push(new TemplateSystem());

    //Start GameLoop
    Engine.create({ fps: 60, started: true, callback: this.update });
  }

  //GameLoop update method
  update = (deltaTime: number): void | Promise<void> => {
    this.sceneSystems.forEach((system: any) => {
      system.update(deltaTime / 1000, 0, this.entities);
    });
  };
}
```

This is the template defined for new Scenes. The Scene class and name property should be changed. All systems need to be imported and
attached to the scene. All entities need to be imported into the scene and loaded. If you want a game loop, that's defined here and
intialized.

## Entities

```typescript
import { v4 as uuidv4 } from "uuid";
import { Entity } from "../../_SqueletoECS/entity";

export class TemplateEntity {
  static create() {
    return Entity.create({
      id: uuidv4(),
      components: {
        foo: { data: "Welcome to Squeleto ECS" }, //this is tied to templateComponent.ts
      },
    });
  }
}
```

Here is the template for creating an entity. Let's walk through the different parts of this The Entity class uses the factory pattern,
so all you need to provide is the details of the create method. The rest is handled.

- Entity Name: the Entity name will come from the class definition, in this case its `TemplateEnity` but that will need to be changed.

i.e. PlayerEntity, BaddieEntity, MapEntity... etc, etc

- id: this is handled automatically, each entity created will have a unique identifier

- components object THIS IS IMPORTANT

  the components object is where you assign different components (or properties) to this entity type, these will specifically correlate
  to component definitions, the key text and the value data are specifically defined in the components

## Components

### Loading Components

in the main.ts file, you'll need this.

```ts
//main.ts
//Content Modules
import { LoadComponents } from "./Components/_components";

//Components
LoadComponents();
```

this calls a utility file called `_components.ts` from the components directory. All components your game will use needs to be loaded
up prior to changing any scenes. this will automatically put into your main.ts, but the `_components.ts` must be updated accordingly

```ts
//_components.ts
// initialize all your system components here
// simply import then and create a new instance in the array
// for example
// import { Name } from "./nameComp";
// export function LoadComponents(){
//  [new Name(),... and all your other components follow]
// }

import { TemplateComp } from "./templateComponent";

// The template component is demonstrated by default, you'll probably
// want to replace it

export function LoadComponents() {
  [new TemplateComp()];
}
```

you will import and instantiate all the component classes in this array

### Component Template

```ts
import { Component } from "../../_SqueletoECS/component";

// you can define the incoming types when the component is created
export interface ITemplateComponent {
  data: TemplateType;
}
export type TemplateType = string;

// this is the exported interface that is used in systems modules
export interface TemplateComponent {
  foo: TemplateType;
}

// classes should have:
// if UI element, a template property with the peasy-ui template literal
// if no UI aspect to the system, do not define a template
// a 'value' property that will be attached to the entity
export class TemplateComp extends Component {
  // UI template string literal with UI binding of value property
  public template = `
    <style>
      .template-component {
      position: fixed;
      top:50%;
      left:50%;
      transform: translate(-50%,-50%);
      }
    </style>
    <template-comp class="template-component">\${value}</template-comp>
    `;

  //setting default value
  public value: TemplateType = "";
  public constructor() {
    //@ts-ignore
    super("foo", TemplateComp, true);
  }

  public define(data: ITemplateComponent): void {
    if (data == null) {
      return;
    }
    this.value = data.data;
  }
}
```

Here is the template for creating a component. There is some magic here so let's walk through what's going on.

- `ITemplateComponent` defines the incoming type data from the entity definition. This should change to someother name like
  `INameComponent` or `IVelocityComponent`

Remember back to the entity component object? This is where that is defined.

```ts
components: {
        foo: { data: "Welcome to Squeleto ECS" }, //this is tied to templateComponent.ts
      },
```

- `TemplateComponent` defines the outgoing type data from the entity definition. This is used in the systems portion of the ECS to
  determine if the current entity has this component on it. This should change to someother name like `NameComponent` or
  `VelocityComponent`

- `TemplateType` defines the datastructure that 'can' be used in the component. If your dealing with a Vector like Velocity, this can
  be where you define that data type as such. If its a string, this is where its defined as a string, for example on a Name component,
  if its a value like a z-index, this is where you can define this as a number... and so on.

- `template` property: this is if this component has a UI or dom impact. If you want something to render to the DOM, this is where you
  define the template string to be injected into the entity DOM element. Name components would use this to attach the name property to
  the UI, or this can be sprite components to render the entity sprites... and so on.

- `value:TemplateType=""` this is demostrating how to set the default value for your compoent. in this example, we are typing it as the
  TemplateType discussed earlier, but thats up to you. It could be a string, or number, or whichever you want. the value property gets
  REPLACED by the name of the component when its created, which is a neat little magic trick.

- `super("foo", TemplateComp, true);` this line is where the name of the component gets defined. This component property on this enity
  will show up as 'foo'. If you want the property to be 'name' or 'velocity' or 'sprite' this is where it gets changed/defined. Make
  note that the TemplateComp is passed into the super() call as well, so this would need to be modified when changed

  - if the last parameter is true, then the name of the component ('foo' here), becomes the property of the entity
  - if false is passed into this parameter, then the whole class itself is attached as the property, which may have some benefits

- `value definition`

  - this is when/how the incoming data of the entity definition makes its way into the component and is setup

```ts
public define(data: ITemplateComponent): void {
    if (data == null) {
      return;
    }
    this.value = data.data;
  }
```

## Systems

#### ECS system template

```ts
import { Entity } from "../../_SqueletoECS/entity";
import { System } from "../../_SqueletoECS/system";
import { TemplateComponent } from "../Components/templateComponent";

// type definition for ensuring the entity template has the correct components
// ComponentTypes are defined IN the components imported
export type TemplateEntity = Entity & TemplateComponent;

export class templateSystem extends System {
  public constructor() {
    super("template");
  }

  public processEntity(entity: TemplateEntity): boolean {
    // return the test to determine if the entity has the correct properties
    return entity.foo != null;
    // entities that have 'foo' property can use this system
  }

  // update routine that is called by the gameloop engine
  public update(deltaTime: number, now: number, entities: TemplateEntity[]): void {
    entities.forEach(entity => {
      // This is the screening for skipping entities that aren't impacted by this system
      // if you want to impact ALL entities, you can remove this
      if (!this.processEntity(entity)) {
        return;
      }
      console.log("Template Component Update Yay!");
      // insert code here on how you want to manipulate the entity properties
    });
  }
}
```

This is the template for systems provided with the blank project. Let's break it down.

- Imports

- Enity and System get imported, also the `TemplateComponent` exported value from the component impacted by this system, this will
  change name based on the component file

- `export type TemplateEntity = Entity & TemplateComponent;` this allows for the processEntity() method to discern which properties it
  monitors for sorting out components not impacted by the system

- `super("template");` please name your systems accordingly... like 'movement' or something

- `processEntity` this method is responsible for filtering out any entities which don't have the correct components attached to matter
  to this system

- `update` this method is called by the peasy-engine handler that's configured in the Scene Template
