# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added

- ComfyUI-style `%node.widget%` placeholders in `path_template`
- best-effort resolution against node title, custom `Node name for S&R`, class name, and node id
- in-node descriptions and field tooltips for `Save Image Clean` and `Strip Model Extension`

### Changed

- `%date:...%` formatting now supports single-letter ComfyUI tokens such as `M`, `d`, `h`, `m`, and `s`
- `HH` remains supported as a compatibility alias in addition to the ComfyUI-style `h` and `hh`
- active loader detection now recognizes more UNET, text encoder, and checkpoint loader naming patterns, including common custom-node variants such as diffusion-model loaders and GGUF-style loaders

## [0.1.0] - 2026-04-21

### Added

- initial Git repository structure
- `Save Image Clean` node with legacy and template-based save modes
- automatic upstream detection for active UNET and CLIP loader names
- template variables for model, clip and date-based output paths
- `Strip Model Extension` utility node
- repository documentation for installation, usage and contribution workflow
