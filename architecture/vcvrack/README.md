# faust2vcvrack

The **faust2vcvrack** tool compiles a Faust DSP program in a folder containing the VCV Rack plugin C++ source code and a Makefile to compile it.  

`faust2vcvrack [-midi] [-nvoices <num>] [additional Faust options (-vec -vs 8...)] <file.dsp>`

Here are the available options:

- `-midi to activate MIDI control (TODO)`
- `-nvoices <num> to produce a polyphonic self-contained DSP with <num> voices, ready to be used with MIDI (TODO)`

The generated folder has to be moved in the *Rack SDK* folder and compiled from there. 

- `[switch:N]` (with N from 1 to 2) has to be used in a `button` or `checkbox` item to connect it to the switch number N.
- `[knob:N]` (with N from 1 to 2) has to be used in a `vslider`, `hslider` or `nentry` item to connect it to the knob number N. The knob [0..1] range will be mapped to the slider/nentry [min..max] range.

## DSP examples

Here is a simple example showing how oscillators can be controlled by parameters:

```
import("stdfaust.lib");

// UI controllers connected using metadata
freq = hslider("freq [knob:1]", 200, 50, 5000, 0.01);
gain = hslider("gain [knob:2]", 0.5, 0, 1, 0.01);
gate = button("gate [switch:1]");
check = checkbox("check [switch:2]");

// DSP processor
process = os.osc(freq) * gain * gate, os.sawtooth(freq) * gain * check;
```