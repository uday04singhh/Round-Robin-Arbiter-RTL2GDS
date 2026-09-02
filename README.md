# 4-Master Round Robin Arbiter — RTL to GDSII

A 4-master round robin arbiter written in SystemVerilog, taken through a complete open-source RTL-to-GDSII physical design flow using **OpenLane** (built on **OpenROAD**) targeting the **SkyWater 130nm (sky130A)** PDK.

This repository contains the RTL source and configuration files needed to reproduce the full flow — synthesis, floorplanning, placement, clock tree synthesis, routing, and sign-off (DRC/LVS) — resulting in a fully routed, manufacturable GDSII layout.

## Results Summary

| Metric | Value |
|---|---|
| Core Utilization | 58.02% |
| Die Area | ~0.0019 mm² |
| Setup Slack | +6.78 ns |
| Hold Slack | +0.49 ns |
| Total Power | 1.55 × 10⁻⁴ W |

## Prerequisites

This project requires **OpenLane** to be installed and set up, including Docker and the sky130A PDK.

Follow the official OpenLane installation and quickstart guide before proceeding:
👉 https://openlane.readthedocs.io/en/latest/getting_started/index.html

Official repository:
👉 https://github.com/The-OpenROAD-Project/OpenLane

## How to Run

**1. Clone this repository into OpenLane's `designs` directory**

```bash
cd <path_to_your_openlane_installation>/designs
git clone <this_repo_url> round_robin_arbiter
```

**2. Enter the OpenLane Docker environment**

```bash
cd <path_to_your_openlane_installation>
make mount
```

**3. Run the full RTL-to-GDSII flow**

From inside the container:

```bash
cd /openlane
./flow.tcl -design ./designs/round_robin_arbiter -tag my_run -overwrite
```

**4. Check the results**

Once the flow completes, key reports and outputs will be available under:

```bash
designs/round_robin_arbiter/runs/my_run/
├── reports/       # Timing, area, power, DRC/LVS reports
├── results/final/gds/   # Final manufacturable GDSII layout
└── logs/          # Full logs for every flow stage
```

View the final metrics summary:

```bash
cat designs/round_robin_arbiter/runs/my_run/reports/metrics.csv
```

View the timing sign-off report:

```bash
cat designs/round_robin_arbiter/runs/my_run/reports/signoff/31-rcx_sta.summary.rpt
```

**5. (Optional) View the layout graphically**

Using KLayout:

```bash
klayout designs/round_robin_arbiter/runs/my_run/results/final/gds/*.gds
```

Using the OpenROAD/OpenLane GUI:

```bash
./flow.tcl -design ./designs/round_robin_arbiter -tag my_run -interactive
```

Then inside the Tcl prompt:

```tcl
package require openlane
or_gui
```

## Configuration

Key design parameters are set in `config.json`, including:

- `CLOCK_PORT` / `CLOCK_PERIOD` — clock definition and target frequency
- `FP_CORE_UTIL` — core utilization / cell density
- `DESIGN_IS_CORE` — whether the design is treated as a standalone chip or a reusable macro

Feel free to modify these and re-run with a different `-tag` to experiment with different timing/area/density trade-offs.

## License

This project uses fully open-source tools — OpenLane, OpenROAD, Yosys, Magic, KLayout, Netgen — and the open SkyWater sky130A PDK.
