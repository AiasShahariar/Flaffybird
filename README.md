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
  a bullet back and kill the snake for $5. A new breed joins every 10 points,
  all the way to 160, and every breed you've unlocked is equally likely to
  show up.

## The snakes

| Score | Breed | What it does |
|---|---|---|
| 50 | Vipers | Aimed shots |
| 60 | Twin Fangs | Two heads, fires 2.5x as often |
| 70 | Sight Adders | Shoots where you *will* be — change your mind to break the lock |
| 80 | Ledge Crawlers | Fires at the ceiling, then skims along it. Get off the roof |
| 90 | Splitfangs | Forks in two near you. Cut it early and the whole volley dies |
| 100 | Rattlers | Three shots on a beat, then a rest. Don't press between beats |
| 110 | Lantern Coils | Ignores you — parks and blocks one side of the gap |
| 120 | Wraith Coils | Follows you, but turns slowly. Outclimb it |
| 130 | Picket Adders | A slow wall of four. One cut takes all of them |
| 140 | Ash Bloomers | Parks and grows. Cut it from arm's length |
| 150 | Basilisks | A beam you **cannot** parry — pale and still, not red. Move |
| 160 | Mirrorfangs | Blocks your parry while its guard is up. Wait for the ring to drop |

Gold carries over between runs and buys five permanent upgrades in the
**Shop** — from Kingfisher's Patience at $60 up to Still Water at $1500.
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
