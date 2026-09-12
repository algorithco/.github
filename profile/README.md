# Algorith — Builders of future technology

> Uzbekistan-based builder collective focused on Telegram automation, TON / Web3 escrow, developer tooling, and web experiments.

[![Org](https://img.shields.io/badge/org-algorithco-black?logo=github)](https://github.com/algorithco)
[![Location](https://img.shields.io/badge/location-Uzbekistan-1e81b0)](https://github.com/algorithco)
[![Repos](https://img.shields.io/badge/repos-5%20public-blue)](https://github.com/orgs/algorithco/repositories)
[![Flagship](https://img.shields.io/badge/flagship-p2p%20TON%20escrow-0098EA?logo=ton)](https://github.com/algorithco/p2p)

## About

**Algorith** ([@algorithco](https://github.com/algorithco)) builds practical, production-oriented software — from Telegram bots and Mini Apps to network tooling and browser benchmarks.

What we care about:

- **Telegram-native products** — bots, Mini Apps, userbots, account-sale escrow
- **TON / Web3** — W5 signer isolation, Tact escrow contracts, jettons (TON / USDT)
- **Dev UX & infra** — QUIC-first tunnels, Docker Compose micro-services, CI + secret scanning
- **Open, reproducible builds** — documented envs, healthchecks, signed releases where applicable

Location: **Uzbekistan**

## Repositories overview

| Repository | What it is | Stack | License |
|------------|------------|-------|---------|
| [**p2p**](https://github.com/algorithco/p2p) — ⭐ flagship | TON P2P escrow for Telegram: grammY bot + Mini App + Express API, isolated W5 signer, ubot / utradebot, Postgres. Off-chain ledger with optional on-chain TON escrow. Live at [@savdochi_uzbot](https://t.me/savdochi_uzbot) | TypeScript, React / Tailwind, Express, Postgres, Tact, teleproto | AGPL-3.0 |
| [**trqshcli**](https://github.com/algorithco/trqshcli) | Release artifacts for the `trqsh` CLI — QUIC-first tunnels to localhost (HTTP / TCP / UDP, real TLS, interactive console). Source lives in [uzcreator/trqsh](https://github.com/uzcreator/trqsh) | Go (upstream), Shell / npm / PyPI wrappers | Apache-2.0 (upstream) |
| [**idstickbot**](https://github.com/algorithco/idstickbot) | Sticker & Custom Emoji ID bot — instant `file_id` / `custom_emoji_id` with tap-to-copy. Trilingual EN / RU / UZ. Live at [@idstickrobot](https://t.me/idstickrobot) | Python 3.12, aiogram 3.30, Supabase, Render | MIT |
| [**bench-MiMo-V2.5-minecraft**](https://github.com/algorithco/bench-MiMo-V2.5-minecraft) | Minecraft clone benchmark — browser voxel engine, procedural terrain, no external assets | JavaScript, Three.js r128, Canvas 2D, Simplex noise | — |
| [**.github**](https://github.com/algorithco/.github) | Org profile and community health (this README) | Markdown | — |

### 1. p2p — TON Escrow Bot [flagship]

> `algorithco/p2p` — 196 commits, TypeScript, AGPL-3.0

Peer-to-peer escrow service for Telegram: two parties trade TON or USDT safely. Funds held by on-chain `Escrow` (Tact) when deployed, backend keeps full off-chain deal ledger so product works before wallet/contract exists.

**Architecture (6 services + Postgres):**

- `webapp/` — nginx Mini App on `:8080`, proxies `/api/*` → backend
- `backend/` — API-only grammY bot + Express REST on `:3000` (`/api/docs`, `/api/info`)
- `signer/` — isolated W5 (V5R1) wallet microservice on `:3001`
- `ubot/` — channel/group takeover userbot (teleproto) on `:3002`
- `utradebot/` — account-sale escrow (teleproto) on `:3003`
- `postgres` — deals, messages, trades

**Engineering highlights:** Conventional Commits + release-please, ESLint / Prettier / `tsc`, CodeQL + Gitleaks + Trivy, Docker + GHCR with provenance, non-root services, `x-api-key` + Telegram `initData` HMAC auth, rate limits.

Start here: [p2p README](https://github.com/algorithco/p2p#readme) · [Topics: escrow, ton, telegram-mini-app, tact, w5](https://github.com/algorithco/p2p)

### 2. trqshcli — public HTTPS for localhost

> `algorithco/trqshcli` — release-only repo

```bash
curl -fsSL https://trqsh.uz/install.sh | sh
trqsh login
trqsh http 3000
```

QUIC-first with TCP fallback, `http` / `tcp` / `udp` tunnels, Let's Encrypt TLS, slash-command console (`/pin traffic`, `/requests`, `/qr`, `/copy`), background tunnels, reserved subdomains, signed `checksums.txt` (cosign). Install via shell, npm (`@trqsh-uz/trqsh`), PyPI (`trqsh`), Homebrew, Scoop, winget, apt/dnf.

Maintained by Otabek Hamroqulov. See [trqsh.uz](https://trqsh.uz).

### 3. idstickbot — Sticker & Emoji ID utility

> `algorithco/idstickbot` — Python, MIT, 9 commits

Send any sticker → get `file_id`, `file_unique_id`, `set_name`, emoji. Send Premium emoji → get `custom_emoji_id`. Correct UTF-16 entity handling, tap-to-copy `<code>` blocks, persistent reply keyboard, per-user language (EN/RU/UZ), silent-in-groups design, Render + Supabase deploy ready with `/health`.

### 4. bench-MiMo-V2.5-minecraft — voxel benchmark

> `algorithco/bench-MiMo-V2.5-minecraft` — JavaScript, 18 commits

Zero-dependency browser Minecraft: chunked world + face culling, simplex-noise terrain (hills, caves, beaches), canvas texture atlas, AABB physics, raycast mine/place, trees, infinite streaming, hotbar, chat commands (`/gamemode`, `/fly`, `/time`, `/tp`, `/seed`).

```bash
python -m http.server 8000
# open http://localhost:8000
```

## Tech stack

`TypeScript` · `Python` · `JavaScript` · `grammY` · `aiogram` · `React / Tailwind` · `Express` · `Postgres / Supabase` · `TON / Tact / W5` · `teleproto` · `Three.js` · `Docker / Compose` · `nginx`

## Getting started

1. Pick a repo above — each has its own Quickstart.
2. For `p2p`, you need `BOT_TOKEN` + Postgres for off-chain mode; `SIGNER_MNEMONIC` only for on-chain.
3. For `idstickbot`, you only need `BOT_TOKEN` from [@BotFather](https://t.me/BotFather).
4. For `trqshcli`, no build needed — download from Releases.

## Contributing & security

- No direct pushes to `main` — short-lived branch → PR → green CI → squash-merge (see `p2p` model).
- Use Conventional Commits (`feat:`, `fix:`, `docs:`, …).
- Never commit `.env`. Rotate any credential ever committed.
- Report security issues privately via repo Security tabs.

## Links

- Org: https://github.com/algorithco
- Flagship: https://github.com/algorithco/p2p
- Live bots: [@savdochi_uzbot](https://t.me/savdochi_uzbot) · [@idstickrobot](https://t.me/idstickrobot)
- Tunneling: https://trqsh.uz

---
*This profile lives in [`algorithco/.github`](https://github.com/algorithco/.github) as `profile/README.md` and is shown on the organization overview page.*
