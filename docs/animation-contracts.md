# Lead-time animation contract support

`time-leadtime-animation` accepts the exact CMA CREF and SEVIR VIL forecast
contracts listed in `catalog/constraints/time-leadtime-animation/v2-field-inputs.json`.
The optional `reference_observation` port separately lists the corresponding
observation contracts. Each alternative pins the contract id, revision and SHA256.
The original precipitation contract stays declared; this does not claim its legacy
runtime Product is implemented by the current local handler.

Hub must support the `accepted_contracts` consumer constraint to resolve these
alternatives. Without that declaration it continues requiring identical contracts.
An arbitrary contract with the same name, a changed checksum, or an observation
connected to the forecast port does not match an alternative. Rendering retains
source field units and grids, rather than converting CREF into VIL.

The source evidence file `catalog/evidence/cma-runtime-source/v1.json` records
published runtime commits, source paths and reproducible `git archive` SHA256
values. It verifies source revision and availability for the CMA reader, adapter,
verification and visualization implementations. CPU-only execution establishes
that a GPU is not required for this demo; CPU/memory minima remain unbenchmarked.

This evidence does not establish rights to redistribute CMA observations or
SEVIR-trained weights and does not replace external Contract Conformance Evidence.
Their license and conformance warnings therefore remain provisional. Forecast
skill for the CREF cross-domain demo remains unvalidated.
