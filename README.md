# Digital IC Design Flow: from Verilog to Layout with Synopsys Tools

Coursework from **Introduction to Integrated Circuit Design** (Tecnológico de Costa Rica, 2nd semester 2025). Over ten assignments I went from CMOS fundamentals to a full RTL-to-layout flow, using Synopsys Design Compiler for synthesis and Synopsys ICC2 for floorplanning, placement, clock tree synthesis and routing, on the SAED 90 nm educational library.

The reports are written in Spanish. The PDFs in [`Tareas/Entregas`](Tareas/Entregas) are my submitted deliveries, with scripts, reports and analysis.

## What I can do with this flow

| Area | What I did |
| --- | --- |
| **Logic synthesis** | Wrote Tcl scripts for `analyze`, `elaborate`, `link` and `compile_ultra` in Design Compiler, with and without timing constraints, and compared the results. |
| **Timing constraints (SDC)** | Defined clocks, uncertainty, latency, input and output delays, max transition and max fanout. |
| **Static timing analysis** | Calculated setup and hold slack by hand, and read PrimeTime-style timing reports to find critical paths. |
| **Physical design** | Created floorplans from a target utilization and aspect ratio, built the power mesh and supply nets, and ran placement, clock tree synthesis and routing in ICC2. |
| **Optimization** | Ran `place_opt`, `clock_opt` and `route_opt`, generated reports after every stage, and measured the cost of each improvement in area, cells, buffers and power. |
| **Analysis** | Explained why designs did or did not close timing, and proposed fixes. |
| **CMOS fundamentals** | Studied CMOS gate design, layout rules and stick diagrams, and worked through NAND, NOR, AOI and multiplexer cells at transistor level. |

## The flow

```mermaid
flowchart LR
    A[Verilog RTL] --> B[Synthesis<br/>Design Compiler]
    B --> C[Floorplan and power mesh<br/>ICC2]
    C --> D[Placement<br/>place_opt]
    D --> E[Clock tree synthesis<br/>clock_opt]
    E --> F[Routing<br/>route_opt]
    F --> G[Reports and QoR analysis]
```

## Results at a glance

**ALU, assignments 4 to 9** (SAED 90 nm, 1 ns target period)

| Stage | Result |
| --- | --- |
| Synthesis without constraints | 2,114 cells, critical path of 750.81 ps |
| Synthesis with a 1 ns clock | WNS of -159.70 ps, 92 violating paths, 58 % more cells and 23.7 % more area |
| Post-layout at 1 ns | WNS improved to -10.37 ps, because synthesis estimates were pessimistic for this small design |
| Tighter 0.8 ns clock | Timing got worse, not better: more logic was added but violations grew |
| High utilization (90 %) | Chip area dropped 14.5 %, but congestion degraded timing |
| Conclusion | Best trade-off for this ALU: 1 ns period with 80 to 85 % utilization. Going faster needs architectural changes such as pipelining or faster adders. |

**Micro8088 processor, final assignment** (full flow, 1 GHz target, 98 % utilization, 16:9 floorplan)

| Metric | Value |
| --- | --- |
| Leaf cells | 10,097 (610 flip-flops) |
| Clock tree | 313 clock buffers |
| Worst negative slack | -18.45 ns on a path with 71 logic levels |
| Design rule violations | 2,167 nets, all max transition or max capacitance. No geometric DRC violations were reported. |
| Total power | 4.69 mW |

The Micro8088 did **not** close timing at 1 GHz. The report analyzes why, with a critical path of 18.65 ns, and proposes a path to convergence: relax the clock period, pipeline the critical path, lower the utilization, and fix the design rule violations step by step.

## Assignments

| # | Topic | File |
| --- | --- | --- |
| 1 | CMOS logic and layout: pull-up and pull-down networks, design rules, stick diagrams, and gate schematics such as NAND, AOI and multiplexers | [Tarea_1_VLSI_merged.pdf](Tareas/Entregas/Tarea_1_VLSI_merged.pdf) |
| 2 | CMOS theory: FinFET vs RibbonFET, PowerVia and backside power, 3D chip packaging with chiplets | [Tarea_2_VLSI.pdf](Tareas/Entregas/Tarea_2_VLSI.pdf) |
| 3 | Not included in this repository | |
| 4 | Logic synthesis without constraints (ALU): scripts, power, area, cell and QoR reports | [tarea_4_VLSI.pdf](Tareas/Entregas/tarea_4_VLSI.pdf) |
| 5 | Timing: setup and hold slack, clock skew and jitter, crosstalk, PVT, reading a timing report | [tarea_5_VLSI.pdf](Tareas/Entregas/tarea_5_VLSI.pdf) |
| 6 | Placement and CTS concepts, and the ICC2 commands behind them | [tarea_6_VLSI.pdf](Tareas/Entregas/tarea_6_VLSI.pdf) |
| 7 | Synthesis with constraints at 1 ns and 0.8 ns, and comparison against assignment 4 | [tarea_7_VLSI.pdf](Tareas/Entregas/tarea_7_VLSI.pdf) |
| 8 | Floorplan and placement in ICC2, post-layout reports at 1 ns and 0.8 ns | [Tarea8_VLSI_David_Miranda.pdf](Tareas/Entregas/Tarea8_VLSI_David_Miranda.pdf) |
| 9 | Optimization flow (`place_opt`, `clock_opt`, `route_opt`), reports per stage, cost versus benefit | [tarea9_VLSI.pdf](Tareas/Entregas/tarea9_VLSI.pdf) |
| 10 | Final project: complete flow on the Micro8088 processor | [tarea_10_VLSI.pdf](Tareas/Entregas/tarea_10_VLSI.pdf) |

## Scope and honesty notes

- This is university coursework, not production work. The goal was to learn the flow and to analyze the results, not to tape out a chip.
- The designs were assigned by the course. The course statements, the Synopsys manual and the technology libraries are not included here.
- The Synopsys tools need a license, so the flow cannot be reproduced from this repository alone.

## About me

Electronics engineer focused on industrial automation, with a background in FPGA and digital design. More projects are on my [GitHub profile](https://github.com/AlbertoDAMG30), and you can reach me on [LinkedIn](https://www.linkedin.com/in/david-miranda-electronica).
