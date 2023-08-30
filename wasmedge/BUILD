load("@io_bazel_rules_go//go:def.bzl", "go_library", "go_test")

cc_library(
    name = "wasmedge",
    srcs = select({
        "@io_bazel_rules_go//go/platform:linux_amd64": ["build/lib/vm/libwasmedgeVM.a"]
    }),
    hdrs = glob(["build/include/**/*.h"]),
    includes = ["build/include"],
    visibility = ["//visibility:public"],
)

go_library(
    name = "go_default_library",
    srcs = [
	    "ast.go",
	    "async.go",
	    "callframe.go",
	    "cgo.go",
	    "chostfunc.go",
	    "cli.go",
	    "compiler.go",
	    "configure.go",
	    "executor.go",
	    "hostfunc.go",
	    "instance.go",
	    "limit.go",
	    "limit_test.go",
	    "loader.go",
	    "log.go",
	    "plugin.go",
	    "result.go",
	    "statistics.go",
	    "store.go",
	    "string.go",
	    "validator.go",
	    "value.go",
	    "version.go",
	    "vm.go"
    ],
    cdeps = [":wasmedge"],  
    cgo = True,
    clinkopts = select({
        "@io_bazel_rules_go//go/platform:windows": [
            "-luserenv -lole32 -lntdll -lws2_32 -lkernel32 -lbcrypt",
        ],
        "//conditions:default": ["-lm -ldl"],
    }),
    copts = [
        "-Ibuild/include",
    ] + select({
        "@io_bazel_rules_go//go/platform:windows": [
            "-DWASM_API_EXTERN= -DWASI_API_EXTERN=",
        ],
        "//conditions:default": [],
    }),
    importpath = "https://github.com/arnabnandikgp/WasmEdge-go/wasmedge",
    visibility = ["//visibility:public"],
)

go_test(
    name = "go_default_test",
    srcs = glob(["**/*_test.go"]),
    embed = [":go_default_library"],
    deps = [
        "@com_github_stretchr_testify//require:go_default_library",
    ]
)
