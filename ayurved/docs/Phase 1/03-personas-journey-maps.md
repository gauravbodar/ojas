# User Personas & Journey Architecture
**Agent:** UX Architect  
**Phase:** 1 — Strategy  
**Project:** Charaka Saṁhitā Ayurveda Empire  
**Date:** March 2026

---

## Primary Personas

---

### Persona 1 — "The Intelligent Seeker"
**Name:** Priya, 34  
**Location:** Sydney / London / Toronto  
**Occupation:** UX Designer / Marketing Manager / Consultant  
**Income:** $80K–$120K AUD/GBP equivalent

**Profile:**
- Deep into self-improvement: reads, podcasts, journals
- Has tried meditation apps, clean eating, yoga — but feels something is missing
- Skeptical of pseudoscience; wants depth and reasons, not vibes
- Reads Atomic Habits, follows functional medicine content
- Has heard of Ayurveda but thinks it's "complicated" or "for Indian people only"

**Core pain points:**
- Wellness feels fragmented — different apps, no coherent system
- Decision fatigue around food, supplements, sleep
- Wants to feel like she has a framework, not a collection of hacks

**What she wants from Charaka:**
- A system that makes sense, explained like an intelligent adult
- To understand *why* something works, not just *what* to do
- Products she can trust, with an origin story

**Acquisition channel:** Long-form content, podcast, Instagram saved posts, referrals

---

### Persona 2 — "The Diaspora Reconnector"  
**Name:** Arjun, 29  
**Location:** Melbourne / Birmingham / Toronto  
**Occupation:** Software Engineer / Doctor / Finance  
**Income:** $100K+

**Profile:**
- Indian heritage, grew up with turmeric milk and "dadi ke nuskhe" (grandma's remedies)
- Feels disconnected from roots; wants to reconnect on his terms, not as cultural obligation
- Highly educated; instinctively skeptical of his own culture's traditions (Western education conditioning)
- Wants credibility behind the practice — scientific angles, historical evidence

**Core pain points:**
- Shame-curiosity tension: "Is Ayurveda actually legit or just cultural nostalgia?"
- Wants to share it with Western colleagues without embarrassment
- Feels let down by "spiritual" branding around Indian wellness

**What he wants from Charaka:**
- Intellectual credibility — the philosophy grounded in reasoning
- Modern, non-embarrassing packaging he'd be happy to have on his desk
- A bridge between the heritage he half-knows and a system he can practice

**Acquisition channel:** Reddit, YouTube, LinkedIn, word-of-mouth from Priya-type friends

---

### Persona 3 — "The Burnt-Out Builder"
**Name:** James, 41  
**Location:** London / NYC / Singapore  
**Occupation:** Startup Founder / Senior Executive  
**Income:** $150K+

**Profile:**
- High-performer with health as a performance tool, not a spiritual practice
- Biohacking adjacent: tracks HRV, cold plunges, creatine stacks
- Body is starting to show cracks: gut issues, poor sleep, anxiety, inflammation
- Conventional medicine says "you're fine" but he knows he isn't
- Has money; doesn't have time to research

**Core pain points:**
- Wants a system, not a product menu
- Needs results he can measure (not feelings, but sleep quality, energy, focus)
- Corporate wellness programs feel generic and useless

**What he wants from Charaka:**
- A clear, personalized protocol: "Given your constitution, do this, not that"
- Premium products he doesn't have to research himself
- B2B: wants to bring this into his company wellness program

**Acquisition channel:** Podcast (Huberman-adjacent), LinkedIn thought leadership, B2B outreach

---

### Persona 4 — "The Wellness Professional"
**Name:** Sarah, 38  
**Location:** Byron Bay / Bali / Portland  
**Occupation:** Yoga teacher / Nutritionist / Health Coach  
**Income:** $60K–$80K

**Profile:**
- Already has some Ayurveda knowledge — knows doshas, has done Panchakarma
- Uses Ayurvedic principles with clients but lacks a systematic framework
- Wants to deepen her credential and offer something more robust
- Runs small retreats, wants to offer Ayurvedic protocols

**Core pain points:**
- The Ayurveda content she finds is either too basic or too Sanskrit-heavy
- Can't afford to do a full BAMS degree
- Wants a practitioner-level understanding she can offer clients

**What she wants from Charaka:**
- Advanced courses that treat her as a professional, not a beginner
- Practitioner B2B packages — products she can recommend and resell
- Community of peers at the same level

**Acquisition channel:** Instagram, wellness educator communities, referral from Priya

---

## Core User Journey Map

### Journey: Intelligent Seeker (Priya) → First Purchase

```
AWARENESS
   ↓
Finds a Charaka article via Google search ("Ayurvedic morning routine")
OR sees an Instagram Reel explaining "What your digestive type actually means"
OR hears a Charaka founder on a podcast
   ↓
INTEREST
   ↓
Reads 2–3 pieces of cornerstone content on site
Takes the free Prakriti assessment quiz
Receives personalised dosha results via email
   ↓
CONSIDERATION
   ↓
Email sequence — 5 emails over 10 days:
  Day 0: "Your Prakriti result: Vata-Pitta dominant. Here's what that means."
  Day 3: "What Charaka says Vata-Pitta types should eat in autumn"
  Day 5: "The one morning ritual Charaka recommends for your type"
  Day 7: "Why your gut is the centre of everything in Ayurveda"
  Day 10: "The Dinacharya Kit — designed for Vata-Pitta types. First order 15% off."
   ↓
CONVERSION
   ↓
First purchase: Dinacharya Starter Kit ($89 AUD / $59 USD)
   ↓
POST-PURCHASE
   ↓
Product arrives with a printed Charaka passage and usage guide
Day 3: email "How's your first week going?"
Day 7: invite to Discord community
Day 14: offer on Charaka Foundations Course ($197)
   ↓
RETENTION
   ↓
Monthly seasonal kit subscription
Course completion → ambassador referral program
Retreat event invitation
```

---

## UX Architecture — Digital Touchpoints

### Website Information Architecture

```
charaka.com
├── /learn
│   ├── /what-is-charaka         ← Free cornerstone content
│   ├── /the-8-branches          ← Free cornerstone content  
│   ├── /prakriti-quiz           ← Free, email-gated results
│   └── /courses                 ← Paid
│       ├── /foundations
│       ├── /skin-and-beauty
│       ├── /mind-and-sleep
│       └── /gut-and-digestion
├── /shop
│   ├── /dinacharya-kit          ← Hero product
│   ├── /seasonal-rituals        ← Subscription
│   ├── /skincare                ← Charaka Twak line
│   └── /tonics                  ← Rasayana line
├── /community                   ← Discord / Circle embed
├── /for-practitioners           ← Sarah persona
├── /for-business               ← James persona (B2B)
└── /about
    ├── /our-advisors            ← Vaidya credentials
    └── /the-text                ← Charaka Saṁhitā introduction
```

### AI Dosha Engine UX Flow (MVP)

```
Landing: "Discover your Prakriti in 5 minutes"
   ↓
12 questions across 4 domains:
  — Physical constitution (body type, digestion, skin)
  — Mental constitution (sleep, stress response, memory)
  — Seasonal sensitivity
  — Current imbalance indicators
   ↓
Results page: 
  — Primary dosha + secondary dosha
  — "What this means for you" — 4 personalised insights
  — 3 specific product recommendations
  — Email gate: "Get your full Prakriti report"
   ↓
Email capture → Nurture sequence → First purchase
```

---

## Design Principles

1. **Information density over blank space.** Our users are readers. Don't design for people who skim.
2. **One clear next step.** Every page has one CTA. No confused visitors.
3. **Education before commerce.** Product pages include 40% educational content.
4. **Sanskrit with translation.** Use terms from the Charaka Saṁhitā but always translate in brackets. Never hide the source.
5. **Mobile-first for discovery, desktop for depth.** Cornerstone articles need desktop layouts. Quiz and checkout — mobile flawless.
