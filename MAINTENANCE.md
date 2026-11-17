# Personal Fork Maintenance

This repository is Daniel's Murphy M4 flavor of CrossPoint Reader. It follows
the official application as closely as possible and carries only the focused
hardware, build, and packaging differences required for the Murphy M4.

## Sources of changes

- Keep this repository's `main` branch usable on the Murphy M4.
- Treat [CrossPoint Reader](https://github.com/crosspoint-reader/crosspoint-reader)
  as the primary upstream application repository.
- CrossInk Reader may also be used as a source for suitable features and fixes.
- Rebase the focused Murphy commits onto the current official integration branch,
  presently `upstream/develop`, so the application remains straightforward to
  compare and update.
- Keep personal changes in small, focused commits so upstream changes remain easy
  to inspect, cherry-pick, adapt, or revert.
- Drop local workarounds and feature patches when official CrossPoint or FreeInk
  SDK provides the equivalent behavior.
- Preserve Murphy M4 support when resolving conflicts. Do not import unrelated
  device-specific behavior unless it is intentionally wanted here.

Suggested remotes:

```text
origin      https://github.com/czdaniel/crosspoint-murphy-m4.git
upstream    https://github.com/crosspoint-reader/crosspoint-reader.git
crossink    <official CrossInk Reader repository URL>
```

## FreeInk SDK

Use `freeink-sdk` as a submodule sourced from the official
[FreeInk SDK repository](https://github.com/Free-Ink/freeink-sdk). Pin it to a
specific public commit that contains the required Murphy M4 support so clean
clones and GitHub Actions builds are reproducible.

Do not make routine SDK modifications in this repository. Prefer updating the
submodule pointer to an appropriate official SDK commit. If an SDK change ever
becomes necessary, develop and upstream it separately, then repoint this
repository after it is publicly available.

## Releases

- Release builds must compile the `murphy_m4` PlatformIO environment.
- Release artifacts must come from `.pio/build/murphy_m4/`.
- Before tagging, verify that every submodule commit is reachable from its URL in
  `.gitmodules` and perform a clean recursive-submodule build.
- Tags are immutable historical releases. Fix a broken release on `main` and
  create a new tag rather than silently moving a published tag.

## Deferred Murphy investigations

- Touch input can intermittently require two or three taps to register. Daniel
  observed this during the original port, and multiple Murphy M4 users have
  independently reported the same behavior with CrossPoint. Investigate the
  Murphy touch-controller transaction/timing path when returning to this; do
  not assume that application gesture handling is the root cause. Compare
  normal and CNC/batch variants, behavior after idle or power-state changes,
  and firmware with reportedly better touch response before proposing an SDK
  workaround. Example reports: [October 2026 intermittent taps](https://www.reddit.com/r/ereader/comments/1wvna6t/murphy_m4_has_anyone_tried_pandacat_ai_firmware/)
  and [October 2026 repeated-tap comparison](https://www.reddit.com/r/ereader/comments/1xgnxs9/murphy_m4_the_superior_ereader/).

## Default decision rule

When there is ambiguity, prefer official CrossPoint behavior and APIs. Keep the
smallest possible Murphy-specific delta, retain Murphy M4 hardware support, and
leave FreeInk SDK on the same official, publicly reachable commit used upstream
whenever that revision supports the device.
