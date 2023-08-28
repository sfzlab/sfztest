# sfizz part 3 `pitch_keycenter=sample`

<sub><sup>kinwie edited this page on 12 Nov 2020 · 7 revisions</sup></sub>

sfizz is a new free sfz player in a very active development stage.
At this moment of writing, it is in beta version 0.5.1.
More information here : <https://sfz.tools/sfizz/>

## `pitch_keycenter=sample`

This less-known and less-used `pitch_keycenter=sample` is a very helpful opcode
to tidy-up some complex sfz programming. A details can be read here :
<https://sfzformat.com/opcodes/pitch_keycenter> It's a beta opcode since sfz v1
specs and implemented in rgc sfz and Cakewalk players.
Sforzando support it and now sfizz also fully support it since sfizz v0.5.0
with an exclusive behavior.

pitch_keycenter=sample opcode use root key metadata that available inside sample
file (wav/aiff), _similar to loop metadata_. So to use this opcode,
the samples need to be well-edited and put the root key info properly.
This can be done with audio editors like Wavosaur, Audacity, etc.

This is the description of how it works among players :

* **rgc sfz** and **Cakewalk players** : It works at all headers and
  can't be override by `key` nor `pitch_keycenter=N`
* **sforzando** : It works only at `<region>` header and can get override
  by `key` and `pitch_keycenter=N`
* **sfizz** : It works at all headers and can be override only by `pitch_keycenter=N`.
  It also can be used for Flac samples.

An example in practical use case, is for creating a "neighbor round-robin",
which an instrument has samples for every key, then shift the key mapping up and down.
Editing this kind of mapping can be easily done with a text editor that has a column-edit feature,
like Notepad++. Here's a simplified example without using `pitch_keycenter=sample`,
where we need to add another `pitch_keycenter` to the shifted samples to retain
the root key (because the use of `key`). This example only shows 10 samples,
imagine when you have thousand of samples you want to shift-mapped like this.

```c
<global>
seq_length=3

<group> seq_position=1
<region> key=49 sample=alto_01.wav
<region> key=50 sample=alto_02.wav
<region> key=51 sample=alto_03.wav
<region> key=52 sample=alto_04.wav
<region> key=53 sample=alto_05.wav
<region> key=54 sample=alto_06.wav
<region> key=55 sample=alto_07.wav
<region> key=56 sample=alto_08.wav
<region> key=57 sample=alto_09.wav
<region> key=58 sample=alto_10.wav

<group> seq_position=2
<region> key=49 pitch_keycenter=50 sample=alto_02.wav
<region> key=50 pitch_keycenter=51 sample=alto_03.wav
<region> key=51 pitch_keycenter=52 sample=alto_04.wav
<region> key=52 pitch_keycenter=53 sample=alto_05.wav
<region> key=53 pitch_keycenter=54 sample=alto_06.wav
<region> key=54 pitch_keycenter=55 sample=alto_07.wav
<region> key=55 pitch_keycenter=56 sample=alto_08.wav
<region> key=56 pitch_keycenter=57 sample=alto_09.wav
<region> key=57 pitch_keycenter=58 sample=alto_10.wav
<region> key=58 pitch_keycenter=56 sample=alto_08.wav

<group> seq_position=3
<region> key=49 pitch_keycenter=51 sample=alto_03.wav
<region> key=50 pitch_keycenter=49 sample=alto_01.wav
<region> key=51 pitch_keycenter=50 sample=alto_02.wav
<region> key=52 pitch_keycenter=51 sample=alto_03.wav
<region> key=53 pitch_keycenter=52 sample=alto_04.wav
<region> key=54 pitch_keycenter=53 sample=alto_05.wav
<region> key=55 pitch_keycenter=54 sample=alto_06.wav
<region> key=56 pitch_keycenter=55 sample=alto_07.wav
<region> key=57 pitch_keycenter=56 sample=alto_08.wav
<region> key=58 pitch_keycenter=57 sample=alto_09.wav
```

Then, for comparison, we can use pitch_keycenter=sample in sfizz.
The mapping process will be a lot more faster as we only need to shift the key
value using column-editing. The key range is shifted but the root key stay intact.

```c
<global>
seq_length=3
pitch_keycenter=sample

<group> seq_position=1
<region> key=49 sample=alto_01.wav
<region> key=50 sample=alto_02.wav
<region> key=51 sample=alto_03.wav
<region> key=52 sample=alto_04.wav
<region> key=53 sample=alto_05.wav
<region> key=54 sample=alto_06.wav
<region> key=55 sample=alto_07.wav
<region> key=56 sample=alto_08.wav
<region> key=57 sample=alto_09.wav
<region> key=58 sample=alto_10.wav

<group> seq_position=2
<region> key=49 sample=alto_02.wav
<region> key=50 sample=alto_03.wav
<region> key=51 sample=alto_04.wav
<region> key=52 sample=alto_05.wav
<region> key=53 sample=alto_06.wav
<region> key=54 sample=alto_07.wav
<region> key=55 sample=alto_08.wav
<region> key=56 sample=alto_09.wav
<region> key=57 sample=alto_10.wav
<region> key=58 sample=alto_08.wav

<group> seq_position=3
<region> key=49 sample=alto_03.wav
<region> key=50 sample=alto_01.wav
<region> key=51 sample=alto_02.wav
<region> key=52 sample=alto_03.wav
<region> key=53 sample=alto_04.wav
<region> key=54 sample=alto_05.wav
<region> key=55 sample=alto_06.wav
<region> key=56 sample=alto_07.wav
<region> key=57 sample=alto_08.wav
<region> key=58 sample=alto_09.wav
```
