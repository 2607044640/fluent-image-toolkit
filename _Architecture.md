# Fluent Image Toolkit Architecture

**id:** `fluent-image-toolkit` · **class:** `ImageToolkitPlugin` (`main.js`) · **manifest:** v1.4.4, `minAppVersion` 1.8.7, `isDesktopOnly: true`

Community plugin extension of Image Toolkit. Adds instant hover `Ctrl+C` image-to-clipboard copying, `Ctrl+X` cut with embed removal, high-resolution SVG export, and floating zoom capsule button.

## Invariants

1. **Standalone Community Architecture**: Published as `fluent-image-toolkit` under `C:\ObsidianPublish\fluent-image-toolkit` with twin junctions to development and active note vaults.
2. **Instant Hover Clipboard (`Ctrl + C` / `Cmd + C`)**:
   - `mouseover` / `mouseout` on `document` tracks the currently hovered `<img>` element into `plugin.hoveredImg`.
   - Global `keydown` with `{ capture: true }` intercepts `Ctrl + C` / `Cmd + C`.
   - If mouse hovers on an image and no text is selected (or selection is just image link `![[...]]`), default markdown copying is suppressed (`preventDefault`, `stopPropagation`), and `ImgUtil.copyImage(hoveredImg)` executes immediately.
3. **Dual Clipboard Export**:
   - Writes native image directly to system clipboard via Electron (`electron.clipboard.writeImage`) for instant compatibility with native desktop applications.
   - Writes `image/png` Blob via `navigator.clipboard.write` for browser and web contexts.
4. **Modal Hotkey Support**: Inside the popup preview modal (`ContainerView`), `Ctrl + C` is also bound to copy the currently previewed image without requiring mouse clicks on the toolbar copy icon.
5. **Direct Click Suppression (Passthrough to Obsidian)**: Direct clicks on images in notes/editor are not intercepted by the plugin. Clicking passes through to native Obsidian handling, avoiding accidental modal popups and double-layer rendering.
6. **Top-Right Hover Zoom Button**: Hovering over an image displays a floating `(+)` (`Zoom in`) capsule button positioned at the top-right of the image (or aligned directly to the left of `</>` in Live Preview embeds). Clicking this button is the explicit trigger to open the preview modal.
7. **Default Expanded View & Anti-Ghosting**:
   - Initial modal sizing (`calculateImgZoomSize`) defaults to fitting ~85% of the viewport (proportional expansion), eliminating tiny-scale rendering especially for SVGs with Chromium 300x150 fallbacks.
   - Modal backdrop uses `rgba(15, 23, 42, 0.92)` with `backdrop-filter: blur(4px)` to prevent underlying note images from showing through as a distracting second layer.
8. **High-Resolution Vector SVG Clipboard Export**: When copying SVG images (`.svg` or `data:image/svg+xml`), `ImgUtil.copyImage` parses the vector root, injects high-resolution explicit dimensions (2.5x rendered size or at least 2400px), and re-rasterizes via a vector Blob URL rather than Chromium's blurry 300x150 fallback, ensuring copied SVG diagrams/charts paste with razor-sharp clarity.
9. **Full Image Cut (`Ctrl + X` / `Cmd + X`) with Note Embed Removal**:
   - Global `keydown` with `{ capture: true }` intercepts `Ctrl + X` / `Cmd + X` when targeting an image: (1) mouse hovering an image (`plugin.hoveredImg`), (2) active editor selection matching image syntax (`ImgUtil.parseImageEmbed`), or (3) cursor positioned on an image embed line.
   - Suppresses native text cut (`preventDefault`, `stopPropagation`) so Obsidian does not place raw markdown text `![[Pasted image ...]]` on the system clipboard.
   - **Single Mutation Pipeline**: Vault `TFile` instances are resolved directly via `app.vault.getResourcePath(file)` and fed through the unified `ImgUtil.copyImage` pipeline.
   - Deletes the image wikilink/embed line from the active note editor via `editor.replaceRange` / `editor.replaceSelection` (or `vault.process` in reading view), removing the image from the note while preserving full `Ctrl + Z` undo history in CodeMirror.
   - Inside the popup preview modal (`ContainerView`), `Ctrl + X` also copies the binary image to clipboard, deletes the source embed from the note, and closes the modal.
