# Time/Lead-time Animation — provisional usage notes (template v1)

Status: provisional NiB Hub v0 catalog entry declared internally by the
Nowcast-in-a-Box project. This is a draft declaration of an intended
Visualization Product, not an upstream verified product identity.

## What this entry declares

Rendering of forecast fields as an animation along the lead-time axis. The
primary input is the forecast field (named input `forecast`, required); a
reference observation field may be shown for comparison (named input
`reference_observation`, optional). The output uses the shared
VisualizationArtifact contract with an animation artifact kind, MP4 format,
and lead-time axis capability declared as a draft Product-specific
constraint.

Frame ordering, frame rate, duration, and output size are Product-owned
configuration; v0 exposes none of them.

## Configuration

The v0 configuration schema exposes no Product parameters. The deterministic
configuration for this ProductVersion is the empty object `{}`.

## Delivery state (unresolved for deployment)

- No rendering implementation source or delivery reference is declared
  (`delivery_references` is empty).
- License and resource requirements are unverified and not declared.
- External Contract Conformance Evidence has not been obtained; the entry
  must not be treated as ready.
