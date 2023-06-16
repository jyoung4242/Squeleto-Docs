# Sounds

Sounds drivers at that moment are handled by Howler.js, I will most likely move to peasy-audio upon its release and availability

## Usage

There are two audio classes available, BGM and SFX

Their usages are very similar.

For short sfx, the intent is to us SFX Class, for music in bacground, use BGM.

## Example

```ts
import { SFX } from "../_Squeleto/Sound API";

//register sounds
SFX.register({ name: "snort", src: Assets.audio("snort").src });
SFX.play("snort");
```

## Methods

- register(music: { name: string; src: string; gain?: number })

Takes the sound information passed and registers entry to set of sounds for that class.
returns the set of currently registered sounds.

- play(music: string)

Takes the string that is the registered name of the sound and plays it immediately. Note for BGM class, if there is an active bgm music playing at that time, it will fade out the active musice over half a second prior to starting the next music. This doesn't happen for SFX.

- clear()

This clears the registered set of sounds/music.
