# Polyaft choke with velocity range part 2

<sub><sup>kinwie edited this page on 13 Jun 2020 · 2 revisions</sup></sub>

Advancing previous cymbal choke test, here's an example to set the release-triggered
with multi-velocities samples. The test file for this is **"Polyaft choke velocity.sfz"**,
consist of 2 keyswitches example. One using Left Pedal for easy testing
just within sforzando's GUI and the second is using Polyphonic Aftertouch CC130
for _edrums_ mapping mode.

First, notice that `lovel` and `hivel` are not working for this kind of CC usage.
So for that replacement, we use `locc131` and `hicc131` which are the extended CC
for note-on velocity and they will works. The release-trigger samples used
are snare, kick and choke for easier sound-identifying.

Performing this test is very simple, just like this :

* Soft hit, velocity 0 - 42 : the choke sound is snare sample
* Medium hit, velocity 43 - 84 : the choke sound is kick sample
* Hard hit, velocity 85 - 126 : the choke sound is choke sample

Last notice, is that the choke highest velocity is set to 126 to prevent it sounding without the crash hit first.

* An additional workaround can be made to solve the _"no choke at velocity 127"_ issue
  by adding a `*silence` region to that vel-range with `locc131=127` `hicc131=127`

