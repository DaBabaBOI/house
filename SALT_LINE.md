# Salt Line

A two-player duel that runs entirely in one HTML file: `saltline.html`.
No build step, no server, no dependencies. Open it, or host it anywhere static.

## The idea

Two keepers hold the last lighthouse on a drowned coast. The tower holds one of
them, so they settle it the old way — signal mirrors, at dusk. Between rounds you
read their logs, and it becomes clear the duel was never about the tower.

Best of five; first to three rounds keeps the light.

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
  second, and pressing early fails.
- **Ammo is finite and traceable.** A spent mote returns to you only after it
  fizzles, so a wild volley leaves you empty and readable.
- **No stalling.** After 45 seconds the dark closes in a ring at a time.
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

## Online play

Menu option `2` hosts, option `3` joins. Because there is no backend, the two
browsers introduce themselves by copy-paste: the host generates an invitation
code, the guest pastes it and generates a reply code, the host pastes that back.
After that the browsers talk to each other directly over WebRTC.

The host owns the simulation. The guest sends only its input — as counters rather
than one-shot events, so a dropped packet never eats a shot or a special — and
draws the state it is sent. Character select runs on the same channel, so both
players see each other's cursor move before anything is locked in.

Peer-to-peer connections traverse most home networks via STUN. Some strict
corporate or carrier-grade NATs need a TURN relay, which a static page has no way
to provide; on those networks, play locally.
