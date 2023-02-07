##
## This CMake configuration is meant to be included as a subdirectory
## in the project which uses the filetransfer API. Define the following
## variables:
##
## - filetransfer_api_libs: The gRPC and related dependencies
## - grpc_cpp_plugin: Location of the protobuf gRPC C++ plugin
##

# Create library 'file_transfer_api' from .proto file definition
set(PROTO_FILES
    ansys/api/tools/filetransfer/v1/file_transfer_service.proto
)
add_library(file_transfer_api ${PROTO_FILES})

target_link_libraries(file_transfer_api PUBLIC ${filetransfer_api_libs})
target_include_directories(file_transfer_api PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

protobuf_generate(TARGET file_transfer_api LANGUAGE cpp)
protobuf_generate(TARGET file_transfer_api LANGUAGE grpc GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc PLUGIN "protoc-gen-grpc=${grpc_cpp_plugin}")
