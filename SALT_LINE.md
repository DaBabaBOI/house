# Salt Line

A two-player duel that runs entirely in one HTML file: `saltline.html`.
No build step, no server, no dependencies. Open it, or host it anywhere static.

## The idea

Two sisters keep the last lighthouse on a drowned coast. The tower holds one
keeper, so they settle it the old way — signal mirrors, at dusk. Between rounds
you read their logs, and it becomes clear the duel was never about the tower.

Best of five; first to three rounds keeps the light.

## Why it is skill, not luck

Nothing in the simulation is random. There are no crits, no spread, no spawns
you cannot predict — `Math.random` is called only for spark particles, which
touch nothing that decides a round. Everything below is a decision:

- **Aim is welded to movement.** You fire wherever you are moving. Repositioning
  and aiming are the same act, so you cannot line up a shot for free.
- **Motes arm after their first wall.** Your own shot is inert while it is
  leaving you and lethal to *everyone* after one bounce. Fire into a corridor
  and you have to remember where it went. Half the deaths in this game are
  self-inflicted, and all of them are your fault.
- **The guard is a counterattack.** A parry catches a mote only inside a ~110°
  arc in front of you, and sends it back along whatever direction you face —
  about ±55° of freedom, so you can bank the return instead of just handing it
  back. The window is roughly a fifth of a second, and pressing early fails.
- **Ammo is finite and traceable.** Three motes each. A spent mote returns to
  you only after it fizzles, so a wild volley leaves you empty and readable.
- **No stalling.** After 45 seconds the dark closes in a ring at a time, until
  the arena is too small to hide in.
- **Symmetric arenas.** Every layout is identical under a 180° rotation, so
  neither side inherits an advantage from the geometry.

## Controls

| | Ines (left) | Cato (right) |
|---|---|---|
| Move & aim | `W A S D` | arrow keys |
| Fire | `Space` | `.` |
| Guard / parry | `Q` | `,` |
| Dash | `E` | `/` |

`Esc` returns to the menu. Online, both players use the left-hand set.

## Online play

Menu option `2` hosts, option `3` joins. Because there is no backend, the two
browsers introduce themselves by copy-paste: the host generates an invitation
code, the guest pastes it and generates a reply code, the host pastes that back.
After that the browsers talk to each other directly over WebRTC.

The host owns the simulation. The guest sends only its input — as counters
rather than one-shot events, so a dropped packet never eats a shot — and draws
the state it is sent. That means the two screens cannot disagree about who died.

Peer-to-peer connections traverse most home networks via STUN. Some strict
corporate or carrier-grade NATs need a TURN relay, which a static page has no
way to provide; on those networks, play locally.
