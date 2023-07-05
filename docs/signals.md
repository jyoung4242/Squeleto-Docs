[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/bukotsunikki.svg?style=social&label=Follow%20%40jyoung424242)](https://twitter.com/jyoung424242)

# 👋 Introducing `Signals`

<h1 align="center">Signals!!!!</h1>
<h4 align="center">Native Pub/Sub events in Squeleto</h4>

A signal is a custom broadcast message from a sender that other entities can subscribe to listening to.

## How they work

![Signals](/signals.png?raw=true "Signals")

## Sending Usage

```ts
import { Signal } from "../../_Squeleto/Signals";
const mySignal = new Signal("start game", "Player1");
mySignal.send();
```

## Listening Usage

```ts
import { Signal } from "../../_Squeleto/Signals";
const mySignal = new Signal("start game");
mySignal.listen(() => {
  console.log("signal received");
});
```

## API

constructor(name: string, from?: string | GameObject){}

    params:
    - name: string, name of signal
    - from?: either string or GameObject

send([...params])

    params:
    - params: array of any, so data can be sent with the signal

listen(callback: Function)

    params:
    - callback: Function that is run when signal received, CAN be an anonymous function
    params fed into signal will be passed to this function
    i.e. (e)=>{...}

stopListening()

    params:
    - none
