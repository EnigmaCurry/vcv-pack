EnigmaCurry's VCV pack
==============================

This is my collection of modules for [VCV Rack](https://vcvrack.com/).


## Transport

Transport is a DAW-style play/stop/record control, with clocked
punch-in/punch-out (`Quantize Arming`), allowing to play and record anything for
a specific number of clock cycles (bars).

![Transport](screenshots/Transport.png)

 * Set `LENGTH` to the number of clock cycles (eg. beats, bars) that you wish to
   record. (Or click the button to disable it, and to record unlimited length.)
 * Receive a clock signal (`CLK`) to count the cycles or bars (If you want to
   count bars, usually first divide the incoming CLK by four).
 * Send a reset signal (`RST`) from Transport back to your clock generator. This
   will reset when playing finishes.
 * Tap the `ARM` button, or send a trigger from another source (eg. STROKE), to
   arm recording on next play (or to start recording when already playing.)
 * Tap the `PLAY` button, or send a trigger from another source (eg. STROKE), to
   start playing. If recording is armed, recording will start too.
 * `PGAT` is output high when playing. `PTRG` triggers a pulse on start and
   stop.
 * `RGAT` is output high when recording. `RTRG` triggers a pulse on start and stop.
 * Connect `PTRG` to your Clock Generator `RUN` input.
 * Connect `RGAT` to the recorder GATE input, or `RTRG` to its TRIGGER input.
 * In the right click context menu of Transport, set `Quantize Arming` if you
   want the recording to start exactly on the next beat/bar x1,2,4,8,16 etc. The
   arm button will flash while waiting for the next quantized beat in order to
   start or stop recording.
 * In the right click context menu, set `Stop on record length` if you wish to
   stop playback after the `LENGTH` counter is reached. (Normally `LENGTH` only
   affects the recording time.)

Here is an example patch using these third party modules:
[Kick](https://library.vcvrack.com/Autodafe-DrumKit/DrumsKick),
[STROKE](https://library.vcvrack.com/Stoermelder-P1/Stroke),
[CLKD](https://library.vcvrack.com/ImpromptuModular/Clocked-Clkd), and
[Recorder](https://library.vcvrack.com/VCV-Recorder/Recorder):

![Transport Patch](screenshots/TransportPatch.png)

 * STROKE receives your keyboard shortcuts: `SPACEBAR` and `SHIFT+SPACEBAR`.
   (Right click to teach these keyboard shortcuts to STROKE.)
 * Connect the STROKE `SPACEBAR` output to the `PLAY` input of Transport.
 * Connect the STROKE `SHIFT+SPACEBAR` output to the `ARM` input of Transport.
 * From the right click context menu of CLKD, turn **OFF** `Outputs reset high
   when not running`. This will ensure that the first beat corresponds with the
   first clock cycle.
 * Divide the first clock output of CLKD by 4. (ie. 4 beats per bar). Send this
   signal to the `CLK` input of Transport.
 * Connect the `RST` output from Transport to the `RESET` input of CLKD.
   Transport will send a reset on playback stop.
 * Connect the `PTRG` output from Transport to the `RUN` input of CLKD.
 * Trigger the Kick module (or your own) from the clock, and connect the output
   to the Recorder and Audio-8 inputs.
 * Press `SHIFT+SPACEBAR` to arm the recorder. The light will flash until
   playback starts.
 * Press `SPACEBAR` to play/record.
 * Once the specified length is reached, recording stops.
 * At any time press `SPACEBAR` again to stop playing, or `SHIFT+SPACEBAR` to
   stop recording.
 * If you wish recording to be quantized to the clock, enable the option in the
   right click context menu of Transport: `Quantize Arming`, set this to a
   multiple of the clock (1, 2, 4, 8, 16 etc).
 * If you wish playback to stop after the Recrod length is reached, use the
   context menu option `Stop on record length`.

In this scenario, if you had set CLKD BPM to 120, and the Transport record
`LENGTH` to 8, and you recorded for the entire duration, the recorded .wav file
[should be exactly 16s
long](https://toolstud.io/music/bpm.php?bpm=120&bpm_unit=4%2F4) (8 bars * 4
beats/bar * 500ms = 16s). In reality, it records a .wav file that is 15.99s, so
its not exactly frame accurate, maybe this timing can be improved??
