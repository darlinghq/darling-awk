project(awkexe)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

include(darling_exe)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
# add_definitions(-DSUPPORT_UTMPX)

# Hide warnings
add_definitions(
	-Wno-pointer-sign
	-Wno-format
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")
set(CMAKE_EXE_LINKER_FLAGS "-nodefaultlibs -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")
#set(CMAKE_SHARED_LINKER_FLAGS "-nodefaultlibs -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/awk)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/gen)

include(InstallSymlink)
set(BINARY_PACKAGING_MODE ON)

set(awk_sources
	src/b.c
	src/lex.c
	src/lib.c
	src/main.c
	src/parse.c
	src/proctab.c
	src/run.c
	src/tran.c
	src/ytab.c
)

add_darling_executable(awkexe ${awk_sources})
set_target_properties(awkexe PROPERTIES OUTPUT_NAME awk)

install(TARGETS awkexe
	DESTINATION libexec/darling/usr/bin)

install(FILES src/awk.1 DESTINATION libexec/darling/usr/share/man/man1)
