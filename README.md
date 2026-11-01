# Motion Canon

A motion design skill for coding agents (Claude Code, Codex, Cursor and others that read `SKILL.md`) that covers UI
motion and motion graphics. It combines Disney's twelve principles of animation with the published techniques of
Emil Kowalski, Jakub Krehel and Jhey Tompkins.

It has two modes:

- **Create**: build components, transitions, kinetic type, galleries, charts and camera moves with purposeful motion.
- **Audit**: review existing motion and report what breaks, naming the principle each finding breaks.

## Install

```sh
npx skills add 0xS4N4TG/motion-canon
```

## What is in it

- `skills/motion-canon/SKILL.md`: the modes, the twelve principles, the three designer lenses and how context weights them, the frequency
  gate, duration guidelines and accessibility.
- `skills/motion-canon/references/twelve-principles.md`: each principle for UI and for motion graphics, with how to build it and how it
  fails, and a table of which to lead with for each kind of work.
- `skills/motion-canon/references/motion-graphics.md`: what changes for video. It covers deterministic frames, timing in frames, a camera
  move vocabulary, holds, cutting on the beat, and arrangements (grids, rings, paths, spheres and how items face the
  camera).
- The rest is from the original skill:
  - the per-designer references;
  - the motion cookbook and creation gotchas;
  - the audit checklist and anti-checklist;
  - accessibility and performance;
  - the HTML audit report.

## Credit

This skill is built on [design-motion-principles](https://github.com/kylezantos/design-motion-principles) by Kyle Zantos
(MIT). The designer lenses distil the designers' public work and are not endorsed by them. The twelve principles are
from Frank Thomas and Ollie Johnston's *The Illusion of Life: Disney Animation* (1981).

MIT License. See `LICENSE`.
