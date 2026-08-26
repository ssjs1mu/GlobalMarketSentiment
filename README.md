# 📊 Global Market Sentiment Tracker — Live

A single-page, zero-build, live-updating market sentiment dashboard.
Open `index.html` in any browser, or use the GitHub Pages deployment below.

**Live site:** https://ssjs1mu.github.io/GlobalMarketSentiment/

## What's live (auto-refreshing)

| Section | Source | Refresh |
|---|---|---|
| Indian markets (Nifty 50, Bank Nifty, Sensex, Midcap, Fin Services, IT, heavyweight stocks) | Yahoo Finance public quote API (`v7/finance/spark`, batched) | every 30 s |
| Global indices & futures (ES/NQ/YM/RTY, FTSE, DAX, CAC, Euro Stoxx, Nikkei, Hang Seng, Shanghai, ASX, KOSPI) | Yahoo Finance | every 30 s |
| Commodities (Gold, Silver, WTI, Brent, NatGas, Copper, Platinum, Bitcoin) | Yahoo Finance | every 30 s |
| US Treasury yields (13W, 5Y, 10Y, 30Y) | Yahoo Finance (^IRX/^FVX/^TNX/^TYX) | every 30 s |
| Currencies (USD/INR, DXY, EUR/USD, GBP/USD, USD/JPY, USD/CNY) | Yahoo Finance, with ECB/Frankfurter reference fallback | every 30 s / 30 min |
| Fear & Greed gauge + 30-day history | CNN Business Fear & Greed feed | every 10 min |
| India VIX & CBOE VIX gauges | Yahoo Finance | every 30 s |
| Market breadth gauge & mood badge | computed from the live quotes | every 30 s |
| Ticker tape | TradingView widget (independent live redundancy) | continuous |

## Resilience (no fake data)

1. Every feed is fetched **directly first**; if the browser is blocked by CORS,
   the page automatically retries through a chain of public CORS relays
   (`corsproxy.io` → `allorigins` → `codetabs` → Yahoo secondary host),
   remembering the last relay that worked per feed.
2. If every transport fails, the last known value is kept and flagged **amber
   (stale)**; cards that never received data show a neutral "awaiting live feed"
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
