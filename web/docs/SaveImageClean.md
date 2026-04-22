# Save Image Clean

Save images with cleaner folder names and predictable output paths.

This node supports two modes:

- Legacy mode: leave `path_template` empty
- Template mode: set `path_template` to build the full relative path yourself

## Legacy Mode

Legacy mode uses this path structure:

```text
<output_root>/<subfolder>/<model_folder>/<clip_folder>/<filename_datetime>.png
```

Example:

- `subfolder`: `portraits`
- `model_folder`: `manual-model`
- `clip_folder`: `manual-clip`
- `filename_datetime`: `%Y-%m-%d_%H-%M`

Result:

```text
portraits/manual-model/manual-clip/2026-04-22_15-30.png
```

## Template Mode

Template mode uses `path_template` as the full relative output path.

Recommended starting template:

```text
%ACTIVE_UNET%/%ACTIVE_CLIP%/%date:yyyy-MM-dd_hh-mm%
```

Example manual-folder template:

```text
%SUBFOLDER%/%MODEL_FOLDER%/%CLIP_FOLDER%/%date:yyyy-MM-dd_hh-mm%
```

Example result:

```text
portraits/manual-model/manual-clip/2026-04-22_15-30.png
```

## Template Variables

- `%ACTIVE_UNET%`: auto-detected active UNET or diffusion model name, without known file extension
- `%ACTIVE_CLIP%`: auto-detected active CLIP or text encoder name, without known file extension
- `%MODEL_SHORT%`: shortened form of `%ACTIVE_UNET%`
- `%CLIP_SHORT%`: shortened form of `%ACTIVE_CLIP%`
- `%MODEL_FOLDER%`: manual `model_folder` value, without known file extension
- `%CLIP_FOLDER%`: manual `clip_folder` value, without known file extension
- `%SUBFOLDER%`: sanitized `subfolder` value

### Variable Meaning Example

Input values:

- detected UNET loader name: `jibMixZIT_v10.safetensors`
- detected CLIP loader name: `mradermacher - Huihui-Qwen3-4B-abliterated-v2.Q8_0.gguf`
- manual `model_folder`: `manual-model`
- manual `clip_folder`: `manual-clip.gguf`
- `subfolder`: `portraits`

Resolved values:

- `%ACTIVE_UNET%` -> `jibMixZIT_v10`
- `%ACTIVE_CLIP%` -> `mradermacher - Huihui-Qwen3-4B-abliterated-v2.Q8_0`
- `%MODEL_SHORT%` -> `jibMixZIT_v10`
- `%CLIP_SHORT%` -> `Huihui-Qwen3-4B-abliterated-v2.Q8_0`
- `%MODEL_FOLDER%` -> `manual-model`
- `%CLIP_FOLDER%` -> `manual-clip`
- `%SUBFOLDER%` -> `portraits`

Meaning:

- `ACTIVE_*` = automatically detected from the active workflow
- `*_SHORT` = detected active value, but shortened for cleaner names
- `*_FOLDER` = manual field value from the node

## ComfyUI-Style Placeholders

`path_template` also supports ComfyUI-style `%node.widget%` placeholders.

Examples:

- `%KSampler.seed%`
- `%Empty Latent Image.width%`
- `%Empty Latent Image.height%`

The node resolves these from the execution prompt and can match:

- node title
- `Node name for S&R`
- node class name
- internal node id

If a reference is ambiguous, the node raises an error instead of guessing.

## Date Formatting

Inside `%date:...%`, the node supports these ComfyUI-style tokens:

- `yyyy`
- `yy`
- `MM`
- `M`
- `dd`
- `d`
- `hh`
- `h`
- `mm`
- `m`
- `ss`
- `s`

Example:

```text
%date:yyyy-MM-dd_hh-mm%
```

Unsupported text inside `%date:...%` remains unchanged.

## Strftime Formatting

Inside `%strftime:...%`, the node supports this Python `strftime` subset:

- `%Y`
- `%y`
- `%m`
- `%d`
- `%H`
- `%M`
- `%S`
- `%f`
- `%%`

Example:

```text
%strftime:%Y-%m-%d_%H-%M-%S%
```

This subset is intentionally small so behavior stays stable across platforms.

## Loader Detection

In template mode, the node walks upstream through the prompt graph and tries to detect active names from:

- UNET loaders such as `UNETLoader` and `UnetLoaderGGUF`
- diffusion-model loader variants such as `Load Diffusion Model`
- CLIP and text encoder loaders such as `CLIPLoader`, `DualCLIPLoader`, `TripleCLIPLoader`, `QuadrupleCLIPLoader`, and `TextEncoderLoader`
- checkpoint loaders such as `CheckpointLoaderSimple`, `ImageOnlyCheckpointLoader`, and `unCLIPCheckpointLoader`

If detection fails, the node falls back to `model_folder` and `clip_folder`.

## Collision Modes

- `increment`: append `-2`, `-3`, and so on
- `overwrite`: replace the existing file
- `error`: stop if the target already exists
- `seconds`: retry with a seconds-based filename

## Metadata

Saved PNG files preserve:

- `prompt`
- `extra_pnginfo`
