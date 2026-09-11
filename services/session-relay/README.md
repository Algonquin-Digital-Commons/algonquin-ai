# AC Session Relay

The Session Relay is an institution-controlled, content-blind transport for
cross-device AI sessions. It routes end-to-end encrypted envelopes between mobile
clients and Session Hosts and stores only bounded ciphertext and minimum routing
metadata.

The relay must not hold transcript keys, workspace files, model credentials, or
remote-shell authority. Required behaviors include authenticated devices, replay
protection, per-session ordering/cursors, bounded offline retention, revocation,
rate limits, metadata-minimized observability, and opaque push wake events.

