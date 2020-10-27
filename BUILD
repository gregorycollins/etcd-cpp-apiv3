# -*- bazel -*-

package(default_visibility = ["//visibility:public"])

load("@rules_proto//proto:defs.bzl", "proto_library")
load("@rules_cc//cc:defs.bzl", "cc_binary", "cc_library", "cc_proto_library")
load("@com_github_grpc_grpc//bazel:cc_grpc_library.bzl", "cc_grpc_library")

proto_library(
    name = "protos",
    srcs = [
        "proto/gogoproto/gogo.proto",
        "proto/google/api/annotations.proto",
        "proto/google/api/http.proto",
        "proto/etcdserver.proto",
        "proto/kv.proto",
        "proto/auth.proto",
        "proto/rpc.proto",
        "proto/v3lock.proto",
    ],
    strip_import_prefix = "proto",
    deps = ["@com_google_protobuf//:descriptor_proto"],
)
cc_proto_library(name = "cc_protos", deps = [":protos"])

cc_grpc_library(
    name = "rpc_cc_grpc",
    srcs = [":protos"],
    grpc_only = True,
    deps = [":cc_protos"],
)

cc_library(
    name = "etcd-cpp-apiv3",
    linkstatic = True,
    srcs = glob(["src/**/*.cpp"]),
    deps = [
        ":cc_protos",
        "@boost//:boost",
        "@boringssl//:ssl",
        "@cpprest//:cpprest",
        "@com_github_grpc_grpc//:grpc++",
        "@com_github_grpc_grpc//:grpc++_reflection",
        "@com_github_grpc_grpc//:gpr",
        ])
