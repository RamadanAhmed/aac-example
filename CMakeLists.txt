cmake_minimum_required(VERSION 3.14)

project(aac-example)

add_library(acc-lib)

add_library(aac::lib ALIAS acc-lib)

target_sources(acc-lib
               PRIVATE
               src/fdkaac_dec.cpp
               src/fdkaac_enc.cpp
               src/wavreader.c
               src/wavwriter.c)

target_include_directories(acc-lib
                           PUBLIC 
                           include/
                           "$ENV{VCPKG_SERVER}/installed/x64-windows/include")

target_link_libraries(acc-lib 
                      PRIVATE 
                      "$ENV{VCPKG_SERVER}/installed/x64-windows/lib/fdk-aac.lib")

target_compile_features(acc-lib PRIVATE cxx_std_17)

add_executable(aac-example-encoder)

target_sources(aac-example-encoder
               PRIVATE
               test/enc_test.cpp)

target_link_libraries(aac-example-encoder PRIVATE aac::lib)

add_executable(aac-example-decoder)

target_sources(aac-example-decoder
               PRIVATE
               test/dec_test.cpp)

target_link_libraries(aac-example-decoder PRIVATE aac::lib)
