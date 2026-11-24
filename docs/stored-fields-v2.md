# Stored physical fields v2

`contracts/runtime/run-v2.schema.json` defines new `nib.run.v2` manifests.
Readers dispatch by `schema` and retain `nib.run.v1` support. The v1 schema,
versioned catalog contracts, usage templates and example remain unchanged.
V2 keeps the existing retrospective task: both context and target observations
are required. It does not add a live forecast task or a multivariable tensor.

## Physical boundary and stored layout

Each field contains one named variable in declared units, with dimensions
`(time, y, x)`. Values are float32 and the equally shaped mask is boolean;
`true` means missing. Unmasked values must be finite. Data identifiers,
variables and units are nonempty strings, not a SEVIR/CMA enumeration. Each
spatial dimension has at least two cells. The schema does not force 128x128.

The common descriptor layout is unchanged: `path`, `sha256`, `shape`, `dtype`,
`variable`, `unit`, `grid`, and `times`. A relative NPZ path and its SHA-256 bind
all four arrays: `values`, `mask`, `x`, and `y`. There is no separate mask file.
`x` and `y` are finite, strictly increasing one-dimensional coordinates stored
inside the NPZ, with lengths matching the spatial dimensions. `grid.crs` and
`grid.axis_units` are explicit nonempty strings. Keep grid provenance in the
descriptor; do not replace native axes with a guessed bbox. Read NPZ with pickle
disabled and reject traversal, absolute paths and symlink escapes.

Forecast descriptors additionally require `init_time`, `lead_minutes`,
`model_id`, `model_version`, and `weight_sha256`. Public fields contain physical
values in their declared units, including SEVIR's documented encoded `raw` VIL
scale. Backend tensor layouts, node names and normalization stay in the model
implementation. Weight locators are delivery metadata, not tensor semantics.

## Sample and time identity

The run's `data` declares `id`, `version`, and positive integer
`cadence_minutes`. `sample` requires `sample_id`, `data_id`, positive integer
`cadence_minutes`, `context_start_time`, and `init_time`. Its data identity and
cadence must agree with `data`. Source-specific `event_id`, `row`, and
`provenance` remain optional. No source implicitly inherits SEVIR identity.

All times are explicit UTC strings ending in `Z`, strictly increasing and
matching the first array dimension. Context begins at `context_start_time`
and ends at `init_time`. Context and target frames follow the declared
observation cadence; target starts one cadence after initialization.
Forecast leads are strictly increasing positive integer minutes. Their spacing
need not equal, or be divisible by, observation cadence. Each forecast valid
time equals initialization plus its corresponding lead, and initialization
matches the last context time. Model lead declarations are authoritative.
Diagnostics pair exact valid times with matching variable, unit, CRS and axes;
an unmatched observation time is unavailable, never silently relabeled.

## Aggregate batches

The handler's aggregate writer emits `batch: true`, positive `sample_count`,
nonempty `samples`, and optional `failed_samples` strings. Each sample copies
its child's sample metadata and adds `path: samples/<child-id>`. The count
must equal the number of listed children; child paths must be unique.
Failures describe windows or stages that did not complete, so a listed child
can still lack a forecast. Inspect each child's status and forecasts.

The aggregate retains `sample`, `data`, `observations`, and `forecasts` from
the first listed child, plus aggregate status and diagnostics. Those copied
array paths are relative to that child directory, not the batch root. Consumers
must enumerate `samples`, open each `<path>/manifest.json`, then resolve its
array descriptors within that child. The aggregate is not one combined cube.
The current aggregate writer omits `atlas_revision`; v2 permits that only for
batches. Every child run still requires its own atlas revision. A future writer
may also include a batch revision without changing this schema.

## Model declarations and manual registration

`contracts/models/<data>/<model>.json` mirrors the local runtime declaration.
`input` declares variable, units, exact context length, cadence, spatial shape,
dtype and missing-value policy. `output` declares variable, units, actual leads,
spatial shape, dtype, missing-value policy and whether its grid equals input.
The current SEVIR and CMA models consume the latest five frames from six stored
context frames. Their product input/output constraint files repeat those public
requirements and reference the declaration by path and content SHA-256.

The generic descriptor schemas are `catalog/contracts/observation-field/v2.json`
and `forecast-field/v2.json`. SEVIR and CMA catalog contracts specialize the
current products; their fixed cadences and shapes are product capabilities,
not restrictions on the generic run schema. Each of the ten runtime products
lists only its v2 entry, with hashed contracts, constraints and usage
references. `runtime/products-v2.json` selects those entries and adds CMA.
Its mapping format remains `nib.runtime-products.v1`; product version and mapping
format version are separate.

Register manually, without an exporter:

1. Review the local model declaration and mirror only its public contract and
   delivery metadata. Add the data/model pairing in `runtime/adapters-catalog.json`.
2. Add new versioned catalog contracts, input/output constraints and usage
   templates. Keep existing versioned files immutable.
3. Append the product version, retaining v1. Reference exact paths and compute
   each `sha256` from the referenced file bytes, for example with
   `shasum -a 256 catalog/contracts/sevir-observations/v2.json`.
4. Add the product/version mapping to the matching runtime product list. Check
   every referenced file and hash, including model declaration references.
5. Validate schema structure, examples, rejection cases, and runtime conformance.
   Schema validation alone does not establish readiness or forecast skill.

V2 entries are `declared`, with `upstream_version: unpublished`, no delivery
references and no inherited verified CMA evidence. Publication must select real
immutable runtime and atlas commits containing these changes. Resolve any tag
against its repository before recording the commit; a contract revision such as
`v2` or model version `0.1.0` is not a Git SHA. The mirrored Hugging Face locators
still use `main` with existing content digests. A release must resolve their
immutable upstream revisions too. No commit or upstream revision is invented
here, and no local contract update proves an image was built or published.

## Validation boundary and examples

Draft 2020-12 validates declaration structure, required metadata, minimum grid
size, positive cadence/leads, distinct leads/times, and path syntax. Enable
date-time format checks. JSON Schema cannot compare array lengths to shape,
sort arbitrary leads, compare sample/data identity, count child references, or
verify `init + lead`. Runtime checks these relations, file SHA-256, array dtype,
masks, finite values, axes, exact time alignment and model constraints.

`examples/run-ingested-v2.json` adapts the existing real-read v1 metadata without
changing its NPZ descriptors. It does not claim a new data read. The synthetic
`examples/run-batch-v2.json` demonstrates the aggregate layout and a failure;
it does not include child directories or arrays. Both remain development
fixtures, not evidence of a published atlas revision or completed inference.
