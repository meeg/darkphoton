# top-level signals

## fast I/O

GIN[0]: RF clock, see below

GIN[1]: trigger input

## clocks

GIN[0]: RF clock (18.8 ns period, 53.1 MHz)

LCLK: V1495 bus clock (25 ns period, 40 MHz)

CLK0p, CLK90p: fast clock - for RF-clocked firmware, x4 multiplication of RF clock (4.7 ns period, 212 MHz)

CK62: slow clock (1/4 division of fast clock) - for RF-clocked firmware, same period as RF clock

## registers

REG_RDEN

REG_WREN

REG_ADDR

REG_DIN[15..0]

REG_DOUT[15..0]: output of mux

USR_ACCESS

SCRATCH_OUT[15..0]: written through SCRATCH register, input to TrigSeq3 (bit 12), ZeroSuppr3 (bits 9..8), PipeTime3 (bits 6..0)

C_DOUT[31..0]: written through C_CONTROL_L and C_CONTROL_H registers, input to DLYPW_IO3

A_DIN[31..0]: output of DLYPW_IO3, read out through A_STATUS_L and A_STATUS_H registers

## other important stuff

USR_A: output of DLYPW_IO3

RA: output of PipeTime3

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

### ChTSCount3

in PipeTime3, ZeroSuppr3
