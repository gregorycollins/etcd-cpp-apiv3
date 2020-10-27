# -*- bazel -*-

package(default_visibility = ["//visibility:public"])

load("@rules_proto//proto:defs.bzl", "proto_library")
load("@rules_cc//cc:defs.bzl", "cc_binary", "cc_library", "cc_proto_library")
load("@com_github_grpc_grpc//bazel:cc_grpc_library.bzl", "cc_grpc_library")

proto_library(
    name = "gogo_proto",
    srcs = ["proto/gogoproto/gogo.proto"],
    strip_import_prefix = "proto",
    deps = ["@com_google_protobuf//:descriptor_proto"],
)
cc_proto_library(name = "gogo_cc_proto", deps = [":gogo_proto"])

proto_library(
    name = "http_proto",
    strip_import_prefix = "proto",
    srcs = ["proto/google/api/http.proto"],
)
cc_proto_library(name = "http_cc_proto", deps = [":http_proto"])

proto_library(
    name = "annotations_proto",
    strip_import_prefix = "proto",
    srcs = ["proto/google/api/annotations.proto"],
    deps = [
        "@com_google_protobuf//:descriptor_proto",
        ":http_proto",
    ],
)
cc_proto_library(name = "annotations_cc_proto", deps = [":annotations_proto"])

proto_library(
    name = "etcdserver_proto",
    srcs = ["proto/etcdserver.proto"],
    strip_import_prefix = "proto",
    deps = [
        ":gogo_proto",
        ":annotations_proto",
        ":http_proto",
    ]
)
cc_proto_library(name = "etcdserver_cc_proto", deps = [":etcdserver_proto"])

proto_library(
    name = "kv_proto",
    srcs = ["proto/kv.proto"],
    strip_import_prefix = "proto",
    deps = [
        ":gogo_proto",
    ]
)
cc_proto_library(name = "kv_cc_proto", deps = [":kv_proto"])

proto_library(
    name = "auth_proto",
    srcs = ["proto/auth.proto"],
    strip_import_prefix = "proto",
    deps = [
        ":gogo_proto",
    ]
)
cc_proto_library(name = "auth_cc_proto", deps = [":auth_proto"])

proto_library(
    name = "rpc_proto",
    strip_import_prefix = "proto",
    srcs = ["proto/rpc.proto"],
    deps = [
        ":gogo_proto",
        ":annotations_proto",
        ":kv_proto",
        ":auth_proto",
    ]
)
cc_proto_library(name = "rpc_cc_proto", deps = [":rpc_proto"])

proto_library(
    name = "v3lock_proto",
    strip_import_prefix = "proto",
    srcs = ["proto/v3lock.proto"],
    deps = [
        ":gogo_proto",
        ":annotations_proto",
        ":http_proto",
        ":rpc_proto",
        ":etcdserver_proto",
    ],
)
cc_proto_library(name = "v3lock_cc_proto", deps = [":v3lock_proto"])

cc_grpc_library(
    name = "rpc_cc_grpc",
    srcs = [":rpc_proto"],
    grpc_only = True,
    deps = [":rpc_cc_proto"],
)
cc_grpc_library(
    name = "v3lock_cc_grpc",
    srcs = [":v3lock_proto"],
    grpc_only = True,
    deps = [":v3lock_cc_proto", ":rpc_cc_grpc"],
)

cc_library(
    name = "all_proto",
    srcs = [],
    linkstatic = True,
    deps = [
        ":auth_cc_proto",
        ":etcdserver_cc_proto",
        ":kv_cc_proto",
        ":rpc_cc_proto",
        ":v3lock_cc_proto",
        ":rpc_cc_grpc",
        ":v3lock_cc_grpc",
    ])

cc_library(
    name = "etcd-cpp-apiv3",
    srcs = glob(["src/**/*.cpp"]),
    deps = [
        ":all_proto",
        "@boost//:boost",
        "@boringssl//:ssl",
        "@cpprest//:cpprest",
        "@com_github_grpc_grpc//:grpc++",
        "@com_github_grpc_grpc//:grpc++_reflection",
        "@com_github_grpc_grpc//:gpr",
        ])
