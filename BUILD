package(default_visibility = ["//visibility:public"])

load("@rules_cc//cc:defs.bzl", "cc_library", "cc_shared_library", "cc_import")

exports_files(["LICENSE"])

swss_common_hdrs = glob([
    "common/*.h",
    "common/*.hpp",
], allow_empty = True)

swss_common_c_api_hdrs = glob([
  "common/c-api/*.h",
])

filegroup(
    name = "hdrs",
    srcs = swss_common_hdrs + swss_common_c_api_hdrs,
)

# Not all sources are required.
# In fact, including some sources like `common/table.cpp` makes downstream targets not work
SWSS_COMMON_SRCS = [
   "common/events_common.cpp",
   "common/events_service.cpp",
   "common/events.cpp",
   "common/logger.cpp",
   "common/redisreply.cpp",
   "common/configdb.cpp",
   "common/dbconnector.cpp",
   "common/dbinterface.cpp",
   "common/sonicv2connector.cpp",
   "common/table.cpp",
   "common/json.cpp",
   "common/producertable.cpp",
   "common/producerstatetable.cpp",
   "common/zmqproducerstatetable.cpp",
   "common/rediscommand.cpp",
   "common/redistran.cpp",
   "common/redisselect.cpp",
   "common/select.cpp",
   "common/selectableevent.cpp",
   "common/selectabletimer.cpp",
   "common/consumertable.cpp",
   "common/consumertablebase.cpp",
   "common/consumerstatetable.cpp",
   "common/zmqconsumerstatetable.cpp",
   "common/ipaddress.cpp",
   "common/ipprefix.cpp",
   "common/ipaddresses.cpp",
   "common/macaddress.cpp",
   "common/netdispatcher.cpp",
   "common/netlink.cpp",
   "common/nfnetlink.cpp",
   "common/notificationconsumer.cpp",
   "common/notificationproducer.cpp",
   "common/linkcache.cpp",
   "common/portmap.cpp",
   "common/pubsub.cpp",
   "common/tokenize.cpp",
   "common/exec.cpp",
   "common/saiaclschema.cpp",
   "common/subscriberstatetable.cpp",
   "common/timestamp.cpp",
   "common/warm_restart.cpp",
   "common/luatable.cpp",
   "common/countertable.cpp",
   "common/redisutility.cpp",
   "common/restart_waiter.cpp",
   "common/profileprovider.cpp",
   "common/zmqclient.cpp",
   "common/zmqserver.cpp",
   "common/asyncdbupdater.cpp",
   "common/redis_table_waiter.cpp",
   "common/interface.h",
   "common/c-api/util.cpp",
   "common/c-api/dbconnector.cpp",
   "common/c-api/configdbconnector.cpp",
   "common/c-api/consumerstatetable.cpp",
   "common/c-api/producerstatetable.cpp",
   "common/c-api/subscriberstatetable.cpp",
   "common/c-api/zmqclient.cpp",
   "common/c-api/zmqserver.cpp",
   "common/c-api/zmqconsumerstatetable.cpp",
   "common/c-api/zmqproducerstatetable.cpp",
   "common/c-api/table.cpp",
   "common/c-api/logger.cpp",
   "common/c-api/events.cpp",
   "common/performancetimer.cpp",
]

cc_library(
    name = "common",
    srcs = SWSS_COMMON_SRCS,
    # srcs = glob(
    #     ["common/*.cpp"],
    #     ["common/loglevel.cpp", "common/loglevel_util.cpp"]
    # ),
    hdrs = swss_common_hdrs + swss_common_c_api_hdrs,
    copts = [
        "-fPIC",
        "-std=c++14",
        # TODO: this is not required with apt.installed debs.
        # "-I/usr/include/libnl3", # Expected location in the SONiC build container"
    ],
    # Not needed with apt.install
    includes = [
        "common",
    ],
    # TODO BL: if I remove this, the deb archive doesn't link. I should figure out why
    linkopts = ["-lboost_serialization"],
    # Approach 1:
    deps = [
        "@bookworm//libhiredis-dev:libhiredis",
        "@bookworm//nlohmann-json3-dev:nlohmann-json3",
        "@bookworm//libnl-3-dev:libnl-3",
        "@bookworm//libnl-route-3-dev:libnl-route-3",
        "@bookworm//libnl-nf-3-dev:libnl-nf-3",
        "@bookworm//libyang2-dev:libyang2",
        "@bookworm//libzmq3-dev:libzmq3",
        "@bookworm//uuid-dev:uuid",
        "@bookworm//libboost-dev:libboost",
        "@bookworm//libboost-serialization-dev:libboost-serialization",
    ],
    # Approach 2: BCR entries compiled from source
    # deps = [
    #     "@boost.algorithm",
    #     "@boost.serialization",
    #     "@nlohmann_json//:json",
    #     "@libuuid//:libuuid",
    #     "@swig//:swig",
    # ],
    visibility = ["//visibility:public"],
)

cc_library(
    name = "libswsscommon",
    hdrs = swss_common_hdrs + swss_common_c_api_hdrs,
    include_prefix = "swss",
    strip_include_prefix = "common",
    deps = [":common"],
)

# cc_binary(
#     name = "libswsscommon.so",
#     # hdrs = swss_common_hdrs + swss_common_c_api_hdrs,
#     # include_prefix = "swss",
#     # strip_include_prefix = "common",
#     deps = [":libswsscommon"],
#     linkstatic = False,
#     linkshared = True,
# )

# cc_import(
#   name = "libswsscommon_shared",
#   shared_library = ":libswsscommon.so",
#   # deps = [":common"],
# )
# cc_library(
#     name = "libswsscommon_shared",
#     hdrs = swss_common_hdrs + swss_common_c_api_hdrs,
#     include_prefix = "swss",
#     strip_include_prefix = "common",
#     deps = [":common"],
#     linkstatic = False,
# )

# cc_import(
#   name = "libbsd",
#   shared_library = "@bookworm//libbsd-dev:libbsd",
#   linkopts = [
#     "-Wl,--remap-inputs=/usr/lib/x86_64-linux-gnu/libbsd.so.0.11.7=$(execpath @bookworm//libbsd-dev:libbsd)",
#   ],
# )

# TODO BL: Figure out why this makes the libswsscommon_archive build successfully,
#          If I just depend on `common` it can't find libboost_serialization.
#          Is it just a matter of names, or is it just going to blow up at runtime?
#          Not even that, it's just failing at compile time because of unresolved boost symbols
cc_shared_library(
  name = "libboost_maybe",
  shared_lib_name = "libboost_serialization.so",
  deps = [
        "@bookworm//libboost-serialization-dev:libboost-serialization",
        # "@bookworm//libboost-serialization1.74-dev:libboost-serialization1.74",
  ],
)

cc_library(
  name = "testboost",
  srcs = [
    "test.c",
  ],
  linkopts = [
    "-lboost_serialization",
  ],
)

cc_import(
  name = "boost",
  deps = [
  #     "@bookworm//libboost-serialization-dev:libboost-serialization",
    "@@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:usr/lib/x86_64-linux-gnu/libboost_serialization.so",
  ],
  shared_library = "libboost_serialization.so",
)

# TODO BL: Make this nicer
# BLTMP_LIBBSD_REPO_NAME = " _U_A_Arules_Udistroless++apt+bookworm_Ulibbsd-dev-amd64_U0.11.7-2_S_S_Clibbsd_Uimport___Uexternal_Srules_Udistroless++apt+bookworm_Ulibbsd-dev-amd64_U0.11.7-2_Susr_Slib_Sx86_U64-linux-gnu"
cc_shared_library(
    name = "libswsscommon_archive",
    shared_lib_name = "libcommon.so",
    deps = [
        "//:common",
        # "//:testboost",
        # "@bookworm//libbsd-dev:libbsd",
        # "libbsd",
      # TODO BL: just to test
        # "@bookworm//libboost-dev:libboost",
        # "@bookworm//libboost-serialization-dev:libboost-serialization",
        # "@@rules_distroless++apt+bookworm_libboost1.74-dev-amd64_1.74.0-ds1-21//:libboost1.74",
        # ":boost",
        # "@bookworm//libboost-serialization1.74-dev:libboost-serialization1.74",
        "@@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:libboost-serialization1.74_wodeps",
    ],
    dynamic_deps = [
      # ":libboost_maybe",
    ],
    additional_linker_inputs = [
    #   "@bookworm//libbsd-dev:libbsd",
      # TODO BL: just to test
      # "@@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:usr/lib/x86_64-linux-gnu/libboost_serialization.so",
      # From @@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:libboost-serialization1.74_wodeps
      "@@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:usr/lib/x86_64-linux-gnu/libboost_serialization.so", "@@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:usr/lib/x86_64-linux-gnu/libboost_wserialization.so",
    ],
    user_link_flags = [
      # "-Wl,--verbose",
    #   # TODO BL: Let's pray that this works
      "-Wl,--remap-inputs=/usr/lib/x86_64-linux-gnu/libbsd.so.0.11.7=/dev/null",
      # From @@rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21//:libboost-serialization1.74_wodeps
      "-L$(BINDIR)/external/rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21/usr/lib/x86_64-linux-gnu",
      "-Wl,-rpath-link=$(BINDIR)/external/rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21/usr/lib/x86_64-linux-gnu",
      "-Wl,-rpath=/usr/lib/x86_64-linux-gnu",
      # From the same target, but with absolute paths
      "-L/home/blorente/.cache/bazel/_bazel_blorente/18e6b334ca347a689d90222d36b083c7/sandbox/linux-sandbox/28/execroot/_main/bazel-out/k8-fastbuild/bin/external/rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21/usr/lib/x86_64-linux-gnu/",
      "-Wl,-rpath-link=/home/blorente/.cache/bazel/_bazel_blorente/18e6b334ca347a689d90222d36b083c7/sandbox/linux-sandbox/28/execroot/_main/bazel-out/k8-fastbuild/bin/external/rules_distroless++apt+bookworm_libboost-serialization1.74-dev-amd64_1.74.0-ds1-21/usr/lib/x86_64-linux-gnu",
    ],
)

# cc_binary(
#     name = "libswsscommon_archive",
#     # srcs = ["py3/swsscommon_wrap.cpp"],  # Generated by SWIG
#     deps = [
#         "//:libswsscommon",
#         # "@bookworm//python3-dev:python3",
#     ],
#     # copts = ["-fvisibility=hidden", "-fPIC"],
#     linkstatic = 1,
#     linkshared = 1,
# )
cc_library(
  name = "swsscommon_hdrs",
  hdrs = [":hdrs"],
  include_prefix = "swss",
  strip_include_prefix = "common",
  includes = [
      "common",
  ],
  deps = [
        "@bookworm//libhiredis-dev:libhiredis",
        "@bookworm//nlohmann-json3-dev:nlohmann-json3",
        "@bookworm//libnl-3-dev:libnl-3",
        "@bookworm//libnl-route-3-dev:libnl-route-3",
        "@bookworm//libnl-nf-3-dev:libnl-nf-3",
        "@bookworm//libyang2-dev:libyang2",
        "@bookworm//libzmq3-dev:libzmq3",
        "@bookworm//uuid-dev:uuid",
        "@bookworm//libboost-dev:libboost",
        "@bookworm//libboost-serialization-dev:libboost-serialization",
  ],
  # strip_include_prefix = "swss",
)

cc_import(
  name = "libswsscommon_shared",
  shared_library = ":libswsscommon_archive",
  deps = [
      "//:libswsscommon",
      # ":swsscommon_hdrs",

  ],
)
#
# cc_library(
#   name = "transitive",
#   srcs = ["transitive.c"],
#   deps = [
#   ],
#   linkopts = [
#     "-Wl,from_cc_library_transitive",
#   ],
# )
#
# cc_library(
#   name = "onlyzmq",
#   srcs = ["test.c"],
#   deps = [
#         "@bookworm//libzmq3-dev:libzmq3",
#         ":transitive",
#   ],
#   linkopts = [
#     "-Wl,from_cc_library",
#   ],
# )
#
# cc_import(
#   name = "zmq_import",
#   deps = [
#     ":onlyzmq",
#   ],
#   linkopts = [
#     # "-Wl,--remap-inputs=/usr/lib/x86_64-linux-gnu/libbsd.so.0.11.7=$(execpath @bookworm//libbsd-dev:libbsd)",
#     "-Wl,from_cc_import",
#   ],
# )
#
# cc_shared_library(
#     name = "onlyzmq_archive",
#     shared_lib_name = "onlyzmq.so",
#     deps = [
#       "//:zmq_import",
#       "//:onlyzmq",
#      # "@@rules_distroless++apt+bookworm_libbsd0-amd64_0.11.7-2//:libbsd0_libbsd_wrapper"
#         # "@bookworm//libbsd-dev:libbsd",
#         # "libbsd",
#         # "@bookworm//libboost-dev:libboost",
#         # "@bookworm//libboost-serialization-dev:libboost-serialization",
#     ],
#     # additional_linker_inputs = [
#     #   "@bookworm//libbsd-dev:libbsd",
#     # ],
#     # user_link_flags = [
#     #   "-Wl,--remap-inputs=/usr/lib/x86_64-linux-gnu/libbsd.so.0.11.7=$(execpath @bookworm//libbsd-dev:libbsd)",
#     # ],
# )
#
# cc_shared_library(
#   name = "libbsd_cclib",
#   shared_lib_name = "libbsd.so.0.11.7",
#   deps = [
#     "@bookworm//libbsd-dev:libbsd",
#   ],
# )

