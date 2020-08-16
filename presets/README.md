# Performance effects, Maschine-style

Maschine has a very cool performance FX mode for instantaneous control of insert FX: the effect is only enabled when the strip is touched while the finger position controls a parameter. This works great when working inside Maschine for quickly throwing in effects while switching patterns and scenes.
It's also possible to do this with Maschine as an FX module with Jam
in Bitwig but with annoying limitations:

* Limited routing: can send up to 2 stereo channels to Maschine at most (with a sidechain input).
* Limited Jamming: with a single Jam controlling both Bitwig and Maschine's FX simultaneously is impossible.
* Limited selection: Maschine provides only a handful of FX to choose from. FX in Bitwig are just awesomer.

Wouldn't it be great if this mode existed in Bitwig itself? Well, now it sort of does!

# Quick start

The "touch FX" mode works by combining several components:

* "Touch FX layer" container combines 8 "PERFORM" style controls with 8 FX chains meant to be used as inserts.
* "Touch FX bus group" is a group scene that contains the "Touch FX layer" container together with 8 pre-routed tracks that serve as busses.
* Script changes designed to work with the container preset.

Let's say we want to apply performance FX to a signal coming out of a track.

## Setup

1. Load the "Touch FX bus layer" to create the "Touch FX" group.
2. Route the source track output to one of the tracks in "Touch FX" group.
3. In the group's "Touch FX layer" device, navigate to the layer corresponding to the track.
4. The layer is its own audio chain - put your desired effects after Audio Receiver device.

## Playing

1. Make sure the "Touch FX layer" device is selected. Selecting the "Touch FX" track by pressing the track button on Jam is usually enough since it's the first device on that track.
2. Hit "MACRO".
3. Touch the corresponding touchstrip.

You'll hear the effect engage (100% wet). After releasing the strip you'll hear unaffected source audio again, plus any effect tails.

## More setup

By default the macro knob is not connected to anything interesting, all it does is flip the Mix knob on the inner receiver to 1 when the macro knob is above 0. The knob is a modulation source, route it to your FX parameters as desired.

# How it works and where to go from here

This solution is a bit of a hack. We combine two parameters (a variable parameter value and a toggle) into one by treating 0 as "off". Ideally we'd be able to control a knob and a toggle button.

The extended script does a couple of things:

* When a device is selected, it looks for macro knobs with labels ending in " TFX". 
* When a touchstrip corresponding to one of those knobs is released, it forces the knob to 0.

Note that any performance knob on any device will work this way, as long as it's on the active page. The presets are simply a convenience.

The layer preset is a container with 8 modulation knobs added to "Perform" page ("macros" in API v1 speak). Each layer has the same basic structure: a bus audio receiver followed by effects. An additional "dry sum" layer routes dry audio when effect is not enabled. The knob routes audio to either the effect or the dry bus, by enabling one of the two audio receivers.

The group preset contains 8 "bus" tracks, source tracks can be routed to them directly.

## Caveats

The "off" detection is laggy (as of BWS 3.2.7). This is probably because Bitwig has to convert a 7-bit CC value to a smooth continuous signal, and it takes a bit of time for that signal to smoothly drop to 0.

Therefore:

* The time between touchstrip release and the effect cutover is short but noticeable, you can even see the knob jump to zero faster than Quantize reacts. The good news is this lags seems pretty consistent.
* If you manage to slide the strip all the way to zero the effect will disengage even though the strip is still being touched. This is the price for stealing a value to widen a parameter type. Overall it's not a bad deal.