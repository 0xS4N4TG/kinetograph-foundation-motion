# The Twelve Principles

Disney's twelve principles of animation (Frank Thomas and Ollie Johnston, *The Illusion of Life*, 1981), applied to UI
motion and to motion graphics. The three designer lenses decide *how much* motion a piece should have; the twelve
principles decide whether the motion that is there *reads as alive*.

Load this file in Create mode for anything with more than one moving part, and in Audit mode whenever motion "feels
wrong" but no single rule in the checklist catches it. Each principle has: what it is, how it shows up in UI, how it
shows up in motion graphics and video, how to build it, and how it fails.

---

## 1. Squash and Stretch

Deforming an object (compressing or elongating it) to give a sense of weight, flexibility and mass while keeping its
overall volume constant. The most fundamental principle.

- **UI:** a button that compresses a little on press (`scale: 0.97`), a toggle knob that stretches along its travel, a
  pull-to-refresh blob, a badge that pops. Keep it small: UI elements are rigid objects with a little give, not rubber.
- **Motion graphics:** a ball, a dot, a logo mark landing; a card that squashes on impact with the edge of frame; a
  bar in a chart that overshoots and settles.
- **Build:** keep the volume: if `scaleY = s`, then `scaleX = 1 / s` (or `1 / sqrt(s)` for a softer read). Stretch
  along the direction of travel, squash on contact, perpendicular to the surface. Scale about the contact point
  (`transform-origin` at the bottom for a landing), not the centre.
- **Fails when:** volume changes (it reads as inflating, not deforming); it is applied to text or rigid UI chrome; the
  squash happens before contact.

## 2. Anticipation

Preparing the audience for a major action through a smaller preceding movement, like bending the knees before a jump.

- **UI:** a drawer that pulls in a few pixels before sliding out; a send button that dips before the message flies; a
  card that leans back before it is flicked away. Only for deliberate, rare actions: anticipation adds latency.
- **Motion graphics:** a camera that eases back before it pushes in; a word that drops 4 px before it launches; a
  chart line that dips before it climbs. The beat before the beat.
- **Build:** a short move (10 to 20% of the main move's distance, 15 to 25% of its time) in the opposite direction,
  with an ease-in, then the main move. Springs with a negative initial velocity give it for free.
- **Fails when:** it is on a high-frequency interaction (Emil's frequency gate wins); it is as big as the action; every
  element anticipates at once.

## 3. Staging

Presenting an action so that it is completely clear, using camera placement, lighting and silhouette to guide focus.

- **UI:** one thing moves at a time where the eye should be; the rest holds still or dims. Focus rings, scrims and
  depth (blur, scale) put the subject forward.
- **Motion graphics:** the camera frames the subject, the background is quieter than the subject, the subject reads in
  silhouette (as a solid shape, it should still be recognisable). Leave the subject room in the direction it moves.
- **Build:** before animating, decide the single point of interest for each beat. Dim, blur or scale back everything
  else (`opacity 0.4`, `blur 6px`, `scale 0.96`). Composite by depth: foreground sharp, background soft.
- **Fails when:** two things compete for attention in the same beat; the subject is centred but the motion pulls the
  eye to an edge; type is set over the busiest part of the picture.

## 4. Straight Ahead Action and Pose-to-Pose

- **Straight ahead:** made frame by frame from start to finish, for spontaneous, fluid motion (fire, water, particles,
  hand-drawn boil).
- **Pose-to-pose:** the key poses first, then the in-betweens, for tight control over dramatic moments.
- **UI:** almost all UI motion is pose-to-pose: a start state, an end state, an easing between. Straight-ahead
  belongs to simulations: physics, particles, noise fields.
- **Motion graphics:** keyframe the story beats (pose-to-pose), and let the texture run straight ahead on top of them:
  a camera keyed on four poses, with a noise-driven handheld drift added.
- **Build:** pose-to-pose with keyframes `[[frame, value], ...]` and one easing per segment. Straight ahead as a pure
  function of the frame and a seed (`noise(seed, frame)`), never of the previous frame, so any frame renders alone.
- **Fails when:** a hand-keyed move has too many keys (it wobbles); a simulation depends on the frame before it (it
  cannot scrub or render in parallel).

## 5. Follow Through and Overlapping Action

- **Follow through:** parts keep moving after the main body has stopped (hair, a coat, a tail, a lanyard).
- **Overlapping action:** different parts move at different rates rather than starting and stopping together.
- **UI:** a list whose items arrive staggered (20 to 60 ms apart); a modal whose content settles 50 ms after its
  frame; an icon inside a button that overshoots after the button lands.
- **Motion graphics:** a headline whose words land one after another; a card whose shadow catches up; a phone whose
  screen content lags its frame by a frame or two when it whips.
- **Build:** stagger (`delay = i * step`); drag (child follows the parent through a spring or a lag of a few frames);
  overshoot on the children, not the parent. The lead element moves first and stops first.
- **Fails when:** stagger is so long the last item arrives after the user has moved on (cap the total at about
  300 ms in UI); everything overshoots by the same amount; the stagger runs in a direction unrelated to the layout.

## 6. Slow In and Slow Out (Ease In and Ease Out)

More frames at the start and end of an action, fewer in the middle, to mimic real acceleration and deceleration.

- **UI:** enter with ease-out (fast then gentle: the element arrives decisively), exit with ease-in (gentle then fast:
  it leaves without lingering), move on screen with ease-in-out. Never `linear` for spatial motion.
- **Motion graphics:** camera moves ease in and out of holds; a push-in that has to keep going past a cut is
  ease-in only, so the cut comes at speed.
- **Build:** strong curves, not CSS's default `ease`: ease-out `cubic-bezier(0.16, 1, 0.3, 1)`, ease-in
  `cubic-bezier(0.7, 0, 0.84, 0)`, ease-in-out `cubic-bezier(0.65, 0, 0.35, 1)`. Or springs (stiffness, damping),
  which handle interruption.
- **Fails when:** every move uses the same curve; linear on anything that travels; a long ease-out on a frequent
  action (it feels laggy).

## 7. Arcs

Living things and organic objects follow natural, slightly curved paths rather than rigid, mechanical straight lines.

- **UI:** an element that moves both across and down travels on a curve (offset the x and y easings, or use a path);
  a card flicked off the screen also rotates; a floating action button opens its children along an arc.
- **Motion graphics:** cameras that dolly and truck at once move on a curve; thrown objects follow a parabola; a
  pointer moving to a button curves, it does not rail.
- **Build:** different easings on x and y (x ease-out, y ease-in gives a curve), a quadratic or cubic bezier path
  sampled by progress, or rotation that follows the tangent of the path. Mechanical things (a slider, a progress bar)
  stay straight on purpose.
- **Fails when:** the arc is so wide it looks like a detour; a UI control that should feel precise wanders.

## 8. Secondary Action

Smaller supporting movements that add depth and personality to the primary action, like swinging arms while walking.

- **UI:** a success check that draws in while the button settles; a shimmer across a card as it lands; a counter that
  ticks while a bar grows.
- **Motion graphics:** dust kicked up by a landing, a light sweep across a logo, particles shed by a fast move, a
  subtle parallax in the background during a camera move.
- **Build:** lower contrast, smaller amplitude and slightly later than the primary action. It must support the primary
  action and never compete with it (see Staging).
- **Fails when:** the secondary action is bigger, brighter or earlier than the primary one; it is there on every beat.

## 9. Timing

The number of frames given to an action, which sets its speed, weight and emotional tone.

- **UI:** micro feedback 100 to 200 ms, small transitions 200 to 300 ms, large surfaces 300 to 500 ms, choreographed
  intros 800 to 1200 ms. Heavier, bigger things take longer; small things are quick.
- **Motion graphics:** think in frames. At 30 fps: a hard hit 2 to 4 frames, a snappy move 6 to 10, a considered move
  15 to 24, a hold 12 to 30 so a line can be read (about 0.3 s a word plus 0.5 s). At 60 fps double them. Cut on the
  beat of the music; land moves on it, not after it.
- **Build:** decide the timing in frames before choosing easings. Give each beat a hold: motion without rest is noise.
- **Fails when:** everything is the same duration (a robotic rhythm); text leaves before it can be read; a heavy object
  moves as fast as a light one.

## 10. Exaggeration

Pushing movement further than strict reality to enhance comedic or dramatic impact, without losing believability.

- **UI:** reserved for rare, celebratory moments: a confetti burst, an onboarding hero, an error shake (3 short
  oscillations, about 6 px). Productivity UI stays near zero (Emil).
- **Motion graphics:** a camera whip with heavy motion blur, a word that scales from 0 to 120% before settling, a
  chart bar overshooting 15% on a key number.
- **Build:** exaggerate the timing and the key pose, not every frame. Overshoot with a spring (`damping` lower) or an
  ease like `cubic-bezier(0.34, 1.56, 0.64, 1)`.
- **Fails when:** it is everywhere (nothing stands out), or it breaks the physics the piece has set up.

## 11. Solid Drawing

Giving a drawn object a true sense of three-dimensional volume, weight and balance.

- **UI:** consistent light direction across shadows, highlights and gradients; elements that tilt in 3D keep their
  perspective (`perspective: 800px` to `1200px`), with the shadow moving opposite the tilt.
- **Motion graphics:** devices and cards in 3D with a real perspective camera, depth of field that matches the focus
  distance, shadows that stay under their objects, parallax that matches depth (far moves less than near).
- **Build:** one camera and one light per scene. Derive shadow offset and blur from the object's height above the
  surface. Never fake 3D with independent scale and skew that contradict each other.
- **Fails when:** shadows point different ways; a 3D card's content is flat-scaled instead of rotated; parallax layers
  move in proportions that imply the wrong depths.

## 12. Appeal

Giving a design charisma and charm: something the audience finds interesting to watch.

- **UI:** a clear hierarchy, pleasing proportions, a restrained palette, one memorable signature motion per product
  rather than ten. Appeal is taste: what you leave out.
- **Motion graphics:** strong shapes, a limited palette, considered type, generous negative space, texture (grain,
  paper, halation) that makes the frame feel made rather than generated.
- **Build:** design the still frame first: every frame of the piece should hold up as a still. Then the motion.
- **Fails when:** stock gradients, centred everything, generic glow, eight effects at once: the AI-slop look
  (`anti-checklist.md`).

---

## Using them together

| When you are... | Lead with | Then check |
|---|---|---|
| Building a single UI transition | 6 Slow in/out, 9 Timing | 1 Squash (only on press), 5 Overlap (children) |
| Building a choreographed intro or hero | 3 Staging, 9 Timing | 2 Anticipation, 5 Overlap, 8 Secondary, 12 Appeal |
| Moving a camera | 3 Staging, 6 Slow in/out, 7 Arcs | 11 Solid drawing (parallax, depth of field) |
| Animating a character, a mark or a mascot | 1 Squash, 2 Anticipation, 5 Follow through | 7 Arcs, 10 Exaggeration |
| Animating data (charts, counters) | 9 Timing, 5 Overlap | 6 Slow in/out (land on the value), 10 Exaggeration (only the key number) |
| Auditing motion that "feels off" | 9 Timing, 6 Slow in/out | 3 Staging (too many things at once), 12 Appeal |

The principles are weighted by the same context as the designers: a productivity tool uses 6, 9 and a little 5; a
kids' app or a launch film uses all twelve.
