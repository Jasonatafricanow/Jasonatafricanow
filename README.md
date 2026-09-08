# Jiasen Wang

I build long-lived AI systems around continuity, authority, and longitudinal
understanding rather than larger prompts.

## What I Work On

My work focuses on turning ambiguous cognition questions into inspectable
systems with explicit boundaries:

- durable runtime state and authority;
- longitudinal structure and learning across time;
- observable presence without giving the projection ownership of cognition.

## Projects

### [Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)

A stateful cognition runtime for long-lived AI agents. MR owns runtime
identity, admission, persistence, memory consumption, telemetry, and the
authority boundary from which model inference continues.

### [LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)

A contract-first engine and research surface for structures learned across
time. LCE is independent from MR and does not automatically become current
runtime authority.

### [MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat)

A spatial presence experiment that extends inspection beyond dashboards while
remaining projection-only. Habitat does not own cognition, memory, or the
primary interaction surface.

## System Map

```text
Agent host / interaction
          |
          v
   Mind Runtime  ------ optional MR-side boundary ------>  LCE
          |
          v
    Observation
          |
          v
      Habitat
   spatial projection
```

## Selected Architecture Decisions

- [Building Long-Lived AI Systems](ARCHITECTURE-JOURNEY.md)
- [Independent portfolio review](PORTFOLIO-INDEPENDENT-REVIEW.md)
- Memory is not understanding.
- Retrieval is not authority.
- LCE became a separate engine rather than another MR pipeline.
- Observation Window became a spatial-presence question, not a virtual-world
  platform.

## Engineering Philosophy

Build for the original problem, not for the maximum available capability.

Capability is not the same as usefulness. Architecture should preserve the
target function before expanding the feature surface.
