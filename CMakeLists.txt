cmake_minimum_required(VERSION 3.12)
project(uemacs C)

# Set C standard and properties
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

# Program name and sources
set(PROGRAM "em")

set(SRC
    ansi.c
    basic.c
    bind.c
    buffer.c
    crypt.c
    display.c
    eval.c
    exec.c
    file.c
    fileio.c
    ibmpc.c
    input.c
    isearch.c
    line.c
    lock.c
    main.c
    pklock.c
    posix.c
    random.c
    region.c
    search.c
    spawn.c
    tcap.c
    termio.c
    vmsvt.c
    vt52.c
    window.c
    word.c
    names.c
    globals.c
    version.c
    usage.c
    wrapper.c
    utf8.c
    util.c
)

# Platform-specific definitions
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    add_compile_definitions(
        AUTOCONF
        POSIX
        USG
        _XOPEN_SOURCE=600
        _GNU_SOURCE
    )
elseif(CMAKE_SYSTEM_NAME STREQUAL "FreeBSD")
    add_compile_definitions(
        AUTOCONF
        POSIX
        SYSV
        _FREEBSD_C_SOURCE
        _BSD_SOURCE
        _SVID_SOURCE
        _XOPEN_SOURCE=600
    )
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    add_compile_definitions(
        AUTOCONF
        POSIX
        SYSV
        _DARWIN_C_SOURCE
        _BSD_SOURCE
        _SVID_SOURCE
        _XOPEN_SOURCE=600
    )
else()
    add_compile_definitions(AUTOCONF)
endif()

# Compiler flags
add_compile_options(
    -Wall
    -Wstrict-prototypes
    -O2
    -s
)

# Find curses library
find_library(CURSES_LIBRARY NAMES curses ncurses)
if(NOT CURSES_LIBRARY)
    message(FATAL_ERROR "Curses library not found")
endif()

# Create the executable
add_executable(${PROGRAM} ${SRC}) 

# Link against curses
target_link_libraries(${PROGRAM} ${CURSES_LIBRARY})

# Set target properties
set_target_properties(${PROGRAM} PROPERTIES
    OUTPUT_NAME "em"
    C_STANDARD 99
    C_STANDARD_REQUIRED ON
)

# Installation
install(TARGETS ${PROGRAM}
    DESTINATION bin
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
)

install(FILES emacs.hlp
    DESTINATION lib
    PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ
)

# Custom targets for additional functionality
add_custom_target(sparse
    COMMAND sparse -D__LITTLE_ENDIAN__ -D__x86_64__ -D__linux__ -D__unix__ ${SRC}
    COMMENT "Running sparse static analyzer"
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_custom_target(clean-all
    COMMAND ${CMAKE_COMMAND} -E remove ${PROGRAM} *.o core tags
    COMMENT "Cleaning all build artifacts"
)

add_custom_target(tags
    COMMAND ctags ${SRC}
    COMMENT "Generating tags"
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

# Enable verbose output if requested
option(VERBOSE "Enable verbose build output" OFF)
if(VERBOSE)
    set(CMAKE_VERBOSE_MAKEFILE ON)
endif()

