# LTspice Current-Fed Half-Bridge Simulation

This directory contains an LTspice simulation for a current-fed half-bridge converter stage. The schematic models the MOSFET bridge drive, input current-feed network, transformer/coupled inductors, rectifier/output stage, and a TI UCC27282 half-bridge gate driver model.

## Files

- `Current_Fed_Half_Bridge.asc` - main LTspice schematic.
- `UCC27282.asy` - custom LTspice symbol for the UCC27282 gate driver.
- `ucc27282.lib` - TI transient SPICE model for the UCC27282.

## Simulation Setup

The schematic includes these simulation directives:

```spice
.param Tsw={1/Fsw} Ton={duty*Tsw} Toff={TSw-Ton-2*dt} dt=100n duty=0.45 Fsw=200k
.tran 0 0.002 0 0.000001
K1 L1 L2 1
```

Nominal operating parameters:

- Switching frequency: `Fsw = 200 kHz`
- Duty cycle: `duty = 0.45`
- Dead time: `dt = 100 ns`
- Transient duration: `2 ms`
- Maximum timestep: `1 us`
- Input source: `Vin = 40 V`
- Gate-drive supply: `VDD = 12 V`

## Main Circuit Elements

- `U1` - UCC27282 high/low-side gate driver.
- `M1`, `M2`, `M3` - `IPP048N12N3` MOSFETs.
- `L3` and `C7` - input current-feed/filter elements.
- `L1` and `L2` - coupled inductors/transformer winding pair.
- `D1`, `D2`, `C4`, `C6`, and `R9` - rectifier, output capacitance, and load.
- `V2`, `V3` - complementary gate-drive pulse sources for `LI` and `HI`.

## Running the Simulation

1. Open `Current_Fed_Half_Bridge.asc` in LTspice.
2. Keep `UCC27282.asy` and `ucc27282.lib` available in the same project directory, or install the symbol/model into your LTspice symbol and library search paths.
3. Run the transient simulation.
4. Probe the relevant nets listed below.

If LTspice cannot find the UCC27282 symbol, update the symbol reference in the schematic or copy `UCC27282.asy` into the expected LTspice symbol path. The schematic currently references the driver symbol as `lib\\JAL\\UCC27282`.

## Useful Probe Nodes

- `Vin` - converter input voltage.
- `LI`, `HI` - gate-driver logic inputs.
- `LO`, `HO` - low-side and high-side gate-driver outputs.
- `VSH` - high-side switch node.
- `VGSH` - measured high-side gate-to-source voltage.
- `VHigh` - high-side supply node.
- `Vout` - output voltage node.
- `GNDhigh` - output-side return.
- `VOUT_mes` - behavioral output voltage measurement, `V(Vout)-V(GNDhigh)`.
- `vr1` - behavioral measurement of `V(vsh)-V(vshcap)`.
- `vr2` - behavioral measurement of `V(vsecd)-V(vsecc)`.

## Notes

- The UCC27282 model is a TI PSPICE transient model released by Texas Instruments in 2018 and included here as `ucc27282.lib`.
- The schematic depends on LTspice device models for parts such as `IPP048N12N3`, `VS-E5TX1506`, and `VS-E5TH1506`. If those models are not available in your LTspice installation, add the required vendor libraries or replace the parts with available equivalents.
- The schematic uses micro-unit values that may appear as special characters in the raw `.asc` text. LTspice should display these values normally when the schematic is opened.
