# Flaffybird

A roguelike Flappy Bird. Everything — HTML, CSS, JS — lives in a single
self-contained `index.html`. No build step, no dependencies, no server.
Editing that one file *is* the whole workflow.

Deployed via GitHub Pages at https://aiasshahariar.github.io/flaffybird/
(Settings → Pages → deploy from `main`, root). Pushing to `main` redeploys
automatically about a minute later.

## Game structure

Three progression phases, in order:

1. **Fledgling phase (0–45)** — plain Flappy Bird plus a roguelike upgrade
   chooser every 5 points. Pool of 9 `UPGRADES`; each choice deals 3 cards.
   By 45 the pool is exhausted, so no chooser fires after that. The score
   counter blinks red when one point away from a choice.
2. **Checkpoint (45)** — reaching 45 once sets a persistent flag. Later runs
   resume at 45 with all 9 upgrades via `applyCheckpoint()`; Start/Restart
   relabel themselves, and a **New Run** button forces a fresh 0-start.
3. **Snake ambush (50+)** — `SNAKE_SCORE`. Bird gains HP (`maxHp`) and a
   katana. Armed snakes cling to pipe sides and fire. The PARRY button (or left
   click in desktop mode, or P/Enter) opens a 0.16s window that flips a bullet
   to `friendly`, which then homes back and kills its snake for gold.

   **`SNAKE_TYPES` is the extension point** — currently three rows: vipers at 50,
   two-headed Twin Fangs at 60 firing 2.5x as often, and XLR8 Snakes at 70 whose
   round travels at 5x speed. A row is `{ id, from,
   name, tip, heads, rate, shot, body, mark }`; `shot` picks a case in
   `fireSnake()`, `rate` divides the shot interval, and `heads` drives three
   things at once — the fork in `drawSnake`, the muzzle positions, and the
   volley size, since **every head fires together**. All of them read
   `headOffset()`, which is why the shots come from the heads you can see.
   `heads` is also the breed's threat cost, so a two-round volley reserves two
   of the four slots. `bv` scales round speed; `windup` makes the breed charge
   first and is **required** above about 2x — a 5x round crosses from muzzle to
   bird in ~0.2s, well inside human reaction time, so the read has to be the
   charge tell, not the round.

   Ten further breeds with distinct projectiles (predictive lane, ceiling
   skimmer, splitter, burst, parked blocker, homer, wall, growing disc,
   unparryable beam, guarded shooter) were built and then cut on request — see
   commit 375a892 if any of them is ever wanted back.

   Rules that survive regardless of roster size:
   - **Solo stretches.** Each breed owns the 10 points from its own threshold
     (`soloBreedFor`): inside that range it is the ONLY thing that spawns. Past
     the last breed's stretch every unlocked breed spawns with equal odds. A
     snake goes on **every** pipe.
   - **Threat cost, not sprite count.** `threat` must be `let` and incremented
     as we fire — as a hoisted `const`, N snakes fired against one stale read
     and overshot the cap of 4.
   - **No firing from offscreen-left** (`mx < 8`) or from under the PARRY
     button. Shots used to spawn 67px off-screen.
   - **`pressure`** raises the fire rate with score. Its dilution term applies
     to **mixed pools only** — in a solo stretch `meanRate` *is* that breed's
     rate, so dividing by it cancels `rate` algebraically and flattens every
     breed to one cadence.
   - **A parried round always goes back the way it came.** The parry reverses
     its velocity immediately; the homing branch only overrides that while the
     shooter is alive. Without the reversal a round whose shooter had already
     died just kept drifting forward as a harmless blue dot — common with
     two-round volleys, where the first kill orphans the second round. An
     orphaned round still cuts down any snake it passes.
   - **The `landing` gate is the real ceiling.** It blocks any new shot while
     one is within 0.5s of arriving, so no two arrivals can land inside one
     0.45s parry cooldown. With a snake on every pipe this serialises arrivals
     to roughly 0.6/s and matters far more than any breed's `rate`.

   Parry reach is `p.parryR`, absolute — **never** `bird.r * 4.2`.
   `applyCheckpoint()` grants Hummingbird Form, which shrank reach from 80px to
   49px on the standard route into the phase.

Cutting across all of that: **background zones**, a purely cosmetic layer keyed
off score. `ZONES` is a table of `{ from, sky, cloud, sand, grass, horizon,
hatch, pipe, pipeEdge, pipeNodes, decor }` — day (0–45) and bamboo dusk
(`ZONE_DUSK_SCORE`, 46), which currently runs to the end. Crossing a threshold
cross-fades over `ZONE_FADE` seconds. Zone 0 reproduces the original look
exactly, so nothing below 46 changed.

## Key conventions

- **State machine**: `START / PLAYING / GAMEOVER / CHOICE`. `update()`
  returns early on `CHOICE` so the world freezes behind the cards.
- **Everything scales to the viewport.** `params()` recomputes gravity, flap
  velocity, pipe width/gap/spacing, and bird radius each frame from `H`, so
  the game plays the same on a phone and a desktop window. Never hardcode
  pixel distances for gameplay — derive them in `params()`.
- **Upgrade/shop effects are deliberately isolated.** Each is one entry in
  `UPGRADES`/`SHOP_ITEMS` plus a small hook where its system lives
  (bullet-hit branch, `killSnake()`, the collision loop, `params()`). This is
  intentional: the user reserves the right to cut any of them, and removing
  one should be a couple of line deletes, not surgery.
- **Zones are data, not code paths.** A new background is one `ZONES` entry plus
  an optional `decor` function; nothing else in `draw()` changes. Two gotchas:
  `decor` references hoisted `function` declarations defined much further down
  (a `const`/arrow would throw a TDZ error at load), and the zone must never be
  hung off `checkpointUnlocked` — that flag is a one-shot lifetime latch. The
  zone is derived from `score` every frame, so it needs no rewind snapshot;
  `syncZoneInstant()` snaps it in `resetGame()`, `applyCheckpoint()` (which sets
  score *after* `resetGame`), and `doRewind()`.
- **Two scroll accumulators, deliberately.** `groundOffset` is `% 48` for the
  ground hatch; `bgScroll` is unbounded because decor identity is
  `floor(offset/step)` and wrapping would make stalks morph in place. The zone
  cross-fade advances on **raw** dt (presentational, one call site), while
  `bgScroll` advances on the Falcon-Focus-slowed dt at both `groundOffset` sites
  so parallax stays locked to the ground.
- **Dev Mode** (Settings → Dev Mode) has 45 / 70 / 100 / 130 buttons calling
  `devStart(n)` — full upgrade kit, snake phase armed, full HP, zone synced,
  `breedsSeen` pre-set so unlock banners don't replay. Each lands inside a
  different breed's solo stretch for testing.
- **Persisted keys** (all localStorage, all optional/try-wrapped):
  `flappy-best`, `flappy-gold`, `flappy-shop`, `flappy-checkpoint`,
  `flappy-parry-pos`, `flappy-desktop`.
- **Mobile-first.** iOS Safari zoom/scroll are blocked via `touch-action`,
  `preventDefault` on touch/gesture events, and a fixed body. Input runs
  through `pointerdown` (not click) so taps register with no delay. Keep it
  that way — synthesized clicks are suppressed by the touch preventDefault.
- Canvas is sized by `devicePixelRatio` in `resize()`; pipes are rescaled on
  width change so rotation doesn't teleport them.

## Testing

There is no test suite. Verify changes by **driving the real game headlessly**
before pushing — this has caught several real bugs (an always-visible "NEW
BEST!" badge, a stale HP-bar cap):

```bash
cd /tmp && npm install playwright-core     # chromium is preinstalled
# launch with executablePath: '/opt/pw-browsers/chromium-1194/chrome-linux/chrome'
```

The pattern that works: copy `index.html` to a scratch file, inject a test
hook next to `let diedAt = 0;` exposing internals —

```js
window.__t = { s: () => ({ state, score, y: bird.y, vy: bird.vy, ... }),
               setScore: n => { score = n; } };
```

— then fly the bird with a bot loop that reads `__t.s()` and clicks whenever
the bird is below the next gap's centre. Use `setScore(44)`/`setScore(49)` to
reach a phase quickly, handle `state === 3` by clicking an upgrade card, and
assert on state plus `pageerror` being empty. Screenshot to check visuals.

Also always syntax-check before committing:
`node -e "new Function(require('fs').readFileSync('index.html','utf8').match(/<script>([\s\S]*)<\/script>/)[1])"`

## Context

- The user plays on an iPhone in Safari, so test at a phone viewport
  (390×844) and keep touch targets generous.
- The game originated in a side branch of the user's `personalized-librarian`
  repo (`claude/flappy-bird-html5-ywg2al`) before moving here; that branch is
  a historical copy and is no longer the source of truth. **This repo is.**
- The user is non-technical with terminal/Windows tooling — prefer precise,
  copy-pasteable steps over assuming familiarity.
