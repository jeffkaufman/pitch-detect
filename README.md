# Usage

## Build

### Mac

1. Install portaudio:
   ```
    $ wget http://www.portaudio.com/archives/pa_stable_v190600_20161030.tgz
    $ tar -xvzf pa_stable_v190600_20161030.tgz
    $ cd portaudio
    $ ./configure --disable-mac-universal
    $ make
    $ sudo make install
   ```

2. Build it:
   ```
    make zeros-mac
   ```

It will make a virtual MIDI source (`whistle-pitch`), which you can then pipe
into a synthesizer.

There's also a version with its own built-in bass synthesizer, which is a
plug-in version for a DAW (like Reaper):

* source: https://github.com/jeffkaufman/iPlug2

* mac vst: https://www.jefftk.com/BassWhistleVST3-v1.zip


#### To use with OSC

1. Build liblo
   ```
   cd liblo-0.30/
   ./configure
   make
   sudo make install
   ```

2. Run: `make run-osc`

### Linux

1. Install portaudio:
   ```
   sudo apt install portaudio19-dev
   ```

2. Build it:
   ```
    make zeros-mac
   ```

It will detect pitches and generate audio.

To run on boot, `/etc/systemd/system/pitch-detect.service` should have:

```
[Unit]
Description=Pitch Detection and	Synthesis

[Service]
ExecStart=/home/pi/pitch-detect/zeros-linux
Restart=always
KillSignal=SIGQUIT
Type=simple

[Install]
WantedBy=multi-user.target
```

Then `sudo systemctl enable pitch-detect` and `sudo systemctl daemon-reload`.

## Run

1. Put on headphones, use a directional mic, or otherwise avoid letting the
   output of this program mix with the input.

2. Run it and whistle:
   ```
    make run-mac    # or
    make run-linux
   ```

It will print out frequencies.

## Microphone tips:

* Works best with a directional microphone with a windscreen (vocal mics like
  the E835 or SM58 have one built in).

* I use a Sennheiser E835 with an xlr to 3.5mm adapter into a USB
  sound card.  This isn't how the microphone is designed to be used
  (it wants a pre-amp) but it works well enough and it's nice not to
  have another piece of hardware.

* You want to be as close to the microphone as you can bear.

* Depending on your microphone you will probably need to tweak MIN_ENERGY to find
  the right threshold to trigger at.  If it's failing to detect your whistle, set
  it to a lower number.  If it's triggering on noise, set it to a higher one.  If
  there's no zone where it reliably triggers on whistling and reliably doesn't
  trigger on noise then improve your microphone, your microphone technique, or
  your whistling technique.
