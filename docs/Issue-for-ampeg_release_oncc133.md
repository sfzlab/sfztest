# Issue for ampeg_release_oncc133

<sub><sup>kinwie edited this page on 13 Jun 2020 · 11 revisions</sup></sub>

This is an example of a more _"sophisticated-yet-simplified"_ envelope setting, reflect from the Inital version of _Maestro Grand Piano_ sfz that I put at : <https://github.com/sfzinstruments/MatsHelgesson.MaestroConcertGrandPiano> Which was later changed to conventional setting because of an issue raised by the use of CC133 for `ampeg_release` when involving sustain pedal.

That setting preserved in a test file **"Ampeg_oncc133 test.sfz"** for documentation purpose.
The test file consist of 2 examples in keyswitch for comparison.
Here's are the explanation for the extended CC usage for _amplitude envelope_
and the problem scenario.

The hold and decay are using CC133, like these :

```c
ampeg_hold=0.5
ampeg_hold_oncc133=-0.5
ampeg_decay=20
ampeg_decay_oncc133=-20
```

Where the hold and decay time will decreased from lower key to higher key, longer to shorter.

* CC133 is the extended CC for keyboard note number.
* In the 1st keyswitch example : **Dampened high-notes**, the release also use CC133 :

```c
ampeg_release=2
ampeg_release_oncc133=-2
```

This scenario didn't have too much noticed issue with the sustain pedal use.

* In the 2nd keyswitch example : **Undampened high-notes**,
which is typical for piano release setting, like this :

```c
ampeg_release_oncc131=0.4
ampeg_release_oncc133=5
ampeg_release_curvecc133=7
```

With the custom curve :

```c
curve_index=7
v021=0.4
v036=0.08
v084=0.08
v090=0.4
v091=1
v108=1
```

Using CC131 (note-on velocity) is for making the release time increase from lovel to hivel, 0 to +0.4 seconds. For CC133 with value at 5 seconds, it utilize the use of custom curve definition to shape the release time across key-range.

* At key 21, will have release time 2 seconds and decrease to key 36
* At key 36 to 84, release time 0.4 seconds
* At key 90, release time 2 seconds, increased from key 84
* At key 91 to 108, release time 5 seconds

At the moment, this kind of setting will have issue when using the sustain pedal. The issue was found by @jlearman, with action sequence as :

* Hold sustain pedal down
* Press low note (e.g. 60/C) and release
* Press Undampened high-note (e.g. 91/G) and release
* Release sustain pedal

The low note C continues to ring, which is supposed off.

So, this documentation posted for future reminder if similar case occurred using CC133.

