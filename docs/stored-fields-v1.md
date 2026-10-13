# Stored SEVIR fields v1

A run is identified by a unique run ID and a manifest under configured
`runs/<id>/`. The manifest is `nib.run.v1`; arrays are NPZ with pickle disabled.
A model/time alone never identifies a run. Readers are bound to one manifest.

## Scientific identity

Data ID/version, event ID and window context-start time identify the sample.
`init_time` is the last context frame, not the first. Twenty target/forecast
frames occur at +5 through +100 minutes. All times are explicit UTC. Observation
and prediction valid times, variable and units must match before scoring.

VIL values use the source's encoded `raw` scale. They are not dBZ, rain rate or
physical kg/m². Model adapters restore scale 255 before persistence. Shape is
`(time, y, x)`, float32; named NPZ arrays are `values`, boolean `mask` (true means
missing), and one-dimensional `x`, `y` axes. Unmasked values are finite. Missing
values never become artificial zero observations.

Descriptors give relative paths, SHA-256, shape, dtype, variable/unit, times and
explicit grid CRS. Forecast descriptors also declare model ID/version, pinned
weight digest, init time and actual leads. No arbitrary file, shell command or
Docker argument is accepted through this protocol. Readers reject traversal,
symlink escapes, bad checksum, dimensions and unsupported schema versions.

## SEVIR georeferencing

CATALOG's VIL row declares event-specific LAEA projection and corners. It does
not define a universal SEVIR bbox. Grid x/y remain projected metres for scoring.
The upstream tutorial's `LaeaProjection` uses its lower-left projected point as
pixel (0,0), y increases northward:
https://github.com/MIT-AI-Accelerator/eie-sevir/blob/master/examples/SEVIR_Tutorial.ipynb

The local preprocessing uses PyTorch bilinear `align_corners=False` without a
vertical flip. For source size S, output size N, output index i maps to source
pixel `(i+0.5)*S/N-0.5`. Applying upstream pixel coordinates gives an offset of
1 km for 384→128 and spacing 3 km. Coordinates must not be replaced with bbox
linspace or labelled degrees. Record preprocessing semantics as provenance.

The processed file does not preserve original missing-value masks through its
bilinear transform. The initial reader rejects events with nonzero
`pct_missing`; serving those events requires mask-aware preprocessing from the
original data. This is an explicit current capability limit, not filled data.

## Provider

A container-owned provider implements capabilities, list/get observations, and
list/get predictions. Construct it with a run directory; names/time/leads are
resolved only within that run. It caches each verified cube once, so 20 lead
requests do not reopen a Forecast 20 times. No model inference occurs in a query.
Unavailable fields raise a typed error; unavailable time and masked pixels are
different cases. Visualization and verification convert the same stored fields
to their local types through thin adapters.

Creating an existing run or writing an existing forecast fails. Repeating
inference creates a new run. Diagnostics use new attempt IDs and separate
stores under the source run; provenance binds input digests and ruler. All
outputs remain below the configured runs root, including failed attempts.

JSON schema validates declaration shape. Readers additionally check array shape,
UTC cadence, checksum, mask, axis order and init+lead equality. The schema is not
an array or scientific conformance validator.
