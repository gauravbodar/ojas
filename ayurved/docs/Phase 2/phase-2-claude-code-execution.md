# Phase 2 — Foundation & Scaffolding
**Project:** OJAS (working name) — Charaka-Inspired Ayurveda Empire  
**Playbook source:** C:\AWS\agency-agents\strategy\playbooks\phase-2-foundation.md  
**Project root:** C:\AWS\ayurved\  
**Duration:** 3–5 days  
**Gate Keepers:** DevOps Automator + Evidence Collector  
**Stack:** Next.js 14 · Supabase · Vercel · Cloudflare · Resend · Stripe (Phase B)

---

## Pre-Conditions ✅

- [x] Phase 1 gate passed — all strategy documents complete
- [x] Founder decisions locked (see 06-founder-decisions-locked.md)
- [x] Tech stack confirmed: custom build, not SaaS wrappers
- [x] Lead vertical: education-first, Phase A = free
- [x] Geography: global from day 1

---

## Claude Code Execution Path

> Copy-paste these prompts into Claude Code in sequence.
> Working directory for all commands: `C:\AWS\ayurved\`

---

### STEP 0 — Project Bootstrap
**Run this first in Claude Code before anything else:**

```
You are the DevOps Automator and Senior Project Manager working on the OJAS Ayurveda project.

Project root: C:\AWS\ayurved\
Playbook: C:\AWS\agency-agents\strategy\playbooks\phase-2-foundation.md

Task: Bootstrap the project scaffold.

1. Create a new Next.js 14 project with App Router at C:\AWS\ayurved\
   - TypeScript: yes
   - Tailwind CSS: yes
   - ESLint: yes
   - src/ directory: yes
   - App Router: yes

2. Create this folder structure inside the project:
   src/
   ├── app/
   │   ├── (marketing)/     ← public pages
   │   │   ├── page.tsx     ← homepage
   │   │   ├── learn/       ← content library
   │   │   └── prakriti/    ← quiz
   │   ├── (dashboard)/     ← authenticated area
   │   │   └── my-profile/  ← dosha results
   │   └── api/
   │       ├── quiz/        ← quiz submission
   │       └── subscribe/   ← email capture
   ├── components/
   │   ├── ui/              ← design system primitives
   │   ├── quiz/            ← Prakriti quiz components
   │   ├── content/         ← article/blog components
   │   └── layout/          ← header, footer, nav
   ├── lib/
   │   ├── supabase/        ← db client + types
   │   ├── email/           ← Resend templates
   │   └── quiz/            ← dosha scoring logic
   ├── styles/
   │   └── design-tokens.css
   └── content/             ← MDX articles (Charaka library)

3. Install dependencies:
   - @supabase/supabase-js @supabase/ssr
   - resend
   - @mdx-js/react next-mdx-remote
   - framer-motion
   - zod
   - react-hook-form @hookform/resolvers

4. Create .env.local with placeholder keys:
   NEXT_PUBLIC_SUPABASE_URL=
   NEXT_PUBLIC_SUPABASE_ANON_KEY=
   SUPABASE_SERVICE_ROLE_KEY=
   RESEND_API_KEY=
   NEXT_PUBLIC_APP_URL=http://localhost:3000

5. Create README.md documenting the stack and folder structure.

Deliver: Working Next.js scaffold that runs on `npm run dev`
```

---

### STEP 1 — Design System (UX Architect)
**Run after Step 0:**

```
You are the UX Architect working on the OJAS Ayurveda project.

Brand direction: Modern, premium, global. Inspired by Charaka Saṁhitā philosophy.
NOT clinical white. NOT Instagram-pink. Think: ancient manuscripts, raw botanicals, terracotta, forest mornings.

Task: Implement the full design system at C:\AWS\ayurved\src\styles\design-tokens.css
and C:\AWS\ayurved\src\components\ui\

Design tokens to implement:

COLOR PALETTE:
- Forest green (primary): #1B4332 / light: #D1FAE5
- Terracotta (secondary): #C2410C / light: #FFEDD5
- Aged gold (accent): #92400E / light: #FEF3C7
- Warm cream (background): #FEFCE8
- Parchment (surface): #FDF8F0
- Charcoal (text): #1C1917
- Muted (secondary text): #78716C
- Border: #E7E5E4

TYPOGRAPHY:
- Display/Headings: Playfair Display (serif) — signals authority and age
- Body: Inter (sans-serif) — modern readability
- Mono: JetBrains Mono — for Sanskrit transliterations/codes

SPACING: 4px base unit, scale: 4, 8, 12, 16, 24, 32, 48, 64, 96, 128

COMPONENTS to build in src/components/ui/:
1. Button.tsx — primary (gold), secondary (outline), ghost variants
2. Card.tsx — parchment background, subtle terracotta border
3. Input.tsx — minimal, warm styling
4. Badge.tsx — for dosha type labels (Vata/Pitta/Kapha color coded)
5. Typography.tsx — H1-H6, Body, Caption components

Google Fonts to add to layout.tsx:
- Playfair Display: 400, 500, 700
- Inter: 400, 500

Deliver:
- src/styles/design-tokens.css (CSS custom properties)
- src/components/ui/ (all 5 components, TypeScript, no external UI library)
- Updated app/layout.tsx with fonts and design tokens loaded
```

---

### STEP 2 — Supabase Schema (Backend Architect)
**Run in parallel with Step 1:**

```
You are the Backend Architect working on the OJAS Ayurveda project.

Supabase project will be set up at: https://supabase.com
Local dev: use Supabase CLI

Task: Create the complete database schema as SQL migrations.
File location: C:\AWS\ayurved\supabase\migrations\

Tables required:

1. profiles
   - id (uuid, references auth.users, primary key)
   - email (text, not null)
   - full_name (text)
   - created_at (timestamptz, default now())
   - updated_at (timestamptz)

2. prakriti_assessments
   - id (uuid, primary key, default gen_random_uuid())
   - user_id (uuid, references profiles, nullable — allow anonymous)
   - session_id (text) — for anonymous tracking pre-signup
   - email (text) — captured at end of quiz
   - responses (jsonb) — full quiz answers
   - vata_score (integer)
   - pitta_score (integer)
   - kapha_score (integer)
   - primary_dosha (text) — 'vata' | 'pitta' | 'kapha'
   - secondary_dosha (text, nullable)
   - created_at (timestamptz, default now())

3. subscribers
   - id (uuid, primary key, default gen_random_uuid())
   - email (text, unique, not null)
   - first_name (text)
   - primary_dosha (text)
   - source (text) — 'quiz' | 'newsletter' | 'content'
   - tags (text[]) — for segmentation
   - subscribed_at (timestamptz, default now())
   - unsubscribed_at (timestamptz, nullable)

4. content_articles
   - id (uuid, primary key)
   - slug (text, unique, not null)
   - title (text)
   - branch (text) — one of 8 Charaka branches
   - dosha_relevance (text[]) — which doshas this helps
   - published_at (timestamptz)
   - view_count (integer, default 0)

5. email_events
   - id (uuid, primary key)
   - subscriber_id (uuid, references subscribers)
   - event_type (text) — 'sent' | 'opened' | 'clicked' | 'converted'
   - sequence_name (text)
   - created_at (timestamptz, default now())

Also create:
- Row Level Security policies for each table
- supabase/seed.sql — sample dosha content for dev
- src/lib/supabase/client.ts — browser client
- src/lib/supabase/server.ts — server client (for Server Components)
- src/lib/supabase/types.ts — TypeScript types generated from schema

Deliver: Complete migration SQL + Supabase client setup
```

---

### STEP 3 — Prakriti Quiz Engine (Core Feature)
**Run after Steps 1 + 2:**

```
You are the Frontend Developer + AI Engineer working on the OJAS Ayurveda project.

Task: Build the complete Prakriti assessment quiz.
This is the MOST IMPORTANT feature of Phase A — everything flows from it.

Location: C:\AWS\ayurved\src\

QUIZ LOGIC (src/lib/quiz/):

1. questions.ts — 12 questions across 4 domains:
   Domain 1: Physical constitution (3 questions)
     Q1: Body frame — Slender/Medium/Broad
     Q2: Skin type — Dry & rough / Warm & oily / Thick & cool
     Q3: Hair — Dry, thin, frizzy / Fine, oily, premature grey / Thick, lustrous, wavy
   
   Domain 2: Digestion & energy (3 questions)  
     Q4: Digestion — Irregular, variable / Strong, sharp, efficient / Slow, heavy, steady
     Q5: Appetite — Variable, forget meals / Strong, irritable if missed / Moderate, can skip meals
     Q6: Energy — Bursts of energy, tires quickly / Intense focused energy / Steady endurance
   
   Domain 3: Mind & sleep (3 questions)
     Q7: Sleep — Light, variable, vivid dreams / Moderate, can't sleep when stressed / Deep, heavy, hard to wake
     Q8: Memory — Learn fast, forget fast / Sharp, precise / Learn slowly, remember long
     Q9: Stress response — Anxiety, worry / Irritation, frustration / Withdrawal, comfort-seeking
   
   Domain 4: Seasonal & lifestyle (3 questions)
     Q10: Preferred climate — Warm / Cool / Any but dislike humidity
     Q11: Exercise — Enjoy light frequent movement / Intense, competitive / Slow, steady, can be resistant
     Q12: Daily rhythm — Variable schedule / Structured schedule / Like routine, slow to start

2. scoring.ts — Dosha scoring algorithm:
   Each answer maps to: { vata: number, pitta: number, kapha: number }
   Sum scores → normalize to percentages → primary (highest) + secondary (second highest if >25%)
   
   Return: { primary: 'vata'|'pitta'|'kapha', secondary: string|null, scores: {vata, pitta, kapha}, percentages: {...} }

3. results.ts — Result profiles for each dosha type (6 combinations):
   Pure Vata / Pure Pitta / Pure Kapha / Vata-Pitta / Pitta-Kapha / Vata-Kapha
   Each profile includes:
   - headline (one sentence describing this type)
   - strengths: string[]
   - currentChallenges: string[] (what happens when imbalanced)
   - charaka_insight: string (a paraphrased insight from Charaka Saṁhitā)
   - recommendations: { diet: string[], routine: string[], avoid: string[] }

QUIZ COMPONENTS (src/components/quiz/):

1. QuizContainer.tsx — manages state, progress, navigation
2. QuizQuestion.tsx — single question with 3 answer options, animated transitions
3. QuizProgress.tsx — step indicator (1–12)
4. QuizResults.tsx — results page with dosha breakdown, email capture form
5. DoshaChart.tsx — visual bar chart showing Vata/Pitta/Kapha percentages (no chart library, pure CSS)

APP ROUTE (src/app/(marketing)/prakriti/):
- page.tsx — landing page "Discover Your Prakriti"
- quiz/page.tsx — quiz experience
- results/page.tsx — results display

API ROUTE (src/app/api/quiz/submit/route.ts):
- Accepts: { responses, scores, email, firstName }
- Saves to Supabase prakriti_assessments + subscribers tables
- Triggers welcome email via Resend
- Returns: { success, doshaProfile }

DESIGN REQUIREMENTS:
- Full-screen, distraction-free quiz experience
- One question at a time, smooth slide transitions (framer-motion)
- Progress bar at top
- Green/terracotta color scheme from design tokens
- Mobile-first — must be flawless on phone
- No external quiz libraries — pure custom React

Deliver: Complete working quiz that saves to Supabase and triggers email
```

---

### STEP 4 — Email System (Growth Infrastructure)
**Run after Step 3:**

```
You are the Growth Hacker + Backend Architect working on the OJAS Ayurveda project.

Task: Build the email system using Resend.
Location: C:\AWS\ayurved\src\lib\email\ and src\app\api\

1. src/lib/email/templates/ — React Email templates:

   a. WelcomeDosha.tsx — triggered immediately after quiz
      Variables: { firstName, primaryDosha, doshaHeadline, topRecommendation }
      Content: "Your Prakriti result: [Dosha]. Here's what Charaka says this means."
      
   b. DoshaDay3.tsx — Day 3 in nurture sequence
      Variables: { firstName, primaryDosha, season }
      Content: "What Charaka says [Dosha] types should eat in [season]"
      
   c. DoshaDay5.tsx — Day 5
      Variables: { firstName, primaryDosha }
      Content: "The one morning ritual Charaka recommends for [Dosha] types"
      
   d. DoshaDay7.tsx — Day 7
      Content: "Why your gut is the centre of everything in Ayurveda"
      
   e. DoshaDay10.tsx — Day 10 (soft product intro — placeholder for Phase B)
      Content: "Your Charaka morning kit — curated for [Dosha] types"

2. src/lib/email/send.ts — Resend client wrapper:
   - sendWelcomeEmail(to, firstName, dosha)
   - scheduleNurtureSequence(to, firstName, dosha) — uses Resend's scheduled send

3. src/app/api/subscribe/route.ts — Newsletter signup (no quiz required)
   - Validates email
   - Adds to Supabase subscribers
   - Sends welcome (generic, no dosha)

All emails:
- From: hello@[domain] (placeholder)
- React Email components with inline styles
- Mobile responsive
- Brand colors: forest green + terracotta + parchment
- Footer: "Inspired by the Charaka Saṁhitā, written 2,000 years ago."
- Unsubscribe link required

Deliver: All 5 email templates + Resend integration + schedule logic
```

---

### STEP 5 — Content System (MDX + CMS)
**Run after Step 4:**

```
You are the Frontend Developer + Content Creator working on the OJAS Ayurveda project.

Task: Build the content library system for the Charaka free article library.
Location: C:\AWS\ayurved\src\

1. Content architecture (MDX files):
   src/content/learn/
   ├── what-is-ayurveda.mdx          ← Branch 1: Kaya Chikitsa intro
   ├── the-8-branches-of-charaka.mdx  ← Overview of all branches  
   ├── understanding-your-dosha.mdx   ← Prakriti explained
   ├── dinacharya-morning-routine.mdx ← Daily routine
   ├── the-charaka-approach-to-food.mdx ← Ahara (food as medicine)
   └── [18 more — stubs for now]

2. Each MDX file frontmatter schema:
   ---
   title: string
   description: string
   branch: 'kaya' | 'shalya' | 'shalakya' | 'kaumarabhritya' | 'agadatantra' | 'rasayana' | 'vajikarana' | 'bhutavidya'
   doshaRelevance: ('vata' | 'pitta' | 'kapha')[]
   readingTime: number (minutes)
   publishedAt: string (ISO date)
   relatedArticles: string[] (slugs)
   ---

3. Components for articles (src/components/content/):
   a. ArticleLayout.tsx — full article wrapper with sidebar
   b. ArticleHeader.tsx — title, branch badge, reading time, dosha tags
   c. ArticleSidebar.tsx — related articles + quiz CTA
   d. BranchBadge.tsx — colored badge for each of 8 branches
   e. DoshaTag.tsx — small Vata/Pitta/Kapha pill
   f. CharakaQuote.tsx — pull quote component with parchment styling
   g. QuizCTA.tsx — embedded CTA "Discover your Prakriti" (appears mid-article)

4. App routes:
   src/app/(marketing)/learn/
   ├── page.tsx — library index, filterable by branch/dosha
   └── [slug]/page.tsx — individual article

5. Write 3 full articles (not stubs) to validate the system:
   - "Understanding Your Dosha: What Charaka Actually Said" (1,200 words)
   - "Dinacharya: The Charaka Morning Routine Explained for Modern Life" (1,000 words)  
   - "The 8 Branches of Charaka Saṁhitā: A Modern Guide" (800 words)
   
   Writing style: Intelligent, educational, NO Sanskrit gatekeeping (always translate terms), cite Charaka as source, wellness language only (no medical claims).

6. Homepage (src/app/(marketing)/page.tsx):
   Sections:
   a. Hero — "The 2,000-year-old operating system for human health. Finally made practical."
   b. What is Charaka — brief philosophy intro
   c. Take the Prakriti quiz — prominent CTA
   d. Browse the library — 3 featured articles
   e. The 8 branches — visual grid
   f. Newsletter signup

Deliver: Working content system with MDX + 3 full articles + homepage
```

---

### STEP 6 — Deployment + CI/CD (DevOps)
**Run after Steps 1–5:**

```
You are the DevOps Automator working on the OJAS Ayurveda project.

Task: Set up deployment pipeline.
Project at: C:\AWS\ayurved\

1. Vercel configuration:
   Create vercel.json:
   - Framework: Next.js
   - Environment variables: all from .env.local
   - Regions: global edge (iad1 as primary)
   - Headers: security headers (CSP, HSTS, X-Frame-Options)

2. Cloudflare setup instructions (document only — manual step):
   - DNS: point domain to Vercel
   - Proxy: enable Cloudflare proxy
   - Cache rules: cache static assets, bypass for /api/*
   - Security: enable Bot Fight Mode, rate limiting on /api/quiz/submit

3. GitHub Actions CI (.github/workflows/ci.yml):
   Triggers: push to main, PR to main
   Steps:
   - Install dependencies (npm ci)
   - TypeScript check (npx tsc --noEmit)
   - ESLint check
   - Build check (npm run build)
   - If on main: auto-deploy to Vercel

4. Environment management:
   Create .env.example with all keys documented (no values)
   Create docs/environment-setup.md — step-by-step for new developers

5. Supabase CLI setup:
   Create supabase/.gitignore (exclude secrets)
   Document: npx supabase init, link, db push workflow

Deliver: vercel.json + GitHub Actions CI + environment docs
```

---

### STEP 7 — Evidence Collector Verification
**Run last — before Phase 2 gate:**

```
You are the Evidence Collector running Phase 2 gate verification for the OJAS project.

Verify and document each of these checkpoints:

1. [ ] npm run dev — application loads at localhost:3000 without errors
2. [ ] Homepage renders correctly with brand colors
3. [ ] Prakriti quiz navigates through all 12 questions
4. [ ] Quiz calculates dosha result correctly (test with all-Vata answers)
5. [ ] Quiz results page renders with email capture form
6. [ ] API route /api/quiz/submit returns 200 (test with Postman/curl)
7. [ ] Supabase table receives quiz submission
8. [ ] Email sends via Resend (check dashboard)
9. [ ] /learn page renders article library
10. [ ] Individual article page renders MDX correctly
11. [ ] Mobile responsive — quiz works on 375px width
12. [ ] npm run build — production build succeeds with no errors
13. [ ] TypeScript — tsc --noEmit passes clean
14. [ ] Vercel deployment succeeds

For each: note PASS/FAIL and any error messages.
Create: C:\AWS\ayurved\docs\phase-2-verification.md with full results.

Gate decision: PASS (all 14 green) → Phase 3 | FAIL → list blocking issues
```

---

## Parallel Execution Plan

```
Day 1:   [Step 0] Bootstrap  →  [Step 1] Design System (parallel)
                             →  [Step 2] Supabase Schema (parallel)

Day 2:   [Step 3] Quiz Engine  (depends on Step 1 + 2)

Day 3:   [Step 4] Email System  (depends on Step 3)
         [Step 5] Content System  (can parallel with Step 4)

Day 4:   [Step 6] Deployment
         Integration testing

Day 5:   [Step 7] Evidence Collector Gate
```

---

## Phase 2 Quality Gate

| # | Criterion | Status |
|---|-----------|--------|
| 1 | Next.js app builds without errors | ☐ |
| 2 | Design tokens applied globally | ☐ |
| 3 | Supabase schema deployed | ☐ |
| 4 | Quiz end-to-end working | ☐ |
| 5 | Email sends on quiz completion | ☐ |
| 6 | 3 articles render correctly | ☐ |
| 7 | Homepage live | ☐ |
| 8 | Mobile responsive (375px+) | ☐ |
| 9 | Vercel deployment live | ☐ |
| 10 | TypeScript clean | ☐ |

**Gate: 10/10 PASS → Phase 3 (Build)**

---

## File Map — What Gets Created

```
C:\AWS\ayurved\
├── src/
│   ├── app/
│   │   ├── (marketing)/
│   │   │   ├── page.tsx                    ← Homepage
│   │   │   ├── learn/page.tsx              ← Content library
│   │   │   ├── learn/[slug]/page.tsx       ← Article
│   │   │   └── prakriti/
│   │   │       ├── page.tsx                ← Quiz landing
│   │   │       ├── quiz/page.tsx           ← Quiz
│   │   │       └── results/page.tsx        ← Results
│   │   └── api/
│   │       ├── quiz/submit/route.ts
│   │       └── subscribe/route.ts
│   ├── components/
│   │   ├── ui/                             ← Design system
│   │   ├── quiz/                           ← Quiz components
│   │   ├── content/                        ← Article components
│   │   └── layout/                         ← Header, footer
│   ├── lib/
│   │   ├── supabase/                       ← DB client + types
│   │   ├── email/                          ← Resend + templates
│   │   └── quiz/                           ← Scoring logic
│   ├── styles/
│   │   └── design-tokens.css
│   └── content/learn/                      ← MDX articles
├── supabase/
│   └── migrations/                         ← SQL schema
├── .github/workflows/ci.yml
├── vercel.json
├── .env.local (gitignored)
├── .env.example
└── docs/
    ├── environment-setup.md
    └── phase-2-verification.md
```
