cmake_minimum_required(VERSION 3.18)

# Name of project
set(PROJ_NAME Project)
project(${PROJ_NAME})

# Specify project files: header files and source files
set(HDRS
    file_utils.h
    game.h
    game_object.h
    player_game_object.h
    shader.h
    geometry.h
    sprite.h
    # add the hud class
    hud.h
    # add the timer class
    timer.h
    # add collectible classes
    collectible_game_object.h
    health_collectible.h
    emp_battery_collectible.h
    # add enemy game object class
    enemy_game_object.h
    # add projectile classes
    projectile.h
    wave_projectile.h
    bomb.h
    # add the three enemy types
    attacker_enemy_game_object.h
    patrol_enemy_game_object.h
    runner_enemy_game_object.h
    boss_enemy_game_object.h
    # add the class for the player's second weapon
    emp_ring.h
    text_game_object.h
    particle_system.h
    particles.h
)
 
set(SRCS
    file_utils.cpp
    game.cpp
    game_object.cpp
    main.cpp
    player_game_object.cpp
    shader.cpp
    sprite.cpp
    # add the hud class
    hud.cpp
    # add the timer class
    timer.cpp
    # add collectible classes
    collectible_game_object.cpp
    health_collectible.cpp
    emp_battery_collectible.cpp
    # add enemy game object class
    enemy_game_object.cpp
    # add projectile classes
    projectile.cpp
    wave_projectile.cpp
    bomb.cpp
    # add the three enemy types
    attacker_enemy_game_object.cpp
    patrol_enemy_game_object.cpp
    runner_enemy_game_object.cpp
    boss_enemy_game_object.cpp
    # add the class for the player's second weapon
    emp_ring.cpp
    text_game_object.cpp
    particle_system.cpp
    particles.cpp
    sprite_vertex_shader.glsl
    sprite_fragment_shader.glsl
    text_fragment_shader.glsl
    particle_vertex_shader.glsl
    particle_fragment_shader.glsl
)

# Add path name to configuration file
configure_file(path_config.h.in path_config.h)

# Add executable based on the source files
add_executable(${PROJ_NAME} ${HDRS} ${SRCS})

# Directories to include for header files, so that the compiler can find
# path_config.h
target_include_directories(${PROJ_NAME} PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

# Require OpenGL library
set(OpenGL_GL_PREFERENCE LEGACY)
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIR})
target_link_libraries(${PROJ_NAME} ${OPENGL_gl_LIBRARY})

# Other libraries needed
set(LIBRARY_PATH $ENV{COMP2501_LIBRARY_PATH} CACHE PATH "Folder with GLEW, GLFW, GLM, and SOIL libraries")
include_directories(${LIBRARY_PATH}/include)
if(NOT WIN32)
    find_library(GLEW_LIBRARY GLEW)
    find_library(GLFW_LIBRARY glfw)
    find_library(SOIL_LIBRARY SOIL)
elseif(WIN32)
    find_library(GLEW_LIBRARY glew32s HINTS ${LIBRARY_PATH}/lib)
    find_library(GLFW_LIBRARY glfw3 HINTS ${LIBRARY_PATH}/lib)
    find_library(SOIL_LIBRARY SOIL HINTS ${LIBRARY_PATH}/lib)
endif(NOT WIN32)
target_link_libraries(${PROJ_NAME} ${GLEW_LIBRARY})
target_link_libraries(${PROJ_NAME} ${GLFW_LIBRARY})
target_link_libraries(${PROJ_NAME} ${SOIL_LIBRARY})

# The rules here are specific to Windows Systems
if(WIN32)
    # Avoid ZERO_CHECK target in Visual Studio
    set(CMAKE_SUPPRESS_REGENERATION TRUE)
 
    # This will use the proper libraries in debug mode in Visual Studio
    set_target_properties(${PROJ_NAME} PROPERTIES DEBUG_POSTFIX _d)

    # Set the default project in VS
    set_property(DIRECTORY PROPERTY VS_STARTUP_PROJECT ${PROJ_NAME})
endif(WIN32)
