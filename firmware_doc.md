Firmware IDs:

400-430: L0

440-470: L1

480: L2

This doc is based on RF-clocked 460.

# top-level signals

## fast I/O

GIN[0]: RF clock, see below

GIN[1]: trigger input

## clocks

GIN[0]: RF clock (18.8 ns period, 53.1 MHz)

LCLK: V1495 bus clock (25 ns period, 40 MHz)

CLK0p, CLK90p: fast clock - for RF-clocked firmware, x4 multiplication of RF clock (4.7 ns period, 212 MHz); for self-clocked, x25/4 multiplication of bus clock (4 ns period, 250 MHz)

CK62: slow clock (1/4 division of fast clock) - for RF-clocked firmware, same period as RF clock

## register-related

REG_RDEN

REG_WREN

USR_ACCESS

REG_ADDR[15..0]: written on any VME read or write, input to ZeroSuppr3 and DLYPW_IO3

REG_DIN[15..0]: written on any VME write, input to ZeroSuppr3

REG_DOUT[15..0]: output of mux; from coin_reference if !REG_ADDR[9], from ZeroSuppr3 otherwise

SCRATCH_OUT[15..0]: written through SCRATCH register, input to TrigSeq3 (bit 12), ZeroSuppr3 (bits 9..8), PipeTime3 (bits 6..0)

C_DOUT[31..0]: written through C_CONTROL_L and C_CONTROL_H registers, input to DLYPW_IO3;  C_DOUT[31..16], or register C_CONTROL_H, is only used inside DLYPW_IO3

A_DIN[31..0]: output of DLYPW_IO3, read out through A_STATUS_L and A_STATUS_H registers

F_DOUT (or F_DOUTreg)[31..0]: written through F_DATA_L and F_DATA_H; only used in L0, as input to pulse generator and muxes

WR_CL: output of DLYPW_IO3, pulses on a write to C_CONTROL_L

RD_AL: output of DLYPW_IO3, pulses on a read from A_STATUS_L

ReRun: !C_CONTROL_L[15]

## VME address space
names are from driver structs

### data
0x1000-0x1046 mapped in struct


#### SCRATCH (0x1020)
SCRATCH[9..8]: TDC time window (used in ZeroSuppr3)

SCRATCH[6..0]: TDC delay (used in PipeTime3), units of slow clock, typical values 1f-2e

SCRATCH[12]: enable GIN[1] trigger (used in TrigSeq3)

SCRATCH[13]: something to do with splat blocks for L1, according to the CRL? doesn't seem to be used in the firmware

#### C_CONTROL_L (0x101a)

also known as USR_A

C_CONTROL_L[15]: !ReRun

C_CONTROL_L[14]: select which counter to use in PipeTime3

##### high

C_CONTROL_L[8..2]: input to ASUM in PipeTime3: added to delay

C_CONTROL_L[13..9,1..0]: DCH input to the top counter in PipeTime3

##### low 

#### C_CONTROL_H (0x101c)

channel delay settings: 

### VME
0x8000-0x8021 mapped in struct

### mem
0x1100-0x11ff mapped in struct

### TDC
0x1200-0x13ff mapped in struct

### pulse
0x1400-0x1Fff in 6 blocks: 0x1400-0x15ff, 0x1600-0x17ff, 0x1800-0x19ff, 0x1a00-0x1bff, 0x1c00-0x1dff, 0x1e00-0x1fff

## user address space

### delays
0x2000-0x3fff "assigned" in firmware (DLYPW_IO3.WRDLYPW mask) but only 0x2000-0x20ff wired to RAM, only 0x2000-0x207f read from RAM

0x2000-0x2000+nch used in driver

## other important stuff

USR_A[15..0]: output of DLYPW_IO3 - this is C_DOUT[15..0], or register C_CONTROL_L; input to DLYPW_IO3, PipeTime3, TrigSeq3 (bit 14)

### RA[13..0]

output of PipeTime3

RA[13..12]: used by ZeroSupp3, selects a 32-group

RA[11..9]: used by GRP32ch, selects a 4-group

RA[8..2]: used by GRP4ch, RAM address

RA[1..0]: used by GRP4Ch, selects a channel

# important registers

# blocks

## v1495usr_hal

## tristate_if

## coin_reference

## ZeroSuppr3
uses ChTSCount3

## spare_if

## Out32ch

## coin_logic_test

## PipeTime3
uses Pulser1, ChTSCount3

## DLYPW_IO3
uses Pulser1

## TrigSeq3
uses Pulser1

## code_460

## GRP32ch

### GRP4ch

#### OutPipe1a

#### ChTK16nsB

#### Smaple4sh16a

## utility blocks

### Pulser1

in PipeTime3, DELAYPW_IO3, TrigSeq3

on input rising edge, outputs a positive pulse

### ChTSCount3

in PipeTime3, ZeroSuppr3

