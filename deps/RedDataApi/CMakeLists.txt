add_library(RedDataApi INTERFACE)
target_compile_definitions(RedDataApi INTERFACE HAS_REDDATA)
target_include_directories(RedDataApi INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/include")
target_link_libraries(RedDataApi INTERFACE RED4ext::SDK RedLib)
