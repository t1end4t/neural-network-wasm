# Minimum CMake version required
cmake_minimum_required(VERSION 3.10)

# Set the project name and the C++ standard
project(MyProject VERSION 1.0 LANGUAGES CXX)

# Specify the C++ standard to use
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Specify include directories (if you have header files)
# include_directories(include)

# Add the executable target
add_executable(MyExecutable src/main.cc)

# If you have multiple source files, you can list them or use a wildcard:
# file(GLOB SOURCES "src/*.cpp")
# add_executable(MyExecutable ${SOURCES})

# source: https://discourse.nixos.org/t/get-clangd-to-find-standard-headers-in-nix-shell/11268/8
# removes the need to specify -DCMAKE_EXPORT_COMPILE_COMMANDS=1 on the cmake cli
set(CMAKE_EXPORT_COMPILE_COMMANDS ON CACHE INTERNAL "")
# cmake-generated compile_commands.json
if(CMAKE_EXPORT_COMPILE_COMMANDS)
  set(CMAKE_CXX_STANDARD_INCLUDE_DIRECTORIES 
      ${CMAKE_CXX_IMPLICIT_INCLUDE_DIRECTORIES})
endif()
