find_package(protobuf CONFIG REQUIRED)
find_package(gRPC CONFIG REQUIRED)
find_package(Threads)
# PROTOBUF_GENERATE_CPP(ProtoSources ProtoHeaders ${ProtoFiles} PLUGIN protoc-gen-grpc=${GRPC_CPP_PLUGIN_PATH})



set(PROTO_FILES
    kv.proto
    auth.proto
    rpc.proto
    gogoproto/gogo.proto
    google/api/annotations.proto
    google/api/http.proto
)

add_library(etcdRPC  ${PROTO_FILES})
target_link_libraries(etcdRPC
    PUBLIC
        protobuf::libprotobuf
        gRPC::grpc
        gRPC::grpc++
)

target_include_directories(etcdRPC
    PUBLIC
        ${CMAKE_CURRENT_BINARY_DIR}
)


get_target_property(grpc_cpp_plugin_location gRPC::grpc_cpp_plugin LOCATION)

protobuf_generate(TARGET etcdRPC LANGUAGE cpp)

# compile the GRPC services
protobuf_generate(
    TARGET
        etcdRPC
    LANGUAGE
        grpc
    GENERATE_EXTENSIONS
        .grpc.pb.h
        .grpc.pb.cc
    PLUGIN
        "protoc-gen-grpc=${grpc_cpp_plugin_location}"
)
