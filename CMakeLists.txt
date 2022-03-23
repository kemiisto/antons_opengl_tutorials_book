cmake_minimum_required(VERSION 3.2)

if(WIN32 AND NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE "c:/Users/$ENV{USERNAME}/Downloads/vcpkg/scripts/buildsystems/vcpkg.cmake")
endif()

project(antons_opengl_4_tutorials)

set(CMAKE_CXX_STANDARD 23)

find_package(GLEW REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)

link_libraries(GLEW::GLEW glfw glm::glm)
include_directories(.)

#if (MSVC)
#    add_compile_options(/W3 /WX)
#else()
#    add_compile_options(-Wall -Wextra -pedantic -Werror)
#endif()

set(EXAMPLES
    00_hello_triangle
#    01_extended_init
#    02_shaders
#    03_vertex_buffer_objects
#    04_mats_and_vecs
#    05_virtual_camera
#    06_vcam_with_quaternion
#    07_ray_picking
#    08_phong
#    09_texture_mapping
#    10_screen_capture
#    11_video_capture
#    12_debugging_shaders
#    13_mesh_import
#    14_multi_tex
#    15_phongtextures
#    16_frag_reject
#    17_alpha_blending
#    18_spotlights
#    19_fog
#    20_normal_mapping
#    21_cube_mapping
#    22_geom_shaders
#    23_tessellation_shaders
#    24_gui_panels
#    25_sprite_sheets
#    26_bitmap_fonts
#    27_font_atlas
#    28_uniform_buffer_object
#    29_particle_systems
#    30_skinning_part_one
#    31_skinning_part_two
#    32_skinning_part_three
#    33_extension_check
#    34_framebuffer_switch
#    35_image_kernel
#    36_colour_picking
#    37_deferred_shading
#    38_texture_shadows
#    39_texture_mapping_srgb
#    40_compute_shader
#    41_shader_hot_reload
)

foreach(EXAMPLE ${EXAMPLES})
    file(GLOB SRC_FILES ${EXAMPLE}/*.cpp)
    add_executable(${EXAMPLE} ${SRC_FILES})
    set_target_properties(${EXAMPLE}
        PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${EXAMPLE}"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${EXAMPLE}"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${EXAMPLE}")
    file(GLOB RESOURCES ${EXAMPLE}/*.frag ${EXAMPLE}/*.vert ${EXAMPLE}/*.glsl ${EXAMPLE}/*.png)
    foreach(File IN LISTS RESOURCES)
        message(STATUS ${File})
        add_custom_command(
            TARGET ${EXAMPLE} PRE_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different ${File} ${CMAKE_BINARY_DIR}/${EXAMPLE}
            COMMENT "Copying file: ${File}")
    endforeach()
endforeach(EXAMPLE)
