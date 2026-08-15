# Personal Fork Maintenance

This repository is Daniel's personal Murphy M4 flavor of CrossPoint Reader. It is
a playground for private-use features and experiments. Other people may use it,
but compatibility with Daniel's device and preferences takes priority over
behaving like an independently maintained upstream distribution.

## Sources of changes

- Keep this repository's `main` branch usable on the Murphy M4.
- Treat [CrossPoint Reader](https://github.com/crosspoint-reader/crosspoint-reader)
  as the primary upstream application repository.
- CrossInk Reader may also be used as a source for suitable features and fixes.
- Fetch and cherry-pick focused commits from either project instead of replacing
  this repository wholesale or coupling its history to another fork.
- Keep personal changes in small, focused commits so upstream changes remain easy
  to inspect, cherry-pick, adapt, or revert.
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

## Default decision rule

When there is ambiguity, prefer a small application-level change in this
repository, retain Murphy M4 behavior, and leave FreeInk SDK on an official
publicly reachable commit.
