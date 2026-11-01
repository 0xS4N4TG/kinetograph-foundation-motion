# Motion Graphics and Video

The designer lenses come from UI. Video and motion graphics (Kinetograph, Remotion, After Effects, Motion Canvas, Rive
timelines) follow the same twelve principles (`twelve-principles.md`) but with different defaults: there is no
interaction, the viewer cannot pause or re-read, and every frame is a still that has to hold up.

## What changes from UI

| UI | Motion graphics |
|---|---|
| The user triggers motion; frequency decides how much | The edit triggers motion; the beat decides when |
| Durations in milliseconds, short | Timing in frames; holds matter as much as moves |
| Motion should go unnoticed | Motion is the content, but each beat has one subject |
| `prefers-reduced-motion` | Flashes under 3 a second; no full-frame strobes; readable holds |
| Interruptible springs | Deterministic: every frame is a pure function of the frame number |

## Rules

1. **Every frame is a function of the frame.** No timers, no state carried from the previous frame, no
   `Math.random()`: use a seeded `random(seed)` or noise of `(seed, frame)`. A frame must render the same alone, out
   of order, and in parallel.
2. **Design the stills first.** Pick 3 to 5 key frames and make each a good still (composition, type, palette). Then
   key the motion between them (pose-to-pose).
3. **One subject a beat.** Staging: the camera, the light and the depth say where to look. Everything else is quieter.
4. **Holds.** Give every line of type time to be read: about 0.3 s a word plus 0.5 s. Give every big move a settle.
5. **Cameras move like cameras.** Ease in and out of holds; dolly and truck together on an arc; add a little
   straight-ahead drift (handheld noise, under 1% of the frame) on long holds; motion blur on fast moves (a shutter of
   180 degrees is the film default).
6. **Depth is consistent.** Parallax in proportion to depth, depth of field that matches the focus distance, one light.
7. **Texture makes it made.** Grain, paper, halation, a slight vignette: subtle, and on the whole frame, not per
   element.
8. **Cut on the beat.** Moves land on the downbeat; cuts happen on it or a frame before. A move that runs into a cut
   should be at speed (ease-in only), not settling.
9. **Restraint is still the rule.** Count the effects in a shot. More than two at once (blur + glow + shake + grain +
   chromatic aberration) is slop.

## Camera moves vocabulary

| Move | What it is | Use for |
|---|---|---|
| Push in / pull out | Camera moves toward or away (or a scale about the subject) | Emphasis; reveals |
| Truck / pedestal | Camera slides sideways / up and down | Following, revealing a row |
| Orbit | Camera circles the subject, looking at it | Showing an object's form; product shots |
| Dolly zoom | Push in while widening the field of view | A dramatic beat, once |
| Whip pan | A fast pan with heavy motion blur | A cut between beats |
| Rack focus | Focus shifts from one depth to another | Moving attention without moving the camera |
| Handheld drift | Small noise on position and rotation | Life on long holds |

## Timing at 30 fps (double for 60)

| Beat | Frames |
|---|---|
| Hard hit, flash, impact | 2 to 4 |
| Snappy UI-style move | 6 to 10 |
| Considered move | 15 to 24 |
| Camera move | 24 to 60 |
| Hold for a short line | 30 to 45 |
| Stagger between items | 2 to 4 |

## Layout and arrangement

Arrangements of many items (galleries, grids, carousels) read as one object when every item follows one rule:

- **Rectangular grid, radial (ring), path (items along a curve), spherical (items on a sphere).**
- **Orientation:** items can face the camera (billboard), face forward along the path's tangent, face the centre,
  or keep a fixed orientation. Face-the-camera reads as a gallery; tangent orientation reads as a moving train.
- Animate the arrangement, not each item: rotate the ring, advance along the path, orbit the sphere, and stagger any
  per-item change by its index or its distance from the focus.
