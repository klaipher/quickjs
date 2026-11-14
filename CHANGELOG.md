# quickjs (Nanogram fork) changelog

Branch `nanogram`. Base: TelegramMessenger/quickjs `b526a817`. Newest first. Details per change:
`NANOGRAM-CHANGES.md`.

## Unreleased

- Records: this file.
- Dumps only with `-DTELEGRAM_QJS_ENABLE_DUMPS`.
- `JS_SetStopped`: after the host stops a script, nothing in it can catch the stop.
- `JS_SetMemoryLimitFunc`: a host hook when the memory limit refuses an allocation.
- `JS_SetWallClockFunc`, `JS_SetRandomSeed`: host time and randomness for deterministic tests.
- `JS_SetGlobalObjectClass`: a host class for the global object.
- String access for the DOM: `JS_GetStringLength`, `JS_FlattenString`, `JS_GetStringView`, `JS_SubString`, `JS_ConcatStrings2`.
- `JS_SetClassProtoInitFunc`: class prototypes on first use.
- `JS_SetHostAutoInitFunc`, `JS_DefineHostAutoInitProperty`: host auto-init properties.
