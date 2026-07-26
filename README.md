# Flaffybird

A roguelike Flappy Bird — one self-contained HTML file, no dependencies, no build step.

**Play:** https://aiasshahariar.github.io/flaffybird/

## How it plays

- **0–45** — tap/click/space to flap. Every 5 points the game pauses and deals
  3 upgrade cards from a pool of 7. The score blinks red when a choice is one
  pipe away.
- **45** — checkpoint. Once you've reached it, Start and Restart resume here
  with every upgrade collected. **New Run** starts a fresh climb.
- **46+** — the world changes. The sky fades into a bamboo forest at dusk,
  with silhouetted groves scrolling past, hanging paper lanterns and a setting
  sun. Pipes turn to bamboo, and it stays that way from here on.
- **50+** — snake ambush. You get 10 HP and a katana; armed snakes on the
  pipes shoot at you for 1 damage each. Time the **PARRY** button to deflect
  a bullet back and kill the snake for $5. **Vipers** own 50-59 and two-headed
  **Twin Fangs** own 60-69 — both heads fire at once, 2.5x as often. **XLR8
  Snakes** own 70-79 — blue, they charge up and then fire a round at 5x speed,
  so parry on the flash rather than reacting to the shot — and they keep firing
  at your back after you overtake them. **Bombardiers** own 80-89 — their round
  flies at 3x and detonates near you into an enormous red fireball, so cut it early. From 90 all four mix,
  and every pipe carries a snake. A parried shot always flies back the way
  it came, and kills whatever it reaches.

- **80+** — you get a **DASH**: a short forward lunge on a one-second cooldown.
  The ground you gain is yours to keep, so dashes stack you further up the
  screen — but the further forward you sit, the less warning you get. Right
  click on desktop, or the DASH button on a phone.

Gold carries over between runs and buys the **Stormcutter** ($500) — parry at
the very last possible moment and the blade throws lightning instead of
returning the shot, killing the snake and healing you a heart. The window is
about a twentieth of a second, so it is a genuine flex.
**Settings** lets you drag the PARRY button anywhere and switch on desktop
controls (space to flap, left click to parry).

## The upgrades (0–45)

- 🛡️ **Guardian Feather** — absorbs one crash; restore it by threading 3 gaps dead-centre
- ⛏️ **Woodpecker Beak** — smash through a pipe, recharges after 10
- 🪂 **Glider Wings** — hold to glide instead of falling
- ⏪ **Rewind Totem** — one fatal crash rewinds 2 seconds
- 🪶 **Hummingbird Form** — tiny, nimble bird with quick flaps
- 🌙 **Moon Feathers** — low gravity, feather-fall
- 🪁 **Kite Tail** — hang in the air at each flap's apex

## Development

Everything is in `index.html` — open it in a browser to play locally. Pushing
to `main` redeploys the live site automatically. See `CLAUDE.md` for
architecture notes and the headless testing approach.
