project(libowfat C)

cmake_minimum_required(VERSION 2.6)

set(CMAKE_COLOR_MAKEFILE ON)
set(CMAKE_VERBOSE_MAKEFILE ON)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

include(CheckFunctionExists)
check_function_exists(alloca HAVE_ALLOCA)
if (HAVE_ALLOCA)
    file(WRITE ${CMAKE_SOURCE_DIR}/havealloca.h "#include <stdlib.h>\n")
    file(APPEND ${CMAKE_SOURCE_DIR}/havealloca.h "#include <alloca.h>\n")
else()
    file(WRITE ${CMAKE_SOURCE_DIR}/havealloca.h "#include <stdlib.h>\n")
    file(APPEND ${CMAKE_SOURCE_DIR}/havealloca.h "#include <malloc.h>\n")
endif()

check_function_exists(sendfile HAVE_SENDFILE)
if (HAVE_SENDFILE)
    add_definitions(-DHAVE_SENDFILE)
    file(WRITE ${CMAKE_SOURCE_DIR}/havesendfile.h "")
endif()

check_function_exists(poll HAVE_POLL)
if (HAVE_POLL)
    configure_file(${CMAKE_SOURCE_DIR}/iopause.h2 ${CMAKE_SOURCE_DIR}/iopause.h)
else()
    configure_file(${CMAKE_SOURCE_DIR}/iopause.h1 ${CMAKE_SOURCE_DIR}/iopause.h)
endif()

check_function_exists(kevent HAVE_KQUEUE)
if (HAVE_KQUEUE)
    add_definitions(-DHAVE_KQUEUE)
    file(WRITE ${CMAKE_SOURCE_DIR}/havekqueue.h "")
endif()

check_function_exists(if_indextoname HAVE_N2I)
if (HAVE_N2I)
    add_definitions(-DHAVE_N2I)
    file(WRITE ${CMAKE_SOURCE_DIR}/haven2i.h "")
endif()


include(CheckSymbolExists)
check_symbol_exists(sockaddr_in6 "netinet/in.h" HAVE_IPV6)
if (HAVE_IPV6)
    add_definitions(-DLIBC_HAS_IP6)
    file(WRITE ${CMAKE_SOURCE_DIR}/haveip6.h "")
endif()

check_symbol_exists(sin6_scope_id "netinet/in.h" HAVE_SCOPE)
if (HAVE_SCOPE)
    add_definitions(-DLIBC_HAS_SCOPE_ID)
    file(WRITE ${CMAKE_SOURCE_DIR}/havescope.h "")
endif()

include(CheckTypeSize)
set(CMAKE_EXTRA_INCLUDE_FILES sys/socket.h)
check_type_size("socklen_t" SOCKLEN)
set(CMAKE_EXTRA_INCLUDE_FILES)
if (HAVE_SOCKLEN)
    file(WRITE ${CMAKE_SOURCE_DIR}/havesl.h "")
else()
    file(WRITE ${CMAKE_SOURCE_DIR}/havesl.h "typedef int socklen_t;\n")
endif()


include(CheckIncludeFiles)
check_include_files("sys/devpoll.h" HAVE_DEVPOLL)
if (HAVE_DEVPOLL)
    add_definitions(-DHAVE_DEVPOLL)
    file(WRITE ${CMAKE_SOURCE_DIR}/havedevpoll.h "")
endif()

check_include_files("sys/epoll.h" HAVE_EPOLL)
if (HAVE_EPOLL)
    add_definitions(-DHAVE_EPOLL=1)
else()
    add_definitions(-DHAVE_EPOLL=2)
endif()

set(CMAKE_BUILD_TYPE Debug)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pipe -W -Wall -O2 -fomit-frame-pointer")

include_directories(${PROJECT_SOURCE_DIR})

set(ARRAY_SRC
	${PROJECT_SOURCE_DIR}/array/array_allocate.c
	${PROJECT_SOURCE_DIR}/array/array_bytes.c
	${PROJECT_SOURCE_DIR}/array/array_cat.c
	${PROJECT_SOURCE_DIR}/array/array_cat0.c
	${PROJECT_SOURCE_DIR}/array/array_catb.c
	${PROJECT_SOURCE_DIR}/array/array_cate.c
	${PROJECT_SOURCE_DIR}/array/array_cats.c
	${PROJECT_SOURCE_DIR}/array/array_cats0.c
	${PROJECT_SOURCE_DIR}/array/array_equal.c
	${PROJECT_SOURCE_DIR}/array/array_fail.c
	${PROJECT_SOURCE_DIR}/array/array_get.c
	${PROJECT_SOURCE_DIR}/array/array_length.c
	${PROJECT_SOURCE_DIR}/array/array_reset.c
	${PROJECT_SOURCE_DIR}/array/array_start.c
	${PROJECT_SOURCE_DIR}/array/array_trunc.c
	${PROJECT_SOURCE_DIR}/array/array_truncate.c
	${PROJECT_SOURCE_DIR}/array/iarray_allocate.c
	${PROJECT_SOURCE_DIR}/array/iarray_free.c
	${PROJECT_SOURCE_DIR}/array/iarray_get.c
	${PROJECT_SOURCE_DIR}/array/iarray_init.c )

add_library(array STATIC ${ARRAY_SRC})

set(BUFFER_SRC
	${PROJECT_SOURCE_DIR}/buffer/buffer_0.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_0small.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_1.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_1small.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_2.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_close.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_feed.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_flush.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_fromsa.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get_new_token_sa.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get_new_token_sa_pred.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get_token.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get_token_pred.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get_token_sa.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_get_token_sa_pred.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_getc.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_getline.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_getline_sa.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_getn.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_getnewline_sa.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_init.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_init_free.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_mmapread.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_peek.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_put.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_put8long.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putalign.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_puterror.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_puterror2.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putflush.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putlong.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putlonglong.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putm_internal.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putm_internal_flush.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putnlflush.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_puts.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putsa.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putsaflush.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putsalign.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putsflush.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putspace.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putulong.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putulonglong.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_putxlong.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_seek.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_stubborn.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_stubborn2.c
	${PROJECT_SOURCE_DIR}/buffer/buffer_tosa.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_iam.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_info.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_infosys.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_puts.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_warn.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_warnsys.c
	${PROJECT_SOURCE_DIR}/buffer/errmsg_write.c )

add_library(buffer STATIC ${BUFFER_SRC})

set(BYTE_SRC
	${PROJECT_SOURCE_DIR}/byte/byte_chr.c
	${PROJECT_SOURCE_DIR}/byte/byte_copy.c
	${PROJECT_SOURCE_DIR}/byte/byte_copyr.c
	${PROJECT_SOURCE_DIR}/byte/byte_diff.c
	${PROJECT_SOURCE_DIR}/byte/byte_equal_notimingattack.c
	${PROJECT_SOURCE_DIR}/byte/byte_rchr.c
	${PROJECT_SOURCE_DIR}/byte/byte_zero.c )

add_library(byte STATIC ${BYTE_SRC})

set(CASE_SRC
	${PROJECT_SOURCE_DIR}/case/case_diffb.c
	${PROJECT_SOURCE_DIR}/case/case_diffs.c
	${PROJECT_SOURCE_DIR}/case/case_lowerb.c
	${PROJECT_SOURCE_DIR}/case/case_lowers.c
	${PROJECT_SOURCE_DIR}/case/case_starts.c )

add_library(case STATIC ${CASE_SRC})

set(CDB_SRC
	${PROJECT_SOURCE_DIR}/cdb/cdb.c
	${PROJECT_SOURCE_DIR}/cdb/cdb_hash.c
	${PROJECT_SOURCE_DIR}/cdb/cdb_make.c
	${PROJECT_SOURCE_DIR}/cdb/cdb_traverse.c )

add_library(cdb STATIC ${CDB_SRC})

set(DNS_SRC
	${PROJECT_SOURCE_DIR}/dns/dns_dfd.c
	${PROJECT_SOURCE_DIR}/dns/dns_domain.c
	${PROJECT_SOURCE_DIR}/dns/dns_dtda.c
	${PROJECT_SOURCE_DIR}/dns/dns_ip.c
	${PROJECT_SOURCE_DIR}/dns/dns_ip6.c
	${PROJECT_SOURCE_DIR}/dns/dns_ipq.c
	${PROJECT_SOURCE_DIR}/dns/dns_ipq6.c
	${PROJECT_SOURCE_DIR}/dns/dns_mx.c
	${PROJECT_SOURCE_DIR}/dns/dns_name.c
	${PROJECT_SOURCE_DIR}/dns/dns_nd.c
	${PROJECT_SOURCE_DIR}/dns/dns_nd6.c
	${PROJECT_SOURCE_DIR}/dns/dns_packet.c
	${PROJECT_SOURCE_DIR}/dns/dns_random.c
	${PROJECT_SOURCE_DIR}/dns/dns_rcip.c
	${PROJECT_SOURCE_DIR}/dns/dns_rcrw.c
	${PROJECT_SOURCE_DIR}/dns/dns_resolve.c
	${PROJECT_SOURCE_DIR}/dns/dns_sortip.c
	${PROJECT_SOURCE_DIR}/dns/dns_sortip6.c
	${PROJECT_SOURCE_DIR}/dns/dns_transmit.c
	${PROJECT_SOURCE_DIR}/dns/dns_txt.c )

add_library(dns STATIC ${DNS_SRC})

set(FMT_SRC
	${PROJECT_SOURCE_DIR}/fmt/fmt_8long.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_8longlong.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_asn1derlength.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_asn1dertag.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_double.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_fill.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_httpdate.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_human.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_humank.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_long.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_longlong.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_minus.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_pad.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_plusminus.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_str.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_strm_internal.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_strn.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_tohex.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_ulong.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_ulong0.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_ulonglong.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_utf8.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_xlong.c
	${PROJECT_SOURCE_DIR}/fmt/fmt_xlonglong.c )

add_library(fmt STATIC ${FMT_SRC})
