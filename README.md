# Chiacon — Enterprise Website Redesign

A full-stack website rebuild for an enterprise technology consulting firm — migrating from a template platform to a custom, production-grade Next.js application.

**Delivered solo · 4 weeks · 27+ pages · Deployed on Vercel**

### [→ View Live Site](https://chiacon-website.vercel.app)

`Next.js 16` · `TypeScript` · `Tailwind CSS` · `Framer Motion` · `Three.js` · `Notion CMS` · `Vercel`

---

## Screenshots

| | |
|---|---|
| ![Homepage — Dark](./screenshots/hero-dark.png) | ![Homepage — Light](./screenshots/hero-light.png) |
| **Homepage** — Dark mode | **Homepage** — Light mode |
| ![Services](./screenshots/services.png) | ![Solutions](./screenshots/solutions.png) |
| **Services** | **Solutions** |
| ![About](./screenshots/about.png) | ![Contact](./screenshots/contact.png) |
| **About** | **Contact** |

---

## Overview

The client was running on a template website builder — no code ownership, limited SEO control, and a generic design that didn't match the credibility of an enterprise consulting firm. I was brought on to redesign and rebuild the entire web presence on a modern, fully-owned stack.

The result is a fast, accessible, fully responsive website with a custom design system, dark/light mode, a self-serve content workflow for the marketing team, and a Lighthouse score of 90+ across every page.

---

## My Role

I owned the project end to end — from discovery and planning through design, engineering, and launch.

- **Discovery & strategy** — translated business goals into a complete site architecture and content plan
- **Design system** — built a minimal, enterprise-grade visual language from scratch
- **Engineering** — architected and coded the entire front end and back end solo
- **SEO & performance** — structured data, sitemap, redirects, and a 90+ Lighthouse target
- **Launch** — production deployment and go-live

---

## How I Approached It

### 1. Understood the problem before building

Before writing any code, I mapped what the website actually needed to *do*, not just how it should look. Three priorities shaped every decision that followed:

- **Credibility** — the design had to read as a serious enterprise presence
- **Independence** — the marketing team needed to publish content without a developer in the loop
- **Continuity** — no SEO value could be lost in the migration

### 2. Planned the whole build up front

I wrote a complete project roadmap — sitemap, page specs, component plan, rendering strategy, and a day-by-day execution schedule — before touching the codebase. When ambiguous decisions came up mid-build, the answer was already documented.

### 3. Built foundation first, pages last

I worked bottom-up: design tokens → reusable UI components → layout shell → full pages. By the time I assembled pages, every building block already worked exactly as intended — no rework.

### 4. Made deliberate engineering trade-offs

A few decisions I'm happy with:

- **Self-serve content** — wired the blog to a tool the marketing team already used daily, so publishing is a one-click action with zero developer involvement.
- **Lightweight live analytics** — added per-post engagement tracking with a tiny serverless layer instead of pulling in a heavy third-party analytics product.
- **Custom WebGL visuals** — hand-wrote a GPU shader for one hero background where I needed perfectly smooth, infinitely looping motion that off-the-shelf tools couldn't match.
- **Smart rendering** — static generation by default for speed, incremental regeneration only where live content demanded it. This kept performance scores high everywhere.

### 5. Design with restraint

I benchmarked the visual language against firms like ThoughtWorks, Stripe, and Vercel — confident whitespace, typography doing the heavy lifting, and motion that guides attention rather than showing off. Every visual element had to earn its place by making the content more credible, not just busier.

---

## Tech Stack

**Framework & Language**
Next.js 16 (App Router) · React 19 · TypeScript · Tailwind CSS

**Animation & 3D**
Framer Motion · Three.js / React Three Fiber · custom GLSL shaders · Lottie · light/dark theming

**Back end & Integrations**
Serverless API routes · headless CMS integration · serverless KV store · transactional email · schema validation · bot protection

**Infrastructure**
Vercel (edge CDN) · structured data & sitemap · security headers · SEO-preserving redirects

---

## Results

| Metric | Outcome |
|---|---|
| Pages shipped | 27+ |
| Lighthouse (all pages) | 90+ across Performance, Accessibility, Best Practices, SEO |
| Content workflow | Self-serve — no developer needed to publish |
| SEO migration | Zero link equity lost |
| Timeline | 4 weeks, solo |

---

## Context

Built during my internship at an enterprise technology consulting firm (2026). The source code is private (client-owned); this repository documents the architecture, design decisions, and engineering approach behind the project.

---

## Links

- **Live Site** — [chiacon-website.vercel.app](https://chiacon-website.vercel.app)
- **Portfolio** — [nishant7p.github.io](https://nishant7p.github.io)

---

*Nishant Tomer · IIIT-Delhi · B.Tech CSAM 2023–27*
