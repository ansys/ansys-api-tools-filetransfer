# #
# # This CMake configuration is meant to be included as a subdirectory
# # in the project which uses the filetransfer API.
# # The gRPC and protobuf packages must be findable by CMake.

find_package(gRPC REQUIRED)
find_package(PROTOBUF REQUIRED)

# Create library 'file_transfer_api' from .proto file definition
set(PROTO_FILES
    ansys/api/tools/filetransfer/v1/file_transfer_service.proto
)
add_library(file_transfer_api ${PROTO_FILES})

target_link_libraries(file_transfer_api ${gRPC_LIBRARIES} ${PROTOBUF_LIBRARIES})
target_include_directories(file_transfer_api PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

protobuf_generate(TARGET file_transfer_api LANGUAGE cpp)
protobuf_generate(TARGET file_transfer_api LANGUAGE grpc GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc PLUGIN "protoc-gen-grpc=${GRPC_CPP_PLUGIN_PROGRAM}")
