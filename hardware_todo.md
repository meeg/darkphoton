# preamp-SiPM connection
* preamp end: strip back the shield, solder + and - wires to SiPM pads and drain wire to one of the SMA grounds

* SiPM end: strip back the shield and drain wire, solder + and - wires to header pins, 
use heatshrink to isolate the shield from the header pins

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
