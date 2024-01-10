workspace(
    name = "tflm-multiplatform",
)

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Depend on Pigweed's toolchain definitions. In the Pigweed project's example,
# echo, they use Pigweed as as git submodule. Here we use git_repository() to
# avoid the minor hassle of git submodules.
PIGWEED_COMMITISH="a096b21292368087aa6afcc52cce245c067720fa"
git_repository(
    name = "pigweed",
    remote = "https://pigweed.googlesource.com/pigweed/pigweed",
    commit = PIGWEED_COMMITISH,
)

# Pigweed's register_pigweed_cxx_toolchains() called below requires a
# repository named @pw_toolchain be defined. Presumably this will be handled
# internally in a future version of Pigweed? @pw_toolchain is also used
# explicitly when defining platform()s. Note we're cloning the Pigweed
# repository a second time here.
git_repository(
    name = "pw_toolchain",
    remote = "https://pigweed.googlesource.com/pigweed/pigweed",
    commit = PIGWEED_COMMITISH,
    strip_prefix = "pw_toolchain_bazel",
)

# Create a [CPID][] client repository via which Pigweed's toolchain definitions
# will download binary packages containing required toolchains.
load( "@pigweed//pw_env_setup/bazel/cipd_setup:cipd_rules.bzl", "cipd_client_repository")
cipd_client_repository()

# Register Pigweed-defined toolchains. Note using Pigweed-defined toolchains
# causes the Linux x86_64 build to use Pigweed's clang++ toolchain---rather
# than the system C++ toolchain, as does the current TFLM Bazel build.
load("@pigweed//pw_toolchain:register_toolchains.bzl", "register_pigweed_cxx_toolchains")
register_pigweed_cxx_toolchains()

# References:
# [CIPD]: https://chromium.googlesource.com/infra/luci/luci-go/+/HEAD/cipd/README.md
