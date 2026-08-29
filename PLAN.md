# Block Blaster Multiplication

## Codex Implementation Plan

**Status:** Approved product and technical plan  
**Target player:** One 8-year-old child  
**Primary device:** iPad Safari  
**Hosting:** GitHub Pages  
**Scope:** A focused 2D block-blasting multiplication recall game for facts from `0 × 0` through `12 × 12`  
**Typical use:** One 5- or 10-minute mission at least twice per week

---

## 1. Codex directive

Build a polished MVP named **Block Blaster Multiplication** that follows this plan. The finished game must run as a static GitHub Pages site without a backend, account, build process, package manager, or database.

### Required deliverables

1. `index.html`
   - Contains all application HTML, CSS, and JavaScript.
   - Loads Phaser from a pinned CDN URL.
   - Contains no framework other than Phaser.
   - Uses no TypeScript.
   - Uses no external art, fonts, audio, APIs, analytics, ads, or tracking.
2. `README.md`
   - Brief local testing and GitHub Pages publishing instructions.
   - Explain that progress is stored only in the current browser.

Keep all game-specific code and styles inside `index.html`. Generate the visuals procedurally with Phaser Graphics, Canvas/WebGL primitives, CSS, and Web Audio so the repository does not need an asset folder.

### Non-negotiable constraints

- Use multiplication prompts from `0 × 0` through `12 × 12` inclusive.
- The game drills recall only. Do not teach arrays, skip counting, multiplication strategies, or conceptual lessons.
- Use a large touch keypad. Do not use multiple choice for ordinary practice.
- Do not display a per-question timer or countdown.
- Correct answers always cause visible block damage.
- Faster correct recall causes bonus damage, but a slow correct answer must still cause damage and must never fail the mission.
- An incorrect answer must trigger the correction flow specified in this document.
- Store one child’s data in `localStorage` on one browser.
- The parent dashboard is open and requires no PIN.
- Do not copy Roblox names, logos, avatars, sounds, maps, UI, or other protected assets. Use an original blocky arcade aesthetic.
- Do not add lives, health loss, streak loss, public rankings, advertisements, purchases, or penalties that can erase earned progress.

---

## 2. Product intent

The player likes first-person shooters, obbies, and soccer games. The MVP uses a **2D faux first-person target-gallery format**: a blaster appears at the bottom of the screen, block structures appear in the distance, and each correct multiplication answer fires a shot.

The educational activity must remain central to the game. A multiplication answer directly powers every shot rather than merely unlocking an unrelated game after a worksheet-like drill.

### Primary product goals

- Make short multiplication practice appealing enough to repeat voluntarily.
- Strengthen accurate retrieval of multiplication facts.
- Improve recall speed gradually without visible time pressure.
- Revisit weak facts during the same mission and across later days.
- Give the parent a clear view of long-term accuracy, response time, mastery, and improvement.

### Explicit non-goals for the MVP

- Multiplayer or online leaderboards.
- Cross-device synchronization.
- User accounts or cloud storage.
- Division questions, missing-factor questions, word problems, or conceptual instruction.
- A true first-person 3D world.
- Obby and soccer game modes.
- A level editor.
- Downloadable native iPad application.
- Server-side data or notifications.

Obby and soccer modes may be listed as possible later expansions, but they must not complicate the MVP architecture.

---

## 3. Research-derived design principles

### 3.1 Active retrieval

A classroom study of second-grade pupils found that retrieval practice using flashcards produced larger short-term and one-week improvements in multiplication fluency than restudying by chanting tables. Ordinary game prompts must therefore require the player to retrieve and enter an answer rather than recognize it from several choices.

Source: [Ophuis-Cox, Catrysse, and Camp, *Applied Cognitive Psychology* (2023)](https://onlinelibrary.wiley.com/doi/10.1002/acp.4141)

### 3.2 Short, repeated sessions

The Institute of Education Sciences recommends devoting about 10 minutes per intervention session to fluent retrieval of arithmetic facts. This game must offer 5- and 10-minute missions and make progress across sessions more important than one long session.

Source: [IES, Assisting Students Struggling with Mathematics](https://ies.ed.gov/ncee/wwc/practiceguide/2)

### 3.3 Spacing, mixed practice, and delayed review

IES guidance supports active quizzing, using quiz results to identify what needs more study, and retrieval distributed over time. Facts must therefore be mixed, scheduled adaptively, and tested again on later days before mastery is awarded.

Sources:

- [IES, Organizing Instruction and Study to Improve Student Learning](https://ies.ed.gov/ncee/wwc/practiceguide/1)
- [IES overview of retrieval, spacing, and interleaving](https://ies.ed.gov/learn/blog/lightbulb-moment-how-ies-sparks-research-teaching-and-practice)

### 3.4 Low-pressure timing

Homeschool forum discussions show mixed reactions to timed fact practice. Parents frequently value short sessions, adjustable pacing, visible stopping points, focused weak-fact practice, and the ability to avoid a stressful countdown. These reports are anecdotal rather than controlled research, but they support measuring response speed silently and using it only for positive game effects.

Representative discussions:

- [Reddit r/homeschool: Multiplication facts apps](https://www.reddit.com/r/homeschool/comments/13dta54/multiplication_facts_apps/)
- [Reddit r/homeschool: Multiplication memorization without tears](https://www.reddit.com/r/homeschool/comments/xbw170/how_to_memorize_the_multiplication_tables_without/)
- [Well-Trained Mind: Non-timed math fact apps and websites](https://forums.welltrainedmind.com/topic/696530-not-timed-math-fact-appswebsites/)
- [Reddit r/unschool: How unschoolers learn math](https://www.reddit.com/r/unschool/comments/1bvf1g1/how_do_unschoolers_learn_math/)

### 3.5 Useful patterns in existing web games

These products are references for design patterns, not assets or code to copy:

| Product or site | Pattern worth retaining | Pattern to avoid or modify |
|---|---|---|
| [XtraMath](https://xtramath.org/) | Short adaptive practice, progress tracking, fact-level focus | Do not expose a stressful question countdown |
| [Amplify Math Fluency Practice](https://fluency.amplify.com/) | Spaced repetition and targeted review | The MVP does not need visual teaching cards |
| [Multiplication.com](https://www.multiplication.com/games/multiplication-games) | Variety, coins, movement, and game themes | Do not separate the drill from the main action |
| [Timestables.com](https://www.timestables.com/multiplication-games/) | Practice through 12, table-specific and mixed modes, touch keypad | Do not make timed tests the default |
| Forum mentions of Number Run, Reflex-style programs, Times Tales, cards, and memory games | Clear stopping points, progress, replay, and focused practice | Avoid long unpausable sessions and unrelated reward loops |

---

## 4. Technical architecture

### 4.1 Hosting model

GitHub Pages can serve static HTML, CSS, and JavaScript directly from a repository. The site must work when `index.html` is published from the repository root.

Source: [GitHub Pages documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages)

### 4.2 Phaser version

Use **Phaser 3.90.0** from jsDelivr and pin the version exactly:

```html
<script src="https://cdn.jsdelivr.net/npm/phaser@3.90.0/dist/phaser.min.js"></script>
```

Phaser supports inclusion through a CDN, and 3.90.0 is the final Phaser 3 release listed in the official Phaser changelog. Pinning it avoids an unexpected major-version upgrade.

Sources:

- [Phaser setup documentation](https://docs.phaser.io/phaser/getting-started/set-up-dev-environment)
- [Official Phaser changelog](https://github.com/phaserjs/phaser/blob/master/CHANGELOG.md)

Do not silently upgrade to Phaser 4 during the MVP implementation.

### 4.3 Page structure

Use HTML overlays for menus, prompts, keypad controls, shop, settings, summaries, and the dashboard. Use Phaser only for the target scene, blaster, projectiles, particles, block destruction, and other animated effects.

Recommended top-level DOM structure:

```text
#app
  #home-screen
  #mission-screen
    #game-container
    #mission-hud
    #answer-panel
  #mission-summary
  #map-screen
  #shop-screen
  #dashboard-screen
  #settings-screen
  #modal-layer
  #toast-layer
```

### 4.4 JavaScript organization

Place application code in one final inline script and organize it with classes inside an IIFE to avoid leaking globals other than the Phaser library.

Recommended classes:

- `StorageService`
- `FactCatalog`
- `MasteryEngine`
- `AdaptiveScheduler`
- `SessionController`
- `EconomyService`
- `UnlockService`
- `DashboardRenderer`
- `AudioService`
- `UIController`
- `BlockBlasterScene extends Phaser.Scene`

Use a small `EventTarget`-based event bus so the DOM UI, mastery logic, and Phaser scene do not call deeply into one another.

### 4.5 Central configuration

Put all thresholds, prices, intervals, labels, and unlock requirements in one frozen `APP_CONFIG` object near the start of the script. Do not scatter magic numbers throughout the code.

---

## 5. Responsive iPad-first design

### 5.1 Viewport and layout

Use:

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

The page must support both iPad orientations.

- **Landscape:** Phaser game area on the left; problem and keypad in a fixed-width panel on the right.
- **Portrait:** Phaser game area on top; problem and keypad below.
- Do not require the player to rotate the device.
- Respect `env(safe-area-inset-*)` values.
- Apply `touch-action: none` only to the game interaction region and `touch-action: manipulation` to buttons.
- Prevent accidental text selection and page scrolling during a mission without disabling browser zoom on the entire site.

### 5.2 Touch targets

- Numeric buttons must be at least `60 × 60 CSS pixels`, preferably larger on standard iPads.
- Use large spacing between **Fire**, **Backspace**, and **I Don’t Know**.
- Limit entered answers to three digits because the largest answer is `144`.
- Support physical keyboard numbers, Backspace, Enter, and Escape as a secondary input method.

### 5.3 Phaser scaling

Use `Phaser.Scale.RESIZE` and reposition scene elements in response to the resize event. The scene must remain usable across common iPad viewport sizes and browser chrome changes.

### 5.4 Accessibility and comfort

- Include sound and reduced-motion toggles.
- Initialize Web Audio only after the user taps a start button, as required by mobile browsers.
- Never flash the screen rapidly.
- Use color plus text or icons to communicate mastery states.
- Avoid harsh error buzzers, red full-screen flashes, or shame-oriented language.
- Use system fonts only.

---

## 6. Main user flow

### 6.1 First launch

1. Show the title, a short description, and a **Start Recon Mission** button.
2. Allow an optional callsign; default to `Player`.
3. Explain in one sentence: “Answer multiplication facts to blast the block targets.”
4. Explain that there is no visible question timer.
5. Start a five-minute Recon Mission.

### 6.2 Later launches

The home screen contains:

- **Quick Mission — 5 minutes**
- **Full Mission — 10 minutes**
- **Choose Map**
- **Blaster Shop**
- **Progress**
- **Settings**

Also show:

- Current coin balance.
- Equipped blaster.
- Current weekly goal progress, such as `1 of 2 missions this week`.
- A compact overall mastery indicator.

Do not use a daily streak that can be lost. The weekly goal resets without a penalty.

### 6.3 Mission completion

At the end of active mission time:

1. Finish the current prompt or correction sequence.
2. Do not start another prompt.
3. Show a summary with:
   - First-try accuracy.
   - Questions answered.
   - Blocks destroyed.
   - Structures cleared.
   - Coins earned.
   - Newly fluent or mastered fact families.
   - A neutral improvement message based on recent sessions.
4. Offer **Play Again**, **Shop**, **Progress**, and **Home**.

---

## 7. Recon Mission

The Recon Mission is a disguised placement activity, not a formal test screen.

### 7.1 Duration

- Five active minutes.
- No visible countdown.
- Show only a calm segmented mission-progress bar.

### 7.2 Probe construction

Create a balanced randomized probe list of approximately 30 facts:

- 8 facts involving `0`, `1`, `2`, `5`, or `10`.
- 8 facts involving `3`, `4`, `11`, or `12`.
- 10 facts involving `6`, `7`, `8`, or `9`.
- 4 square or reverse-orientation checks.

Avoid exact duplicates. Include a few commutative reversals such as both `7 × 8` and `8 × 7`, but do not spend the whole mission testing mirrored prompts.

Ask as many probes as fit naturally in five active minutes. Unseen facts remain `new`.

### 7.3 Recon interpretation

Record every response through the normal attempt pipeline.

- Correct and relatively quick: mark as a strong placement signal.
- Correct but careful: mark as a tentative placement signal.
- Incorrect or **I Don’t Know**: mark as a weak placement signal.

No fact can become `mastered` during Recon. Later mixed and delayed retrieval is still required.

At the end, unlock **Neon Depot** in addition to the starting map so the first session produces a visible reward.

---

## 8. Core mission gameplay

### 8.1 Visual composition

Create an original faux first-person 2D scene:

- Blaster at the bottom center.
- Crosshair near the center.
- Block structure or wall as the target.
- Background appropriate to the selected map.
- Mild camera recoil and particles after a correct answer.
- Block fragments that disappear quickly to protect iPad performance.

Use procedural shapes and generated textures. Do not download images.

### 8.2 Question cycle

1. Choose the next fact through `AdaptiveScheduler`.
2. Render a large prompt such as `7 × 8`.
3. Clear and enable the answer field and keypad.
4. Start the hidden response clock only after the prompt is fully visible.
5. Let the player enter up to three digits and tap **FIRE**.
6. Evaluate only the first submitted answer as the retrieval attempt.
7. Run the correct or incorrect flow.
8. Save progress after the result is recorded.
9. Move to the next prompt after a short animation, normally under 700 ms.

Do not auto-submit when the typed value happens to equal the answer. Require **FIRE** or Enter so the UI does not reveal the answer length or correctness prematurely.

### 8.3 Correct-answer flow

- Stop the hidden response clock.
- Record a first-try correct retrieval.
- Calculate damage using the equipped blaster and hidden response-speed bonus.
- Fire one animated projectile.
- Destroy the calculated number of blocks around the impact point.
- Award coins.
- Show brief positive copy:
  - `Hit!`
  - `Power Shot!` for the top speed tier.
  - `Target Cracked!` when a structure is cleared.
- Continue without asking the player to dismiss a modal.

Do not display the numerical response time during missions.

### 8.4 Incorrect-answer flow

Use this exact behavior:

1. Stop the response clock.
2. Record the first submission as incorrect.
3. Do not remove lives, coins, mission progress, owned items, maps, or prior mastery credit.
4. Display neutral copy in this form:

   `Almost — 7 × 8 = 56`

5. Clear the answer input and require the player to enter `56` correctly once.
6. Correction entry is not a new retrieval attempt and is not used for mastery or response-speed statistics.
7. After correct re-entry, fire a small training shot that destroys exactly one block but awards no correction coins.
8. Put `7 × 8` into a retry queue to appear again after 3–5 other facts.
9. Schedule the fact for early review in the next session.
10. If the mission ends before the retry appears, retain it as a high-priority fact for the next mission.

If the player enters another wrong value during required correction, keep showing the correct equation and wait for the correct entry. Do not create multiple wrong attempts from the correction phase.

### 8.5 “I Don’t Know” behavior

Provide an **I Don’t Know** button. It triggers the same correction and retry flow as an incorrect first answer. Record it as `usedHelp: true` and not first-try correct. This gives the player an alternative to random guessing.

### 8.6 Mission clock

- Quick Mission: `5 × 60,000` ms of active time.
- Full Mission: `10 × 60,000` ms of active time.
- Pause active time while:
  - The document is hidden.
  - A menu, dashboard, or settings overlay is open.
  - The game is explicitly paused.
- Do not count hidden-tab time toward response time.
- Show a segmented progress bar rather than seconds remaining.
- When time expires, complete the current retrieval or correction before ending.

---

## 9. Hidden response speed and block damage

Response speed provides only a **positive bonus**. Correctness is always more important than speed.

### 9.1 Adjusted response time

Record raw response time with `performance.now()`.

Normalize slightly for the number of digits the player must tap:

```text
adjustedMs = max(500, rawMs - 250 × (answerDigitCount - 1))
```

Examples:

- One-digit answer: subtract `0` ms.
- Two-digit answer: subtract `250` ms.
- Three-digit answer: subtract `500` ms.

Do not use correction-entry time.

### 9.2 Personal baseline

At the start of each mission, freeze a personal baseline for that mission:

```text
qualifyingTimes = last 30 adjusted first-try-correct times
                  excluding facts containing a factor of 0 or 1

if fewer than 10 qualifying times exist:
    personalBaselineMs = 6500
else:
    personalBaselineMs = clamp(median(qualifyingTimes), 4000, 8000)
```

Recalculate only between missions so damage thresholds do not move while the player is playing.

### 9.3 Speed bonus

```text
ratio = adjustedMs / personalBaselineMs

ratio <= 0.60  -> speedBonus = 3
ratio <= 0.85  -> speedBonus = 2
ratio <= 1.15  -> speedBonus = 1
ratio >  1.15  -> speedBonus = 0
```

### 9.4 Damage calculation

```text
damage = equippedWeapon.baseDamage + speedBonus
```

- Minimum correct-answer damage is always `1`.
- Correction-entry training shot damage is exactly `1`.
- Damage removes that many one-hit blocks, limited by the blocks remaining in the current structure.
- Do not show labels such as “slow,” “too slow,” or “weak shot.”
- Top-tier speed may show `Power Shot!`; lower tiers simply show positive hit effects.

---

## 10. Block structures

### 10.1 Structure generation

Generate structures procedurally from grids of one-hit blocks.

MVP patterns:

- Solid wall.
- Stepped tower.
- Arch.
- Twin towers.
- Pyramid.
- Fort gate.

Each structure should contain approximately 24–50 blocks. Choose a size based on the equipped weapon so structures are neither cleared by one answer nor frustratingly long.

### 10.2 Destruction

- Select blocks nearest the crosshair impact point first.
- Expand outward to adjacent blocks until the damage count is reached.
- Animate destroyed blocks with short tweens and lightweight particles.
- Spawn a new structure after the current one is cleared.
- Cap active fragments and particles to protect iPad memory and GPU performance.

### 10.3 Structure clear bonus

When the last block is destroyed:

- Award `10` bonus coins.
- Play a short collapse effect.
- Increment lifetime structures cleared.
- Spawn a fresh target after no more than 900 ms.

---

## 11. Economy and blaster shop

### 11.1 Coin awards

For a normal first-try correct answer:

```text
coinsEarned = 2 + actualBlocksDestroyed
```

Additional awards:

- Structure clear: `+10` coins.
- Complete a Quick Mission: `+10` coins.
- Complete a Full Mission: `+20` coins.
- Correction entry: `0` coins.
- Later retry answered correctly on the first try: normal coin award.

Never subtract coins.

### 11.2 Blasters

Implement these five procedurally drawn models:

| ID | Name | Cost | Base damage | Notes |
|---|---|---:|---:|---|
| `pulse_mk1` | Pulse Blaster Mk I | 0 | 1 | Owned at start |
| `twin_pulse` | Twin Pulse | 180 | 2 | Two-barrel visual |
| `plasma_carbine` | Plasma Carbine | 450 | 3 | Larger energy bolt |
| `block_breaker` | Block Breaker | 900 | 4 | Heavy recoil effect |
| `nova_cannon` | Nova Cannon | 1600 | 5 | Strongest MVP model |

A purchased blaster remains permanently owned. The player may equip any owned model.

### 11.3 Shop requirements

- Show current coins.
- Show owned, equipped, locked, and affordable states clearly.
- Preview each blaster with its name and base damage.
- Require one confirmation tap before spending coins.
- Persist purchase and equipment immediately.
- Do not use randomized rewards, loot boxes, or consumables.

---

## 12. Maps and environments

Maps change the procedural background, target block styling, ambient effects, and structure mix. They do not change math difficulty.

| ID | Name | Unlock condition |
|---|---|---|
| `training_yard` | Training Yard | Available immediately |
| `neon_depot` | Neon Depot | Complete Recon Mission |
| `canyon_outpost` | Canyon Outpost | Reach 100 lifetime first-try correct answers |
| `sky_fortress` | Sky Fortress | Master 25 fact families |
| `lunar_bunker` | Lunar Bunker | Master 55 fact families |
| `core_citadel` | Core Citadel | Master 80 fact families |

There are 91 commutative fact families from `0 × 0` through `12 × 12` when mirrored forms such as `7 × 8` and `8 × 7` are grouped together.

The player may select any unlocked map from the home screen. Persist the selection.

---

## 13. Fact model: 169 prompts and 91 families

The game must represent all 169 ordered prompts:

```text
0 × 0 through 0 × 12
1 × 0 through 1 × 12
...
12 × 0 through 12 × 12
```

Also group commutative pairs into 91 fact families:

```text
familyKey = `${min(a,b)}x${max(a,b)}`
orderedKey = `${a}x${b}`
```

Examples:

- `7 × 8` ordered key: `7x8`.
- `8 × 7` ordered key: `8x7`.
- Both share family key `7x8`.

Track prompt-orientation evidence separately, but calculate the final mastery state at family level. A non-square family cannot become fluent or mastered until both orientations have been retrieved correctly.

The 13 × 13 dashboard grid mirrors the family state into both symmetric cells while retaining orientation-specific statistics in the detail view.

---

## 14. Mastery model

### 14.1 States

Each fact family has one of these states:

1. `new`
2. `learning`
3. `developing`
4. `fluent`
5. `mastered`

`reviewDue` is a computed flag, not a separate state.

### 14.2 Fluency target

At the end of each mission, calculate the hidden target used for future mastery decisions:

```text
fluencyTargetMs = clamp(personalBaselineMs × 0.90, 3500, 6500)
```

Store the target used for each session. Do not show this threshold to the child. The dashboard may show actual median response time without presenting the threshold as a deadline.

### 14.3 Evidence rules

A **retrieval attempt** is only the first submitted answer to a prompt. Required correction entry does not count as retrieval evidence.

A **delayed review pass** is a first-try correct answer when:

- The family is due for review.
- At least six calendar days have passed since the qualifying earlier successful retrieval used to schedule the review.

### 14.4 State transitions

#### `new` → `learning`

Transition after the first retrieval attempt, regardless of result.

#### `learning` → `developing`

Require all of the following:

- At least 3 lifetime first-try correct answers in the family.
- At least 3 correct among the last 4 retrieval attempts.
- For a non-square family, each orientation has appeared at least once.

#### `developing` → `fluent`

Require all of the following:

- At least 5 lifetime first-try correct answers in the family.
- At least 5 correct among the last 6 retrieval attempts.
- Median adjusted response time of the last 3 first-try correct answers is at or below `fluencyTargetMs`.
- Evidence comes from at least 2 separate sessions.
- For a non-square family, each orientation has at least 1 first-try correct answer.

#### `fluent` → `mastered`

Require all of the following:

- At least 7 lifetime first-try correct answers in the family.
- At least 5 correct among the last 6 retrieval attempts.
- Median adjusted response time of the last 3 first-try correct answers is at or below `fluencyTargetMs`.
- First-try correct evidence exists on at least 2 distinct calendar dates.
- At least 1 delayed review pass occurred after a gap of 6 or more calendar days.
- For a non-square family, each orientation has at least 2 first-try correct answers.

A family cannot become mastered in one day.

### 14.5 Response to errors

On an incorrect first attempt or **I Don’t Know**:

- Reset the review interval to one day.
- Schedule a same-session retry after 3–5 intervening prompts.
- Give the family a high priority in the next mission.
- If currently `mastered`, downgrade only to `fluent`.
- If currently `fluent`, downgrade to `developing` only when at least 2 of the last 4 retrieval attempts are incorrect.
- If currently `developing`, downgrade to `learning` only when at least 2 of the last 4 retrieval attempts are incorrect.
- Never erase lifetime statistics.

### 14.6 Review intervals

After a first-try correct retrieval, schedule the next due date according to state:

| State after evaluation | Next interval |
|---|---:|
| `learning` | 1 day |
| `developing` | 3 days |
| `fluent` | 7 days |
| `mastered`, first maintenance pass | 14 days |
| `mastered`, second maintenance pass | 30 days |
| `mastered`, later maintenance passes | 60 days |

If the player does not practice on the exact due date, the family remains overdue until it is seen.

---

## 15. Adaptive question scheduler

### 15.1 Priorities

Use this order:

1. Same-session retry facts whose 3–5-question gap has elapsed.
2. Overdue reviews.
3. Recently incorrect or help-used facts.
4. `learning` families.
5. `developing` families.
6. `fluent` families.
7. `mastered` maintenance facts.
8. New facts.

### 15.2 New-fact limits

- Quick Mission: introduce no more than 2 new families.
- Full Mission: introduce no more than 4 new families.
- Recon Mission is exempt.
- New families must not exceed 20% of ordinary mission prompts.

### 15.3 Normal weighted selection

When no retry is forced, use approximately:

- 35% overdue or review-due families.
- 35% weak, recently wrong, or `learning` families.
- 20% `developing` or `fluent` families.
- 10% mastered maintenance or easy confidence-building families.

Treat these as weights, not rigid quotas. Redistribute unavailable weight among remaining groups.

### 15.4 Focus set

At mission start, build a focus set containing:

- Up to 6 weak or due families.
- Up to the new-family limit.
- 2 fluent or mastered warm-up families.

Prefer facts from the focus set while still mixing in cumulative review.

### 15.5 Orientation selection

For a non-square family:

- Prefer the orientation with fewer first-try correct answers.
- Do not use the same ordered prompt twice in a row.
- Do not let one orientation exceed the other by more than two retrieval attempts unless correcting a weakness.

### 15.6 Repetition controls

- Exact same prompt must normally have at least 3 intervening prompts.
- Same-session error retry must appear after 3–5 intervening prompts.
- Avoid more than 3 consecutive prompts sharing the same first factor.
- Once `0` and `1` families are developing or better, limit them to roughly 10% of ordinary prompts unless due or recently missed.
- Do not construct long table-by-table runs in the default adaptive mission.

### 15.7 Selection pseudocode

```text
nextPrompt():
    if an eligible retry exists:
        return retry with the longest wait

    candidates = all families not blocked by repetition rules
    bucket each candidate by due/weak/learning/developing/fluent/mastered/new
    remove new candidates when the mission's new-family limit is reached
    assign base weight from bucket
    multiply weight for focus-set membership
    multiply weight for recent errors or overdue age
    reduce weight for recently shown families
    choose one family using weighted random selection
    choose the less-practiced orientation
    return ordered prompt
```

Use a seeded random helper within a session so behavior is reproducible during tests.

---

## 16. Local data model

Use one key:

```text
blockBlasterMultiplication.v1
```

Recommended shape:

```js
{
  schemaVersion: 1,
  profile: {
    callsign: "Player",
    createdAt: "ISO timestamp",
    weeklyGoal: 2,
    selectedWeaponId: "pulse_mk1",
    selectedMapId: "training_yard"
  },
  settings: {
    soundEnabled: true,
    reducedMotion: false
  },
  economy: {
    coins: 0,
    lifetimeCoins: 0,
    blocksDestroyed: 0,
    structuresDestroyed: 0,
    lifetimeFirstTryCorrect: 0
  },
  unlocks: {
    ownedWeapons: ["pulse_mk1"],
    unlockedMaps: ["training_yard"]
  },
  orderedFacts: {
    "7x8": {
      attempts: 0,
      firstTryCorrect: 0,
      totalWrong: 0,
      helpCount: 0,
      lastSeenAt: null,
      recent: []
    }
  },
  factFamilies: {
    "7x8": {
      state: "new",
      dueAt: null,
      reviewIntervalDays: 0,
      maintenancePasses: 0,
      delayedReviewsPassed: 0,
      correctDates: [],
      sessionIdsWithCorrect: [],
      recent: [],
      placementSignal: null
    }
  },
  sessions: [],
  dailyAggregates: {},
  activeSession: null
}
```

### 16.1 Recent attempt entry

Store only the most recent 12 retrieval attempts per ordered fact and per family:

```js
{
  at: "ISO timestamp",
  sessionId: "uuid-like string",
  a: 7,
  b: 8,
  correct: true,
  usedHelp: false,
  rawMs: 4200,
  adjustedMs: 3950,
  wasDueReview: false,
  source: "adaptive|retry|recon|maintenance"
}
```

### 16.2 Session summary

```js
{
  id: "session id",
  type: "recon|quick|full",
  mapId: "training_yard",
  weaponId: "pulse_mk1",
  startedAt: "ISO timestamp",
  endedAt: "ISO timestamp",
  activeMs: 300000,
  retrievalAttempts: 34,
  firstTryCorrect: 29,
  firstTryWrong: 5,
  accuracy: 0.8529,
  medianAdjustedMs: 4800,
  baselineMs: 6100,
  fluencyTargetMs: 5490,
  blocksDestroyed: 112,
  structuresDestroyed: 3,
  coinsEarned: 154,
  newlyFluentFamilies: ["6x7"],
  newlyMasteredFamilies: ["3x8"]
}
```

Retain at most the latest 200 session summaries and up to 365 daily aggregates. This is sufficient for long-term trends without unbounded `localStorage` growth.

### 16.3 Persistence rules

- Save after every completed retrieval attempt.
- Save after every purchase, equipment change, setting change, and mission end.
- Wrap parsing and saving in `try/catch`.
- Validate `schemaVersion` and essential fields before use.
- On malformed data, preserve the raw value under a timestamped recovery key before initializing clean data.
- Do not send any progress information over the network.

### 16.4 Interrupted-session recovery

Update `activeSession` after each prompt. On reload:

- If an active mission is less than 24 hours old, offer **Resume Mission** or **End Mission**.
- Resume with the saved active time, baseline, map, weapon, retry queue, and scheduler seed.
- If it is older than 24 hours, safely finalize it as interrupted without a completion coin bonus.

---

## 17. Parent progress dashboard

The dashboard is accessible from the home screen without a PIN.

### 17.1 Summary cards

Show:

- Fact families mastered: `X / 91`.
- Prompt orientations practiced: `X / 169`.
- Overall skill score.
- First-try accuracy across all sessions.
- First-try accuracy for the last 5 sessions.
- Median adjusted response time across all sessions.
- Median adjusted response time for the last 5 sessions.
- Total completed sessions.
- Total active practice time.
- Missions completed this week against the goal of 2.

### 17.2 Overall skill score

Use a gradual score so progress is visible before mastery:

| Family state | Weight |
|---|---:|
| `new` | 0.00 |
| `learning` | 0.25 |
| `developing` | 0.50 |
| `fluent` | 0.75 |
| `mastered` | 1.00 |

```text
overallSkillScore = sum(stateWeight for all 91 families) / 91 × 100
```

Show both this score and the stricter mastered count. Do not call the gradual score “percent mastered.”

### 17.3 Improvement comparison

When at least 10 completed sessions exist, compare the most recent 5 with the previous 5:

- Accuracy delta in percentage points.
- Median adjusted response-time delta.
- Increase in mastered families.

Use neutral labels:

- `Improving`
- `Holding steady`
- `Needs more data`
- `Accuracy dipped recently; weak facts are being reviewed`

Do not label the child as behind, failing, slow, or below grade level.

### 17.4 Trend charts

Create lightweight inline SVG charts without another library:

1. Accuracy by session.
2. Median adjusted response time by session.
3. Mastered-family count by session.

Show the last 20 completed sessions by default. Include accessible text summaries below each chart.

### 17.5 13 × 13 mastery grid

- Rows and columns labeled `0` through `12`.
- Each cell represents one ordered prompt.
- Symmetric cells show the same family mastery state.
- Add a small marker when that exact orientation has insufficient practice.
- Cell tap opens detail:
  - State.
  - Ordered-prompt attempts and accuracy.
  - Family attempts and accuracy.
  - Median response time.
  - Last practiced date.
  - Next review date.
  - Whether delayed retention has been confirmed.

Use color plus an icon or letter so the grid remains interpretable without color alone.

### 17.6 Weak-fact list

Show up to 10 families ranked by:

1. Recent incorrect answers.
2. Help use.
3. Overdue review age.
4. Slow median relative to the hidden target.
5. Low state.

Display both orientations for non-square families, such as `7 × 8 / 8 × 7`.

### 17.7 Session history

Show the latest 20 sessions with:

- Date.
- Mission type.
- Duration.
- Retrieval attempts.
- First-try accuracy.
- Median response time.
- Blocks destroyed.
- Coins earned.
- Newly mastered families.

### 17.8 Dashboard controls

MVP controls:

- Close dashboard.
- Reset all progress, protected by a two-step confirmation that requires typing `RESET`.

Optional P1 controls:

- Export all progress as a JSON file.
- Import a previously exported JSON backup after schema validation and explicit confirmation.

Do not delay the MVP for export/import.

---

## 18. Audio and visual feedback

### 18.1 Audio

Generate short effects with Web Audio oscillators and gain envelopes:

- Menu tap.
- Blaster shot.
- Power shot.
- Block break.
- Structure collapse.
- Coin award.

Do not use an unpleasant wrong-answer buzzer. A wrong answer may use a quiet neutral tone or no sound.

### 18.2 Reduced motion

When enabled:

- Disable camera shake.
- Reduce particle counts.
- Replace large recoil and block scatter with short fades.
- Keep damage and progress visible.

### 18.3 Performance budgets

- Target smooth play on contemporary iPads in Safari.
- Cap simultaneous particles at approximately 80.
- Pool block and projectile objects where practical.
- Avoid expensive per-frame DOM layout reads.
- Pause Phaser when the document is hidden.
- Remove event listeners and timers when leaving a screen.

---

## 19. State machine

Use an explicit application state enum:

```text
BOOT
HOME
RECON_INTRO
MISSION_ACTIVE
MISSION_PAUSED
CORRECTION_REQUIRED
MISSION_ENDING
MISSION_SUMMARY
MAP_SELECT
SHOP
DASHBOARD
SETTINGS
```

Important rules:

- `CORRECTION_REQUIRED` prevents the scheduler from advancing.
- Opening an overlay from an active mission moves to `MISSION_PAUSED` and pauses both active time and response time.
- Only `MISSION_ACTIVE` accepts a new first answer.
- Screen transitions must be idempotent and must not attach duplicate listeners.

---

## 20. Error handling

- If Phaser fails to load from the CDN, show a plain HTML error with a retry button and the message that an internet connection is needed to load the game engine.
- If WebGL creation fails, allow Phaser to fall back to Canvas through `Phaser.AUTO`.
- If `localStorage` is unavailable, show a warning and allow temporary play without claiming progress will persist.
- Never discard malformed stored data before placing a recovery copy in another local key.
- Guard against double taps on **FIRE**, purchases, and navigation buttons.
- Ignore an empty answer.
- Reject values above three digits without submitting.

---

## 21. Testing strategy without a build system

### 21.1 Built-in self-test mode

When the URL contains `?test=1`, do not start the normal game. Run deterministic tests in the browser and render a PASS/FAIL report.

Test pure functions for:

- Generation of all 169 ordered prompts.
- Generation of all 91 commutative families.
- Product correctness from `0 × 0` through `12 × 12`.
- Adjusted response-time calculation.
- Baseline and fluency-target clamping.
- Damage tiers.
- State transitions.
- Delayed-review requirements.
- Review interval changes.
- Scheduler repetition limits.
- Both orientations required for non-square mastery.
- Storage serialization and migration validation.
- Improvement comparison calculations.

Use a fixed test clock and seeded random number generator.

### 21.2 Manual test checklist

#### GitHub Pages

- Root URL loads with no path assumptions.
- Reload preserves progress.
- HTTPS causes no mixed-content warnings.
- No console errors.

#### iPad Safari

- Landscape and portrait are both usable.
- Rotation does not reset a question.
- Keypad buttons do not trigger page scrolling or unwanted zoom.
- Audio begins after a user gesture.
- Returning from another tab does not inflate response time.
- A page reload can resume an active mission.

#### Learning behavior

- No per-question countdown is visible.
- A correct slow answer always damages at least one block.
- A faster answer adds damage according to the frozen mission baseline.
- One wrong answer immediately enters correction mode.
- Correction entry does not count as successful retrieval.
- Missed facts return after 3–5 other prompts.
- A fact cannot become mastered in one session or one day.
- A delayed review of at least six days is required for mastery.
- Both `7 × 8` and `8 × 7` must be retrieved correctly before their family is mastered.
- Mastered facts remain in occasional maintenance review.

#### Dashboard

- Totals match session data.
- Recent-5 versus previous-5 improvement is calculated correctly.
- Grid contains 13 rows and 13 columns of fact cells.
- Cell detail distinguishes ordered-prompt data from family mastery.
- Reset requires explicit confirmation.

#### Shop and maps

- Coins never become negative.
- Purchases persist after reload.
- An owned blaster can be equipped.
- Damage changes according to base damage.
- Map unlock conditions fire once and persist.

---

## 22. Acceptance criteria

The MVP is complete only when all of these are true:

1. The repository can be published directly through GitHub Pages with no build action.
2. `index.html` contains the complete game implementation and loads pinned Phaser 3.90.0 from a CDN.
3. The game is comfortably playable with touch on an iPad in landscape and portrait.
4. Recon Mission runs on first launch and gathers a balanced placement sample.
5. Quick and Full Missions run for 5 and 10 active minutes respectively.
6. All 169 ordered multiplication prompts are available to the scheduler.
7. The keypad accepts answers from `0` through `144`.
8. No visible per-question countdown exists.
9. Every first-try correct answer fires the blaster and destroys at least one block.
10. Faster correct answers add hidden bonus damage.
11. Wrong answers use the exact neutral correction, re-entry, and delayed-retry flow.
12. The adaptive scheduler prioritizes due and weak facts while limiting new facts.
13. Mastery requires accuracy, hidden speed, separate-day evidence, both orientations, and a delayed review.
14. Coins, five blasters, a shop, six maps, and unlocks work and persist.
15. The open dashboard shows overall progress, improvement trends, a 13 × 13 grid, weak facts, and session history.
16. Progress persists locally after closing and reopening the page.
17. No child data, analytics, or gameplay data is sent to a server.
18. `?test=1` runs deterministic browser-based tests.
19. The game contains no copied Roblox intellectual property.
20. There are no known console errors or broken controls on current iPad Safari and a current desktop browser.

---

## 23. Suggested implementation sequence

### Phase 1: Static shell and persistence

- Build all HTML screens and responsive CSS.
- Implement `APP_CONFIG`, fact catalogs, default data, storage validation, and screen navigation.
- Add `?test=1` harness before complex game logic.

### Phase 2: Mastery and scheduler

- Implement ordered facts, family aggregation, attempt recording, state transitions, review dates, baseline calculation, and adaptive selection.
- Complete deterministic tests for these pure functions.

### Phase 3: Mission UI

- Implement keypad, hidden response clock, first-answer evaluation, correction state, retry queue, active-time handling, interruption recovery, and summary calculations.
- Use a placeholder HTML target until logic is stable.

### Phase 4: Phaser scene

- Add procedural maps, blaster, target structures, projectiles, block destruction, particles, recoil, and reduced-motion behavior.
- Connect scene actions to application events.

### Phase 5: Economy and unlocks

- Add coins, shop, five weapons, map unlocks, equipment, and mission bonuses.

### Phase 6: Dashboard

- Add summary cards, trend SVGs, mastery grid, details, weak-fact ranking, session history, and reset.

### Phase 7: iPad hardening

- Test orientation changes, page visibility, Web Audio activation, touch targets, accidental scrolling, localStorage recovery, and Phaser performance.

### Phase 8: Documentation and final verification

- Write `README.md`.
- Test publication from GitHub Pages.
- Run the complete automated and manual checklist.

---

## 24. GitHub Pages publishing instructions for the README

Include these concise steps:

1. Create a GitHub repository.
2. Put `index.html` and `README.md` in the repository root.
3. Push or upload the files to the `main` branch.
4. Open **Settings → Pages**.
5. Under **Build and deployment**, select **Deploy from a branch**.
6. Select `main` and `/ (root)`, then save.
7. Open the published Pages URL after GitHub finishes deployment.

Also include a warning that clearing Safari website data, changing browsers, using private browsing, or changing devices can remove locally stored progress.

---

## 25. Post-MVP ideas, explicitly out of scope

- Obby Builder mode sharing the same mastery engine.
- Soccer Shootout mode sharing the same mastery engine.
- Parent-selected table practice.
- JSON export/import promoted from P1 to default.
- Multiple local child profiles.
- Installable Progressive Web App and offline Phaser copy.
- Division and missing-factor practice.
- Optional child-facing fact map.
- Additional blaster cosmetics and maps.

Do not implement these until the acceptance criteria for the focused Block Blaster MVP are complete.
