# Events and Event Management

The Sqeueleto Event system creates an asynchronus event 'runner' that executes a list of 'events' one after another until the list is complete, or if a loop, restarts at the beginning of the list.

One use case for this an idle behavior loop for an NPC that you want to constantly be executing.

You can set the idle behavior for a Game Object similar to the NPC object in Demo 1.

The events as defined are specific content for your game, and can be imported into your game object definition.

In this instance the behavior eventmanager is set to loop, so when not in a cutscene, this NPC will keep running this loop over and over.

```ts
this.behaviorLoop.loadSequence([
  new WalkEvent("down", 60),
  new npcChangeMap("outside", 105, 75),
  new WalkEvent("down", 10),
  new WalkEvent("up", 10),
  new npcChangeMap("kitchen", 70, 150),
  new WalkEvent("up", 60),
]);
```

## EventManager

To use an EventManager in your game, you can import it and define it in a gameobject.

```ts
this.behaviorLoop = new EventManager(this, "LOOP");
```

Event Managers need a reference to the calling game object, this, and also need to understand HOW they operate, and this is defined as either:

```ts
export type EventConfigType = "LOOP" | "CUTSCENE";
```

To execute a list of events, the sequence of the EventManager needs loaded.
We've seen an example of this earlier above, but in essence:

```ts
EventManager.loadSequence(seq: GameEvent[])
```

You will pass an array of Events to the method.

When ready to execute the loop, run the start method.

```ts
EventManager.start();
```

### List of EventManager methods of note:

- loadSequence(GameEvent[])
  > Loads the event loop to execute, if called with new list, clears old list of events and replaces it
- start()
  > Starts the execution of the list of events. IMPORTANT note: if EventManager is configured for cutscene, this returns a Promise that resolves at the end of the execution list so one can perform an async await on this execution and then resumes after all events finish. In "LOOP" format, this doesn't happen
- pause()
  > Pauses execution of event list at the NEXT event step, will finish current Event in excution
- resume()
  > Resumes execution of event list at the NEXT event step

## Events

In your game content, you may want to create asynchronous events that can be executed. These may include for example:

- Walking Events
- Dialog Events
- Standing Events
- Menu Modals Events

These Events will have to be incorporated into your game content specifically.

### Boilerplate of an Event

As an example, this is an asynchronous console.log routine that can be used for Diagnostic purposed during development. While it may seem silly to wrap a console.log into an Event like this, it serves a purpose.

Your Event is a class that extends GameEvent, in your constructor you will pass the name of your event in the super call.
If you need to track which GameObject is executing the event (recommended), setup the who reference for the calling gameobject.
This is nice if you want any gameobject to be able to call your event.

the init routine is the method called by the EventManager when its executing your routine, as it returns the async Promise back to the EventManager.
This is the method that 'does the event'.

It is critical that there is an exit path for the Promise, so resolve MUST be called at somepoint or your EventManager will freeze and never continue on.

```ts
import { GameEvent } from "../../_Squeleto/EventManager";
import { GameObject } from "../../_Squeleto/GameObject";

export class LogEvent extends GameEvent {
  who: GameObject | undefined;
  message: string;
  resolution: ((value: void | PromiseLike<void>) => void) | undefined;

  constructor(message: string) {
    super("log");
    this.who = undefined;
    this.message = message;
  }

  init(who: GameObject): Promise<void> {
    return new Promise(resolve => {
      this.who = who;
      console.log(`${this.who.name} logged: ${this.message}`);
      resolve();
    });
  }
}
```

If your event is dependent on something else happening other places, a pattern that includes a pub/sub approach is recommended.

For this situation, this pattern is recommended. Make note of the eventlistener being added in the init() method, and a separate handler for that custom event. Notice how the this.who comes into play in the handler method as this is what manages which GameObject is resolving which Event, as you may have multiple EventManagers executing common custom events in parallel.

The resolve() method for this event is tied to this.resolution, so that it can be executed when the custom event is fired off.

```ts
init(who: GameObject): Promise<void> {
    return new Promise(resolve => {
      document.addEventListener("walkCompleted", this.completeHandler);
      this.who = who;
      this.who.startBehavior("walk", this.direction, this.distance);
      this.resolution = resolve;
    });
  }

  completeHandler = (e: any) => {
    if (e.detail === this.who) {
      document.removeEventListener("walkCompleted", this.completeHandler);
      if (this.resolution) this.resolution();
    }
  };
```

### List of pre-created Events:

- ChangeMap (in Renderer.ts)
  > This takes a mapname string, and staring position parameters for where the object moves to on transition. This changes the active map being rendered so is not applicable for having a NPC leave a map. That will be a custom event created for that NPC. The Demo 1 has such an event drafted.
- SetStoryFlagEvent (in StoryFlag.ts)
  > This takes a reference to the scene's StoryFlagManager, the story flag name (key: string), and the boolean value to set it to
