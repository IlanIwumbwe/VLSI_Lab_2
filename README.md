# VLSI Lab 2 Logbook - Understanding and Editing Layout

## Task 1 - Inverter Layout Mask Layers

### Layer Definitions

**NW (N-Well)**
- N-type region created by phosphorus/arsenic implantation
- Houses PMOS transistors (PMOS needs n-type substrate)

**OD (Oxide Diffusion / Active)**
- Defines where transistors are formed (source, drain, channel regions)
- Areas without OD are covered with field oxide for isolation

**PO (Polysilicon Gate)**
- Forms transistor gates
- Where PO crosses OD = transistor location

**PP (P+ Diffusion)**
- P-type implant regions (boron)
- Creates NMOS source/drain outside N-well

**NP (N+ Diffusion)**
- N-type implant regions
- Creates PMOS source/drain inside N-well

**CO (Contact)**
- Vertical connections between metal and poly/diffusion
- Tungsten plugs through oxide layer

**M1 (Metal 1)**
- First metal layer for local routing
- Used for power rails in standard cells

**prBndry (PR Boundary)**
- Cell boundary definition

---

## Task 2 - XOR Gate Circuit Extraction


![hell](https://github.com/user-attachments/assets/ec957cd4-83ee-4cbe-a72c-a3a53aed4db1)

### CKXOR2D0BWP7T Layout Analysis

**12-Transistor Configuration:**
- Top row: T1, T3, T5, T7, T9, T11 (PMOS - in N-well)
- Bottom row: T2, T4, T6, T8, T10, T12 (NMOS - outside N-well)

### Circuit Operation

**When A1 = 0:**
- Transmission gates T5/T6 pass A2 inversion through
- Output inverted: Z = A2

**When A1 = 1:**
- T5/T6 switch on inverter to invert inversion of A2
- Output inverted: Z = Ā2

**Result:** Z = A1 ⊕ A2

### Transistor Sizing (Measured with "k" command)

see drawing above

<img width="575" height="351" alt="image" src="https://github.com/user-attachments/assets/056179e6-3c04-4f09-97db-3d27069de912" />


---

## Task 3 - Manual Placement

<img width="1514" height="415" alt="image" src="https://github.com/user-attachments/assets/d573ed1e-5f6a-43a1-87e3-c5f207082e48" />

<img width="1402" height="237" alt="image" src="https://github.com/user-attachments/assets/93d3518e-653d-498b-91ec-719555eecee3" />

### Setup
1. Created LAB_2 library linked to tsmcN65 technology
2. Imported `lfsr4_soc.v` netlist as schematic + symbol
3. Changed global net names: removed "!" from VDD/VSS

### VDD/VSS Connection
- Drew M1 wires ("w" command) to power pins
- Labeled wires ("l" command) as VDD or VSS
- Required because Verilog netlists don't include power

### Placement Strategy
Optimal order: DFF1 → XOR → DFF2 → DFF3 → DFF4 → XOR
- Follows data flow
- Minimizes wire length
- Cells aligned on 7-track grid with prBndry abutting

---

## Task 4 - Routing

<img width="1920" height="1440" alt="image" src="https://github.com/user-attachments/assets/4a4206b5-a471-49cc-8f8e-bc90caf136b4" />

### Clock Routing
1. Horizontal M1 trunk above/below cells
2. M2 vertical drops to each CP (clock pin)
3. M1-M2 vias at connection points

### Reset Routing
- Similar to clock: M1 horizontal + M2 vertical drops
- Connected to reset pins of all DFFs

### Data Path Routing
- Neighboring cells: Direct M1 connection
- Multi-cell connections: M1 → via → M2 → via → M1
- Q[4:1] outputs routed to right edge


---

## Task 5 - LVS Verification

### LVS Configuration
- Rule deck: `/usr/local/cadence/kits/tsmc/65n_LP/Calibre/lvs/calibre.lvs`
- SPICE library: `tcbn65lpbwp7t_141a.spi` (contains standard cell models)

### Successful LVS

- 0 unresolved correspondence points
- All devices matched
- CORRECT status



<img width="1239" height="529" alt="image" src="https://github.com/user-attachments/assets/5e98bad5-9696-4fde-b061-13ec4c4a0f65" />






