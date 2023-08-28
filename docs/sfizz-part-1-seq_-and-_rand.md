# sfizz part 1 seq_ and _rand

<sub><sup>kinwie edited this page on 12 Nov 2020 · 12 revisions</sup></sub>

## sfizz improved behavior - part 1

sfizz is a new free sfz player in a very active development stage.
At this moment of writing, it is in beta version 0.5.1.
More information here : <https://sfz.tools/sfizz/>

## Combination of seq_ and _rand

Using round-robin `seq_length`, `seq_position` along with random trigger `lorand`,
`hirand` in some other sfz players mostly result in complete randomness. The intention to combine these two functions in practical usage is to be able to retain the sequence counter of articulations alternation, e.g. up-bow and down-bow of violin, or left-hit and right-hit of snare drum, while each articulation also has random samples. Combining these two functions in sfizz will result a proper triggered sounds, where the alternation seq_ will be retained well and randomness _rand also will be keep going on.

Here is an example to check the details of this behavior.
Notice that `seq_position` are placed on `<group>` (higher header) and
`lorand/hirand` are placed on `<region>` (lower header)
The sounds will always alternate between _human voice_ and _drums hit_ :

```c
<global>
loop_mode=one_shot
key=60
seq_length=2

//human voice count
<group> seq_position=1
<region> lorand=0.0 hirand=0.1 sample=count_01.wav
<region> lorand=0.1 hirand=0.2 sample=count_02.wav
<region> lorand=0.2 hirand=0.3 sample=count_03.wav
<region> lorand=0.3 hirand=0.4 sample=count_04.wav
<region> lorand=0.4 hirand=0.5 sample=count_05.wav
<region> lorand=0.5 hirand=0.6 sample=count_06.wav
<region> lorand=0.6 hirand=0.7 sample=count_07.wav
<region> lorand=0.7 hirand=0.8 sample=count_08.wav
<region> lorand=0.8 hirand=0.9 sample=count_09.wav
<region> lorand=0.9 hirand=1.0 sample=count_10.wav

//drums hit
<group> seq_position=2
<region> lorand=0.00 hirand=0.25 sample=kick.wav
<region> lorand=0.25 hirand=0.50 sample=snare.wav
<region> lorand=0.50 hirand=0.75 sample=hat.wav
<region> lorand=0.75 hirand=1.00 sample=crash.wav
```

OTOH, by reversing the placement, `_rand` then `seq_`,
the result will be a fully randomness, like _"the usual"_ implementation :

```c
<global>
loop_mode=one_shot
key=60

//human voice count
<group> seq_length=10 hirand=0.5
<region> seq_position=01 sample=count_01.wav
<region> seq_position=02 sample=count_02.wav
<region> seq_position=03 sample=count_03.wav
<region> seq_position=04 sample=count_04.wav
<region> seq_position=05 sample=count_05.wav
<region> seq_position=06 sample=count_06.wav
<region> seq_position=07 sample=count_07.wav
<region> seq_position=08 sample=count_08.wav
<region> seq_position=09 sample=count_09.wav
<region> seq_position=10 sample=count_10.wav

//drums hit
<group> seq_length=4 lorand=0.5
<region> seq_position=01 sample=kick.wav
<region> seq_position=02 sample=snare.wav
<region> seq_position=03 sample=hat.wav
<region> seq_position=04 sample=crash.wav
```

Another improved trigger logic in sfizz is that, the regions doesn't need
to be fall on the same velocity boundaries between round-robin groups.
This below example will be properly triggered without any "hole" :

```c
<global>
loop_mode=one_shot
key=60
seq_length=2
amp_veltrack=0

//human voice count
<group> seq_position=1
<region> lovel=001 hivel=012 sample=count_01.wav
<region> lovel=013 hivel=025 sample=count_02.wav
<region> lovel=026 hivel=037 sample=count_03.wav
<region> lovel=038 hivel=050 sample=count_04.wav
<region> lovel=051 hivel=063 sample=count_05.wav
<region> lovel=064 hivel=076 sample=count_06.wav
<region> lovel=077 hivel=089 sample=count_07.wav
<region> lovel=090 hivel=101 sample=count_08.wav
<region> lovel=102 hivel=114 sample=count_09.wav
<region> lovel=115 hivel=127 sample=count_10.wav

//drums hit
<group> seq_position=2
<region> lovel=001 hivel=031 sample=kick.wav
<region> lovel=032 hivel=063 sample=snare.wav
<region> lovel=064 hivel=095 sample=hat.wav
<region> lovel=096 hivel=127 sample=crash.wav
```
