# Using an Android device as a drumpad in Sonic Pi

With Sonic Pi 3.0's MIDI control features, it became possible to use
instruments to control what your code does. I don't have a MIDI
controller at home, but I have an Android smartphone, so I decided to
use Android's MIDI interface to help me.

Fortunately, there are excellent MIDI controller apps out there. I
used [TouchDAW](http://www.humatic.de/htools/touchdaw/) which has a
free version.

## TouchDAW setup

TouchDAW can use MIDI over Wifi, but I simply went for the
MIDI-over-USB approach that's standard in Android. There's a small
catch: TouchDAW uses two different MIDI channels, but Android's API
only lets you use one over USB.

To use this, plug in your Android device (it has to be of version 6.0
or greater) into your computer. Open the drawer menu (where you see
active apps) and tap the USB connection app. Select "MIDI".

Since I want to use the drumpad and keyboard controllers, and those
are on TouchDAW's channel 2, I configured that one to use USB. In
TouchDAW open the three-dots menu > `Settings` > `MIDI Ports` > `Port
2` > `USB`. Don't touch `Port 1`, as if you map this one to USB it'll
take the only available spot.

You should see MIDI cues in Sonic Pi when you use TouchDAW's keyboard
or drumpad.

## Using the drumpad in Sonic Pi

Here's a basic drumpad code I used to map the default drum samples to
TouchDAW's launchpad:

```
live_loop :drumpad do
  use_real_time
  c, v = sync "/midi/pixel_xl_midi_1/1/1/note_on"
  sample case c
  when 36; :drum_heavy_kick
  when 37; :drum_tom_mid_soft
  when 38; :drum_tom_mid_hard
  when 39; :drum_tom_lo_soft
  when 40; :drum_tom_lo_hard
  when 41; :drum_tom_hi_soft
  when 42; :drum_tom_hi_hard
  when 43; :drum_splash_soft
  when 44; :drum_splash_hard
  when 45; :drum_snare_soft
  when 46; :drum_snare_hard
  when 47; :drum_cymbal_soft
  when 48; :drum_cymbal_hard
  when 49; :drum_cymbal_open
  when 50; :drum_cymbal_closed
  when 51; :drum_cymbal_pedal
  when 52; :drum_bass_soft
  when 53; :drum_bass_hard
  when 54; :drum_cowbell
  when 55; :drum_roll
  end
end
```

You'll need to change the note_on MIDI path to match the one your
device generates. You can find it by looking at Sonic Pi's cues log
when you press buttons.

Note that in the default 4x4 configuration, the launchpad won't have
enough keys for sounds above 51. You can configure the drumpad to use
a 6x6 or 8x8 configuration if your device has a large screen.
