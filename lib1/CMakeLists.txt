project(lib1)

add_library(lib1 SHARED empty.cpp)

set(LIB_VERSION "1.0")

if(GIT_FOUND)
    EXECUTE_PROCESS(COMMAND ${GIT_EXECUTABLE} rev-list --count head
        TIMEOUT 5
        OUTPUT_VARIABLE BUNDLE_VERSION
        OUTPUT_STRIP_TRAILING_WHITESPACE)
else()
    set(BUNDLE_VERSION 1)
endif()

# Build: Fix rpath in iOS shared libraries
# If this flag is not set, the final dylib binary ld path will be
# /User/xxx/*/soft_oal.framework/soft_oal, which can't be loaded by iOS devices.
# See also: https://github.com/libjpeg-turbo/libjpeg-turbo/commit/c80ddef7a4ce21ace9e3ca0fd190d320cc8cdaeb
if(NOT CMAKE_SHARED_LIBRARY_RUNTIME_C_FLAG)
    set(CMAKE_SHARED_LIBRARY_RUNTIME_C_FLAG "-Wl,-rpath,")
endif()

set_target_properties(${IMPL_TARGET} PROPERTIES
    FRAMEWORK TRUE
    FRAMEWORK_VERSION C
    MACOSX_FRAMEWORK_NAME "lib1"
    MACOSX_FRAMEWORK_IDENTIFIER "org.adxe.lib1"
    MACOSX_FRAMEWORK_SHORT_VERSION_STRING ${LIB_VERSION}
    MACOSX_FRAMEWORK_BUNDLE_VERSION ${BUNDLE_VERSION}
    XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY ""
    XCODE_ATTRIBUTE_CODE_SIGNING_ALLOWED "NO"
    XCODE_ATTRIBUTE_CODE_SIGNING_REQUIRED "NO"
    # PUBLIC_HEADER "${TARGET_PUBLIC_HEADERS}"
    MACOSX_RPATH TRUE)
    
