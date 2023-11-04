# Getting Started

> Covering the CLI tool

## CLI

![Alt](/Squeleto.png "CLI")

```
npx squeleto
```

Currently, this CLI tool will provide 3 options:

- Start New Project
- Download Tutorial
- Open Documentation

### Start New Project

This setups up the file structure and boilerplate project so one can jump right in and start coding up their new game. The creates and
index.html and main.ts file that pre-sets up a Viewport and loads a starter scene.

### Download Tutorial

This setups up the file structure, source code, and game assets for a top down RPG game. The project has two scenes, a loading page and
the main game loop.

## Game Architeture

![Alt](/gamearch.png "Squeleto Architecture")

### Scenes

Scenes in Squeleto are the fundamental unit of the library. The Scenes is where you would do all of these things: (not natively, you
have to write this stuff...)

- Setup game state
- Create Maps and Levels
- Create Entities
- Setup Story Flags
- Load Assets (sounds and images)
- Setup gameloop engine
- Setup Input handling (keyboard, mouse, gamepad, etc...)

Scenes carry their own UI binding state, so you can create UI elements within each state and then bind those elements to that state.
When this state is bound to your UI tempate, as you change your state value, it will automatically update your UI elements displayed.

#### SCENE STRUCTURE

    - Imports
        - All libraries and modules that are being brought in

    - Class definition
        - Each Scene class extends the Scene class

    - Class Properties
        - all Scene properties, of which some can be bound to the template UI

    - Template Definition
        - string literal that defines the HTML and bindings to be rendered

    - enter()
        - This methods get called when the Scene is switched to
        - used to define all aspects of the scene content

    - exit()
        - This method is called when scene is switched out of

### Imported Modules

All other aspects of your game content will be imported from the Squeleto library.

#### Library Imports

```ts
// Import all your Squeleto and Peasy Modules
import { Scene, SceneManager } from "../../_Squeleto/Scene";
import { Assets } from "@peasy-lib/peasy-assets";
import { Audio } from "@peasy-lib/peasy-audio";
import { Engine } from "@peasy-lib/peasy-engine";
import { Chiptune } from "../Systems/Chiptune";
import { State } from "@peasy-lib/peasy-states";
import { UI } from "@peasy-lib/peasy-ui";
import { Vector } from "../../_Squeleto/Vector";
import { Entity } from "../../_Squeleto/entity";
import { System } from "../../_Squeleto/system";
import { Signal } from "../../_Squeleto/Signals";
```

It will be commonplace to import modules from the Library to use in your scenes or in your game objects. The file diretory above shows
many of the library imports available to import.

#### Content Imports

```ts
// Next import your specific game content (Maps,etc...)
import { Kitchen } from "../Maps/kitchen";
import { OutsideMap } from "../Maps/outside";

// Import your entities
import { HeroEntity } from "../Entities/hero";
import { bookshelfEntity } from "../Entities/bookshelf";
import { CounterEntity } from "../Entities/counter";
import { NPCEntity } from "../Entities/npc2";
import { PlanterEntity } from "../Entities/planter";
import { PizzaSignEntity } from "../Entities/pizzasign";

// Finally Import your systems
import { CameraFollowSystem } from "../Systems/CameraFollow";
import { MovementSystem } from "../Systems/movement";
import { KeyboardSystem } from "../Systems/keyboard";
import { AnimatedSpriteSystem } from "../Systems/animatedSprite";
import { ColliderEntity, CollisionDetectionSystem } from "../Systems/collisionDetection";
import { RenderSystem } from "../Systems/Rendering";
import { EventSystem } from "../Systems/Events";
import { StoryFlagSystem } from "../Systems/StoryFlags";
import { interactionSystem } from "../Systems/Interactions";
import { Dialogue } from "../Systems/dialog";
```

If you are creating game content,which is very likely, you can import those custom content modules into your scene as well. For
example, importing your player GameObject, you level Maps, etc, etc...
