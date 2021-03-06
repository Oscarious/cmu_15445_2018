set(GMOCK_DIR  "${PROJECT_SOURCE_DIR}/third_party/gmock")
set(SRC_DIR "${PROJECT_SOURCE_DIR}/src")
file(GLOB gmock_srcs  ${GMOCK_DIR}/*.cc)
include_directories(SYSTEM ${GMOCK_DIR})
add_library(gtest EXCLUDE_FROM_ALL ${gmock_srcs})
target_link_libraries(gtest ${CMAKE_THREAD_LIBS_INIT})

add_executable(TestHash hash/extendible_hash_test.cpp ../src/hash/extendible_hash.cpp)
add_executable(TestLRU buffer/lru_replacer_test.cpp ../src/buffer/lru_replacer.cpp)
add_executable(TestBufferMgr buffer/buffer_pool_manager_test.cpp
  ${SRC_DIR}/hash/extendible_hash.cpp
  ${SRC_DIR}/buffer/buffer_pool_manager.cpp
  ${SRC_DIR}/buffer/lru_replacer.cpp
  ${SRC_DIR}/disk/disk_manager.cpp
)

set_target_properties(TestHash
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/test"
  )

set_target_properties(TestLRU
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/test"
  )

set_target_properties(TestBufferMgr
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/test"
  )

target_link_libraries(TestHash gtest)
target_link_libraries(TestLRU gtest)
target_link_libraries(TestBufferMgr gtest)