# 📊 JC's Market Sentiment Tracker — Live

A single-page, zero-build, live-updating market sentiment dashboard with an
**attractive 3D-tile design** — data blocks render as glossy, extruded tiles
containing side-by-side tables (stacked automatically when the page margin is
narrow). Open `index.html` in any browser, or use the GitHub Pages deployment
below.

**Live site:** https://ssjs1mu.github.io/GlobalMarketSentiment/

## Refresh model

- **⟳ Refresh Now button** in the header (or press **R**) refreshes *all* live
  feeds on demand — spinning icon while the fetch runs.
- **Every block shows its own auto-refresh interval** as a chip, plus a
  live **"updated HH:MM:SS"** chip that turns amber when a block goes stale.

| Block | Source | Auto-refresh |
|---|---|---|
| Indian markets table (Nifty 50, Bank Nifty, Sensex, Midcap, Fin Services, IT, heavyweight stocks) | Yahoo Finance public quote API (`v7/finance/spark`, batched) | every 30 s |
| Global indices & futures table (ES/NQ/YM/RTY, FTSE, DAX, CAC, Euro Stoxx, Nikkei, Hang Seng, Shanghai, ASX, KOSPI) | Yahoo Finance | every 30 s |
| Commodities table (Gold, Silver, WTI, Brent, NatGas, Copper, Platinum, Bitcoin) | Yahoo Finance | every 30 s |
| US Treasury yields table (13W, 5Y, 10Y, 30Y) | Yahoo Finance (^IRX/^FVX/^TNX/^TYX) | every 30 s |
| Currencies table (USD/INR, DXY, EUR/USD, GBP/USD, USD/JPY, USD/CNY) | Yahoo Finance, with ECB/Frankfurter reference fallback | every 30 s / 30 min |
| Fear & Greed gauge + 30-day history | CNN Business Fear & Greed feed | every 10 min |
| India VIX & CBOE VIX gauges | Yahoo Finance | every 30 s |
| Market breadth gauge & mood badge | computed from the live quotes | every 30 s |
| Ticker tape | TradingView widget (independent live redundancy) | continuous |

## Layout & design

- Each data block is a **3D tile**: layered gradients, bevel highlights,
  extruded keycap-style edges, accent glow, hover lift and per-row price-change
  flash (green/red).
- Tables sit **side by side on wide screens** (2-up for the big tables,
  3-up for the smaller ones) and **stack vertically on narrow screens**.
- Table columns: Instrument · Last · Change · Change % · 1-day sparkline trend.
- Each gauge/history block carries its own refresh-interval chip too.

## Resilience (no fake data)

1. Every feed is fetched **directly first**; if the browser is blocked by CORS,
   the page automatically retries through a chain of public CORS relays
   (`corsproxy.io` → `allorigins` → `codetabs` → Yahoo secondary host),
   remembering the last relay that worked per feed.
2. If every transport fails, the last known value is kept and flagged **amber
   (stale)**; rows that never received data show a neutral "awaiting live feed"
   state. No random/simulated numbers are ever displayed.
3. Pauses polling while the tab is hidden and resumes on focus.

## Not included (and why)

- **FII/DII cash flows** and **Nifty option-chain PCR** — NSE's endpoints
  require server-side cookies/auth and block browser access. To restore them,
  run a tiny backend that proxies `nseindia.com` and serve it from the same
  origin.

## Run locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy

The site is static — it is served from this branch via **GitHub Pages**
(Repo → Settings → Pages → Branch deploy). Pushing to the deployed branch
updates the live site automatically.

---
*Quotes may be delayed up to 15 minutes for some exchanges. Not investment advice.*
