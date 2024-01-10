# tflm-multiplatform

This is a sandbox for exploring the mechanisms for building for multiple
platforms via Bazel. Commentary is liberally given throughout the
implementation to explain how things work.

Building for multiple platforms is inspired by the Pigweed Project's [work on
the topic](https://pigweed.dev/docs/get_started/bazel.html), and depends on
toolchain definitions in Pigweed's `pw_toolchain` module.

# Running

To build and test for Linux x86_64:

    bazel build ...
    bazel test ...

To build and test for an embedded target:

    bazel build --config=arm_cortex_m ...
    bazel test --config=arm_cortex_m ...   # tests via qemu

# Breakage

Note that Pigweed's Bazel toolchain definitions and precompiled toolchains are
a bit of a moving target, and even using a fixed commitish of Pigweed doesn't
seem to insulate from changes in, e.g., the precompiled toolchains. Expect the
integration to break and require maintenance as Pigweed's Bazel support
evolves.

# Status and TODO

Currently the build only works for platforms x86_64/linux and
arm-cortex-m/none. Beyond some cleanup, it next needs to explore elegant ways
of switching in optimized code when it is available for a platform. Farther in
the future is incorporating more obscure toolchains not yet available in Bazel.
