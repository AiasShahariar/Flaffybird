# Flaffybird

A roguelike Flappy Bird — one self-contained HTML file, no dependencies, no build step.

**Play:** https://aiasshahariar.github.io/flaffybird/

## How it plays

- **0–45** — tap/click/space to flap. Every 5 points the game pauses and deals
  3 upgrade cards from a pool of 9. The score blinks red when a choice is one
  pipe away.
- **45** — checkpoint. Once you've reached it, Start and Restart resume here
  with every upgrade collected. **New Run** starts a fresh climb.
- **46+** — the world changes. The sky fades into a bamboo forest at dusk,
  with silhouetted groves scrolling past, hanging paper lanterns and a setting
  sun. Pipes turn to bamboo. At **75** it shifts again, into night.
- **50+** — snake ambush. You get 10 HP and a katana; armed snakes on the
  pipes shoot at you for 1 damage each. Time the **PARRY** button to deflect
  a bullet back and kill the snake for $5.

Gold carries over between runs and buys permanent upgrades in the **Shop**.
**Settings** lets you drag the PARRY button anywhere and switch on desktop
controls (space to flap, left click to parry).

## The upgrades (0–45)

- 🛡️ **Guardian Feather** — absorbs one crash; restore it by threading 3 gaps dead-centre
- ⛏️ **Woodpecker Beak** — smash through a pipe, recharges after 10
- 🪂 **Glider Wings** — hold to glide instead of falling
- 🎯 **Falcon Focus** — time slows while threading a pipe
- ⏪ **Rewind Totem** — one fatal crash rewinds 2 seconds
- 🪶 **Hummingbird Form** — tiny, nimble bird with quick flaps
- 🌙 **Moon Feathers** — low gravity, feather-fall
- 🐾 **Perch Claws** — land on pipe caps and perch
- 🪁 **Kite Tail** — hang in the air at each flap's apex

## Shop

Early Bird ($30) · Extra Hearts ($50) · Golden Blade ($60) · Quickdraw ($80) · Phoenix Feather ($100)

## Development

Everything is in `index.html` — open it in a browser to play locally. Pushing
to `main` redeploys the live site automatically. See `CLAUDE.md` for
architecture notes and the headless testing approach.
