# Antigravity Mobile Monitor Launcher

Mobile web interface for monitoring Antigravity chat via CDP (Chrome DevTools Protocol).

## Prerequisites

1. Start Antigravity with remote debugging enabled (in background):

```bash
antigravity . --remote-debugging-port=9000 &
```

2. Start the Antigravity Claude proxy:

```bash
PORT=3000 antigravity-claude-proxy start
```

3. **Important**: Open the Cascade chat panel in Antigravity before starting the monitor. The server needs to find 3+ execution contexts to capture the chat correctly.

## Installation

```bash
npm install
```

## Usage

Since port 3000 is used by the Claude proxy, run the monitor on port 3001:

```bash
npm start
```

Then access the monitor at `http://localhost:3001` or from mobile at `http://<your-ip>:3001`.

## Troubleshooting

### "Failed to load chat" error

If you see this error on the mobile interface:

1. Make sure the Cascade panel is open in Antigravity
2. Restart the monitor server (`Ctrl+C` then `npm start`)
3. The server should report "Found 3 execution contexts" (not 2)

### Server can't find CDP

If you see `CDP not found`:

- Verify Antigravity is running with `--remote-debugging-port=9000`
- Check that port 9000 is not blocked or used by another process

## How it works

The monitor connects to Antigravity via CDP on port 9000, finds the workbench page, and polls for the `#cascade` element to capture chat snapshots. These are served to the mobile web interface via WebSocket updates.
