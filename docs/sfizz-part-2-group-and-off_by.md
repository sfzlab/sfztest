# sfizz part 2 `group` and `off_by`

<sub><sup>kinwie edited this page on 12 Nov 2020 · 12 revisions</sup></sub>

## sfizz improved behavior - part 2

sfizz is a new free sfz player in a very active development stage.
At this moment of writing, it is in beta version 0.5.1.
More information here : <https://sfz.tools/sfizz/>

## No self-choking with same `group` and `off_by`

There are two different implementations on the _"_exclusive group/choke group_
behaviors which use `group` and `off_by` opcodes for muting function,
e.g. hihats and cymbals choke, etc, which mostly played as `one_shot`

* In original rgc sfz design (and also Cakewalk players), using same number
  for `group` and `off_by` won't make the region (same key) self-choking
  (muted by itself), which means the region/key is polyphonic,
  but it will get choked by different keys of the same `group`.
  This behavior is good and quick for setting choke groups between different keys only.
  But it has one issue since it can't choke on the same key,
  e.g, on a common hihats programming (for eDrums) using CC4 Foot Controller
  to switch between articulations that put all on one key.
  A simple example : Closed hihats that put on `locc4=0` `hicc4=63` can't choke
  the Open hihats that put on `locc4=64` `hicc4=127`, which both on key 42.

* ARIA/sforzando has more strictly behavior. When the `group` and `off_by`
  has the same number, the region/key will self-choking, which means it became monophonic.
  Though it's logically correct, it could make a simple choke setting became
  more complex when polyphonic is wanted.
  But it also solved that _"Same key with CC"_ programming issue.

Then sfizz combines the best of both and result in much more flexible behavior.

* Using same number for `group` and `off_by` in sfizz won't make the key self-choking,
  so it's polyphonic. This also solved the _"monophonic"_ sound issue cause
  by auto-converted sfz instruments, which it usually exported with the same numbers,
  e.g. sfz that are converted from sf2.
* For CC programming to use with eDrums, mostly for controlling hihats articulations
  also working great with sfizz. Assign each articulation to each `group` number
  and then assign different numbers for the `off_by`.
  This can be a bit more complex when more articulations are used,
  e.g. if you have like, 5 to 8 hihats openness degree
  (Tight, Closed, Quarter, Half, 3/4 open, Full open, etc.).
  Maybe some other article to go more in-depth for this kind of settings.

To quick and simple identifying this behavior, try this example below
(using crash samples for more easier to notice) :

```c
<region>
sample=crash.wav
loop_mode=one_shot
key=60
group=1
off_by=1
```

Test that example in sforzando, play key 60 fast and you will hear the crash
sounded cut-off monophonic. Try the same action in sfizz and you will hear
the crash is ringing loosely as it's supposed to.

So, to get that same "monophonic" cut-off sound in sfizz, just simply use `polyphony=1` :

```c
<region>
sample=crash.wav
loop_mode=one_shot
key=60
group=1
polyphony=1
```

Then a simple example for that "Hihats CC4 programming" can be like this
(using different numbers) :

```c
<global>
key=42

<group> loop_mode=one_shot
<region> locc4=000 hicc4=025 group=1 off_by=6 sample=HH_open.wav
<region> locc4=026 hicc4=050 group=2 off_by=7 sample=HH_3_4.wav
<region> locc4=051 hicc4=076 group=3 off_by=8 sample=HH_half.wav
<region> locc4=077 hicc4=101 group=4 off_by=9 sample=HH_1_4.wav
<region> locc4=102 hicc4=127 group=5 sample=HH_closed.wav

<group> end=-1 sample=*silence
<region> locc4=026 hicc4=127 group=6
<region> locc4=051 hicc4=127 group=7
<region> locc4=077 hicc4=127 group=8
<region> locc4=102 hicc4=127 group=9
```
