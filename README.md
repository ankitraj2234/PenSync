# PenSync: Turn Your Android Tablet into a Pro Windows Drawing Tablet

PenSync is an open-source tool that transforms any Android tablet with an active stylus (like the Moto Pad 60 Pro, Samsung Galaxy Tab S-series, etc.) into a professional digital art graphics tablet for Windows PC. It acts as an input-only drawing surface, similar to a Wacom or Huion pen tablet (without screen mirroring), offering ultra-low latency, tilt support, 8192 levels of pressure sensitivity, and customizable express keys for creative software like Photoshop, Krita, and Blender.

| Part | Path | What it is |
|---|---|---|
| Windows app | `windows/src/PenSync.App` | Elevated WPF control panel and host: pairing, USB/Wi-Fi, pen settings, mapping, express keys, driver status, tray icon |
| Core library | `windows/src/PenSync.Core` | Protocol v2, crypto, host runtime, pen pipeline, HID report encoder (UI-free, fully unit-tested) |
| Pen driver | `windows/driver/penvhf` | KMDF + VHF virtual HID pen (pressure 8192 levels, X/Y tilt, eraser, barrel) |
| Android app | `android/tablet` | Pairing, USB/Wi-Fi connection, low-latency stylus capture, express-key panel |
| Diagnostics | `windows/src/PenSync.HostCli`, `android/diagnostic` | Headless test host; Phase-0 capability probe |

## Quick start

1. **PC** (administrator PowerShell, from the repo root):
   ```powershell
   dotnet publish windows\src\PenSync.App -c Release -o windows\dist\PenSync
   powershell -ExecutionPolicy Bypass -File windows\setup.ps1 -Launch
   ```
   This installs the app and the driver. The first run enables test mode: reboot and run it again. See [docs/driver-install.md](docs/driver-install.md).
2. **Tablet:** install `android/tablet/app/build/outputs/apk/debug/app-debug.apk` and enable USB debugging.
3. **Pair:** connect the USB cable, click **Pair new tablet** in PenSync on the PC, then tap **Pair with this PC** on the tablet. Accept only if both screens show the same 6-digit code.
4. Draw. PenSync reconnects automatically next time; tap **Connect**.

### USB vs Wi-Fi

- **USB** (via `adb reverse`) gives the lowest and most stable latency.
  - USB 2 versus USB 3 makes no difference: a pen produces about 30 KB/s.
  - Use any good data cable.
- **Wi-Fi:** enable it on the PC's Connection page. It is restricted to Private networks and the local subnet.
  - Pen data uses encrypted UDP with redundancy, so a lost packet does not stall a stroke.
  - Expect a few extra milliseconds and occasional jitter.

### Security

- Every connection, USB included, is mutually authenticated (ECDSA P-256) and encrypted (AES-256-GCM).
- Only paired tablets connect.
- Pairing requires the PC user to open a 2-minute window and both users to confirm a matching code.
- Express keys send only a key number; the PC decides what each key does.

Details: [ADR-003](docs/architecture/adr-003-protocol-v2-secure-transports.md) and [ADR-004](docs/architecture/adr-004-windows-app-privilege-model.md).

## Documentation

- [docs/HANDOFF.md](docs/HANDOFF.md): build, test, architecture, status and known limits (start here)
- [docs/protocol-v2.md](docs/protocol-v2.md): normative wire protocol
- [docs/driver-install.md](docs/driver-install.md): driver signing and installation
- `docs/architecture/`: ADR-001 to ADR-005
- `docs/research/`: Phase-0 hardware findings
