set(TARGET SIM_PhysIKA)
# Find Houdini
if (MSVC)
    set(Houdini_PATH "C:/Program Files/Side Effects Software/Houdini 20.0.590")
elseif (APPLE)
    set(Houdini_PATH "/Applications/Houdini/Houdini20.0.590/Frameworks/Houdini.framework/Versions/20.0/Resources")
endif ()

set(Houdini_DIR ${Houdini_PATH}/toolkit/cmake)
find_package(Houdini REQUIRED)

# Load Source
add_library(
        ${TARGET}
        SHARED
        hdk/Entrance.cpp

        # Your Node Files Here
)

# Link Houdini Toolkit
target_link_libraries(
        ${TARGET}
        PUBLIC
        Houdini

        HeightField
        ParticleSystem
        RigidBody

        Core Framework IO Rendering GlutGUI
)
target_link_directories(
        ${TARGET}
        PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
)
target_include_directories(
        ${TARGET}
        PUBLIC
        ${CMAKE_CURRENT_BINARY_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}
        hdk
)
target_compile_options(
        ${TARGET}
        PRIVATE
        -D_LIBCPP_ENABLE_CXX17_REMOVED_UNARY_BINARY_FUNCTION
)
houdini_configure_target(${TARGET})


# Load Source
add_executable(HinaTest
        test.cpp
)
add_dependencies(HinaTest
        HeightField
        ParticleSystem
        RigidBody

        Core Framework IO Rendering GlutGUI)
target_link_libraries(
        HinaTest
        PRIVATE
        HeightField
        ParticleSystem
        RigidBody

        Core Framework IO Rendering GlutGUI)
