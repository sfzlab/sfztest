# How Release Trigger Works in SFZ

<sub><sup>kinwie edited this page on 29 Oct 2020 · 11 revisions</sup></sub>

This is a short documentation for Release Trigger behaviors in 3 'major' SFZ players.

The opcodes are : `trigger=release` and `trigger=release_key`.
(See here : <https://sfzformat.com/opcodes/trigger>)

Primary condition : Setting trigger to release or release_key will automatically set the region into one-shot mode. (More details here : <https://sfzformat.com/opcodes/loop_mode>)

Here is the main difference of `release` and `release_key` in original sfz design by Rene Ceballos (implemented in rgc sfz and Cakewalk sfz players) that can be tested with this sequence of playing notes with sustain pedal down then release the notes :

* `release` : The release samples will always play when releasing the sustain pedal
* `release_key` : The release samples won't play when releasing the sustain pedal.
  Release samples only play when sustain pedal is up (not depressed)

## rgc sfz

* Both `release` or `release_key` plays without any previous attack region
* Both functioning as described above and respond to Sustain Pedal position
* In sfz v1, no `polyphony` or `note_polyphony` for limiting,
  so repeated notes will have release voices as many as the attack notes,
  when Sustain pedal is down, then release the pedal. This can be very annoying

## DropZone and other Cakewalk players

* Both `release` or `release_key` require previous attack region
* Both functioning as described above and respond to Sustain Pedal position
* To make `release` or `release_key` plays without any previous attack region, add `rt_dead`=on
* Opcode `note_polyphony` now available to control the release voices
  of repeated notes when Sustain pedal is down, then release the pedal

## Plogue sforzando and ARIA

* `release` require previous attack region
* `release_key` doesn't need previous attack region
* `release` respond to Sustain Pedal position
* `release_key` ignores Sustain Pedal
* Not supported `rt_dead`, default to off
* Same as DropZone, use `note_polyphony` to control release voices

A simple sfz example :

```c
<control>
default_path=Samples\

//simple release trigger example (without note_polyphony)
<region> key=60 sample=kick.wav
<region> key=60 sample=snare.wav trigger=release

//release-key in sforzando
<region> key=62 sample=snare.wav trigger=release_key

//release-key in Cakewalk players
<region> key=64 sample=snare.wav trigger=release rt_dead=on

//*note, Cakewalk players use backslash only
```
