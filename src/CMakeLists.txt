find_package(protobuf CONFIG REQUIRED)
find_package(gRPC CONFIG REQUIRED)
find_package(Threads)
# PROTOBUF_GENERATE_CPP(ProtoSources ProtoHeaders ${ProtoFiles} PLUGIN protoc-gen-grpc=${GRPC_CPP_PLUGIN_PATH})







set(PROTO_FILES
    helloworld.proto
)

add_library(helloworldRPC  ${PROTO_FILES})
target_link_libraries(helloworldRPC
    PUBLIC
        protobuf::libprotobuf
        gRPC::grpc
        gRPC::grpc++
)

target_include_directories(helloworldRPC
    PUBLIC
        ${CMAKE_CURRENT_BINARY_DIR}
)


get_target_property(grpc_cpp_plugin_location gRPC::grpc_cpp_plugin LOCATION)

protobuf_generate(TARGET helloworldRPC LANGUAGE cpp)

# compile the GRPC services
protobuf_generate(
    TARGET
        helloworldRPC
    LANGUAGE
        grpc
    GENERATE_EXTENSIONS
        .grpc.pb.h
        .grpc.pb.cc
    PLUGIN
        "protoc-gen-grpc=${grpc_cpp_plugin_location}"
)

add_executable(myServer server.cpp)
target_link_libraries(myServer 
PUBLIC 
    helloworldRPC
    protobuf::libprotobuf
    gRPC::grpc
    gRPC::grpc++)

    add_executable(myClient client.cpp)
target_link_libraries(myClient 
PUBLIC 
    helloworldRPC
    protobuf::libprotobuf
    gRPC::grpc
    gRPC::grpc++)

add_subdirectory(proto)