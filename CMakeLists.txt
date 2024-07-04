# CMakeList.txt : CMake project for Demo, include source and define
# project specific logic here.
#
find_package(Torch REQUIRED)
# Add source to this project's executable.
add_executable (Demo "Demo.cpp" "Demo.h")
target_link_libraries(Demo "${TORCH_LIBRARIES}")
if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET Demo PROPERTY CXX_STANDARD 20)
endif()

# TODO: Add tests and install targets if needed.
if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET Demo
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:Demo>)
endif (MSVC)