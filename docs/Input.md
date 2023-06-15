# Input Handling

## Approach

The purpose of this library module was to expose an easy API for binding keyboard, mouse, and gamepad actions to events.
This leads to a statically declared InputManager module that looks for the overall mapping configuration to be passed to the `register()` method

The register method is the only needed method in this module, all other are internally used.

## Usage

```ts
import { InputManager } from "../../src/components/InputManager";

//Initialize the Inputmanager
InputManager.register({
  Keyboard: {
    //pass mapping here
  },
  Mouse: {
    //pass mapping here
  },
  Gamepad: {
    //pass mapping here
  },
  Touch: {}, //in development
});
```

### Keyboard Mapping

```ts
//Example
{
    ArrowLeft: {
        name: "leftA",
        callback: this.leftArrow,
        options: {
            repeat: false,
        },
    },
}
```

the format for mapping callbacks to keystrokes is

```ts
 Keycode:{
    name: 'somename',
    callback: someCallback,
    options:{
        repeat: false //boolean
    },
 },
```

A complete list of JS keycodes are here:
[KeyCode List](https://www.freecodecamp.org/news/javascript-keycode-list-keypress-event-key-codes/).

Then later in your scene of GameObject, you declare your callback with its game logic in it.

### Mouse

Mouse mapping is slightly different than keyboard mapping. This is due to two aspects of the return value.
the callbacks assigned to the click events provide back:

- xPos and yPos, this is the RAW x,y coordinates for the click event
- scaledX and scaledY, this is with respect to your viewport, which includes a scaling factor for responsiveness

At time of this writing, only left and right click events supported, more to come.

### Mouse Usage

```ts
Mouse: {
        ViewportScaling: [
          { scalingFactor: 0.75, maxwidth: 675 },
          { scalingFactor: 1.5, maxwidth: 1100, minwidth: 675 },
          { scalingFactor: 2.5, minwidth: 1100 },
        ],
        LeftClick: {
          name: "mouseClick",
          callback: this.mouseClick,
        },
        RightClick: {
          name: "rightClick",
          callback: this.rightClick,
        },
      },
```

Viewport Scaling, you must pass the viewport scaling details to the Mouse Event Handler to calculate depending on the responsive sizing of the viewport, what the relative mouse click location is.

This data is the transform Scale value for scalingFactor, and the media query break points for min and max width params.

### Gamepad

This module is an abstraction of the GamePad API in JS. So there are 'buttons' and axes to map to.
The number of buttons and the axes mapping will be specific to the type of gamepad you're supporting.
This library was developed off of a DualSense PS controller, so for these examples, it'll be 18 buttons and 2 axes.

//TODO - add controller mapping info

The gamepad mapping takes to arrays, buttons and axes.
The button array is simply the callbacks to map to the buttonpress events on the gamepad.
The axes array is a list of object that are defined below.

```ts
Gamepad: {
        buttons: [],
        axes: [],
},
```

#### Buttons

The button array is simply a list of callbacks passed. The index of the array passed will map to the corresponding button number of the gamepad.

i.e. index 0 will map to button 0, callback at index 4 will map to button 4.

Example

```ts
buttons: [
  this.crossButton,
  this.circleButton,
  this.squareButton,
  this.triangleButton,
  this.l1Button,
  this.r1Button,
  this.l2Button,
  this.r2Button,
  null,
  null,
  null,
  null,
  this.dpadUp,
  this.dpadDown,
  this.dpadLeft,
  this.dpadRight,
  null,
  null,
];
```

#### Axes

Each element in the axes array will map to a stick 'direction'

for example Axes array index 0, will map to the left/right axes of the Left analog stick on the DualSense controller....
while index 1 maps tot he up/down analog stick.

The analog sticks report back an variable value from -1 to 1 representing its maximum value spectrum from the sticks being pressed.

The call back mapping passed to the InputHandler sets callbacks associate with thresholds of those values, so when you cross that analog threshold value, it fires off the callback.

In the below example, we're mapping this.leftStickDown to fire when the associated analog value returned from the API crosses the 0.75 threshold. This is monitored in a RAF loop, so as long as that stick is exceeding that threshold, that callback fires EVERY loop.
Plan accordingly.

```ts
{
    "0.75": this.leftStickDown,
    "-0.75": this.leftStickUp,
},
```

One of the goals of this library is to establish mulitple thresholds mapped to a stick, so the usecase would be if you want to fire a WALK event when the stick is 'halfway' pressed, it will fire one callback, and if its fully pressed, it will fire another.

That pattern would look like this.

```ts
{
   "0.8": this.leftStickRight,
   "-0.8": this.leftStickLeft,
   "0.4": this.leftStickWalkRight,
   "-0.4": this.leftStickWalkLeft,
},
```

    The critical aspect to note is that the higher threshold values must be listed first, as if you cross those, it will bail on the iterative review for that axes.

    If you swap the values, a fully pressed stick would falsly fire the walk callback in this instance, which is an unwanted side effect.

### Touch?

Still in development
