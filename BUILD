# Define the platform names that can be specified with --platforms (in our
# case, indireclty via --config=$PLATFORM). Platforms are defined in terms of
# *constraints*. See https://bazel.build/extending/platforms#constraints-platforms.
#
# The constraint_values selected by specifying a particular --platform help
# Bazel select the appropriate toolchain from amongst the toolchains configured
# in WORKSPACE.
platform(
    name = "x86_64",
    constraint_values = [
        "@platforms//os:linux",
        "@platforms//cpu:x86_64",
    ],
)
#
platform(
    name = "arm_cortex_m",
    constraint_values = [
        "@platforms//os:none",
        "@platforms//cpu:armv7e-m",
        "@pw_toolchain//constraints/arm_mcpu:cortex-m4+nofp",
    ],
)

# cc_ target definitions. Note the goal is that they have few or no platform
# dependences in target definitions. If such dependences become widely
# required, define a macro surrounding cc_* calls which set these dependencies
# in one place.
cc_binary(
    name = "hello",
    linkopts = [ "--specs=rdimon.specs", ],
       # This spec is need when linking for embedded targets, so syscalls
       # calls like write() are semihosted and trapped by qemu.
       #
       # This linkopts doesn't cause a faulure on x86_64, but it is unneeded
       # and causes a compiler warning. Fix so that it is only invoked when
       # needed. Other platforms' toolchains my require yet different options.
    srcs = [ "hello.cc", ],
)

cc_test(
    name = "hello_test",
    linkopts = [ "--specs=rdimon.specs", ],
       # See comment on target `hello`.
    srcs = [ "hello.cc", ],
    size = "small",
)
