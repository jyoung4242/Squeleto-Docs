# Camera

The Camera system controls what is viewed inside the Viewport window.

## Follow

It is helpful to assign a gameobject for the camera to follow so it knows where its anchored at.

```ts
//Set Camera
this.renderer.cameraFollow("Player");
```

## Flash

One effect of the camera you can use is if you want the Viewport to flash. It takes a number paremeter that represents milliseconds of the flash effect.

```ts
cameraFlash(duration: number)
```

## Shake

Another available camera effect is to shake the camera to provide a visual effect. This method takes a couple parameters:

- who, the GameObject calling the shake
- direction, `export type ShakeDirection = "horizontal" | "vertical" | "random";`
- magnitude, a number representing how strong of a shake in pixels
- duration, a number, in milliseconds, to represent for how long of time the shake occurs
- interval, a number representing the frequency of the shaking while active

```ts
cameraShake(who: GameObject, direction: ShakeDirection, magnitude: number, duration: number, interval: number)
```

## Show Diagnostic collision bodies

![Alt](/collisions.png "Collision Bodies")

This is in the library for development purposes, sometimes its nice to be able to see the collision bodies on the screen as you develop.

```ts
this.renderer.showCollisionBodies(true); //true turns on, false turns off
```
