# StoryFlags

## Approach and Usage

StoryFlags are boolean indicators that you can set and clear during the course of your game's journey. It is intended to be availalble to set points during the course of a game journey.

For example: maybe you want to change an interaction with an NPC between meeting them for the first time, and talking to them again. In the interaction array you can specify that you want to set a storyflag after that first interaction, and then in the second interaction the storyflag will route activity to another group of events.

StoryFlag data state is owned by the Renderer. That data is passed to all objects and events from there.

In Demo 1, for the npc, we see

```ts
    this.interactionEvents = [
      {
        conditions: {
          threat: false,
          meek: false,
          deaf: false,
          angry: false,
        },
        content: [new DialogEvent(new testConversation(this), this.dm, this.SM.StoryFlags)],
      },
```

when you setup a GameObject, the StoryFlags can be passed to the Object for usage.
in the constructore we'll find that `this.SM = StoryFlags;`.

this allows for all aspects of the gameobject to be able to set/clear StoryFlags via events, AND to respond to the current state of StoryFlags in the different update loops.

You also see in the interactionEvents array above, you can embed conditions that check the StoryFlag manager for validating, which then allows for the content array to be executed.

### Usage

Setting and clearing of StoryFlags can be done as a predefined event.

```ts
new SetStoryFlagEvent(who, s, this.SM.StoryFlags);
```

So in your trigger event list, or in a GameObjects interaction list you can list `SetStoryFlagEvent`. The StoryFlag event takes these params:

- `constructor(manager: StoryFlagManager, key: string, value: boolean)`

In Demo 1 we see this in a Dialog Event...

```ts
//bookcasepopup.ts
const snapshot1: DialogSnapshot = {
  conditions: {
    metBookcase: false,
  },
  content: [
    { type: "none", speed: 70, message: "just a boring bookcase!!! ", avatar: [], end: true, flags: { metBookcase: true } },
  ],
};
const snapshot2: DialogSnapshot = {
  conditions: {
    metBookcase: true,
  },
  content: [{ type: "none", speed: 70, message: "still a boring bookcase ", avatar: [], end: true }],
};
```

StoryFlags come into play when we can pose conditions on different actions, to essentially select what actions to take. Also, within this current Dialog event, there is a connection to StoryFlags that sets the flag `metBoookcase` to true upon completion of the dialog.

This is just one example of how StoryFlags can be implemented.
