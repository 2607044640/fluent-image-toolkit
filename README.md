# Fluent Image Toolkit

An Obsidian plugin for enhanced image viewing, clipboard operations, and document image management.

## Overview

Fluent Image Toolkit streamlines image handling in Obsidian. It introduces instant image copying and cutting directly from notes via keyboard shortcuts, high-resolution SVG export, and an expanded lightbox viewer for detailed image inspection.

## Features

- **Hover Image Copy (`Ctrl + C` / `Cmd + C`)**: Hover over any image in reading or editing view and press `Ctrl + C` (`Cmd + C` on macOS) to copy the image bitmap directly to your system clipboard instead of copying raw Markdown wikilinks.
- **Hover Image Cut (`Ctrl + X` / `Cmd + X`)**: Hover over or select an image and press `Ctrl + X` (`Cmd + X` on macOS) to copy the binary image to your clipboard and remove the image link from the active document. Full undo (`Ctrl + Z`) is preserved.
- **Hover Image Delete (`Delete`)**: Hover over any image and press `Delete` to remove the image embed directly from your document without overwriting your system clipboard.
- **Floating Zoom Button**: Displays a compact zoom button at the top-right of images when hovered, allowing you to open the lightbox viewer without hijacking normal text editor clicks.
- **High-Resolution SVG Rasterization**: SVGs are re-rasterized at 2.5x rendered scale (or at least 2400px) prior to copying, ensuring vector diagrams paste cleanly into desktop apps without Chromium blur.
- **Dual Clipboard Pipeline**: Simultaneously writes native DIB/bitmap formats via Electron and standard PNG blobs via the Web Clipboard API for compatibility with native desktop software and web tools.
- **Lightbox Viewer**: Preview images with zoom, pan, 90-degree rotation, horizontal and vertical flip, and color inversion.
- **Note Image Gallery**: Optional bottom navigation bar to switch between all images in the active note while in preview mode.

## Installation

### Community Plugins (Recommended)

1. Open Obsidian Settings > Community plugins.
2. Turn on Community plugins.
3. Click Browse and search for `Fluent Image Toolkit`.
4. Click Install, then click Enable.

### Manual Installation

1. Download `main.js`, `manifest.json`, and `styles.css` from the [latest release](https://github.com/2607044640/fluent-image-toolkit/releases/latest).
2. Create a folder named `fluent-image-toolkit` inside your vault's `.obsidian/plugins/` directory.
3. Move the downloaded files into that folder.
4. Reload Obsidian and enable the plugin in Settings > Community plugins.

## Usage

- **Copying an Image**: Move your mouse over any image in your note and press `Ctrl + C` (or `Cmd + C`). A notice confirms the image has been copied to your clipboard.
- **Cutting an Image**: Move your mouse over an image (or place your cursor on its line) and press `Ctrl + X` (or `Cmd + X`). The image is copied to your clipboard and its Markdown embed is removed from the file.
- **Deleting an Image**: Move your mouse over an image and press `Delete`. The Markdown embed is removed from the file without modifying your clipboard.
- **Opening the Viewer**: Click the floating `(+)` icon at the top right of any hovered image.
- **Viewer Controls**:
  - `Scroll Wheel`: Zoom in and out.
  - `Drag`: Pan the image.
  - `Arrow Left` / `Arrow Right`: Switch between images in the active document.
  - `Ctrl + C`: Copy the previewed image.
  - `Ctrl + X`: Cut the previewed image, remove its embed from the note, and close the viewer.
  - `Esc`: Close the viewer.

## Configuration

The plugin provides customizable settings in Obsidian Settings > Fluent Image Toolkit:

| Setting | Default | Description |
| :--- | :--- | :--- |
| View Trigger | Editor Areas | Choose where the floating zoom trigger is enabled. |
| View Mode | Normal | Select between Normal lightbox overlay or Pin mode. |
| Movement Speed | 10 | Keyboard panning speed inside the viewer. |
| Zoom Percentage Display | On | Displays temporary zoom percentage on scale changes. |
| Background Color | Semi-transparent | Background color for transparent images in preview. |
| Image Border | Off | Retain border styling around previewed images upon closing. |
| Gallery Navbar | Off | Display a thumbnail navigation bar of document images. |

## License

MIT License. Copyright (c) 2026 Jeff1024.
