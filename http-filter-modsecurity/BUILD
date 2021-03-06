load(
    "@envoy//bazel:envoy_build_system.bzl",
    "envoy_cc_extension",
    "envoy_cc_library",
    "envoy_cc_test",
    "envoy_package",
)

load("@envoy_api//bazel:api_build_system.bzl", "api_proto_package")

envoy_package()

api_proto_package(
    deps = [
        "@envoy_api//envoy/api/v2/core:pkg",
        "@envoy_api//envoy/config/core/v3:pkg",
        "@com_github_cncf_udpa//udpa/annotations:pkg",
    ]
)

envoy_cc_library(
    name = "webhook_fetch_lib",
    srcs = ["webhook_fetcher.cc"],
    hdrs = ["webhook_fetcher.h"],
    repository = "@envoy",
    deps = [
        "@envoy_api//envoy/config/core/v3:pkg_cc_proto",
        "@envoy//source/exe:envoy_common_lib",
    ],
)

envoy_cc_library(
    name = "modsecurity_filter_lib",
    srcs = ["modsecurity_filter.cc", "utility.cc"],
    hdrs = ["modsecurity_filter.h", "utility.h", "webhook_fetcher.h"],
    repository = "@envoy",
    deps = [
        ":pkg_cc_proto",
        ":webhook_fetch_lib",
        "@envoy//source/extensions/filters/http/common:jwks_fetcher_lib",
        "@modsecurity//:lib",
        "@envoy//source/exe:envoy_common_lib",
    ],
)

envoy_cc_extension(
    name = "config",
    srcs = ["config.cc"],
    hdrs = ["config.h"],
    security_posture = "unknown",
    repository = "@envoy",
    deps = [
        ":modsecurity_filter_lib",
        "@envoy//include/envoy/registry",
        "@envoy//source/common/protobuf:utility_lib",
        "@envoy//include/envoy/network:filter_interface",
        "@envoy//include/envoy/server:filter_config_interface",
        "@envoy//source/extensions/filters/http/common:factory_base_lib",
    ],
)

envoy_cc_test(
    name = "modsecurity_filter_integration_test",
    srcs = ["modsecurity_filter_integration_test.cc"],
    repository = "@envoy",
    deps = [
        ":modsecurity_filter_config",
        "@envoy//test/integration:http_integration_lib",
    ],
)
