# Salt Line

A duel for two to six keepers, running entirely in one HTML file:
`saltline.html`. No build step, no server, no dependencies, and **nothing
fetched from the internet** — the networking library is bundled into the file
itself, so it behaves the same opened straight from your Downloads folder as it
does on a web host.

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

It frames the keepers only, never the motes, and eases the zoom slowly with a
deadzone so it settles instead of hunting; the view is snapped to whole pixels
so nothing shimmers as it pans. Press **Z** during a round to stop it following
altogether and hold the whole coast at once.

## Why it is skill, not luck

Nothing in the simulation is random. There are no crits, no spread, no spawns you
cannot predict — `Math.random` is called only for spark particles, which touch
nothing that decides a round. Everything below is a decision:

- **Aim is its own axis.** Point the mirror anywhere with the mouse, or turn it
  with two keys. Aim and movement are separate, so you can back away from one
  mote while answering the keeper who sent it. Point nothing and you simply face
  the way you run.
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
| Move | `W A S D` | arrow keys |
| Aim | the mouse, or `Z` / `X` to turn | `L` / `'` to turn |
| Fire | left-click or `Space` | `.` |
| Guard / parry | right-click or `Q` | `,` |
| Dash | `E` | `/` |
| Special | `F` | `;` |

`V` locks the camera to the whole coast.

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

| | Reaction | Banks | Guards | Hand |
|---|---|---|---|---|
| **Apprentice** | 0.38s | direct shots only | rarely | shaky (±20°) — fires blind, and hits itself doing it |
| **Keeper** | 0.19s | one wall | about half | ±6° |
| **Warden** | 0.09s | two walls | almost always | ±2° |
| **Two Lights** | every frame | three walls | everything it can reach | **perfect — no error at all** |

Everyone aims freely now, so the levels differ by reaction, steadiness of hand,
how many walls they will bank a shot off, and how readily they guard.

Two Lights is not unbeatable, and it would be dishonest to claim otherwise.
Measured with seat orders balanced: it takes 84% of decided rounds against
Apprentice and 69% against Keeper, but only 45% against Warden — the top two
are equals, because two keepers who both guard well mostly trade. Free aim also
made every level deadlier, which narrowed the spread: Warden takes 87% against
Apprentice and 70% against Keeper.

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

The host owns the simulation, so the two screens can never disagree about who
died. The guest sends only its input, as counters rather than one-shot events,
so a dropped packet cannot eat a shot or a special.

Because the channel is deliberately unreliable — a lost packet is skipped rather
than holding everything up behind it — **nothing is ever said only once**. Every
packet carries the receiving seat's number, so a guest that misses the opening
handshake simply learns its seat from the next packet instead of driving the
wrong keeper.

A guest also runs the movement maths on its own keeper immediately rather than
waiting for the round trip, and eases that prediction back toward the host's
version as packets arrive; shots and guards stay with the host. Other keepers
are eased between packets and motes carry on along their last known velocity, so
a lost packet costs a little smoothness rather than a freeze. If the host goes
quiet the guest says so, and recovers by itself when packets resume.

If the code screen says the meeting point is not answering, the broker is
blocked or down on that network — the card names the reason it gave. Paste codes
still work there, because they put nothing in the middle.

Connections go direct where the network allows it, and fall back to a public
TURN relay for the strict company and mobile-carrier networks that refuse direct
routes. Both the broker and the relay are free public services, so neither is
guaranteed to be up; when they are not, paste codes still work, and a
self-hosted PeerServer can be pointed at with `?broker=host:port`.
