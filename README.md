# Terrain Slider App

Interactive 3D terrain generator. Drag sliders for the 9 swept noise
parameters (plus seed) and a native [GLMakie](https://docs.makie.org/)
window re-renders the terrain live. An **Export** button regenerates the
selected parameters at full 4096² resolution and writes a `.npz`
(with the parameter values embedded) plus a styled PNG.

The generation pipeline (value-noise FBM with analytic gradients, domain
warp, shaping, radial falloff) is identical to the parameter-sweep
notebook, so a live 256² preview matches the full-resolution export at
equal grid size.

## Requirements

- Julia ≥ 1.9 (tested on 1.12)
- A working OpenGL setup (GLMakie opens a native window)

## Setup

This repo ships a `Project.toml` (and `Manifest.toml` once you commit it),
so the environment is reproducible:

```bash
git clone https://github.com/<you>/terrain-slider-app.git
cd terrain-slider-app
julia --project=. -e 'import Pkg; Pkg.instantiate()'
```

The first run precompiles GLMakie, which can take several minutes. That is
normal, not a hang.

## Run

```bash
julia --project=. -t auto terrain_slider_app.jl
```

`-t auto` enables multithreading, which makes the live preview noticeably
snappier (heightmap generation is threaded).

You can also run it from a REPL, which keeps the window open and is faster
after the first compile:

```julia
julia --project=. -t auto
julia> include("terrain_slider_app.jl")
```

## Controls

| Slider            | Type    | Live range   |
|-------------------|---------|--------------|
| octaves           | integer | 1 – 10       |
| lacunarity        | float   | 0.5 – 5.5    |
| gain              | float   | 0.10 – 1.0   |
| k                 | float   | 0.5 – 15     |
| domain            | float   | 0.1 – 15     |
| shaping           | float   | 0.1 – 5.0    |
| radial_strength   | float   | 0.1 – 2.5    |
| radial_exponent   | float   | 0.1 – 7.5    |
| max_elev (m)      | float   | 500 – 5500   |
| seed              | integer | 1 – 100      |

Ranges, preview resolution (`PREVIEW_RES`), and the throttle interval are
constants near the top of `terrain_slider_app.jl`.

Exports are written to `live_exports/` (git-ignored).
