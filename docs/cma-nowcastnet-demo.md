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

The CMA runtime image `nib-nowcastnet:cma-demo-embedded` contains the pinned
weight at `/opt/nib/weights/NowcastNet.onnx`; no host artifacts weight is needed.
SHA256: `773b6cbe0b1e0c8cea3b62932dc60b108b60885a54f7445a0074a48d4810b37d`.
Use the matching Handler/resources with embedded-weight support. Both image
build and inference verify the checksum. The current interactive map uses a
separately prepared global terrain/WMO basemap cache; metrics do not need it.

The stored-run schema retains `nib.run.v1` and accepts both existing SEVIR
`vil/raw` descriptors and CMA `CREF/dBZ` descriptors. Runtime validates the
variable/unit pair and cadence against the data declaration.

## Maintainer-approved declarations (2026-10-25, #10)

The maintainer approved verified contract conformance and MIT product-license
metadata for `cma-radar`, `nowcastnet-cma`, `cref-verification`,
`cref-visualization`, and `time-leadtime-animation`. The CMA pairing constraint
and animation output constraint use new v2 documents; v1 bytes remain unchanged
for historical references. Product references pin the new SHA-256 digests.

These are maintainer declarations, not an independent license audit. MIT here
covers the declared NiB Products; source radar data, third-party model code and
pretrained weights retain their own authorization requirements. Verified contract
conformance does not establish forecast skill. Existing frozen Orders retain
their original declarations and warnings until a new revision is validated.
