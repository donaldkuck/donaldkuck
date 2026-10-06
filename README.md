# Donald Kuck

**GPU hardware, RTL-to-system simulation, and quantitative alpha research.**

I work across the full stack of a RISC-V SIMT GPU — Chisel RTL, cycle-accurate
simulation, Linux DRM drivers, QEMU full-system emulation, and EDA verification —
and spend part of my time on quantitative finance: alpha factor research, agent-driven
mining loops, and backtesting infrastructure.


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
