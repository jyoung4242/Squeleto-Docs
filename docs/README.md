# Welcome to Squeleto

[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/bukotsunikki.svg?style=social&label=Follow%20%40jyoung424242)](https://twitter.com/jyoung424242)

> Squeleto is an simple 2d game framework built in Typescript and powered by Vite.

Squeleto is a spin off of 'Skeleton' in spanish. 'El Squeleto' as a framework which is intended to provide a highlevel skeleton or
abstraction for your game development needs.

## Peasy

Squeleto uses the peasy-lib framework to run many of the low level processes, including Asset management, UI binding, Input action
binding, Game Loop, and in the future, will be a physics module and lighting module.

[Link to Peasy GitHub](https://github.com/peasy-lib/peasy-lib/tree/main)

## ECS Structure

Squeleto has a very rudementary Scene based format and each scene encapsulates sections of the game. Within a scene, Squeleto utilizes
an Entity,Component, System (ECS) structure. While other engines maybe be 'easier' to use, the ECS structure is highly extensibility,
as you can customize and tailer your enities however you want.

## Library of Systems and Components

> In the Squeleto Repo, you will find example systems/components/Events for reuse

- CLI tool to help get started
- Scene Templates
- Entity Templates
- System Templates
- Signals are pub/sub style information connections to link different parts of your code
- Multi-player library, there is a pre-configured module to help link to Hathora, a multi-player framework
- Vector module, assists with your typical vector style math, with helpful methods

## CLI

![Alt](/Squeleto.png "CLI")

```
npx squeleto
```

> This command will initiate the CLI too that can:

- Scaffold up blank project boilerplate
- Download demo top down RPG tutorial
- Link to Documentation

[GETTING STARTED](/GettingStarted)
