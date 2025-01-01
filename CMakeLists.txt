cmake_minimum_required(VERSION 3.18 FATAL_ERROR)
project(neural-network-wasm)

find_package(Torch REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

add_executable(neural-network-wasm src/main.cc)

target_link_libraries(neural-network-wasm "${TORCH_LIBRARIES}")
set_property(TARGET neural-network-wasm PROPERTY CXX_STANDARD 17)


# The following code block is suggested to be used on Windows.
# According to https://github.com/pytorch/pytorch/issues/25457,
# the DLLs need to be copied to avoid memory errors.
if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET example-app
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:example-app>)
endif (MSVC)

# source: https://discourse.nixos.org/t/get-clangd-to-find-standard-headers-in-nix-shell/11268/8
# removes the need to specify -DCMAKE_EXPORT_COMPILE_COMMANDS=1 on the cmake cli
set(CMAKE_EXPORT_COMPILE_COMMANDS ON CACHE INTERNAL "")
# cmake-generated compile_commands.json
if(CMAKE_EXPORT_COMPILE_COMMANDS)
  set(CMAKE_CXX_STANDARD_INCLUDE_DIRECTORIES 
      ${CMAKE_CXX_IMPLICIT_INCLUDE_DIRECTORIES})
endif()
