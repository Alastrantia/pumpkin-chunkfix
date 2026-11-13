# pumpkin-chunkfix

Builds [Pumpkin](https://github.com/Pumpkin-MC/Pumpkin) `0.2.1+26.3-26.51` (static ARM64 musl, like the upstream release) with one small patch:

- **Respawn in the same world**: the client drops all chunks on respawn (always for clients translated by ViaVersion/ViaRewind,
  e.g. 1.8/1.12), but Pumpkin kept its list of "already sent" chunks and re-sent only the centre chunk -> the player sees a
  single chunk. The patch unloads the watched chunks first so the whole view distance is sent again (what vanilla does).
- **Teleport**: re-centre the watched chunks right away instead of waiting for the player to cross a chunk border.

The binary is attached to each workflow run as an artifact.
