# pumpkin-chunkfix

Builds [Pumpkin](https://github.com/Pumpkin-MC/Pumpkin) `0.2.1+26.3-26.51` (static ARM64 musl, like the upstream release) with one small patch:

- **Respawn in the same world**: the client drops all chunks on respawn (always for clients translated by ViaVersion/ViaRewind,
  e.g. 1.8/1.12), but Pumpkin kept its list of "already sent" chunks and re-sent only the centre chunk -> the player sees a
  single chunk. The patch unloads the watched chunks first so the whole view distance is sent again (what vanilla does).

The binary is attached to each workflow run as an artifact.

## 0002: packet events for plugins must not crash the server

With any plugin listening to `PacketReceivedEvent`, the WASM host converts every incoming packet into a typed value and
unwraps integer conversions while doing so. A packet whose id is looked up in the wrong protocol state (a play packet read as a
configuration packet) can make such a conversion fail, and the panic stops the whole server. The patch turns those unwraps
into "unknown packet" (`None`) in the two serverbound conversion functions.

