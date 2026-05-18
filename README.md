# DUEL — Retro VR Platform Project

## The Concept

A VR gaming platform that imagines an alternate history: what if VR existed at the same time as the first video games? Starting from the constraints of 1978 and working forward through the 80s and 90s, designing games natively for VR rather than porting flat-screen conventions.

**Hardware setup:**
- Two Quest 3 headsets (passthrough on by default)
- A tablet or iPad between the players — camera view of both players for spectators, improves body tracking
- Local, social, console-like — the room is the platform, not the headset

---

## Why VR Has Stalled

VR inherited the conventions of flat gaming rather than inventing its own. The result: remote-control awkwardness, isolation, and experiences that feel like worse versions of things that work fine on a screen.

Key references that got it right:
- **Wii** — gesture maps to *intent*, not literal movement. A wrist flick becomes a topspin forehand. The game executes the expert version of what you meant.
- **Kinect puppeteering** — third-person character controlled by your body worked well. The problem was 1:1 literal mapping. The format itself (seeing your character do things) is underexplored.
- **Meta Orion Pong demo** — proved that a primitive game natively designed for spatial interaction works immediately.
- **Tilt 5** — got local multiplayer and shared-object-in-the-room right. Limited by the retroreflective board and by everything being below table level. Objects need to project *up* into shared space.
- **The *Her* projector game** — the right model: game inhabits the room, doesn't replace it. Passthrough as default, not fallback.

---

## Core Design Principles

### 1. Gesture = Intent, Not Literal Action
The player makes a gesture. The character executes the expert version of that intent. Like a Wii tennis swing producing a proper topspin forehand — the game fills in the skill gap.

### 2. Scrubable Animation (the iPhone scroll analogy)
Borrowed from iOS scroll physics: you don't control 1:1, but you control *speed and direction*. An animation plays (reload, flip, sword strike) and your gesture scrubs it forward or backward. Key properties:
- Low latency — must feel immediate
- Reversible at any point — reversibility is what makes your brain accept ownership
- Speed of gesture = speed of animation playback

### 3. Commitment Mechanics
Physical actions have a point of no return — like leaving a ledge in a platformer. Before the threshold: you can abort. After it: physics take over. A flip requires commitment. Hesitate mid-rotation and you fail the landing. This creates a skill curve identical to good platformer controls, but through body gesture.

Different gestures have different commitment profiles:
- A punch: almost no commitment window, fast and low-consequence
- A flip: long wind-up, hard point of no return
- A reload: fully reversible throughout

### 4. Presence Before Fidelity
A simple geometric shape floating at chest height, reacting to you, is more magical than a detailed world viewed from above. Objects need to occupy *vertical space* — at eye level, not sunken into a table. The holo-chess scene in Star Wars works because the creatures are at their own scale, in their own space, making eye contact.

### 5. Body Pleasure First, Game Rules Second
Design starting point: what's fun to do with your body with a friend that's currently impossible or impractical indoors? Then wrap the simplest possible rules around that.
- Frisbee indoors
- Sword fighting in a living room
- Throwing things at people (cf. Throw Throw Burrito)
The game follows from the physical pleasure, not from existing genres.

### 6. The Room Is the Level
Passthrough means the coffee table is terrain, the bookshelf is a castle wall, the gap between cushions is a canyon. The Couch Knights demo proved this — the fun was the little character navigating *your specific room*.

---

## The Arena Format

The play space is a **table-tennis table at waist height**, with walls and ceiling above it — giving a 70s enclosed-court feeling. Like a table tennis table defines a game space, the arena defines the VR game boundary while remaining physically present in the room.

- Table surface: ~3m × 1.8m at 0.76m height
- Arena walls and ceiling rise above the table
- Players stand at each end, arms reaching over the table
- The disc/projectile travels in the space above the table
- Wall bounces are in play (billiards-style)
- Spectators see both players and the game on the tablet between them

---

## Game 1: DUEL

**Era:** 1978  
**Players:** 2, face to face  
**Input:** Hands only (no controllers)

### The Object
Each player has two frisbee-disc shields — one per hand. In front of each player, at hand height, is a row of 5 blocks:

```
[ L-PADDLE ] [ L-SPEED ] [ CHEST ] [ R-SPEED ] [ R-PADDLE ]
```

- **Chest block** (centre): ends the game when destroyed. Takes several hits, changes colour as it degrades.
- **Paddle blocks** (outermost): losing one removes that hand's shield permanently.
- **Speed blocks**: losing one slows that hand's attack speed.

### Controls
- **Disc held vertical** = shield (blocking)
- **Disc tilted horizontal** = shoot. The shield shrinks to a dot (button size) at the endpoint. The disc fires in the direction your index finger points, at pong speed.
- **Return mechanic**: disc returns to your hand automatically (Thor/Tron rules). Fast throw = fast return = short exposure window. Slow tentative throw = long exposure. Reversing the tilt before release = aborted shot, no penalty.

### Strategy
- You have one disc per hand. Throwing one leaves that side undefended.
- Shots bounce off side walls (billiards angles).
- Target their paddle block first → they're one-handed → then chest.
- Or go straight for chest while they have full defense — higher risk.
- You can physically step sideways to dodge. Your whole block row moves with you.

### Why it works
- One object, two states (vertical/horizontal), zero learning curve
- The dilemma (attack = vulnerability) is built into the physics
- Legible to spectators — you can read the whole game state at a glance
- Spectacle: someone defending one-handed with their chest block going red is a crowd moment
- Arcade-viable: 2-4 minute games, no setup, instant comprehension

---

## Game Pipeline (rough era sequence)

| Era | Game | New idea introduced |
|-----|------|-------------------|
| 1978 | Pong / Rally Ball | Shared object at eye level in room |
| 1978 | **DUEL** | Two states per hand, attack/defence dilemma |
| 1979 | Volleyball variant | Reactive full-body, defensive play |
| 1980 | Sword fighting | Commitment mechanics, parry/strike |
| 1982 | Couch Knights style | Character-in-your-room, puppeteering |
| 1985+ | Platformer elements | Gesture-as-intent, scrubable animation |

---

## Files

### `pong3d/pong3d.html` — 3D Pong (playable)
The first 1978-tier prototype. Validates the core spatial premise: a ball moving in a shared cubic space, each player defends a wall.

- **Desktop:** Open directly. Mouse controls your paddle (P1). CPU controls P2.
  - Drag to orbit, scroll to zoom
  - Ball speeds up across a rally, resets on score
- **Quest 3 (WebXR):** Serve over local network (`python -m http.server 8080` from `pong3d/`), open in Oculus Browser, tap **Enter VR**
  - Stand at P1 end (z=0). Left hand/controller tracks P1 paddle, right hand/controller tracks P2 paddle
  - The arena is 2m × 2m × 3m at waist height — ball travels in shared vertical space

### `duel/duel.html` — DUEL arena visualization (non-interactive)
Visualization of the DUEL game layout. Shows the table-tennis arena, both players in wireframe, block row (chest + paddle + speed blocks), shield vs. shoot-mode discs, and animated wall-bounce disc trajectory.

- **Desktop:** Open directly in Chrome or Firefox.
- **Quest 3 (WebXR):** Serve from `duel/` directory, tap Enter VR to see at 1:1 scale.

## Running

**Desktop:** Open any `.html` directly in Chrome or Firefox. No server needed.

**Quest 3 (WebXR):** WebXR requires HTTPS or localhost — `file://` won't work.
1. Run `python -m http.server 8080` from the game's subdirectory
2. On Quest 3, open `http://<your-local-ip>:8080/<game>.html` in the Oculus Browser
3. Tap **Enter VR** — arena renders at 1:1 physical scale
