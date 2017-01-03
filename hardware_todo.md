# preamp-SiPM connection
* preamp end: strip back the shield, solder + and - wires to SiPM pads and drain wire to one of the SMA grounds

* SiPM end: strip back the shield and drain wire, solder + and - wires to header pins, 
use heatshrink to isolate the shield from the header pins

# inductors
* what's the B-H curve look like for our ferrite? how much does the inductance get derated at 0.25 T? answer: dead around 0.1 T
* how much inductance do we actually need? what is the effect of losing inductance?
* if we go non-magnetic: how much does that hurt crosstalk?

# power supply
* are we going to get the Fermilab HV boards?
* do we need to worry about making LV and HV distribution boards?
* where can the power supplies go?

## bias trimming
* how closely do the gains need to match?
* what's the spread in breakdown voltage/gain? breakdown voltage spec says 53 +/- 5 V
* is temperature dependence enough to matter? tempco of V_op is 54 mV/C

# signal cabling
* what kind of cable inside the frame? thickness vs. attenuation; shielding (probably want double-shielded - 316D?)
* how do we get the signals out from the frame? bulkhead connectors?
* where do the discriminators go?

# understanding things
* readout chain: what discriminators (what thresholds?)?
* how does the LMS work?

# things to think about
* self-triggering cosmics calibration - all we need is a special trigger, right?
* do we want external trigger paddles?

# etc.
* temperature monitoring: what will it be?
* survey? what precision (positioning and measurement) is needed, what fiducials are appropriate?
* slow controls? monitoring?

# tests
* crosstalk: hook up and power two preamps and SiPMs (with realistic layout for the preamps and power cables), 
pulse one SiPM, watch the other preamp output
* B-field: build Neo magnet box, test preamp S/N with B-field on each of 3 axes
* cable length effects: record output pulse with short SMA cable and with a long cable (chain some LEMO cables?)

# preamp QA

for repeatability, use a single SiPM for all tests and the pulser at a fixed amplitude
check:

* bias connection works
* LV current consumption is normal
* SiPM response: measure amplitude and rise time
