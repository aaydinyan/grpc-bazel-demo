# grpc-bazel-demo

This is a demonstration of GRPC service Bazel build

## Bazel Build & Run
<b>Note: Remove any Protobuf local installation such as brew before proceeding to Bazel build</b>

### Building Go projects from scratch - skip this if you want build as-is
```bash
$ bazel clean
$ rm -f gen/v1/BUILD.bazel
$ rm -f v1/BUILD.bazel
```
```build
# remove these lines from WORKSPACE file
load("//:deps.bzl", "go_dependencies")

# gazelle:repository_macro deps.bzl%go_dependencies
go_dependencies()
```
```build
# remove these lines from BUILD file
go_library(
    name = "go-grpc_lib",
    srcs = ["main.go"],
    importpath = "go-grpc",
    visibility = ["//visibility:private"],
    deps = [
        "//api/gen/v1:gen",
        "@com_github_rookie_ninja_rk_boot//:rk-boot",
        "@com_github_rookie_ninja_rk_grpc//boot",
        "@org_golang_google_grpc//:go_default_library",
    ],
)

go_binary(
    name = "go-grpc",
    data = [
        "boot.yaml",
        "//api/gen/v1:gen",
    ],
    embed = [":go-grpc_lib"],
    visibility = ["//visibility:public"],
)
```

```bash
# run gazelle to manage go and external dependencies defined in go.mod file
$ bazel run //:gazelle
$ bazel run //:gazelle-update-repos
# gazelle will add BUILD files under each dir and modify existing WORKSPACE and BUILD files
````
```build
# fix data dependencies in main BUILD file under project root. (needed for runtime) 
go_binary(
    name = "go-grpc",
    data = [
        "boot.yaml",
        "//api/gen/v1:gen",
    ],
    embed = [":go-grpc_lib"],
    visibility = ["//visibility:public"],
)
```

### Building & Running
```bash
# Compile project 
$ bazel build ...

# Run grpc service
$ bazel run //:go-grpc
```
