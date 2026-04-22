# Strip Model Extension

Remove one known model file extension from the end of a string.

Examples:

- `model.safetensors` -> `model`
- `model.gguf` -> `model`
- `model.ckpt` -> `model`

This is useful when you want a clean folder or filename component from a loader name.
