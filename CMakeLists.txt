cmake_minimum_required(VERSION 3.20)

project(test)

set(CMAKE_XCODE_GENERATE_TOP_LEVEL_PROJECT_ONLY TRUE)

cmake_policy(SET CMP0079 NEW)
cmake_policy(SET CMP0042 NEW)

add_subdirectory(lib1)
add_subdirectory(app1)

target_link_libraries(app1 lib1)

set_target_properties(app1 PROPERTIES 
    XCODE_LINK_BUILD_PHASE_MODE KNOWN_LOCATION
    XCODE_EMBED_FRAMEWORKS_REMOVE_HEADERS_ON_COPY ON
    XCODE_EMBED_FRAMEWORKS_CODE_SIGN_ON_COPY ON
    XCODE_EMBED_FRAMEWORKS lib1)

