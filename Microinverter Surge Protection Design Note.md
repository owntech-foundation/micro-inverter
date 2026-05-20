Surge Protection for the OwnTech microinverter

Author: Stevie Wray – Fairflow Energy Ltd.

Date: 14/05/2026

Revision: V1.0

# Rationale – Why should there be surge protection?

The microinverter is intended to be connected directly to an AC grid.
This grid could have transient over voltages from lightning, grid
switching and inductive faults. These could exceed insulation limits and
cause immediate or long term damage to sensitive components. By
including surge protection, the more sensitive components can continue
to work in a wider range of circumstances.

There is also a regulatory element where withstanding surges are a
requirements for passing EMC tests.

The relevant standards for this are:

EN 62109-1: Safety of power converters for use in photovoltaic power
systems - Part 1: General requirements

EN 62109-2: Safety of power converters for use in photovoltaic power
systems - Part 2: Particular requirements for inverters

EN 61000-4-5: Electromagnetic compatibility (EMC) – Part 4-5: Testing
and measurement techniques – Surge immunity test

# What have we done?

1\. Included an AC side Metal Oxide Varistor (MOV) and Gas Discharge
Tube (GDT) combination to protect against line to earth surges. (Live or
Neutral counting as a line) – RV1, RV3, GD1

2\. Included an AC side MOV to protect against Live to Neutral surges. -
RV2

3\. Included a further set of line to earth and live to neutral MOVs on
the converter side of the K1 grid connection relay. This set of MOVs are
paired with fuses in case of protective device failure. – RV101, RV102,
RV103, F1, F101.

4\. Considered insulation co-ordination to ensure that insulation is
sufficient in each part of the inverter.

NOTE: If a protective device fails, the microinverter will need
maintenance to replace those components.

An excerpt of the microinverter schematic is included below showing
these MOVs.

<img src="Images/Pictures/1000000100000565000001D41D5325A4.png"
style="width:17cm;height:5.761cm" />

## 

# Why have we done it that way?

On the specific device configuration, a major consideration is the
failure mode of the MOVs and the GDTs themselves. If the fault through a
GDT does not clear (reduce current to zero) then the GDT will eventually
catch fire. This is undesirable! As such, the arrangement for line to
earth fault with a MOV in series with a GDT overcomes this. Especially
if the MOV has an internal thermal fuse. The MOVs selected do have an
internal fuse.

There is also a cascade of MOV values starting with the higher values
near the AC connection to the grid, and reducing closer to the internals
of the inverter. The intent here is to ensure that sensitive components,
such as the GaN switches are protected to voltages that they can
withstand.

There is a known limitation with this current design, in that the rating
of the MOVs is still insufficient to reduce an arbitrary pulse to below
the voltage rating of the GaN switches. This is an unlikely event, as
the standard pulse simulated in this document are already a harsh
condition.

With this in mind, the intent is to meet EN61000-4-5 Surge Test Levels
as below, assuming that the AC power port is a class 4, but then
elements within the microinverter are then reduced in class.

This assumes that the standard tests are sufficient for a robust
product. From assessing them, they do seem good enough for the
microinverter’s purposes.

Essentially, the requirements we are aiming to meet are that after the
surge test (4 kV, 2 Ohm source impedance 8/20 µs pulse), the inverter
must:

- Continue to operate as intended (no degradation in performance).
- Not become unsafe (no fire, electric shock, or mechanical hazards).
- Not trip protective devices (e.g., circuit breakers) unless it is part
  of the intended safety function.

The defined voltage levels are found in this table here:

<table>
<tbody>
<tr>
<td colspan="4">Electrical Surge Test Levels (IEC/EN 61000-4-5)</td>
</tr>
<tr>
<td>Class</td>
<td>Test Level<br />
(V)</td>
<td>Max Peak Current @ 2 Ω<br />
(A)</td>
<td></td>
</tr>
<tr>
<td>1</td>
<td>500</td>
<td>250</td>
<td></td>
</tr>
<tr>
<td>2</td>
<td>1000</td>
<td>500</td>
<td></td>
</tr>
<tr>
<td>3</td>
<td>2000</td>
<td>1000</td>
<td></td>
</tr>
<tr>
<td>4</td>
<td>4000</td>
<td>2000</td>
<td></td>
</tr>
<tr>
<td>X</td>
<td>Special</td>
<td>Special</td>
<td></td>
</tr>
<tr>
<td colspan="3">X can be any level specified in product specific
standards.<br />
It can be above, below or between the others.</td>
<td></td>
</tr>
</tbody>
</table>

Wikipedia (<https://en.wikipedia.org/wiki/IEC_61000-4-5>)

Practically, the surge immunity testing is done with a combination
waveform generator, aiming to generate a 8/20 µs current pulse. Note the
waveform times refer to rise time and impulse width. Shown nicely in
this excerpt from the Brightking 621KN14 datasheet.

<img src="Images/Pictures/100000010000023E0000011FD1658116.png"
style="width:15.189cm;height:7.594cm" />

A combination waveform generator can be represented as a set of simple
circuit components, especially for simulation. The following is from a
guide found here:
<https://youspice.com/spice-simulation-of-a-combo-wave-generator/>

[](https://youspice.com/spice-simulation-of-a-combo-wave-generator/)

Cc=7.76μF, Rs1=14.8 Ohm, Rm=1.05 Ohm, Lr=9.74μH, Rs2=23.3 Ohm. The peak
voltage on Rs2 can be 1KV, 2KV,..6KV.

A SIMBA model has been produced with these parameters, pre-charging the
capacitor Cc to the desired voltage rating for the test. In this case it
is generally 4 kV.

# Simulations for verification

## RV1, RV2, RV3: Brightking 621KN14

The device selected for RV1, RV2 and RV3 is the Brightking 621KN14. The
section below shows the expected characteristics of this device, and
shows an example of it under a 4 kV, 2 Ohm input impedance surge
immunity test using a simulated combination wave generator.

<img src="Images/Pictures/10000001000002C60000008175C35CF9.png"
style="width:17cm;height:3.087cm" /><img src="Images/Pictures/10000001000002BB000000236FB3C046.png"
style="width:16.836cm;height:0.85cm" />Varistor voltage is the voltage
between two terminals with specified measuring current of 1mA DC.

<img src="Images/Pictures/100000010000023E0000011FD1658116.png"
style="width:15.189cm;height:7.594cm" />

~0 Amps at less than 620 V

~ 1 mA at 620 V

~ 50 A at 1025 V

~ 2 kA at 1800 V

R = dV / dI = (1800-620) / (2000 – 0.001) = 0.59 Ohms

620 V breakdown voltage, 0.59 Ohm breakdown resistance, 0 Ohm on
resistance. Back to back configuration.

Validated in simulation as a “good enough” model with the combination
wave generator.

<img src="Images/Pictures/10000001000003DA000002A813CF2660.png"
style="width:12.739cm;height:8.784cm" /><img src="Images/Pictures/100000010000027E00000166997030A5.png"
style="width:13.28cm;height:7.452cm" />

## RV101, RV102, RV103: Littelfuse TMOV14RP275E

<img src="Images/Pictures/1000000100000590000001173DCCDC4F.png"
style="width:17cm;height:3.33cm" /><img src="Images/Pictures/100000010000059C00000024A7D3811F.png"
style="width:17cm;height:0.425cm" />

~0 Amps at less than 473 V

~ 1 mA at 473 V

~ 50 A at 710 V

~ 2 kA at 1000 V

R = dV / dI = (1000-473) / (2000 – 0.001) = 0.26 Ohms

473 V breakdown voltage, 0.26 Ohm breakdown resistance, 0 Ohm on
resistance. Back to back configuration.

Validated in simulation as a “good enough” model with the combination
wave generator.

<img src="Images/Pictures/10000001000003DC000002A777A7AAE1.png"
style="width:12.3cm;height:8.453cm" /><img src="Images/Pictures/100000010000027E00000166997030A5.png"
style="width:13.28cm;height:7.452cm" />

## GD1: RUILON 2R600B-8S

The device selected for GD1 is the RUILON 2R600B-8S. The section below
shows the expected characteristics of this device, and shows an example
of it under a 4 kV, 2 Ohm input impedance surge immunity test using a
simulated combination wave generator.

<img src="Images/Pictures/10000001000004820000010D282B58D1.png"
style="width:17cm;height:3.962cm" />

<img src="Images/Pictures/100000010000048B0000003462AA6C6D.png"
style="width:17cm;height:0.758cm" />R = V / I = 15 / 1 = 15 Ohm at “Arc”

<img src="Images/Pictures/10000001000003D4000002A3B6838C09.png"
style="width:12.868cm;height:8.862cm" />The simulation top level (left)
and the internals of the Gas Discharge sub circuit (right)

<img src="Images/Pictures/10000001000002DA0000014D041EE3CB.png"
style="width:9.4cm;height:4.286cm" /><img src="Images/Pictures/10000001000001EA000001E090F0F593.png"
style="width:5.856cm;height:5.738cm" />

Vgdt = 600 V, Vpeak = 4000 V

The GDT model is a thyristor with simple threshold control. There are
two thyristors in anti-parallel for bidirectional operation. Each
thyristor has a diode with a voltage drop of 15 V in series to represent
the plasma voltage of the GDT. There is also a low resistance element
(0.001 Ohm) to consider parasitics at the simplest level.

## Whole AC surge protection model

Whole AC end simulation with a 4 kV, 2 Ohm source impulse test from the
combination wave generator.

This is a line to earth fault scenario with surge voltage and current on
the top plot and capacitor voltages in distance-from-grid order on the
bottom plot.

It can be seen that the ~1350 V seen at the AC connections is reduced to
~650 V peak at C122, and reduced further through C113, C120 and the DC
link capacitor C6 sees virtually no change. There is significant ringing
due to the capacitive and inductive components interacting with each
other, but it is damped to zero within 50 ms.

<img src="Images/Pictures/1000000100000723000001E744B1C2EA.png"
style="width:17cm;height:4.531cm" /><img src="Images/Pictures/10000001000003E2000002C814545DEB.png"
style="width:17cm;height:11.869cm" />

This is a live to neutral surge test with the same plots as above.

It can be seen that the ~1350 V seen at the AC connections is seen in
its entirety by C122, but then subsequent capacitors C113 and C120 never
see a voltage above ~500 V. There is some ringing, but less in this
scenario and it is damped by approximately 20 ms. C6 is largely
unaffected by the surge.

<img src="Images/Pictures/10000001000006D8000001AC74988A9E.png"
style="width:17cm;height:4.152cm" />

<img src="Images/Pictures/10000001000003D7000002A9F3E66538.png"
style="width:17cm;height:11.776cm" />

# 

# 

# Appendix: EN/IEC 61000-4-5 Excerpt

7.5.1 – Impulse voltage test (type test) The impulse voltage test is
performed with a voltage having a 1.2/50 µs waveform; see Figure 6 of
IEC 60060-1. It is intended to simulate overvoltages caused by lightning
or by the switching of equipment. See Table 15 for further details on
the impulse voltage test conditions. Tests on clearances smaller than
those required by Table 13, as permitted by 7.3.7.4.2, and tests on
solid insulation are performed as type tests using the appropriate
voltages from Table 16. Tests relating to the protective separation of
components and devices are performed as a type test before their
assembly into the PCE, unless the test can be performed on the complete
PCE without reducing the stress applied to the protective separation.
The test is performed using the impulse withstand voltages listed in
column 3 or 5 of Table 16.

Where transient limiting devices are used to reduce the impulse voltage
levels, see 7.3.7.1.2, item g), the reduction is verified by a type
test. The values from column 2 or 4 of Table 16 are applied to the PCE,
and measurements are taken in the circuit after the transient limiting
devices, in order to determine the reduced transient level. If it is
necessary to test a clearance designed for altitudes between 2 000 m and
20 000 m, using Table A.2 of IEC 60664-1, the appropriate test voltage
may be determined from the clearance by using Table 13 in reverse.

Table 15 — Impulse voltage test

|  |  |
|----|----|
| Subject | Test conditions |
| Test reference | Clause 19, paragraph 20.1.1 and Figure 6 of IEC 60060-1; 6.1.2.2.1 of IEC 60664-1 |
| Requirement reference | In accordance with 7.3.4.3, 7.3.5.3 and 7.3.7 |
| Pre-conditioning | Live parts belonging to the same circuit shall be connected together. Protective impedances shall be disconnected unless they need to be tested. The impulse voltage shall be applied: 1) between the circuit under test and its environment, and 2) between the circuits to be tested. Power is not applied to the circuits under test. |
| Initial measurement | In accordance with the specification of the PCE, component or device. |
| Test equipment | 1.2/50 µs impulse generator with an effective internal impedance of less than 2 Ω for clearance tests and 500 Ω for component and solid insulation tests. |

|  |  |  |
|----|----|----|
| Subject | a\) | b\) |
| Measurement and verification | Clearances smaller than those required by Table 7. Clearances reduced by transient limiting devices or by circuit characteristics. Solid basic or supplementary insulation. | Solid reinforced insulation. Clearances, components and protective separation devices. Three 1.2/50 µs impulses of each polarity at intervals ≥ 1 s, with peak voltage ±5% in accordance with: |
| Test voltage | Column 2 or 4 of Table 16. | Column 3 or 5 of Table 16. |
| Test voltage note | When the test is performed on a clearance at an altitude below 2 000 m, the test voltage shall be increased in accordance with Table F.5 of IEC 60664-1, reproduced as Table F.2 in this International Standard. | Same condition applies. |

Table 16 — Impulse withstand test voltage

|  |  |  |  |  |
|----|----|----|----|----|
| System voltage (see 7.3.7.2.1) | Insulation between circuits **not directly connected to the mains** and their environment, according to **overvoltage category II** — Basic or supplementary | Insulation between circuits **not directly connected to the mains** and their environment, according to **overvoltage category II** — Reinforced | Insulation between circuits **directly connected to the mains** and their environment, according to **overvoltage category III** — Basic or supplementary | Insulation between circuits **directly connected to the mains** and their environment, according to **overvoltage category III** — Reinforced |
| V | V | V | V | V |
| ≤ 50 | 500 | 800 | 800 | 1 500 |
| 100 | 800 | 1 500 | 1 500 | 2 500 |
| 150 | 1 500 | 2 500 | 2 500 | 4 000 |
| 300 | 2 500 | 4 000 | 4 000 | 6 000 |
| 600 | 4 000 | 6 000 | 6 000 | 8 000 |
| 1 000 | 6 000 | 8 000 | 8 000 | 12 000 |
| — | Interpolation permitted | Interpolation permitted | Interpolation prohibited | Interpolation prohibited |

**Note for columns 2 and 3:** Test voltages for overvoltage categories I
and III may be derived in a similar way from Table 12.

**Note for columns 4 and 5:** Test voltages for overvoltage categories
II and IV may be derived in a similar way from Table 12.
