cmake_minimum_required (VERSION 3.8)

project ("tkSimMsgs")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)


if(NOT GZ_VERSION)
    message(STATUS "Using Gazebo version from environment.")
    set(GZ_VERSION $ENV{GZ_VERSION})
endif()

if(GZ_VERSION STREQUAL "harmonic")
    # Harmonic
    find_package(gz-transport13 REQUIRED)
    find_package(gz-msgs10 REQUIRED)

    set(GZ_MSGS_VER ${gz-msgs10_VERSION_MAJOR})
    set(GZ_TRANSPORT_VER ${gz-transport13_VERSION_MAJOR})

    message(STATUS "Compiling against Gazebo Harmonic")
elseif(GZ_VERSION STREQUAL "ionic")
    # Harmonic
    find_package(gz-transport14 REQUIRED)
    find_package(gz-msgs11 REQUIRED)

    set(GZ_MSGS_VER ${gz-msgs11_VERSION_MAJOR})
    set(GZ_TRANSPORT_VER ${gz-transport14_VERSION_MAJOR})

    message(STATUS "Compiling against Gazebo Ionic")
else()
    message(FATAL_ERROR "This branch is only compatible with Gazebo Harmonic forward.")
endif()

# Define a variable 'MSGS_PROTOS' listing the .proto files
set(MSGS_PROTOS
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/tk/sim_msgs/CarControl.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/tk/sim_msgs/CarState.proto
)

# Call 'gz_msgs_generate_messages()' to process the .proto files
gz_msgs_generate_messages(
    # The cmake target to be generated for libraries/executables to link
    TARGET msgs
    # The protobuf package to generate (Typically based on the path)
    PROTO_PACKAGE "tk.sim_msgs"
    # The path to the base directory of the proto files
    MSGS_PATH ${CMAKE_CURRENT_SOURCE_DIR}/proto
    # List of proto files to process
    MSGS_PROTOS ${MSGS_PROTOS}
    # Depenency on gz-msgs
    DEPENDENCIES gz-msgs${GZ_MSGS_VER}::gz-msgs${GZ_MSGS_VER}
)
