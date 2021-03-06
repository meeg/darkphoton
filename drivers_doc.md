# called directly in ROC25.crl.RFCLK

## v1495Init
```
/*******************************************************************************
* v1495Init - Initialize v1495 Library. 
*
* RETURNS: OK, or ERROR if the address is invalid or board is not present.
********************************************************************************/
STATUS v1495Init (UINT32 addr, UINT32 addr_inc, int nmod)
```

ROC25.crl.RFCLK: download

initializes v1495 structs and tests VME reads

## v1495TimewindowSet
```
/*******************************************************************************
* v1495TimewindowSet - Set the internal TDC readout time window of v1495
*
* RETURNS: error if the input setting is not correct
********************************************************************************/
void v1495TimewindowSet (int id, int val)
```

ROC25.crl.RFCLK: prestart

scratch (0x1020) is set to val 

val is enforced to only have the bits 0xf33f: delay=[0..7], time window=[8..11], don't care [12..15]

## v1495Timeset
```
/*******************************************************************************
* v1495Timeset - Set the v1495 internal delay and jittering acceptable range
*
* RETURNS: setting value or error if the board is not initialized
********************************************************************************/
void v1495Timeset (int n, int id, int sleep)
```

ROC25.crl.RFCLK: prestart

read val from time_{id}.txt files (format is channel:val, but ``channel`` is ignored and assumed to be sequential; default files have val=exx)

set stuff for channels 0..n-1: c_ctrl_l (0x101A) = 0x2000+i, then c_ctrl_h (0x101C) = value

``sleep`` is not used

## v1495ActivatePulser
```
//Activate Pulser (This disables 'data-mode' pass-through of input channels)
void v1495ActivatePulser(int id0)
```

ROC25.crl.RFCLK: prestart

write 0x0001 to f_data_l (0x1038), read it back

## v1495PatternSet
```
/*******************************************************************************
* v1495PatternSet - Set the data for Level0 pulser (and Level2 pulser control)
*
* RETURNS: setting value or error if the board is not initialized
********************************************************************************/
void v1495PatternSet (int level, int id, char charge, char TorB, int patnum)
```

ROC25.crl.RFCLK: prestart

for level 0 or level 1, open pattern_{charge}{TorB}_{patnum}.txt
for i=0..255, load values from file to pulse1..6[i] (0x1400+i, 0x1600+i, etc.) and readback


for level 2, open pattern_2.txt
for i=0..127, load value from file to pulse1[i] (0x1400+i) and readback

## v1495Run
```
void v1495Run (int id)
```

ROC25.crl.RFCLK: go, done usrtrig

calls v1495ciptag(id): write 0x7fff to the last TDC address (0x12ff)

## v1495Reload
```
void v1495Reload (int id)
```

ROC25.crl.RFCLK: download

reloads the FPGA (write 0x0001 to 0x8016)

## v1495TDCReadout
```
int v1495TDCReadout (int id, int ii)
```

ROC25.crl.RFCLK: trigger usrtrig

read and return 0x1200+ii

## v1495TDCcount
```
int v1495TDCcount (int id)
```

ROC25.crl.RFCLK: trigger usrtrig

read last TDC address (0x12ff): if it matches 0x81xx, return [0..7]; else return 0xd1ad

## v1495RevisionRead
```
int v1495RevisionRead (int id)
```

ROC25.crl.RFCLK: trigger usrtrig

read and return revision (0x103C)

## v1495CommonstopRead
```
int v1495CommonstopRead (int id)
```

ROC25.crl.RFCLK: trigger usrtrig

calls v1495TimewindowRead(id) and g1(id,j)

get delay (low 8 bits of scratch)

use g1() to loop through time slices, looking for the RF pulse

# helper functions for the above

## v1495TimewindowRead
```
/*******************************************************************************
* v1495TimewindowRead - readout the time window setting value
** RETURNS: setting value or error if the board is not initialized
********************************************************************************/
int v1495TimewindowRead (int id)
```

called by v1495CommonstopRead

read and return scratch (0x1020)

## v1495ciptag
```
void v1495ciptag (int id)
```

called by v1495Run

write 0x7fff to the last TDC address (0x12ff)

## g1
```
int g1 (int id, int row)
```

called by v1495CommonstopRead

write 0x6001 + row*4 to c_ctrl_l (0x101A), read and return a_sta_l (0x1000)

this returns the pipeline contents for channel 129 (GIN[1], the external trigger input) at the time offset specified by `row`

# tests (are they used?)

# unknown

## v1495Status
```
/*******************************************************************************
* v1495Status - The status of v1495
*
* RETURNS: prints some status of v1495 on screen
********************************************************************************/
void v1495Status (int id)
```

## v1495Reset
```
/*v1495 reset now can't work*/
void v1495Reset (int id)
```

## delay
```
void delay (int ms);
```

## v1495sprintf
```
//This function simply calls sprintf to fill a character string
//It was moved to its own function to prevent interference with nanosleep() -- REM -- 2013-09-13
void v1495sprintf(char* aString, int size, char aCharge, int fileNum, char fileType)
```

## v1495DisactivatePulser
```
//Disactivate Pulser (This enables 'data-mode' pass-through of input channels)
void v1495DisactivatePulser(int id0)
```

## v1495DelayTest
```
void v1495DelayTest(int sec, int nsec)
```

## v1495PulserTest
```
void v1495PulserTest(int id)
```

## v1495PulserGo
```
void v1495PulserGo(int id)
```

## v1495PulserCompare
```
/*Reads TDC and pattern file, calculates expected and measured TDCtimes, compares, outputs result */
/*INCOMPLETE, MADE OBSOLETE BY v1495SimplePulser() and v1495RoadPulser() */
void v1495PulserCompare(int id)
```

## v1495RoadPulser
```
//This function measures the 'Level 2' TDC hits in a 3-board pulser test configuration 
//Currently it only uses the first 10 roads (first ten lines) 
//THIS FUNCTION CONTAINS HARD CODED LEVEL 2 MAPPING
void v1495RoadPulser(int id0, int id1, int id2, char charge, const int numFiles, const int iter, int win0, int win1, int win2 )
```

## v1495SimplePulser
```
/*This function measures the 'Level 1' TDC hits in a 3-board pulser test configuration */
/*It calculates the mean, min, and max of TDCtimes for each channel individually */
/*It should be used with a pattern file that has ffff,ffff,ffff,... in some rows, and all zeros in all other rows. */
void v1495SimplePulser(int id0, int id1, int id2, int win0, int win1, int win2, const int iter)
```

## v1495PulserOnSignal
```
/* OUTDATED */
/* Activates the signal sent out from Level 2, to Level 0, in order to start the pulsers -- REM -- 2013-07-09 */
void v1495PulserOnSignal(int id)
```

## v1495tdcStopSignal
```
/* Activates the signal sent out from Level 2, to G1 on Levels 1 and 2, to stop the TDCs -- REM -- 2013-07-18 */
void v1495tdcStopSignal(int id)
```

## v1495TimesetTest
```
void v1495TimesetTest (int id, int sleep)
```

## v1495Chread
```
void v1495Chread (int id, int sleep)
```

## v1495Chout
```
void v1495Chout (int id)
```

## dummydelay
```
void dummydelay (int delay)
```

## v1495AutoTimeset
```
void v1495AutoTimeset (int id, int sleep, short seedch, int startp)
```

## v1495TDCRead
```
/*******************************************************************************
* v1495TDCRead - This function is to readout the data for debug !DON'T PUT IN .CRL!
* MAY CRASH ROC!!!
* RETURNS: TDC data
********************************************************************************/
void v1495TDCRead (int id)
```

## v1495Writetest
```
void v1495Writetest (int id, int round, int loopn)
```

## v1495Readtest
```
void v1495Readtest (int id, int round, int loopn)
```

## v1495Readtest1
```
void v1495Readtest1 (int id, int round, int loopn)
```

## v1495Readtest2
```
void v1495Readtest2 (int id, int round, int loopn)
```

## v1495Readtest3
```
void v1495Readtest3 (int id, int round, int loopn)
```

## v1495ReadWritetest
```
void v1495ReadWritetest (int id, int round, int loopn)
```

## v1495Memwritese
```
//MAY NOT WORK AFTER TODAY - REM - 2013-03-14
void v1495Memwritese (int id, int mode, int loop, int base)
```

## v1495Memwriterd
```
void v1495Memwriterd (int id, int mode, int loop, int loopn)
```

## v1495Memread
```
void v1495Memread (int id, int round, int loopn)
```

## v1495Memreadcon
```
void v1495Memreadcon (int id, int round, int loopn)
```

## v1495Testread
```
void v1495Testread (int id)
```
