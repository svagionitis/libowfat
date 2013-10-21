project(libowfat C)

cmake_minimum_required(VERSION 2.6)

set(CMAKE_COLOR_MAKEFILE ON)
set(CMAKE_VERBOSE_MAKEFILE ON)

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
