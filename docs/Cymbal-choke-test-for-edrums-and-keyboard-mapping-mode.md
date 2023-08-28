# Cymbal choke test for edrums and keyboard mapping mode

<sub><sup>kinwie edited this page on 13 Jun 2020 · 11 revisions</sup></sub>

This is a test in Plogue sforzando, describe a proper cymbal choke programming, for both edrums and keyboard mapping mode, and also to pursue the unavailable yet `on_locc133` and `on_hicc133` or `on_key` opcode.

More info about Extended CC : <https://sfzformat.com/extensions/midi_ccs>

The test file for this is **"Cymbal choke test.sfz"**. A MIDI file, **"Cymbal choke test.mid"** also available for a "quick play". The samples used are a crash sample and a crash choke/tail sample. The sfz test file consist of 5 examples in Keyswitch to show and compare each case :

## 1. Regular mapping

* This is a simple/common-type of cymbal choke programming. Crash at note G (key 55) will get muted by the choke at note F# (key 54). But since the choke sample is a crash tail, it will sounded everytime note F# is pressed and this is an unwanted issue.

## 2. Left Pedal CC67

* This is a good example of cymbal choke programming by utilizing MIDI CC and taking advantage of `rt_decay` and `trigger=release` function. In this example, the used MIDI CC number is 67, which is can be accessed by sforzando's default Left Pedal. The choke key is moved to the same key as the crash, note G (key 55) and put into _release trigger_ mode. Pressing the crash key will sounds normally. But then pressing the Left Pedal, the crash will get muted. After pressing the crash key, the more late you press the Left Pedal, the choke tail volume will also get lower, which is the advantage of `rt_decay` function. So, programming this way is working for sfz, then we just need to change the MIDI CC number to Polyphonic Aftertouch for edrums mode. For this example, there is an issue because the use of `on_lo/hiccN` opcode, which, the choke tail sample will play everytime the Left Pedal is pressed. So to prevent that, opcode `hicc131=126` has been added, but with the cost of crash at vel 127 won't get choked.

## 3. Polyphonic Aftertouch

* Playing with _edrums_, the cymbal choke is done by grabbing the cymbal pad, which the _edrums_ brain send the Polyphonic Aftertouch message to host. In this example, the Polyphonic Aftertouch is CC130 (the extended CC in sfz format). This example function the same as test No.2 and has been successfully tested with an _edrums_ kit by @redtide. So, this example fits perfectly for _edrums_ mapping mode.

## 4. Triggered by other key

* By combining test No.1 and No.2, to have the choke at other key (54) but not sounded if pressed and to have the advantage of `rt_decay`, this example is much suitable for _keyboard_ mapping mode (to play with keyboard controller or input note via host's piano roll). Though, this is a workaround for what should be works properly (in the 5th example). The issue is that, the crash key has to be keep pressed, then press choke key, then release the crash key to get it choked. In the example MIDI file, the crash note is longer, overlap a bit to the choke note. But it is works better rather than example No.1.

## 5. By other key CC133

* This example is not working yet, the crash won't get choked. It's for pursuing the `on_lo/hicc133` or `on_key` opcode implementation, so like example no.4 but without the issue mentioned above. The `on_lo/hicc133` value is written as 54, which assumed as key-number 54 (the choke trigger key number). Otherwise, using a new opcode `on_key`=54 could probably simplifying this kind of triggering.

* An additional workaround for example no.2 and no.3 can be made to solve the "no choke at velocity 127" issue by adding a *silence region to that vel-range with `locc131=127` `hicc131=127`
