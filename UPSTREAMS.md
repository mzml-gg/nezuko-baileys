# Upstream notes

`nezuko-baileys` is distributed as an independent fork maintained by `mzml-gg`.

The base distribution is `@rexxhayanasi/elaina-baileys@1.4.7`, which already ships a complete VoIP stack, WASM resources, signaling, and audio/video feeders.

`@hiroosy/baileys-lite@0.4.0` was downloaded and audited as part of the integration request. It also ships an independent VoIP stack. Its internal files are not loaded alongside Elaina's stack because two signaling engines would register competing WebSocket handlers and state machines on one socket. The stable `./voip` entry point is provided by `src/voip/index.js` and forwards to the integrated engine in `src/Voip`.

Both upstream projects are MIT-licensed according to their published package metadata. The original `LICENSE` and `NOTICE.md` files are preserved in this repository.
