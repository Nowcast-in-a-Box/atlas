# Stored-field contract validation (#1)

The initial contract was checked against the producer and both diagnostic
consumers in Nowcast-in-a-box's #14 integration work, based on main `d0380c5`.

- Draft 2020-12 schema self-validation passed.
- `examples/run-ingested.json`, derived from an actual read of
  `S696651@2017-06-27T00:35:00Z`, passed schema validation with date-time format checks.
- Traversal and absolute artifact paths, and an unsupported run schema version,
  were rejected.
- Runtime tests passed for ambiguous-event rejection, run/forecast non-overwrite,
  cached 20-lead reads, checked array paths/SHA256/shapes/masks/times, and unit/grid
  identity shared by verification and visualization.
- Native projected-grid scoring retained array values/masks; different CRS or
  projected grid resampling was rejected. Geographic default fingerprints and
  the existing HTTP wire format remained compatible.
- A new threshold generated its missing quantities without invoking ONNX, while
  repeated queries reused stored results. Separate verification attempts retained
  earlier reports; partial and failed attempts did not erase Forecasts.

The runtime's final focused check at this point passed 66 tests. Its real
container, complete-suite and basemap checks are being recorded in that repo;
this document does not claim them passed. The runtime changes are not merged yet.

Source geometry follows the upstream SEVIR Tutorial's lower-left pixel reference
and the local preprocessing's PyTorch `align_corners=False` mapping. Input files
and arrays are external; this repository contains only their declarations.

Limit: the processed file lost original missing-pixel masks during bilinear
preprocessing. The initial source refuses events with nonzero catalog
`pct_missing`; expanding coverage requires mask-aware preprocessing. Atlas v1
currently describes the SEVIR integration slice, not arbitrary datasets.
