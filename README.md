# relay.fizx.uk

> Public info page for the relay.fizx.uk Nostr relay — live status, NIP-11 metadata, supported NIPs.

**Live**: <https://relay.fizx.uk>

## Stack

- [Vite](https://vitejs.dev/) + React 18 + TypeScript
- Tailwind CSS
- lucide-react

(Was static HTML before 2026-05-17 — converted to Vite/React to match the sibling subdomains.)

## Nostr

- **Login**: NIP-07 (browser extension) + NIP-55 (Amber callback URI). Nav-bar widget only — no events published from this page.
- NIP-11 client: fetches `Accept: application/nostr+json` from `https://relay.fizx.uk` to display relay name, description, pubkey, contact, software, version, supported NIPs.
- WebSocket connectivity probe: opens `wss://relay.fizx.uk`, measures TTFB latency, retries every 30 s on failure / 5 min while online.

The page sits in front of the actual relay daemon (nostr-rs-relay v0.9.0 on the host). The nginx vhost routes WSS upgrades and `Accept: application/nostr+json` requests to the relay process (port 8080), everything else to this static SPA.

## Develop

```bash
npm install
npm run dev
```

## Build + deploy

```bash
npm run build
rsync -avz --delete -e "ssh -p 2121" dist/ root@88.218.206.187:/var/www/relay.fizx.uk/ \
  --exclude .well-known --exclude relay-stats.json
```

> The `.well-known/nostr.json` (NIP-05 file) and `relay-stats.json` (cron-rewritten) live only on the server — the `--exclude` flags prevent `--delete` from wiping them.

VPS: `88.218.206.187`. Full server / nginx / SSL / DNS notes for the wider deployment live in the local `code_vibe/CLAUDE.md` (not pushed; this README is the public-facing summary).
