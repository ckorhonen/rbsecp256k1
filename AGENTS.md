# Repository Guide

- This Ruby native-extension gem wraps bundled or system `libsecp256k1`; its gemspec and extension sources define the build.
- Use `make setup`, then `make build` and `make test`. `make lint` runs RuboCop; `make memcheck` requires Valgrind and is reserved for native-memory investigation.
- Do not run `make clean` or `make uninstall` as routine validation because they remove generated artifacts or installed gems.

Run Make targets from the root with Ruby/Bundler and a native C build toolchain. The checked-in CI exercises Ruby 2.7/3.0; consult that legacy matrix before assuming a modern runtime works. Native prerequisites documented in README include automake, libtool, pkg-config, GMP, libffi, and OpenSSL plus compiler/development headers. `ext/rbsecp256k1/` implements the extension, `lib/` exposes Ruby APIs, and `spec/` verifies behavior.

The default `extconf.rb` build downloads a pinned libsecp256k1 archive and checks its SHA-256; a missing cache/network or native dependency is a build prerequisite, not a test failure in the changed Ruby logic. A system-library path exists, but inspect the actual extconf option and library compatibility before selecting it. Complete relevant changes through `make test` (which already depends on `build`) and `make lint`; avoid repeating a separate build without need. Preserve native ownership/cryptographic semantics and unrelated work, and report executed specs and unverified platform or memory checks.
