# cg-assets

Personal collection of CG test assets for custom-renderer research and cross-renderer comparison.

## Shaderball

A modified version of the [USD Working Group **StandardShaderBall**](https://github.com/usd-wg/assets/tree/main/full_assets/StandardShaderBall)
(Apache-2.0), adapted so it renders in my own pipeline.

Changes vs. the original:

- **Lights converted to emissive geometry** — the original UsdLux area lights are replaced by
  emissive meshes (`light_emitter`, `light0-3_emitter`), so any renderer that treats emissive
  surfaces as area lights lights the scene correctly with no special light concept.
- **Emissive UsdPreviewSurface added** to those emitter materials, alongside the existing
  MaterialX network, so the emission survives export to formats that read UsdPreviewSurface (e.g. glTF).
- The studio **enclosure** (numbered floor grid, labelled walls, ceiling) and the shader ball itself
  are kept intact.

### USD version — `StandardShaderBall/usd/`

`Shaderball-blender_export.usda` + `textures/`.

- Materials carry **both** MaterialX (OpenPBR / Autodesk Standard Surface) **and** UsdPreviewSurface networks.
- Lights are emissive meshes: `light_emitter` emits `(10,10,10)`, `light0-3_emitter` emit `(5,5,5)`.
- Wall/ground textures are `.exr` in ACEScg.
- A single near-black constant `DomeLight` remains as a faint ambient fill.

### glTF version — `StandardShaderBall/gltf/`

`.glb` exported from `Shaderball.blend`, as a portable metallic-roughness baseline for renderers
that don't consume USD/OpenPBR.

- Emission > 1 is carried via `KHR_materials_emissive_strength`.
- Textures are transcoded from ACEScg `.exr` to PNG.
- The `DomeLight` is dropped (no glTF equivalent); lighting comes entirely from the emissive geometry.

> **Note (comparing against Blender):** the textures are correctly converted ACEScg → sRGB on
> export. When comparing a rendered Blender viewport against another renderer, remember Blender's
> viewport shows **AgX-tonemapped** output while the raw textures are not — don't mistake that
> difference for a conversion error.

## Source

`StandardShaderBall/Shaderball.blend` is the authoring file; the USD and glTF versions are exported from it.
