# preamp-SiPM connection
* preamp end: strip back the shield, solder + and - wires to SiPM pads and drain wire to one of the SMA grounds

* SiPM end: strip back the shield and drain wire, solder + and - wires to header pins, 
use heatshrink to isolate the shield from the header pins

# damn inductors
* what's the hysteresis curve look like for our ferrite? how much does the inductance get derated at 0.25 T?
* how much inductance do we actually need?
* if we go non-magnetic: how much does that hurt crosstalk?

# power supply
* are we going to get the Fermilab HV boards?
* do we need to worry about making LV and HV distribution boards?
* where can the power supplies go?

# bias trimming
* how closely do the gains need to match?
* what's the spread in breakdown voltage/gain?
* is temperature dependence enough to matter?

# signal cabling
* what kind of cable inside the frame? thickness vs. attenuation; shielding (probably require double-shielded)
* how do we get the signals out from the frame? bulkhead connectors?
* where do the discriminators go?

# tests
* crosstalk: hook up and power two preamps and SiPMs (with realistic layout for the preamps and power cables), 
pulse one SiPM, watch the other preamp output
* cable length effects: record output pulse with short SMA cable and with a long cable (chain some LEMO cables?)

# preamp QA

for repeatability, use a single SiPM for all tests and the pulser at a fixed amplitude
check:

* bias connection works
* LV current consumption is normal
* SiPM response: measure amplitude and rise time
