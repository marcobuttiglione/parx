add_library(parx-platform INTERFACE)
target_include_directories(parx-platform INTERFACE platform/include)

add_library(parx-renderer INTERFACE)
target_include_directories(parx-renderer INTERFACE renderer/include)
target_link_libraries(parx-renderer INTERFACE parx-platform)

add_library(parx-scene INTERFACE)
target_include_directories(parx-scene INTERFACE scene/include)

add_library(parx-input INTERFACE)
target_include_directories(parx-input INTERFACE input/include)
target_link_libraries(parx-input INTERFACE parx-platform)

add_library(parx-assets INTERFACE)
target_include_directories(parx-assets INTERFACE assets/include)

add_library(parx-engine INTERFACE)
target_link_libraries(parx-engine INTERFACE
    parx-platform
    parx-renderer
    parx-scene
    parx-input
    parx-assets
)
