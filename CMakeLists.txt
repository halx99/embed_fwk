project(test)

add_subdirectory(lib1)
add_subdirectory(app1)

target_link_libraries(app1 lib1)

