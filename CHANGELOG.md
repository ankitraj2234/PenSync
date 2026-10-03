# Changelog

All notable changes to this project will be documented in this file.

## [2.0.0] - 2026-10-03
### Added
- **Dynamic Context-Aware Pie Menu:** The Windows app now tracks the active window under your cursor (e.g. Photoshop, Blender) and dynamically beams application-specific shortcuts directly to your Android tablet's radial menu.
- **Pure Trackpad Mode:** Easily disable Left/Middle/Right tools to interact with your tablet screen like a pure glass trackpad, supporting all native gestures.
- **Tap-to-Toggle Radial Menu:** The Pie Menu now supports tap-to-open, drag-to-move, and features a dedicated center hole button to close gracefully.
- **Developer Watermarks & Obfuscation:** Added code obfuscation, R8 compilation, and single-file AOT compilation to prevent reverse engineering.
- **Background Service Integration:** The setup installer now automatically registers a background task with highest privileges to run the server on logon without UAC prompts.

### Changed
- Refactored `PieMenuView` to accept dynamic labels and shortcuts.
- Replaced the deprecated Hold-to-Open gesture on the radial dial with seamless dragging/tapping logic.

### Fixed
- Fixed trackpad mouse input blockages by allowing `touchMouseButton` to be completely deselected (`state 3`).
- Re-architected transport layer for hybrid UDP/TCP with integrated `PING` heartbeats for resilient connections.
