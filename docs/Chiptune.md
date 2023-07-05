[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/bukotsunikki.svg?style=social&label=Follow%20%40jyoung424242)](https://twitter.com/jyoung424242)

<h2 align="center">ChipTune Module</h2>
<h4 align="center">This module is an abstraction of the AutoTracker Project, which is  </h4>
<h4 align="center">an endless randomised algorithmic Chiptune composition by Vitling aka Demoscene Time Machine </h4>

Link to [AutoTracker](https://www.vitling.xyz/toys/autotracker/)

![Screenshot](./autotrackerSS.png?raw=true "Screenshot")

- Copyright 2020 David Whiting
  This work is licensed under a Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/

# Usage

```ts
import { Chiptune } from "../../_Squeleto/Chiptune";

//Starts infinite music
let myBGM = new Chiptune("seedstring");

//Stops infinite music
myBGM.mute();
myBGM = null; //clears out the api
```

## Attribution for the license

-- from Chiptune.ts

```ts
/*
  This class is generated as an abstraction of the Autotracker project.
  https://www.vitling.xyz/toys/autotracker/

  This is my attribution to that project, and a link to the license of it below.
  Modifications to the source include refactoring tracker.ts into an importable class for Squeleto Game Framework

  Copyright 2020 David Whiting
  This work is licensed under a Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
*/
```
