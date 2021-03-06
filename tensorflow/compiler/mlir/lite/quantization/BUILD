load("//tensorflow:tensorflow.bzl", "tf_native_cc_binary")
load(
    "//tensorflow/core/platform:default/build_config.bzl",
    "tf_proto_library",
)

package(
    default_visibility = [
        ":friends",
    ],
    licenses = ["notice"],  # Apache 2.0
)

package_group(
    name = "friends",
    includes = ["//third_party/mlir:subpackages"],
    packages = ["//tensorflow/compiler/mlir/..."],
)

exports_files([
    "quantization_traits.h",
    "quantization_config.h",
])

filegroup(
    name = "quantization_td_files",
    srcs = [
        "quantization.td",
        "@llvm-project//mlir:OpBaseTdFiles",
        "@llvm-project//mlir:QuantizationOpsTdFiles",
    ],
)

tf_proto_library(
    name = "quantization_info_proto",
    srcs = [
        "quantization_info.proto",
    ],
    cc_api_version = 2,
)

cc_library(
    name = "quantization_passes",
    srcs = [
        "import_quant_stats_pass.cc",
    ],
    hdrs = [
        "quantization_passes.h",
    ],
    deps = [
        ":quantization_info_proto_cc",
        "//tensorflow/compiler/mlir/tensorflow:import_utils",
        "//tensorflow/core:lib_proto_parsing",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
        "@llvm-project//llvm:support",
        "@llvm-project//mlir:Analysis",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:StandardOps",
        "@llvm-project//mlir:Support",
    ],
    alwayslink = 1,
)

cc_library(
    name = "quantization_lib",
    srcs = [
        "quantization_driver.cc",
        "quantization_utils.cc",
    ],
    hdrs = [
        "quantization_traits.h",
        "quantization_utils.h",
    ],
    deps = [
        "//tensorflow/core:lib_proto_parsing",
        "@com_google_absl//absl/memory",
        "@llvm-project//llvm:support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:StandardOps",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "quantization_config",
    srcs = [
        "quantization_config.cc",
    ],
    hdrs = [
        "quantization_config.h",
    ],
    deps = [
        "//tensorflow/core:protos_all_cc",
        "@com_google_absl//absl/strings",
        "@llvm-project//llvm:support",
    ],
)

tf_native_cc_binary(
    name = "op_quant_spec_getters_gen",
    srcs = [
        "tools/op_quant_spec_getters_gen.cc",
    ],
    deps = [
        "@llvm-project//llvm:support",
        "@llvm-project//llvm:tablegen",
        "@llvm-project//mlir:TableGen",
    ],
)
