# Donald Kuck

**GPU hardware, RTL-to-system simulation, and quantitative alpha research.**

I work across the full stack of a RISC-V SIMT GPU — Chisel RTL, cycle-accurate
simulation, Linux DRM drivers, QEMU full-system emulation, and EDA verification —
and spend part of my time on quantitative finance: alpha factor research, agent-driven
mining loops, and backtesting infrastructure.

## Work: OpenGPGPU

**Building a verifiable RISC-V SIMT GPU that actually runs Linux.**

OpenGPGPU takes GPU hardware as the core and covers the whole development chain:
from Chisel RTL and SIMT compute/graphics pipelines, to Linux DRM drivers, QEMU
full-system simulation, and EDA signoff.

### Core projects

| Project | Focus | Highlights | Language |
|---|---|---|---|
| **[gpu](https://github.com/OpenGPGPU/gpu)** | RISC-V SIMT GPU | Chisel 7.x RTL; RV32IMF + RVV vector & FP pipelines; unified shading (vertex/fragment share the SIMT cores) plus fixed-function clip/raster/interpolate/output-merger/depth-stencil-blend; Sv32 private VA with ASIDs and a graphics TLB; unified command set (render/compute/copy/fill/blit/strided/resolve/invalidate); L1/L2 + shared memory; Linux DRM/KMS with hardware vblank IRQ, render-to-KMS present, Debian console scanout, MSAA 1x/2x/4x + resolve; 8-thread Verilator by default, with a FlashSim backend | Scala |
| **[FlashSim](https://github.com/OpenGPGPU/FlashSim)** | Cycle-accurate RTL simulator | Imports HW/Comb/Seq through CIRCT (firtool/circt-opt/circt-verilog) and emits C++ that skips idle combinational cones; cycle-aligned with Verilator at an order of magnitude or more faster at low activity; top-level gate-demand hoisting, sibling `&`/`|` prefix CSE, and NBA RHS demux blocking; drops into ARTI/QEMU in place of Verilator | Python / C++ |
| **[ARTI](https://github.com/OpenGPGPU/arti)** | RTL to full-system simulation | Auto-detects AXI4/AXI-Lite/APB/AHB/AXI-Stream and generates SystemC/Verilator plus QEMU-embedded models; auto-discovers IRQs and wires them to a GIC with 100µs polling; dynamic guest-memory scanout (BASE/stride/control/width/height registers, refresh timer, RGBA conversion), fixed-VRAM simple-framebuffer, console boot; multi-threaded Verilator, Debian cloud-init, automatic external driver KO loading, present demos, and FlashSim/Verilator A/B comparison | Python |
| **[ChipAgent](https://github.com/OpenGPGPU/chipagent)** | EDA tooling service layer | 56 MCP tools wrapping real Yosys / OpenSTA / OpenROAD binaries: synthesis, ASAP7 ORFS through DEF/GDS, post-route STA, simulation and waveform transaction analysis, alignment checks, PPA evaluation; SV parameter DSE, multi-file RTL, macro GDS and fixed placement, substring-targeted high-fanout splitting, clock-port autodetection, Docker or host execution | Python |

### Architecture

Unified shader cores paired with dedicated fixed-function units: compute and shading
programs run on the same RISC-V SIMT lanes, while geometry, rasterization, texture
sampling, and output merging are done by purpose-built RTL.

```text
Linux / DRM userspace ── pipe_opengpu / triangle_present
         │
 Linux DRM/KMS driver ── MMIO + IRQ(vblank/completion) + shared memory
         │
   AXI host interface (control) + AXI memory master
         │
┌────────▼─────────────────────────────────────────────┐
│                    OpenGPGPU                         │
│                                                     │
│  Command buffer ──► Geometry ──► Rasterizer         │
│                            │              │          │
│                            ▼              ▼          │
│                RISC-V SIMT shader cores ──► Texture │
│                            │              │          │
│                            └──────────────► Output   │
│                                      merger / depth │
│                                                     │
│           Shared L1/L2 and host physical memory     │
└─────────────────────────────────────────────────────┘
         │
  ARTI guest-memory GraphicHwOps ── QEMU scanout
```


## Interests

- **GPU and hardware architecture** — SIMT execution models, unified shading, texture
  pipelines, memory hierarchies, and the DRAM/AXI interfaces underneath them;
- **Chip design and EDA** — Chisel and Scala for hardware construction, plus Yosys,
  OpenSTA, and OpenROAD for synthesis, timing closure, and PPA analysis;
- **Full-system simulation** — building QEMU models and device trees from RTL, boot
  paths from firmware to userspace, and cycle-accurate fast paths that stay bit-compatible
  with a reference simulator;
- **Linux and open source** — DRM/KMS, device drivers, console bring-up, and the
  toolchains that make the above reproducible;
- **Quantitative finance** — factor modeling, alpha mining, backtesting, and
  reinforcement learning applied to trading systems;
- **Software rendering** — the from-scratch rasterizer in tinyrenderer, as a
  reference for understanding what a GPU pipeline is actually doing.

## Where to look first

Start with **[gpu](https://github.com/OpenGPGPU/gpu)** if you want the hardware; go to
**[ARTI](https://github.com/OpenGPGPU/arti)** for full-system simulation,
**[ChipAgent](https://github.com/OpenGPGPU/chipagent)** for EDA automation, and
**[FlashSim](https://github.com/OpenGPGPU/FlashSim)** for fast RTL simulation.

The organization overview lives at
[OpenGPGPU/profile](https://github.com/OpenGPGPU/blob/main/profile/README.md).
