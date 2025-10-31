# Assembly

## Assembly Instructions

1.  Place and solder U2 (LM13700 SMT package)
2.  D1 and D2 (1N4148)
3.  Horizontal resistors R3, R6, R9, R12, R15, R18
4.  Socket for U1 (DIP-14)
5.  Disc capacitors C1, C2, C3-C5, and C7-C9
6.  Transistors Q1 and Q2 (BC557)
7.  Vertical resistors R1, R2, R4, R5, R7, R8, R10, R11, R13, R14, R16, R17, R19, R20
8.  D3 and D4 (1N7007)
9.  Electrolytic capacitors C6 and C10
10. Power connector J7
11. Trim pots RV1 and RV2 (opposing orientations for equivalent trim direction)
12. Audio jacks J1-J6
13. Control pots RV3 and RV4

## Measurement and Calibration

Calibration: apply a test signal (440Hz sine, 8Vpp) to the audio input
and a DC 1V level to the CV input. Adjust the trim pot until the output
is balanced.

Measurement: apply a test signal (440Hz sine, 8Vpp) to the audio input
and vary the DC level to the CV input.

  CV (V)  |Output Amplitude (Vpp)  |Gain (V/V)
  --------|------------------------|------------
  1.05    |1.62                    |0.202
  2.02    |3.02                    |0.378
  3.09    |4.6                     |0.575
  4.08    |6.00                    |0.750
  4.51    |6.60                    |0.825

The linear regression of the gain response is

$$A = 0.014 + 0.18V$$

where $A$ is the gain in V/V and $V$ is the CV input in Volts. With
$R = 47k\Omega$ in the output stage, the gain at 5V CV input is expected
to be 0.927 V/V. The measured gain at 5V CV input is 0.914 (within 2% of
expected). Using a 49.9k resistor for R would get a bit closer to the
design target.

![image](./assets/images/gain_vs_CV_2025-06-19.png){width="480"}

## BOM

[Download (.csv)](assets/bom.csv)

{%include-markdown "assets/bom.md"%}