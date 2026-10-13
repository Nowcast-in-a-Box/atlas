# GSMaP_NOW Data — provisional usage notes (template v1)

Status: provisional NiB Hub v0 catalog template. This is a packagable
declaration template for configuration delivery, not verified upstream
deployment material and not a claim that data access has been executed.

## What this entry provisions

Acquisition and regional provisioning of the JAXA GSMaP_NOW near-real-time
precipitation product as an independent Data Product for Nowcast-in-a-Box.
GSMaP NOW acquisition and regional processing code was observed inside the
AINPP Xception upstream repository (short commit `2b38214c`); no independent
artifact, access path, or distribution basis has been established for this
Data Product yet. The Hub Package records references only; it does not embed
or transfer meteorological data.

## Configuration

The v0 configuration schema exposes no Product parameters. The deterministic
configuration for this ProductVersion is the empty object `{}`.

## Delivery state (unresolved for deployment)

- No independent delivery reference is declared (`delivery_references` is
  empty); access method and usage/distribution restrictions for GSMaP_NOW
  are unverified.
- Region, temporal/spatial resolution, units, output format, and file naming
  for this Product's observation output are not yet established and remain
  declared unknown (null) in the Product constraints.
- License evidence is unverified; no resource requirements are declared.
