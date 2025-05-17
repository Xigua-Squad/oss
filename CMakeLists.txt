cmake_minimum_required(VERSION 3.20)
project(game_svr)
set(CMAKE_PREFIX_PATH "/vcpkg/installed/x64-linux")
aux_source_directory(./src SRC_LIST)

# 查找包
find_package(gRPC CONFIG REQUIRED)

set(CMAKE_CXX_STANDARD 20)

# 设置vcpkg
set(VCPKG_ROOT "/vcpkg")

# 设置protoc
set(PROTOC ${VCPKG_ROOT}/installed/x64-linux/tools/protobuf/protoc)

# 获取.proto文件列表
file(GLOB PROTO_FILES ${CMAKE_SOURCE_DIR}/proto/*.proto)

# 自动生成protobuf C++代码
execute_process(COMMAND ${PROTOC} --cpp_out=${CMAKE_SOURCE_DIR}/proto -I${CMAKE_SOURCE_DIR}/proto ${PROTO_FILES})

add_executable(${PROJECT_NAME} ${SRC_LIST})
# 链接库
target_link_libraries(${PROJECT_NAME} PRIVATE gRPC::gpr gRPC::grpc gRPC::grpc++ gRPC::grpc++_alts)

