# Salt Line

A duel for two to six keepers, running entirely in one HTML file:
`saltline.html`. No build step, no server, no dependencies. Open it, or host it
anywhere static.

## The idea

Two keepers hold the last lighthouse on a drowned coast. The tower holds one of
them, so they settle it the old way — signal mirrors, at dusk. Between rounds you
read their logs, and it becomes clear the duel was never about the tower.

Best of five; first to three rounds keeps the light. With three or more keepers
it becomes a battle royale instead: last lamp burning, first to two rounds.

## The coast

The world is 1920 × 1200 — four times what fits on screen at once. The camera
follows the living keepers and pulls back as they spread out, so a duel plays
close and a six-way royale plays wide. Nobody is ever off-screen.

## Why it is skill, not luck

Nothing in the simulation is random. There are no crits, no spread, no spawns you
cannot predict — `Math.random` is called only for spark particles, which touch
nothing that decides a round. Everything below is a decision:

- **Aim is welded to movement.** You fire wherever you are moving. Repositioning
  and aiming are the same act, so you cannot line up a shot for free.
- **Motes arm after their first wall.** Your own shot is inert while it is leaving
  you and lethal to *everyone* after one bounce. Fire into a corridor and you have
  to remember where it went.
- **The guard is a counterattack.** A parry catches a mote only inside the arc in
  front of you and returns it along whatever direction you face, so you can bank
  the return instead of just handing it back. The window is roughly a fifth of a
  second, and pressing early fails. If both keepers could catch the same mote on
  the same frame, the nearer mirror takes it and a dead heat goes to neither —
  seat order never decides a duel.
- **Ammo is finite and traceable.** A spent mote returns to you only after it
  fizzles, so a wild volley leaves you empty and readable.
- **No stalling.** After 45 seconds the dark closes in a ring at a time, and it
  does not stop closing. Whoever is worst placed is caught first.
- **Nothing is off-screen.** The camera holds every living keeper in frame, so
  you are never killed by something the view was hiding.
- **Symmetric arenas.** Every layout is identical under a 180° rotation, so
  neither side inherits an advantage from the geometry.

## The keepers

Eight to choose from, each with a passive that shapes the whole round and one
special that has to be earned. Both players pick at the start of every match;
guard takes your pick back if you change your mind.

| Keeper | Passive | Special |
|---|---|---|
| **Ines**, the lampwright | Faster motes, quicker reload | **Second Wick** — three motes in a fan, free of ammo |
| **Cato**, the tidewatcher | Longer, faster dash on a shorter cooldown | **Ebb** — snap back to where you stood a moment ago |
| **Meran**, the glassblower | Wide guard, fast recovery, only two motes | **Prism** — your next mote splits in three on its first wall |
| **Odall**, the wrecker | Slow, fat motes that ride the walls far longer | **Breakwater** — raise a wall of slag in front of you |
| **Silt**, the mudlark | Smaller target, quicker feet, slower trigger | **Duck Under** — light passes through you; no shooting, no guard |
| **Vane**, the signalman | Motes live longer and gain speed off every wall | **Recall** — every mote you own turns around where it is |
| **Keel**, the bellringer | Parried motes fly hard, on a tighter window | **Ringing** — a ring throws every mote away from you, and takes it over |
| **Auger**, the welldigger | Sees the line its shot will take, two walls ahead | **Sinkhole** — open a hole in the floor that drinks light |

### Charge is earned, never given

The special meter does not fill with time — standing still for thirty seconds
earns exactly nothing, and so does running and dashing. It fills only from:

- **a parry** (+45),
- **your own mote surviving a wall** (+10, first three bounces of each mote),
- **slipping past a live mote** (+20 once per mote) — measured at 18–35px of
  centre distance. Closer than 18px is not a near miss, it is a hit.

## Controls

| | Player one | Player two |
|---|---|---|
| Move & aim | `W A S D` | arrow keys |
| Fire | `Space` | `.` |
| Guard / parry | `Q` | `,` |
| Dash | `E` | `/` |
| Special | `F` | `;` |

`Esc` returns to the menu. Online, both players use the left-hand set.

## Battle royale

Three to six keepers, every one for themselves, last lamp burning takes the
round, first to two rounds takes the coast. Online it starts by itself: two in
the tower play the story duel, three or more play royale. Against the tower it
is menu option 5.

Everyone spawns on a ring around the middle, evenly spaced, so no seat starts
better placed than another. Eliminated keepers leave their motes in the air.

## Playing alone

Menu option `4`. The CPU is not given anything you are not:

- it fills **the same input struct your keyboard fills** — eight directions, one
  fire, one guard, one dash, one special;
- it aims in **the same eight directions** a keyboard can produce, so it cannot
  draw an angle you cannot;
- it reads **only what is drawn on the screen** — no velocities you cannot see,
  no knowledge of what you are about to press;
- it is **deterministic**: the same position is answered the same way twice, so
  it can be learned and then beaten, never merely out-rolled.

Difficulty is a capability, not a handicap dial. Each level differs in reaction
time, how many walls it will bank a shot off, how often it commits to a guard,
and whether it fires without a solution:

| | Reaction | Banks | Guards | Aim |
|---|---|---|---|---|
| **Apprentice** | 0.30s | direct shots only | rarely | eight directions — fires blind, and hits itself doing it |
| **Keeper** | 0.17s | one wall | about half | eight directions |
| **Warden** | 0.085s | two walls | almost always | eight directions |
| **Two Lights** | every frame | three walls | everything it can reach | **continuous — no keyboard can do this** |

The first three are held to what a player gets. **Two Lights is not**, and the
difficulty card says so: it aims in any direction rather than the eight a
keyboard makes, reacts every frame, and does not mistime a guard. It is not
built to be fair.

It is also not unbeatable, and it would be dishonest to claim otherwise — it
takes only about 59% of decided rounds against Warden, because two keepers who
both guard well mostly trade. Against anything human-paced it is another story.

Measured over 80 rounds per matchup with seat orders balanced; across 1,200
mirror rounds from varied but symmetric starts, neither seat is favoured
(53.1%, inside the fair band).

On the select screen the CPU waits for your pick, then answers it with a
counter of its own.

## Getting around

Every menu is a list you can click, hover or drive with the arrow keys, Enter,
or the number beside each row. On the keeper select, click a card to take it and
click again to lock it in; guard takes it back.

## Online play

Menu option `2` opens a tower and gives you a **four-letter code** plus a link.
The other player presses `3` and types the code, or just opens the link — it
drops them straight in. Nothing else to copy, nothing to send back.

A small public broker introduces the two browsers to each other. After that the
game data goes directly between you and never touches it. If the broker is
blocked on your network, the game says so and offers the old **paste-code**
route, which needs no broker at all: the host sends one code, the guest sends
one back, and you are connected.

The host owns the simulation. The guest sends only its input — as counters
rather than one-shot events, so a dropped packet never eats a shot or a special
— and draws the state it is sent. Character select runs on the same channel, so
both players see each other's cursor move before anything is locked in.

Connections go direct where the network allows it, and fall back to a public
TURN relay for the strict company and mobile-carrier networks that refuse direct
routes. Both the broker and the relay are free public services, so neither is
guaranteed to be up; when they are not, paste codes still work, and a
self-hosted PeerServer can be pointed at with `?broker=host:port`.
