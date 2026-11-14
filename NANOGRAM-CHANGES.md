# Nanogram changes to TelegramMessenger/quickjs

Branch `nanogram` = TelegramMessenger/quickjs `b526a817` (the macOS app's pin; quickjs-ng v0.17.0 plus the
tdesktop fixes) plus the host API the Nanogram iOS message-widget runtime needs. One commit per change, numbered as
in the iOS app's `third-party/quickjs-ng/VENDOR.md` (changes 1, 2 and 11 were reverted there; change 8 is already in
b526a817). Every change is inert unless the host calls the new API, except change 12.

| iOS change | Commit subject |
| --- | --- |
| 3 | Host auto-init properties (JS_SetHostAutoInitFunc, JS_DefineHostAutoInitProperty) |
| 4 | Class prototypes on first use (JS_SetClassProtoInitFunc) |
| 5 | String access for the DOM (JS_GetStringLength, JS_FlattenString, JS_GetStringView, JS_SubString, JS_ConcatStrings2) |
| 6 | A host-class global object (JS_SetGlobalObjectClass) |
| 7 | Host time and randomness (JS_SetWallClockFunc, JS_SetRandomSeed) |
| 8 | Not needed: b526a817 already propagates an uncatchable error out of a Promise executor |
| 9 | A host hook at the memory limit (JS_SetMemoryLimitFunc) |
| 10 | The host's stop (JS_SetStopped), ported onto this fork's `uncatchable_pending` model (see below) |
| 12 | Dumps only with `-DTELEGRAM_QJS_ENABLE_DUMPS` |

## Provenance proof

`diff(quickjs-ng v0.17.0 quickjs.c, iOS quickjs.c)` was three-way merged onto b526a817 (`git merge-file`). The
result and this branch's `quickjs.c` differ only in change 10 (below) and one comment. `quickjs.h`: the merge is
clean and equal to this branch.

## Differences from the iOS copy

- Change 10: b526a817 reworked uncatchable errors (`rt->uncatchable_pending`, `js_uncatchable_pending(ctx)` at every
  catch, reaction, executor and disposer site), so the iOS patch's per-site edits are not applied. `JS_SetStopped`
  sets `rt->stopped` and the pending state, and `js_clear_uncatchable_pending` never clears it afterwards. After the
  stop every poll throws the uncatchable "interrupted" error itself instead of asking the interrupt handler again.
- Change 1 (the `#ifndef JS_INTERRUPT_COUNTER_INIT` guard) is not carried: nothing sets the macro any more.
- Change 12 also keeps this fork's `QJS_DISABLE_DUMPS` override. This fork's own Debug builds and `qjs -d` need
  `TELEGRAM_QJS_ENABLE_DUMPS` to get the dumps.
