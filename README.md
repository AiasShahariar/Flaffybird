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
  sun. Pipes turn to bamboo, and it stays that way from here on.
- **50+** — snake ambush. You get 10 HP and a katana; armed snakes on the
  pipes shoot at you for 1 damage each. Time the **PARRY** button to deflect
  a bullet back and kill the snake for $5. A new breed joins every 10 points:
  **vipers** at 50, two-headed **Twin Fangs** at 60 that fire 2.5x as often.
  Every breed you've unlocked is equally likely to show up.

Gold carries over between runs. The **Shop** is empty for now.
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

## Development

Everything is in `index.html` — open it in a browser to play locally. Pushing
to `main` redeploys the live site automatically. See `CLAUDE.md` for
architecture notes and the headless testing approach.
