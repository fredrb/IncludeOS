set(IOS ${CMAKE_SOURCE_DIR}/..)

set(NET_SOURCES
  ${IOS}/src/net/buffer_store.cpp
  ${IOS}/src/net/checksum.cpp
  ${IOS}/src/net/super_stack.cpp
  ${IOS}/src/net/inet4.cpp

  ${IOS}/src/net/ethernet/ethernet.cpp
  ${IOS}/src/net/ip4/arp.cpp
  ${IOS}/src/net/ip4/ip4.cpp
  ${IOS}/src/net/ip4/reassembly.cpp

  ${IOS}/src/net/tcp/tcp.cpp
  ${IOS}/src/net/tcp/connection.cpp
  ${IOS}/src/net/tcp/connection_states.cpp
  ${IOS}/src/net/tcp/write_queue.cpp
  ${IOS}/src/net/tcp/read_buffer.cpp
  ${IOS}/src/net/tcp/rttm.cpp
  ${IOS}/src/net/tcp/listener.cpp
  ${IOS}/src/net/ip4/icmp4.cpp
  ${IOS}/src/net/ip4/udp.cpp
  ${IOS}/src/net/ip4/udp_socket.cpp

  ${IOS}/src/net/dns/dns.cpp
  ${IOS}/src/net/dns/client.cpp
  ${IOS}/src/net/dhcp/dh4client.cpp
  ${IOS}/src/net/dhcp/dhcpd.cpp

  ${IOS}/src/net/conntrack.cpp
  ${IOS}/src/net/nat/nat.cpp
  ${IOS}/src/net/nat/napt.cpp

  ${IOS}/src/net/http/header.cpp
  ${IOS}/src/net/http/header_fields.cpp
  ${IOS}/src/net/http/message.cpp
  ${IOS}/src/net/http/request.cpp
  ${IOS}/src/net/http/response.cpp
  ${IOS}/src/net/http/status_codes.cpp
  ${IOS}/src/net/http/time.cpp
  ${IOS}/src/net/http/version.cpp
  ${IOS}/src/net/http/mime_types.cpp
  ${IOS}/src/net/http/cookie.cpp
  ${IOS}/src/net/http/client_connection.cpp
  ${IOS}/src/net/http/client.cpp
  ${IOS}/src/net/http/server_connection.cpp
  ${IOS}/src/net/http/server.cpp
  ${IOS}/src/net/http/response_writer.cpp
  ${IOS}/src/net/ws/websocket.cpp
)
if (CUSTOM_BOTAN)
  set(NET_SOURCES ${NET_SOURCES}
      "${IOS}/src/net/https/botan_server.cpp")
endif()

set(OS_SOURCES
    ${IOS}/src/fs/dirent.cpp
    ${IOS}/src/fs/disk.cpp
    ${IOS}/src/fs/fat.cpp
    ${IOS}/src/fs/fat_sync.cpp
    ${IOS}/src/fs/fat_async.cpp
    ${IOS}/src/fs/filesystem.cpp
    ${IOS}/src/fs/mbr.cpp
    ${IOS}/src/fs/path.cpp
    ${IOS}/src/kernel/cpuid.cpp
    ${IOS}/src/kernel/events.cpp
    ${IOS}/src/kernel/os.cpp
    ${IOS}/src/kernel/rdrand.cpp
    ${IOS}/src/kernel/rng.cpp
    ${IOS}/src/kernel/timers.cpp
    ${IOS}/src/util/async.cpp
    ${IOS}/src/util/logger.cpp
    ${IOS}/src/util/sha1.cpp
    ${IOS}/src/util/statman.cpp
    ${IOS}/src/util/path_to_regex.cpp
    ${IOS}/src/util/percent_encoding.cpp
    ${IOS}/src/util/uri.cpp
  )

set(MOD_SOURCES
    ${IOS}/lib/mana/src/server.cpp
    ${IOS}/lib/mana/src/request.cpp
    ${IOS}/lib/mana/src/response.cpp
    ${IOS}/lib/mana/src/attributes/cookie_jar.cpp
    ${IOS}/lib/mana/src/middleware/butler.cpp
    ${IOS}/lib/mana/src/middleware/cookie_parser.cpp
    ${IOS}/lib/mana/src/middleware/director.cpp
    ${IOS}/lib/mana/src/middleware/parsley.cpp
    ${IOS}/lib/mana/src/components/dashboard/dashboard.cpp
  )

add_library(includeos STATIC ${NET_SOURCES} ${OS_SOURCES} ${MOD_SOURCES})
set_target_properties(includeos PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(includeos PUBLIC
    ${IOS}/lib/mana/include
    ${IOS}/mod/rapidjson/include)

add_library(http_parser STATIC "${IOS}/mod/http-parser/http_parser.c")
set_target_properties(http_parser PROPERTIES LINKER_LANGUAGE C)

install(TARGETS includeos   DESTINATION $ENV{INCLUDEOS_PREFIX}/includeos/linux)
install(TARGETS http_parser DESTINATION $ENV{INCLUDEOS_PREFIX}/includeos/linux)
