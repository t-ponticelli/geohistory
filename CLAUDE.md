# CLAUDE.md — GeoHistory Project Context

> **For Claude Code:** This file is auto-loaded every session. Read this FIRST before doing anything in this repo.

## 🌍 What GeoHistory Is

A once-a-day history + geography trivia game (like Wordle for geography).
- **Live site:** https://playgeohistory.netlify.app
- **GitHub repo:** t-ponticelli/geohistory
- **Owner:** Trevor (tiponticelli@gmail.com)

Each day, players see a themed round of 5 clues. For each clue they read a riddle about a famous place and drop a pin on a world map. Closer pin = more points. After 5 questions they get a score, colored result dots, a streak, friends leaderboard, and a share card.

## 🛠️ Current Stack

- **Frontend:** Vanilla HTML/CSS/JS + React 18 via CDN for the Home screen (no build step)
- **Mapping:** MapLibre GL JS for the game map
- **Database/Auth:** Supabase
  - URL: `https://pmxmxfvtnosyyjkiwvim.supabase.co`
  - Anon key is hardcoded in HTML files
- **Hosting:** Netlify (free Starter plan, 1000 credits/month, every deploy costs ~45 credits)
- **Version control:** GitHub (push to `main` = auto-deploy on Netlify)

## 📁 Current File Structure

```
/
├── index.html          # Main game (~3,300 lines)
├── leaderboard.html    # Daily/weekly leaderboards
├── stats.html          # Personal stats page
├── admin.html          # Question + schedule management
├── auth-callback.html  # Magic link auth redirect handler
└── design_reference/   # Design handoff from professional designer
    └── design_handoff_geohistory/
        ├── README.md   # Full design system docs
        ├── styles.css  # Visual system (OKLCH colors, fonts, components)
        ├── *.jsx       # React component reference
        └── screens/    # PNG mockups of each screen
```

## 🎯 Critical Working Principles

These rules came directly from Trevor — adhere strictly:

1. **Flag issues BEFORE writing code.** Trevor prefers proactive problem identification upfront over mid-build discoveries.
2. **Simplicity over complexity.** Working and simple beats clever.
3. **Batch deploys.** Group changes to conserve Netlify build credits. Each push = ~45 credits. Test locally before pushing.
4. **Tester names.** Use "testtest" or "drewtest" for any test scoring data. Clean up via `DELETE FROM scores WHERE username = 'testtest';` later.
5. **Content review pattern.** When drafting question clues, watch for titles that reveal the answer prematurely.
6. **Communication style.** Trevor communicates tersely — short phrases, decisive. Match this cadence; avoid verbose explanations unless asked.

## ✅ What's Already Built (Don't Rebuild)

- **Auth system:** Username + localStorage only. Users pick a username on first visit (stored in `localStorage` + Supabase `users` table keyed by `client_id`). No Supabase Auth, no email, no magic links. Email/magic-link auth is planned for after the visual rewrite.
- **Practice mode:** "Play Past Games" feature with orange "PRACTICE MODE" badge
- **Scoring curve:** Universal (no domestic/international toggle)
  - 🎯 Elite (0-75 mi) → 100% to 92%
  - 🔥 Great (75-200 mi) → 92% to 80%
  - 👍 Good (200-600 mi) → 80% to 55%
  - 😅 Bad (600-3,500 mi) → 55% to 18%
  - 💀 Miss (3,500-15,000 mi) → 18% to 3% (soft floor, no zero)
- **Streak badge:** Gold pill with 🔥 + number, floats next to checkmark on end/already-played screens
- **Leaderboard:** Day-navigable, with Daily + Weekly tabs
- **Stats page:** Personal performance metrics
- **Split button:** [Leaderboard 🏆 | Stats 📊] on end screens
- **3-AM EST daily reset**

## 🚧 What's Planned (In Priority Order)

### Phase 1: VISUAL REWRITE (this is what we're starting now)
Port the professional design handoff from `design_reference/` into the live game. This is a big effort — see PLAN.md for the batched breakdown.

### Phase 2: GROUPS
- 8-group cap per user
- URL-based invite links (`/join?g=xxx`)
- Daily + weekly tabs on leaderboard (3rd tab section)
- Anonymous users CAN join groups
- Group name + invite link sharing only (no chat, no avatars)

### Phase 3: ADS
- After visual rewrite + groups ship
- Meta first, Reddit second
- Tracking via auth signups
- Defer all ad work until Phase 1 + 2 are stable

## 🎨 Design Decisions Already Made

- **Adopt design handoff visuals:** Yes (full rewrite, Path A)
- **React via CDN:** Yes (matches design package — script tags, no build step)
- **Keep backend logic:** Yes (Supabase, scoring, auth, practice mode all stay)
- **Adopt week strip:** Yes (replaces current ← → past-games browsing)
- **Replace practice mode UI:** Yes (week strip handles it naturally)
- **Friends = Groups:** Yes (reuse "Friends · today" UI for our Groups feature)
- **Display name on leaderboard:** Yes (NEVER show email anywhere)
- **Anonymous play:** Allowed with localStorage guest name (`player1234`)
- **Streak logic:** Show badge for any streak ≥2 days

## ⚠️ Things to NEVER Do

- Never expose email addresses publicly
- Never break the Supabase Auth flow (it works — don't refactor unless asked)
- Never commit changes without explicit approval from Trevor
- Never push to GitHub without testing locally first
- Never write code without first explaining the plan
- Never deploy multiple small changes when one batched deploy works

## 🧪 Local Testing Workflow

Before pushing to GitHub:
1. Start a local server: `python3 -m http.server 8000`
2. Open `http://localhost:8000` in Chrome
3. Test the actual flow (play through, sign in, navigate)
4. Use incognito mode + a `testXX` username to avoid polluting real data
5. Only push to GitHub when fully tested

## 📊 Supabase Tables (Reference)

- `questions` — question bank (id, title, clue, location, answer_lat, answer_lng, year, difficulty, theme, emoji, fact, etc.)
- `daily_rounds` — schedule (date, theme, question_ids[], status)
- `users` — user accounts (id linked to auth.uid(), email, display_name)
- `scores` — game results (user_id, username, date, theme, total_score, question_details JSON)

## 🎬 First Session Workflow

When Trevor starts a session, read this file + PLAN.md, then:
1. Confirm understanding by summarizing the current state in 3 bullets
2. Ask which batch we're working on
3. Wait for go-ahead before any code edits

## 📞 What to Ask Trevor About

- Anything that requires a design decision not already specified
- When you're unsure if something should be built into the existing flow vs. as a new pattern
- Before any database schema changes
- Before any major refactor that touches >3 files
