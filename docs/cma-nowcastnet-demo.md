# CMA Radar / NowcastNet demo declarations

The July 2026 CMA CREF demo declares `cma-radar`, `nowcastnet-cma`,
`cref-verification` and `cref-visualization`. Shared information lives here;
raw radar data, weights, images and run outputs do not.

A region is selected by center longitude/latitude. Runtime constructs a local
WGS84 LAEA grid with 128×128 cells at 3000 m spacing (384 km edge-to-edge).
Observations and forecasts use the same grid; evaluation reads stored arrays
without display reprojection. Original mosaic sampling is 0.01 degrees.

Only July 2026 observation timestamps enter this demo. A usable retrospective
window has six context frames and twenty targets, all separated by six minutes.
The latest context is the forecast initialization; leads are +6 to +120 minutes.
The model consumes the latest five context frames.

CREF is dBZ, while the reused ONNX weight was pretrained on SEVIR VIL. The demo
clips valid CREF to 0..75 dBZ, maps it linearly to 0..255, and applies the existing
SEVIR normalization. Forecast output is inverse-mapped to 0..75 dBZ. This is a
numerical demo mapping, not a physical VIL conversion. Six-minute runtime frame
spacing differs from the five-minute training spacing. Forecast skill is
unvalidated. Do not interpret a successful execution or the resulting scores as
business readiness.

The local weight is copied to `artifacts/cma_radar/nowcastnet/NowcastNet.onnx`.
SHA256: `773b6cbe0b1e0c8cea3b62932dc60b108b60885a54f7445a0074a48d4810b37d`.
No download or training is required. Comparison images and animation use the
stored metre grid and do not require a basemap cache.

The stored-run schema retains `nib.run.v1` and accepts both existing SEVIR
`vil/raw` descriptors and CMA `CREF/dBZ` descriptors. Runtime validates the
variable/unit pair and cadence against the data declaration.
