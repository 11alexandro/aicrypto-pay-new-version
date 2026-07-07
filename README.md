# AICrypto Pay
A full-stack freelance escrow platform built on simulated Web3 payment rails where clients post jobs, funds are locked in escrow by milestone, and developers receive payouts only when work is approved.

---

## Live Demo
🔗 [View Live Project](aicrypto-pay.vercel.app) *(deploy link)*
No sign-in required use the mock wallet profiles to explore client and developer views.

---

## Why I built this
Most portfolio projects show a CRUD app with a database. I wanted to build something that reflects a real financial workflow specifically the kind of escrow logic that Web3 freelance platforms like Gitcoin and Dework use under the hood. The challenge was implementing a payment state machine where funds move through defined states (Draft → Funded → In Progress → Disputed → Released) and each milestone has its own independent escrow lifecycle.

---

## Features

**Escrow State Machine**
Every job moves through a strict status flow: `Draft → Funded → In Progress → Disputed → Released`. Each milestone has its own independent status (`Pending → Funded → Completed → Released`), meaning partial payouts are possible just like real milestone-based escrow contracts. Funds locked in a `Funded` milestone cannot be released without an explicit approval action, and disputed milestones are flagged for arbitration.

**Developer Balance Dashboard**
The home dashboard shows two distinct balance states that matter in any escrow system: Pending Balance (funds locked in active milestones not yet released) and Withdrawable Balance (funds from Released milestones available for withdrawal). A live solvency counter synced from the Node.js server via Socket.IO shows the platform's total active escrow volume ticking in real time.

**Real-Time Multi-Tab Sync via Socket.IO**
When a milestone is approved or a new job is posted, the update broadcasts instantly across all open browser tabs via Socket.IO WebSockets no page refresh required. This mirrors how real-time trading and payment platforms keep multiple sessions in sync.

**Explore Jobs & Search**
A filterable job board lets users search by title, client, or description, and filter by blockchain (Solana, Ethereum, Bitcoin) or category (Design, Auditing). Each job card shows escrow token, amount, and milestone progress at a glance.

**Dispute & Arbitration Flow**
Jobs can be flagged as Disputed, which triggers an arbitration panel in the job detail view. An AI-assisted resolution offer simulates the kind of smart contract oracle arbitration used in decentralized escrow protocols.

**Live Blockchain Activity Feed**
The server broadcasts simulated blockchain transactions every 10 seconds deposits, releases, multi-sig confirmations keeping the dashboard visually active and reflecting realistic escrow platform activity.

**Dark / Light Mode**
Full dark and light theme support across all views.

**Responsive Design**
Full mobile and tablet support with a collapsible sidebar drawer on small screens.

---

## Architecture

```
Client (Browser)
      │
      ▼
React Frontend (App.tsx)
  ├── HomeView — Developer balance dashboard + live feed
  ├── ExploreJobsView — Job board with search/filter
  ├── JobDetailsView — Milestone escrow actions + dispute panel
  └── Socket.IO Client
      │
      ▼ WebSocket (bi-directional)
      │
Node.js + Express Backend (server.ts)
  ├── Socket.IO Server
  │     ├── live_blockchain_activity (broadcast every 10s)
  │     ├── job_added_broadcast
  │     └── milestone_update_broadcast
  └── /api/health endpoint
```

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | React 18 + TypeScript | Component architecture and type safety |
| Styling | Tailwind CSS + Dark Mode | Utility-first responsive design |
| Icons | Lucide React | SVG icon library |
| Build Tool | Vite | Fast HMR and optimized builds |
| Backend | Node.js + Express | REST API + WebSocket server |
| Real-Time | Socket.IO | Bi-directional event broadcasting |
| Deployment | Vercel / Railway | Frontend + backend hosting |

---

## Escrow Logic Explained

The core concept is a **payment state machine** funds can only move forward through defined states, never backwards arbitrarily. Here's how it works in practice:

A client posts a job and funds the escrow by milestone. Each milestone holds a percentage of the total job value (e.g. 30% / 40% / 30%). The developer works on each milestone and submits for approval. The client reviews and either releases the milestone payment or raises a dispute.

A `Funded` milestone means money is locked the developer can't access it and the client can't take it back without a dispute resolution. A `Released` milestone means the developer's share is confirmed and moves to their Withdrawable Balance. A `Disputed` milestone freezes the funds and triggers the arbitration flow.

This is exactly how on-chain escrow contracts work on platforms like Gitcoin Grants or Superfluid the smart contract holds the funds and only releases them when the defined conditions are met. This project simulates that logic in a Node.js state machine without requiring actual blockchain transactions.

---

## Installation

```bash
git clone https://github.com/11alexandro/aicrypto-pay.git
cd aicrypto-pay
npm install
npm run dev
```

Open [aicrypto-pay.vercel.app](aicrypto-pay.vercel.app)

---

## Project Status
Portfolio project demonstrating full-stack escrow platform architecture. All transactions are simulated no real funds or blockchain interactions occur. The escrow state machine, milestone lifecycle, and real-time sync reflect the actual logic used in Web3 freelance payment protocols.

---

## Author
**Alex Valmyr** Full-Stack Developer focused on fintech, Web3 platforms, and payment infrastructure.
GitHub: [11alexandro](https://github.com/11alexandro)
