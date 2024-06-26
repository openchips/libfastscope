cmake_minimum_required(VERSION 3.20)

enable_language(C)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON) # error if compiler doesn't support c17
set(CMAKE_C_EXTENSIONS OFF) # use c17 instead of gnu17


enable_language(CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON) # error if compiler doesn't support c++17
set(CMAKE_CXX_EXTENSIONS OFF) # use c++17 instead of gnu++17

project(libtslitex)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/artifacts/libtslitex")

add_definitions(-DUNICODE -D_UNICODE)

add_subdirectory(litepcie)

add_subdirectory(example)

set(TS_SOURCES
    src/i2c.c
    src/gpio.c
    src/spi.c
    src/afe.c
    src/adc.c
    src/hmcad15xx.c
    src/lmh6518.c
    src/ts_channel.c
    src/thunderscope.c
    )

set(TS_HEADERS
    src/i2c.h
    src/gpio.h
    src/spi.h
    src/adc.h
    src/afe.h
    src/platform.h
    src/lmh6518.h
    src/hmcad15xx.h
    src/ts_channel.h
    )

set(TS_LIB_HEADERS 
    include/thunderscope.h
    include/ts_common.h
    )

add_library(tslitex SHARED ${TS_SOURCES} ${TS_LIB_HEADERS} )

set_target_properties(tslitex PROPERTIES POSITION_INDEPENDENT_CODE 1)


target_link_libraries(tslitex litepcie)
target_include_directories(tslitex PUBLIC include)

file(COPY ${TS_LIB_HEADERS} DESTINATION ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/include)