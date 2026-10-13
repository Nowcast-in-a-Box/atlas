# atlas

Shared NiB declarations and versioned contracts. Implementations, datasets,
weights, caches and run outputs stay in their runtime repositories. Consumers
pin a commit. New contracts are reviewed through pull requests.

The first runtime slice defines [stored SEVIR fields](docs/stored-fields-v1.md)
and its [run manifest schema](contracts/runtime/run-v1.schema.json). The
[ingested-run example](examples/run-ingested.json) was produced by an actual
SEVIR read; it contains descriptors only, not the dataset or arrays. Its
`unpublished` atlas revision denotes a development fixture. Production consumers
record the specific atlas commit they use.

Job/API contracts and catalog migration follow the runtime slice; the schema
alone does not establish model inference or diagnostic readiness.
