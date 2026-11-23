# CMA WADEPre demo

This is a cross-domain demo. The WADEPre ONNX weights were trained on SEVIR VIL and are reused for CMA CREF. Forecast skill is not validated.

The adapter clips CMA dBZ to 0..75, normalizes to 0..1, and linearly maps the 20 five-minute ONNX outputs onto the 20 six-minute CMA leads. The final 120-minute lead holds the 100-minute raw output.

## Local setup

```bash
mkdir -p artifacts/cma_radar/wadepre
cp artifacts/sevir/wadepre/WADEPre.onnx artifacts/cma_radar/wadepre/WADEPre.onnx
docker build \
  --build-context weights=artifacts/cma_radar/wadepre \
  -f adapters/cma_radar/wadepre/Dockerfile \
  -t nib-wadepre:cma-demo-embedded .
docker save -o artifacts/wadepre-cma-demo.tar nib-wadepre:cma-demo-embedded
```

On another machine:

```bash
docker load -i wadepre-cma-demo.tar
```

The Handler resolves the model from `adapters/cma_radar/wadepre/runtime.yaml`. Product `wadepre-cma` maps to `cma-radar`, `nowcastnet-cma`, and the optional CMA diagnostics products.

## Selection and run

Import a `nib-hub-selection-v1` JSON containing `cma-radar@v1`, `nowcastnet-cma@v1`, `wadepre-cma@v1`, and the diagnostic products. Select a July 2026 CMA sample, save the YAML order, and start it. Both models run sequentially: NowcastNet first if listed first, then WADEPre.

Verification writes both models into one metric store. Visualization writes one card per model. A comparison image is emitted when target observations are present; the interactive map itself shows forecasts only.
