This repository demonstrates a UX bug in Bazel's output for `bazel test <target>` when `<target>` is a `cc_test` with `srcs`.

Rather than printing an individual "Compiling foo.cpp" line for each file in `cc_test`s `srcs`, as it does for `cc_library` or `cc_binary`, bazel prints a combined "Testing //:test [2s, 4s]" line. This is misleading because it's not testing at that point, and undesired because it hides useful information about what's happening with the build.

To reproduce, run `bazel test test` in this workspace.

Expected result: You see lines like "Compiling foo.cpp; 2s darwin-sandbox" and "Compiling bar.cpp; 4s darwin-sandbox".
Actual result: You see "Testing //:test [2s, 4s]" instead.

Note that "Testing" definitely does *not* indicate that test binaries are being executed, because there's only one target in this workspace and it doesn't successfully compile.
