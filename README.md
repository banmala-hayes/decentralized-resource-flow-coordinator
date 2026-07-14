# Decentralized Resource Flow Coordinator

A local-first, peer-to-peer workflow board for tracking resource
tokens across processing stages, with real-time sync and per-stage
timing. No backend — state lives in the browser and syncs directly
between peers.

## What it does
- Drag-and-drop tokens across six workflow lanes
- Dual timers per token: total time in system + time in current step
- Live per-lane counts
- Peer-to-peer sync so multiple stations share one live view

## How it works
- Vanilla JavaScript, no server or database
- Peer-to-peer sync over WebRTC via PeerJS
- Stations connect by room ID and mirror state through ADD / MOVE /
  REMOVE messages

## Security review (self-conducted)
This began as a clinical patient-queue tool. Before publishing, I
generalised it (tokens, not patients) to remove sensitive data, then
threat-modelled the sync layer:
- **Guessable room IDs** — `node-` + 4 digits (~9,000 combinations),
  brute-forceable
- **Unauthenticated state broadcast** — any peer that joins receives
  the full current state
- **No message authentication** — MOVE / REMOVE events aren't verified

## Known limitations & planned fixes
- Sync is last-write-wins, not a true CRDT — concurrent edits can
  conflict
- Planned: long random room IDs, a shared-secret handshake before
  state sync, self-hosted PeerServer, and WebCrypto payload encryption
