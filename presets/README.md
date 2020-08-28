# Performance effects, Maschine-style

Maschine has a very cool performance FX mode for instantaneous control of insert FX: the effect is only enabled when the strip is touched while the finger position controls a parameter. This works great when working inside Maschine for quickly throwing in effects while switching patterns and scenes.
It's also possible to do this with Maschine as an FX module with Jam
in Bitwig but with annoying limitations:

* Limited routing: can send up to 2 stereo channels to Maschine at most (with a sidechain input).
* Limited Jamming: with a single Jam controlling both Bitwig and Maschine's FX simultaneously is impossible.
* Limited selection: Maschine provides only a handful of FX to choose from. FX in Bitwig are just awesomer.

Wouldn't it be great if this mode existed in Bitwig itself? Well, now it does!

# In the box

This extension will look for a performance page called `TOUCH` (doesn't need to be the first page). If found, it expects it to contain 8 buttons. The button is toggled on and off when a corresponding touch strip is touched.

A container preset is included with the touch buttons already wired up.

# Quick start

## PERFORM FX on a single track

* Load the `Touch FX layer single` preset onto a track.
* Configure effects.
* Press `MACRO`.
* Jam.

### Configuring effects

The container contains a single layer of 8 FX selectors in series, each wired to the touch gesture via a button. Each selector contains two layers, an empty (dry) layer and another empty layer for the effect. Touching the corresponding strip routes incoming audio to the effect (second layer), releasing the strip bypasses the effect. Both the bypassed audio and the effect tail will be heard.

Load your desired effects in the FX layer in the selector and wire the knobs as desired.

Other configurations are possible (e.g. if FX tail is not needed, replace the selector with the FX and modulate the Mix knob with the button).

## PERFORM FX on multiple tracks (like Maschine)

A little more involved. Here is one way to do it:

* Create a group with 8 tracks, these will serve as insert FX busses
* Load the preset onto the group.
* In "Touch FX layer" replace the single layer with 8 layers, each consisting of an Audio Receiver (disabled) followed by a desired effect.
* Add one more layer for the dry signal when FX are bypassed, containing 8 Audio Receivers (enabled).
* Route audio in the group:
  * Each track should be received by one Receiver in front of an effect and one Receiver in the "dry" layer
  * The touch button should enable the FX Receiver and disable the "dry" receiver
  * Set track inputs and outputs to none
* Route audio tracks in the project to desired group tracks.
* On the JAM select the group track and jam.
