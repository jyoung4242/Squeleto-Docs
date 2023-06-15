# Getting Started

> Covering the CLI tool

## CLI

![Alt](/Squeleto.png "CLI")

```
npx squeleto
```

Currently, this CLI tool will provide 3 options:

- Start New Project
- Download Tutorial 1
- Download Tutorial 2

### Start New Project

This setups up the file structure and boilerplate project so one can jump right in and start coding up their new game.
The creates and index.html and main.ts file that pre-sets up a Viewport and loads a starter scene.

### Download Tutorial 1

This setups up the file structure, source code, and game assets for a top down RPG game. The project has two scenes, a loading page and the main game loop.

### Download Tutorial 2

This setups up the file structure, source code, and game assets for a side-scrolling platformer game. The project has two scenes, a loading page and the main game loop.

## Game Architeture

![Alt](/gamearch.png "Squeleto Architecture")

### Scenes

Scenes in Squeleto are the fundamental unit of the library. The Scenes will do ALL of these things:

- Setup game state
- Create Maps and Levels
- Create game objects
- Setup Story Flags
- Load Assets (sounds and images)
- Setup gameloop renderer
- Setup Input handling (keyboard, mouse, gamepad, etc...)

There are two types of scenes by design, game scenes and non-game scenes. Non-game scenes are what are used for title screens and other general purpose scenes that don't need to rely on a game loop or object rendering.

Game Scenes are just like other scenes, except for the implementation of the Renderer. The Renderer has your game loops (update and physics) as well as your Camera system native to it.

Game Scenes can be leveraged in one of two ways, you can bundle you're whole game up in one scene, or you can split your game up, in a manner that makes sense, and have the different scenes transition to each other. Essentially, they are just states in a state machine.

Scenes carry their own UI binding state, so you can create UI elements within each state and then bind those elements to that state. When this state is bound to your UI tempate, as you change your state value, it will automatically update your UI elements displayed.

### Imported Modules

All other aspects of your game content will be imported from the Squeleto library.

![Alt](/filedirectory.png "Squeleto Library Files")

#### Library Imports

```ts
import { Scene } from "../../_Squeleto/SceneManager";
import { GameRenderer, RenderState, RendererConfig } from "../../_Squeleto/Renderer";
import { Assets } from "@peasy-lib/peasy-assets";
import { StoryFlagManager } from "../../_Squeleto/StoryFlagManager";
```

It will be commonplace to import modules from the Library to use in your scenes or in your game objects.
The file diretory above shows many of the library imports available to import.

#### Content Imports

```ts
import { Kitchen } from "../Maps/kitchen";
import { Player } from "../Game Objects/Player";
import { Counter } from "../Game Objects/Counter";
import { Bookshelf } from "../Game Objects/Bookshelf";
import { NPC1 } from "../Game Objects/npc1";
import { OutsideMap } from "../Maps/outside";
import { DialogManager } from "../PlugIns/DialogueManager";
import { Planter } from "../Game Objects/planter";
import { PizzaThingy } from "../Game Objects/pizzathingy";
```

If you are creating game content,which is very likely, you can import those custom content modules into your scene as well. For example, importing your player GameObject, you level Maps, etc, etc...
