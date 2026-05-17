# relay.fizx.uk

> Public info page for the relay.fizx.uk Nostr relay.

**Live**: <https://relay.fizx.uk>

## Stack

- Static HTML + inline CSS + vanilla JS — no build step

## Nostr

_No Nostr events published by this site._

NIP-11 client + WebSocket connectivity probe. Page sits in front of the actual relay daemon (nostr-rs-relay v0.9.0); nginx routes WSS upgrades and `Accept: application/nostr+json` to the relay process, everything else to this static page.

## Develop

_Open `index.html` in a browser, or `python3 -m http.server` from this dir._

## Build + deploy

```bash
rsync -avz --delete -e "ssh -p 2121" ./ root@88.218.206.187:/var/www/relay.fizx.uk/ \
  --exclude node_modules --exclude .git --exclude .well-known --exclude relay-stats.json
```

> The `.well-known/nostr.json` (NIP-05 file) and `relay-stats.json` (cron-rewritten) live only on the server — never push these from local.

VPS: `88.218.206.187`. Full server / nginx / SSL / DNS notes for the wider deployment live in the local `code_vibe/CLAUDE.md` (not pushed; this README is the public-facing summary).

---

_Sister repo on the other side: <https://github.com/macos-node/relay.upleb.uk>_
