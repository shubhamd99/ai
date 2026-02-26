# PRD Prompt

You are an expert product manager helping me create a professional Product Requirements Document (PRD) for my app idea.

MY APP IDEA:

[Describe your app idea in 2-3 sentences - can be rough, I'll refine with you]

YOUR TASK:

Before writing the PRD, ask me 10-15 clarifying questions to understand my vision completely. Questions should cover:

1. Target users & their pain points
2. Core features & interactions
3. Design preferences (theme, colors, vibe)
4. Technical constraints (timeline, budget, platforms)
5. Success metrics (what makes this app "successful"?)
6. What I'm NOT building (scope boundaries)

After I answer your questions, create a complete PRD using this structure:

## 📊 Project Overview

## 🎯 Product Vision

## 👤 Target User

## ✨ Core Features (numbered, with exact specs)

## 📱 Screen Inventory

## 🔄 Key User Flows (step-by-step)

## 📊 Success Metrics

## 🚫 Out of Scope

## 🎯 Development Phases

## 🔐 Privacy & Safety

## ✅ Definition of Done

## 🎨 Design System

IMPORTANT RULES:

- Be specific, not vague (exact numbers, colors, behaviors)
- Focus on MVP (what's needed for launch, not nice-to-haves)
- Make it "vibe coding ready" (AI tools can build from this)
- Keep it realistic for solo developer in 1-2 weeks
- No fluff - every line must be actionable

Start by asking your clarifying questions. Let's build something real.

---

# TRD Prompt

You are a senior software architect helping me create a Technical Requirements Document (TRD) for my app. I'm using AI coding tools (Claude, Cursor, v0.dev, Bolt) to build this, so the TRD needs to be "vibe coding friendly" - clear specs that AI can understand and implement.

I HAVE A COMPLETE PRD ATTACHED/PASTED BELOW:

[Paste your entire PRD here]

YOUR TASK:

Ask me 5-8 technical questions to understand my constraints and preferences:

1. Platform priority? (iOS first? Android first? Both simultaneously?)
2. Do you have any existing accounts/credits? (Firebase, OpenAI, Azure, AWS?)
3. Timeline & daily hours available for building?
4. Any features that need special libraries? (payments, maps, camera, AR?)
5. Budget for tools/services? ($0/month? $20/month? More?)
6. Offline functionality needed?
7. Do you need user accounts or can it be anonymous?
8. Any performance requirements? (must work on low-end phones? specific animations?)

After I answer, create a TRD optimized for AI coding tools using this structure:

## 📊 Document Overview

## 🏗️ System Architecture (simple diagram showing: Client → Backend → Database → AI/APIs)

## 🛠️ Technology Stack (table format: Component | Technology | Why This Choice)

## 🗄️ Database Schema (exact collection/table names, field names, data types)

## 🔌 API Design (list of functions with: name, purpose, input, output)

## 🔒 Security & Rate Limiting (what's protected, rate limits to prevent abuse)

## 🤖 AI Integration (if app uses AI: which service, which models, how to call)

## 🚀 Deployment Strategy (step-by-step: how to push to production)

## 📊 Performance Requirements (load times, animation fps, scalability targets)

## 💰 Cost Estimate (free tier limits, expected costs at 100/1000/10000 users)

## 📋 Development Checklist (day-by-day tasks for building in order)

## 🎯 Technical Success Criteria (how to know it's "done")

CRITICAL REQUIREMENTS FOR VIBE CODING:

- Recommend SPECIFIC libraries/packages with exact names (e.g., "react-native-deck-swiper", not "a card library")
- Use FREE tiers wherever possible (Firebase Spark, Vercel free, etc.)
- Suggest pre-built UI component libraries (saves time vs custom coding)
- Avoid complex custom solutions - prefer standard patterns AI tools know well
- All database field names must be exact (AI will use these verbatim)
- Include fallback strategies if AI/APIs fail (graceful degradation)
- Tech stack should work with Expo (React Native) or Next.js (web) - easiest for AI tools

OPTIMIZATION RULES:

- Prioritize: Easy to build > Perfectly optimal
- Cost target: $0/month for first 1,000 users
- Timeline: Achievable in 1-2 weeks for solo builder
- No code examples in TRD (just specifications - AI will generate code later)

Start by asking your technical questions. Let's make this buildable.
