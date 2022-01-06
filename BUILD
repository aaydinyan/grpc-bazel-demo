load("@io_bazel_rules_go//go:def.bzl", "go_binary", "go_library")
load("@bazel_gazelle//:def.bzl", "gazelle")

gazelle(name = "gazelle")
# gazelle:prefix go-grpc

gazelle(
    name = "gazelle-update-repos",
    args = [
        "-from_file=go.mod",
        "-to_macro=deps.bzl%go_dependencies",
        "-prune",
        "-build_file_proto_mode=disable_global",
    ],
    command = "update-repos",
)

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
