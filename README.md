# Polymarket Edge Finder

**A full-stack market intelligence platform that surfaces mispriced prediction market contracts using LLM-driven probability estimation.**

*This repository is a project showcase. Source code is private; contact me for a walkthrough or demo.*

---

## The problem

Polymarket lists thousands of active prediction market contracts across politics, economics, sports, and current events. Some are priced efficiently. Many are not. Finding the mispriced ones by hand means reading the news, reading the market, forming a probability estimate, comparing it to the implied probability from the market price, and doing this repeatedly across too many markets to keep track of.

The interesting question is not whether inefficiencies exist. They obviously do. The interesting question is whether an automated system can surface them faster than a human scanning manually.

---

## The solution

A full-stack web application that:

1. **Ingests active Polymarket contracts** through the Polymarket Gamma API
2. **Generates independent probability estimates** using the Anthropic Claude API, prompted with the contract context and current information
3. **Computes the edge** between the LLM's probability estimate and the market's implied probability
4. **Surfaces contracts ranked by edge magnitude**, with the LLM's reasoning available for each one
5. **Refreshes continuously** so new markets and price changes are picked up without manual intervention

The output is not a trading signal; it's a shortlist of markets that appear mispriced and are worth a human closer look. The LLM does the tedious first-pass work; the human does the actual decision.

---

## Screenshots

<!-- Add screenshots to an /assets folder and uncomment: -->
<!-- <img src="assets/dashboard.png" width="100%" alt="Main dashboard showing ranked opportunities"/> -->
<!-- <img src="assets/detail-view.png" width="100%" alt="Detail view with LLM reasoning"/> -->

*Screenshots to be added.*

---

## Architecture

```
                          User (browser)
                                |
                                v
                        React frontend (TypeScript)
                                |
                                v
                         Express backend
                          |          |
                          v          v
                   Gamma API     Anthropic API
                   (Polymarket)     (Claude)
                          |          |
                          +----+-----+
                               |
                               v
                     Edge calculation & ranking
                               |
                               v
                       Return to frontend
```

**Design decisions:**

**Express as a proxy layer, not a datastore.** The backend proxies both APIs rather than caching or persisting market data. This keeps the architecture simple and the data fresh at the cost of higher API call volume, which is fine for a personal project.

**Frontend does the ranking, not the backend.** The backend returns the raw edge data; the frontend sorts and filters. Keeps the backend stateless and lets me iterate on ranking heuristics without redeploying the API layer.

**LLM as an analyst, not an oracle.** The Claude API generates a probability estimate with a reasoning trail, and the reasoning is surfaced to the user. The LLM's estimate is treated as one signal, not the answer. A user can disagree with its reasoning and dismiss the opportunity; this is by design.

---

## Why this project

Three motivations, in order of what I actually got out of it:

**Practical LLM integration.** I wanted to build something non-trivial with the Anthropic API where the LLM was load-bearing, not decorative. Probability estimation with reasoning output is exactly that: the LLM's judgment is the product, and getting the prompting and structured output right was most of the engineering work.

**End-to-end full-stack shipping.** From API design through UI polish, all built and deployed by me. The stack (React, Express, TypeScript) is unremarkable on purpose. The point was to prove I could ship the whole thing.

**Domain interest.** Prediction markets are one of the more interesting information aggregation mechanisms in existence, and getting closer to how they work by building on top of the data was worth the time regardless of the tool's output.

---

## Tech stack

- **Frontend:** React, TypeScript
- **Backend:** Express.js, Node.js
- **APIs:** Polymarket Gamma API (market data), Anthropic Claude API (probability estimation)
- **Deployment:** [describe your deployment target — Vercel, Fly.io, local, etc.]

---

## Status

Working prototype. Not deployed publicly. Available to demo on request.

---

*Personal project. Not affiliated with Polymarket or Anthropic. Not financial advice.*
