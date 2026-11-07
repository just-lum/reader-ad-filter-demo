# Reader Ad Filter — Technical Demo

Sanitized demonstration footage of an on-device ad-filtering stack for a **mobile reading application**.
The clip shows the reader running with filtering enabled: page turns complete without interstitial ad breaks, and the filter's own status panel reports its route counters.

**▶ Video (inline, plays on this page)**

<video src="https://github.com/user-attachments/assets/1ef30720-7892-421f-8b78-5102ab254550" controls width="100%"></video>

Direct link: [demo/reader-ad-filter-demo.mp4](demo/reader-ad-filter-demo.mp4) · 236.6 s · 888×1920 · 24 fps · H.264

Everything below is a **technology name list**. No implementation details, no configuration values, no target identifiers, no operational steps.

---

## Stack (technology names only)

**Packet path**
- `Android VpnService` — local, non-root VPN interface
- `TUN` virtual interface
- User-space `TCP relay` / connection proxying
- Per-connection `TCP` reassembly in user space

**Name resolution**
- `DNS` query parsing and upstream forwarding
- `DoT` (DNS-over-TLS) interception
- `SNI` extraction from the `TLS ClientHello`
- `TLS` handshake inspection **without decryption** — no MITM, no CA installation

**Filtering rules**
- Hostname blocklist with `glob`-style wildcard matching
- Large-scale hostname list loaded from an asset file
- Priority tiers: force-block > blocklist > allowlist
- Per-app scoping (`addAllowedApplication`) and full-device routing (`addRoute`)

**UI-layer automation**
- `AccessibilityService` — `TYPE_WINDOW_CONTENT_CHANGED` / `TYPE_VIEW_SCROLLED` event subscription
- Text-node matching against a skip-word set
- `dispatchGesture` synthesized tap

**App shell**
- `Foreground Service` with `specialUse` type
- `BOOT_COMPLETED` receiver for auto-start
- `SharedPreferences` state persistence
- `FileProvider` for log export

**Media**
- `H.264` / `MP4` (`yuv420p`, 24 fps)

---

## What the video shows

| Segment | Content |
|---|---|
| 0:00–0:02 | Filter control panel — master switch, per-app route counter, log toggle, accessibility status |
| 0:02–3:56 | Continuous reading session: consecutive chapter advancement, monotonically increasing page index, no interstitial break |

Reading region and all device chrome (status bar, page counter, navigation bar) are mosaic-masked and flattened in the published cut. Audio track is removed.

## Scope

Demonstration of a filtering architecture on a single device. The footage is a **content-level** sample only: it documents that continuous page advancement occurs with the filter active, not that every ad-bearing code path is covered.

---

## License

Footage and text in this repository are provided for reference. No source code is included.
