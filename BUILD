package(default_visibility = ["//visibility:public"])

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
    # linkopts = ["-lpthread -lhiredis -lnl-genl-3 -lnl-nf-3 -lnl-route-3 -lnl-3 -lzmq -luuid -lyang"],
    includes = [
        "common",
    ],
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
