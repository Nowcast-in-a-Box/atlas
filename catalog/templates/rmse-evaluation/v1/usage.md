# RMSE Evaluation — provisional usage notes (template v1)

Status: provisional NiB Hub v0 catalog entry declared internally by the
Nowcast-in-a-Box project. This is a draft declaration of an intended
Evaluation Product, not an upstream verified product identity; no library or
implementation name from the earlier demo is used as Product identity.

## What this entry declares

Root-mean-square error verification of a precipitation forecast field against
a paired reference observation field. Both inputs are consumed by named
input ports (`forecast`, `reference`); the paired-field matching requirements
are recorded as Product-specific constraints referenced from the input ports.
The output uses the shared EvaluationResult contract with RMSE's metric
identity declared in Product-specific constraints (scalar value in the input
units, lower is better).

## Configuration

The v0 configuration schema exposes no Product parameters; reduction and
aggregation rules stay Product-owned and are not invented here. The
deterministic configuration for this ProductVersion is the empty object `{}`.

## Delivery state (unresolved for deployment)

- No implementation artifact, tool source, or delivery reference is declared
  (`delivery_references` is empty).
- Provider/maintainer upstream evidence, license, and resource requirements
  are unverified and not declared.
- External Contract Conformance Evidence has not been obtained; the entry
  must not be treated as ready.
