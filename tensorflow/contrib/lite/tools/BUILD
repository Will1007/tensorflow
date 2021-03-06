package(default_visibility = [
    "//visibility:public",
])

licenses(["notice"])  # Apache 2.0

load("//tensorflow/contrib/lite:special_rules.bzl", "tflite_portable_test_suite")
load("//tensorflow:tensorflow.bzl", "tf_cc_binary")

common_copts = ["-Wall"]

py_binary(
    name = "visualize",
    srcs = ["visualize.py"],
    data = [
        "//tensorflow/contrib/lite/schema:schema.fbs",
        "//tensorflow/python:platform",
        "@flatbuffers//:flatc",
    ],
    srcs_version = "PY2AND3",
)

tf_cc_binary(
    name = "generate_op_registrations",
    srcs = ["gen_op_registration_main.cc"],
    deps = [
        "//tensorflow/contrib/lite/tools:gen_op_registration",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "gen_op_registration",
    srcs = ["gen_op_registration.cc"],
    hdrs = ["gen_op_registration.h"],
    deps = [
        "//tensorflow/contrib/lite:framework",
        "//tensorflow/contrib/lite:string",
        "@com_googlesource_code_re2//:re2",
    ],
)

cc_test(
    name = "gen_op_registration_test",
    srcs = ["gen_op_registration_test.cc"],
    data = [
        "//tensorflow/contrib/lite:testdata/0_subgraphs.bin",
        "//tensorflow/contrib/lite:testdata/2_subgraphs.bin",
        "//tensorflow/contrib/lite:testdata/empty_model.bin",
        "//tensorflow/contrib/lite:testdata/test_model.bin",
        "//tensorflow/contrib/lite:testdata/test_model_broken.bin",
    ],
    tags = [
        "no_oss",
        "tflite_not_portable_android",
        "tflite_not_portable_ios",
    ],
    deps = [
        ":gen_op_registration",
        "@com_google_googletest//:gtest",
    ],
)

cc_library(
    name = "verifier",
    srcs = ["verifier.cc"],
    hdrs = ["verifier.h"],
    deps = [
        "//tensorflow/contrib/lite:framework",
        "//tensorflow/contrib/lite:schema_fbs_version",
        "//tensorflow/contrib/lite:string_util",
        "//tensorflow/contrib/lite/schema:schema_fbs",
    ],
)

cc_test(
    name = "verifier_test",
    size = "small",
    srcs = ["verifier_test.cc"],
    tags = [
        "no_oss",
        "tflite_not_portable",
    ],
    deps = [
        ":verifier",
        "//tensorflow/contrib/lite:framework",
        "//tensorflow/contrib/lite:schema_fbs_version",
        "//tensorflow/contrib/lite/schema:schema_fbs",
        "//tensorflow/contrib/lite/testing:util",
        "//tensorflow/core:framework_lite",
        "@com_google_googletest//:gtest",
        "@flatbuffers",
    ],
)

tflite_portable_test_suite()
