# Chiacon — Enterprise Website Redesign

<div align="center">

[![Live Site](https://img.shields.io/badge/Live%20Site-chiacon--website.vercel.app-00ff88?style=flat-square&logo=vercel&logoColor=black)](https://chiacon-website.vercel.app)
[![Next.js](https://img.shields.io/badge/Next.js-16.2.2-black?style=flat-square&logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?style=flat-square&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-v4-38bdf8?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Deployed on Vercel](https://img.shields.io/badge/Deployed-Vercel-black?style=flat-square&logo=vercel)](https://vercel.com)

**Full-stack Wix → Next.js migration for Chiacon Consulting India Pvt. Ltd.**
27+ pages · Dark/Light mode · Notion CMS blog · WebGL visuals · 99 commits · 4 weeks

</div>

---

## Screenshots

| | | |
|---|---|---|
| ![Hero Dark](./screenshots/hero-dark.png) | ![Services](./screenshots/services.png) | ![About](./screenshots/about.png) |
| Hero (Dark Mode) | AI Services Page | About Page |
| ![Hero Light](./screenshots/hero-light.png) | ![Solutions](./screenshots/solutions.png) | ![Contact](./screenshots/contact.png) |
| Hero (Light Mode) | Solutions Hub | Contact + Form |

---

## Background

Chiacon Consulting is an enterprise technology consulting firm based in Gurugram, India. Their services span RPA, Data & Analytics, Quality Assurance, and AI implementation — serving clients like Dabur, Motherson, UiPath, and B-Braun.

When I joined as Associate Consultant Intern (Mar 2026), their website was a Wix template. It worked, but it had real problems:

- No code ownership — every change required Wix's editor
- Generic template design that didn't reflect an enterprise consulting firm
- Limited SEO control — no structured data, no sitemap, no custom headers
- Brand positioning was stuck on "RPA & QA consulting" while the actual work had shifted heavily toward AI

The ask was to redesign and rebuild the website from scratch on a modern stack.

---

## My Approach — From Discovery to Delivery

### Step 1: Understanding the Real Problem

Before writing a single line of code, I spent time understanding what the website actually needed to *do* — not just look like.

Three things stood out from early conversations with the team:

1. **The brand needed repositioning, not just a redesign.** Chiacon was already doing AI-first work — Power Platform + AI, LLM-powered dashboards, transformation delivery. The Wix site called them an "RPA company." That was a credibility gap, not just a visual one.

2. **The marketing team needed independence.** If updating a blog post required a developer, it wouldn't get updated. The new site needed to let non-technical people publish content without touching code or triggering a deploy.

3. **SEO continuity was non-negotiable.** Years of link equity on the old Wix URLs couldn't just disappear. Every old URL had to redirect to a new one.

These three things shaped almost every technical decision I made.

### Step 2: Building the Roadmap Before Writing Code

I wrote a full project roadmap before touching the codebase — a 2,000+ word document covering:

- Complete sitemap and URL structure (27+ routes)
- Page-by-page content specifications
- Component library plan
- Design system tokens and rules
- Rendering strategy per page type (SSG vs ISR)
- Backend architecture (API routes, email, spam protection)
- SEO strategy (301 redirect map, Schema.org types per page)
- Security headers
- A 21-day execution plan broken into Week 1 (components), Week 2 (pages + API), Week 3 (polish + launch)

This felt like over-engineering before starting, but it paid off significantly. When I hit ambiguous decisions mid-build — "should this page be SSG or ISR?" — I already had the answer documented.

### Step 3: AI-First Rebrand (The Non-Technical Decision That Changed Everything)

The biggest decision I made wasn't technical — it was repositioning all four service pages away from their Wix names:

| Old (Wix) | New (Next.js) |
|---|---|
| Robotic Process Automation | AI-Powered Business Automation |
| Data & Analytics | AI-Ready Data Intelligence |
| Quality Assurance | AI Implementation & Integration |
| Project Catalysts | AI Transformation Delivery |

This wasn't cosmetic. I rewrote all the copy, restructured the page hierarchy, and made sure the service-to-solution mapping made sense architecturally. The goal was that a senior engineer or procurement person landing on any service page would understand exactly what Chiacon does and why it's relevant to them — without reading marketing fluff.

### Step 4: Design System First, Then Components, Then Pages

I followed a strict build order:

**Week 1 — Foundation:**
- Design tokens in Tailwind config (`ink`, `paper`, `navy`, `accent` — deliberately minimal)
- Atomic UI components (Button, Card, Badge, SectionHeading, Input, etc.)
- Layout shell (Navbar with dropdown, Footer, mobile menu with scroll lock)
- Shared utilities (AnimatedSection wrapper, Framer Motion presets, Zod schemas)

**Week 2 — Pages + Backend:**
- All 27+ pages composed from the component library
- API routes: contact form (Zod + Turnstile + Resend), blog view/like tracking (Upstash Redis)
- Notion CMS integration for live blog
- All SEO metadata, Schema.org JSON-LD, sitemap.ts, robots.ts
- 14-URL 301 redirect map in `next.config.ts`

**Week 3 — Polish + Launch:**
- Animation tuning (scroll trigger margins, stagger timing, Lenis smooth scroll)
- Lighthouse optimization (90+ target)
- Security headers (CSP, HSTS, X-Frame-Options)
- Cross-browser and responsive QA
- DNS cutover + Vercel production deploy

Building in this order meant that by the time I assembled pages, every primitive already worked exactly as intended. I never went back to fix a Button component because I'd already ironed it out in week 1.

### Step 5: Key Technical Decisions and Why

**Notion as CMS (not Sanity or Contentful)**

The marketing team already used Notion for drafts and meeting notes. Giving them a new CMS to learn would mean they'd never touch the blog. With Notion as the CMS, publishing a post is: write in Notion, change status to Published, done — no developer, no deploy. The integration is also resilient: if Notion auth fails at build time, the blog gracefully skips the fetch rather than breaking the entire build.

**Upstash Redis for blog analytics**

I wanted each blog post to show live view and like counts — real engagement signals that are meaningful for a B2B content strategy. Rather than installing Google Analytics (heavy, privacy-invasive) or a third-party tool (another vendor), I built it directly with Upstash Redis serverless functions. Two API routes, ~40 lines of code total, and it gives Chiacon exactly the data they need: per-post view counts and likes, fetched client-side after hydration so they don't affect SSG build performance.

**Custom GLSL Perlin-noise shader (not just Spline)**

Most of the hero backgrounds use Spline 3D scenes. But for the Power Platform + AI page, I needed something that looped perfectly, ran at 60fps with zero janky transitions, and responded to scroll. I wrote a custom GLSL Perlin-noise animated shader via `@react-three/fiber` instead. It's a few dozen lines of GLSL — smoother than any Spline scene and completely GPU-native.

**SSG by default, ISR only where live data matters**

Every static page (Homepage, About, Services, Solutions, Contact) is built at deploy time and cached at the CDN edge indefinitely. The blog and case studies use ISR with a 60-second revalidation window — fresh enough for real content updates, but still CDN-cached for performance. This distinction kept Lighthouse scores high across all pages.

**Enterprise design restraint over visual flair**

The design reference points I kept in mind: ThoughtWorks, Stripe, Palantir, Vercel. What those sites have in common — confident white space, typography doing the heavy lifting, motion that guides attention rather than showing off. I actively enforced rules in the design system: no gradients on hero backgrounds, no glassmorphism, no decorative icons that don't carry meaning, maximum one marquee on the entire site. Every visual decision had to pass the question: "does this make the content more credible, or just more busy?"

---

## Tech Stack

### Core Framework
| Technology | Version | Role |
|---|---|---|
| [Next.js](https://nextjs.org) | 16.2.2 | App Router · SSG/ISR · API routes · image optimization |
| [React](https://react.dev) | 19 | UI component layer |
| [TypeScript](https://typescriptlang.org) | 5.x | Type safety across all layers |
| [Tailwind CSS](https://tailwindcss.com) | v4 | Utility-first CSS with custom design tokens |

### Animations & 3D
| Technology | Role |
|---|---|
| [Framer Motion](https://framer.com/motion) v12 | Scroll-triggered reveals, page transitions, stagger |
| [Three.js](https://threejs.org) + [@react-three/fiber](https://docs.pmnd.rs/react-three-fiber) | 3D WebGL scenes in service heroes |
| [Spline](https://spline.design) | Interactive 3D backgrounds per service/solution page |
| Custom GLSL shaders | Perlin-noise animated background (Power Platform + AI hero) |
| [tsparticles](https://particles.js.org) | Particle layers for select sections |
| [Lottie](https://lottiefiles.com) | AI, automation, data, and tech animations (20+ `.lottie` files) |
| [next-themes](https://github.com/pacocoursey/next-themes) | Dark/light mode with zero flash on load |

### Backend & Integrations
| Technology | Role |
|---|---|
| [Notion API](https://developers.notion.com) | Headless CMS — live blog content |
| [Upstash Redis](https://upstash.com) | Serverless per-post view + like tracking |
| [Resend](https://resend.com) | Transactional email from contact form |
| [Zod](https://zod.dev) | Schema validation — client and server |
| [React Hook Form](https://react-hook-form.com) | Form state management |
| [Cloudflare Turnstile](https://cloudflare.com/products/turnstile) | Bot protection on contact form |

### Infrastructure
| | |
|---|---|
| **Hosting** | Vercel — SSG pages at CDN edge, ISR for live-data pages |
| **SEO** | Schema.org JSON-LD per page type, `sitemap.ts`, `robots.ts`, full OG/Twitter metadata |
| **Security** | CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy |
| **Redirects** | 14 legacy Wix URLs → new routes via permanent 301s in `next.config.ts` |

---

## Architecture

```
chiacon-website/
├── src/
│   ├── app/                          # Next.js App Router
│   │   ├── page.tsx                  # Homepage (SSG)
│   │   ├── about/page.tsx
│   │   ├── services/
│   │   │   ├── ai-powered-business-automation/page.tsx
│   │   │   ├── ai-ready-data-intelligence/page.tsx
│   │   │   ├── ai-implementation-integration/page.tsx
│   │   │   └── ai-transformation-delivery/page.tsx
│   │   ├── solutions/
│   │   │   ├── power-platform-ai/page.tsx
│   │   │   ├── ai-qdbs/page.tsx
│   │   │   ├── bravo-board/page.tsx
│   │   │   ├── expensify/page.tsx
│   │   │   └── concise-cv/page.tsx
│   │   ├── industries/
│   │   │   ├── page.tsx
│   │   │   └── [slug]/page.tsx       # generateStaticParams
│   │   ├── case-studies/
│   │   │   ├── page.tsx
│   │   │   └── [slug]/page.tsx
│   │   ├── blog/
│   │   │   ├── page.tsx              # Notion CMS listing (ISR 60s)
│   │   │   └── [slug]/page.tsx       # Full post (ISR 60s)
│   │   ├── api/
│   │   │   ├── contact/route.ts      # Zod → Turnstile → Resend
│   │   │   ├── blog/[slug]/view/route.ts   # Upstash Redis
│   │   │   └── blog/[slug]/like/route.ts   # Upstash Redis
│   │   ├── sitemap.ts
│   │   └── robots.ts
│   │
│   ├── components/
│   │   ├── layout/       # Navbar, MobileMenu, Footer, ScrollToTop
│   │   ├── sections/     # 30+ page-section components
│   │   ├── shared/       # AnimatedSection, LottieAnimation, ThemeProvider
│   │   └── ui/           # Button, Card, Badge, Input, Modal, Spinner…
│   │
│   ├── data/             # Typed TypeScript content (services, industries, team…)
│   └── lib/
│       ├── notion.ts     # Notion client + helpers
│       ├── blog-stats.ts # Upstash Redis helpers
│       ├── animations.ts # Framer Motion variant presets
│       ├── schemas.ts    # Zod schemas (contact form, newsletter)
│       └── constants.ts  # Nav structure, site config
│
└── public/
    ├── animations/       # Lottie files by category
    ├── icons/tech-stack/ # SVG tech icons
    └── images/           # Client logos, cert badges, abstract photography
```

### Rendering Strategy

| Page type | Strategy | Rationale |
|---|---|---|
| Homepage, About, Services, Solutions, Contact | SSG | Static content — CDN-cached indefinitely |
| Blog listing + individual posts | ISR · 60s revalidation | Notion content updates without redeploy |
| Case studies listing + detail | ISR · 60s revalidation | Content may update after publish |
| Industries `[slug]` | SSG + `generateStaticParams` | All slugs known at build time |
| API routes (contact, blog stats) | Serverless functions | On-demand, stateless |

---

## Pages (27+ routes)

| Route | Description |
|---|---|
| `/` | Hero · client logos · thesis · capabilities grid · selected work · credentials strip · CTA |
| `/about` | Origin story · journey timeline · team grid · certifications · life at Chiacon |
| `/services` | Services hub overview |
| `/services/ai-powered-business-automation` | Automation practice page |
| `/services/ai-ready-data-intelligence` | Data & analytics practice |
| `/services/ai-implementation-integration` | Integration & QA practice |
| `/services/ai-transformation-delivery` | Transformation delivery practice |
| `/solutions` | Solutions hub with metric strip + product cards |
| `/solutions/power-platform-ai` | Microsoft Power Platform + AI |
| `/solutions/ai-qdbs` | AI Quality Dashboards |
| `/solutions/bravo-board` | Employee recognition platform |
| `/solutions/expensify` | AI expense lifecycle management |
| `/solutions/concise-cv` | AI-assisted CV generation |
| `/industries` + `/industries/[slug]` | 8 verticals — IT/ITES, Healthcare, Automotive, Telecom, FMCG, Energy, Oil & Gas, Real Estate |
| `/case-studies` + `/case-studies/[slug]` | Problem → Approach → Results layout with metric counters |
| `/blog` + `/blog/[slug]` | Notion CMS — live count of views and likes per post |
| `/careers` | Culture section + open positions list |
| `/contact` | Split-layout form with Zod validation, Turnstile, Resend |
| `/faqs` | Accordion FAQ |
| `/privacy-policy` | Legal |

---

## Design System

Enterprise-restraint aesthetic. The benchmark was: "would ThoughtWorks or Stripe ship this?"

**Color tokens (deliberately minimal — 6 neutrals, 1 accent):**

| Token | Hex | Usage |
|---|---|---|
| `ink` | `#0A0A0A` | Primary text, headings |
| `ink-muted` | `#525252` | Secondary text, captions, metadata |
| `paper` | `#FAFAF7` | Warm off-white — primary page background |
| `surface` | `#F4F2EC` | Alternate section background (used sparingly) |
| `border` | `#E5E3DC` | Hairline dividers, input borders |
| `navy` | `#0B1F33` | Footer, dark section backgrounds |
| `accent` | `#1E5EFF` | **Single accent** — CTAs only. If it appears more than 3 times in a viewport, remove some. |

**Enforced design rules:**
- No gradients on backgrounds
- No glassmorphism or neumorphism
- No decorative icons that don't carry meaning
- Maximum one marquee on the entire site
- Scroll animations on sections, not on individual atoms
- `transition-colors` not `transition-all` (performance + no visual bugs)
- Cards use border-color shift on hover, not drop shadows

---

## Performance

Lighthouse scores on Vercel production:

| Page | Performance | Accessibility | Best Practices | SEO |
|---|---|---|---|---|
| Homepage | 94 | 96 | 100 | 100 |
| Service page (avg) | 91 | 95 | 100 | 100 |
| Blog post | 90 | 97 | 100 | 100 |
| Contact | 93 | 96 | 100 | 100 |

**How these were achieved:**
- All images through `next/image` (automatic WebP/AVIF, lazy loading, explicit dimensions to prevent CLS)
- Fonts via `next/font/google` — `display: swap`, preload only weights 400/600/700
- Dynamic imports for below-fold Framer Motion and Three.js components
- Tailwind CSS tree-shakes to < 12KB in production
- Heavy third-party scripts (GA4, Turnstile) loaded `strategy="afterInteractive"`

---

## What I Built

**Discovery & Planning**
- Conducted requirement gathering sessions, identified brand positioning gap (RPA → AI), proposed full rebrand strategy
- Wrote complete project roadmap and 21-day execution plan before writing any code
- Designed entire information architecture: sitemap, URL structure, page hierarchy, content outline

**Engineering**
- Architected and coded the entire codebase solo: ~30 section components, ~20 UI atoms, 15+ typed content files
- Notion API integration with resilient build-time fallback — blog publishing requires zero developer involvement
- Upstash Redis serverless analytics — per-post view and like counts without GA
- Custom GLSL Perlin-noise shader for WebGL hero background
- Full contact pipeline: React Hook Form + Zod (client + server) + Cloudflare Turnstile + Resend email delivery
- 14-URL 301 redirect map preserving full SEO equity from legacy Wix site
- Security headers: CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy

**SEO**
- Schema.org structured data (Organization, Service, BlogPosting, ContactPage) per page type
- Auto-generated `sitemap.ts` and `robots.ts`
- Unique title + description + OG + Twitter metadata on all 27+ pages
- Submitted sitemap to Google Search Console on launch day

---

## Internship Context

This project was built during my internship at **Chiacon Consulting India Pvt. Ltd.** (Mar – Jun 2026) as Associate Consultant Intern. The source code is **private** (client codebase). This repository is a showcase of the architecture, design decisions, and engineering approach.

Parallel work during the same internship:
- AI HR Screening Agent integrated with Microsoft Teams (automated resume parsing + conversational candidate evaluation)
- MCP Server connected to the company data warehouse for automated leadership insight delivery

---

## Links

- 🌐 **Live Site:** [chiacon-website.vercel.app](https://chiacon-website.vercel.app)
- 👤 **Portfolio:** [nishant7p.github.io](https://nishant7p.github.io)

---

*Built by [Nishant Tomer](https://nishant7p.github.io) · Associate Consultant Intern @ Chiacon · IIIT-Delhi CSAM 2023–27*
