# atlas

Shared NiB declarations and versioned contracts. Implementations, datasets,
weights, caches and run outputs stay in their runtime repositories. Consumers
pin a commit. New contracts are reviewed through pull requests.

The first runtime slice defines [stored SEVIR fields v1](docs/stored-fields-v1.md),
[its run manifest schema](contracts/runtime/run-v1.schema.json), and the
[ingested-run example](examples/run-ingested.json). That example was produced by
an actual SEVIR read; it contains descriptors only, not the dataset or arrays.
Its `unpublished` atlas revision denotes a development fixture. Production
consumers record the specific atlas commit they use.

[Stored physical fields v2](docs/stored-fields-v2.md) defines the new
`nib.run.v2` schema, examples, model input/output declarations, and manually
registered versioned catalog references for SEVIR and CMA. The shared field
layout remains single-variable `float32(time,y,x)` with a boolean missing mask.
V2 adds arbitrary data and variable identifiers, explicit cadence, and aggregate
batch manifests. The retrospective context/target task is unchanged. V1 schemas,
contracts, templates, products and runtime mapping are retained. Runtime mapping
files named `products-v1.json` and `products-v2.json` both use
`nib.runtime-products.v1`; that identifier names the mapping format.

The v2 registrations are declarations, not publication evidence. Runtime and
atlas source revisions and immutable image/weight deliveries have not been
published or resolved. Do not treat a declared product as production-ready.
Catalog registration is manual; there is no exporter. Follow the v2 document's
steps and calculate hashes from exact referenced bytes. Never copy evidence from
an older product version to assert a new version.

Job/API contracts and catalog migration follow the runtime slice; a schema alone
does not establish model inference, diagnostic readiness, or forecast skill.

The Hub exports a selection with one data product, one or more compatible models,
and optional diagnostics. Consumers resolve exact product versions and configuration
references in their pinned catalog. The catalog revision identifies the exporting
catalog content. The Handler supplies local paths, events and execution windows.
See the [selection contract](contracts/hub/selection-v1.schema.json) and the
[two-model example](examples/hub-selection-v1.json).

## MLCast data sources

The [MLCast source index](runtime/mlcast-sources.json) lists seven upstream Zarr
sources, with their variables, cadence and example timestamps. The corresponding
[reader declarations](contracts/data) use the `mlcast_` prefix and are registered
in the [adapter catalog](runtime/adapters-catalog.json). Reader implementations
and dataset files stay outside Atlas.
