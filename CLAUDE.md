IMPORTANT: Ensure you’ve thoroughly reviewed the [AGENTS.md](AGENTS.md) file before beginning any work.

---

## Haiku OS port (this fork)

This fork carries a Haiku OS build patch on top of upstream. See [AGENTS.md > Haiku port](AGENTS.md#haiku-port) for the full list of modified files and the reasoning behind each change. Key constraints when working in this fork:

- Haiku has no `<sys/syslimits.h>`; use `<limits.h>` for `PATH_MAX`.
- The BSD sockets API lives in `libnetwork`, not `libc`. Any new target using sockets must link `network` under a `CMAKE_SYSTEM_NAME STREQUAL "Haiku"` guard.
- `pthread_attr_setinheritsched` is declared but unimplemented on Haiku (wrapped in `#if 0` in `pthread.h`); guard any call to it with `!defined(__HAIKU__)`.
- The upstream `fs_get_cache_directory()` platform list must include `__HAIKU__` to hit the XDG `~/.cache` path rather than the `#error Unknown architecture` fallback.
