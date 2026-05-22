cmake_minimum_required(VERSION 4.0)

project(imgui CXX)

add_library(imgui)
add_library(imgui::Imgui ALIAS imgui)

target_sources(
  imgui
  PRIVATE imgui.cpp
          imgui_demo.cpp
          imgui_draw.cpp
          imgui_tables.cpp
          imgui_widgets.cpp
          backends/imgui_impl_glfw.cpp
          backends/imgui_impl_vulkan.cpp
  PUBLIC FILE_SET
         HEADERS
         BASE_DIRS
         .
         FILES
         imgui_internal.h
         imgui.h
         imconfig.h
         imstb_textedit.h
         imstb_rectpack.h
         imstb_truetype.h
         backends/imgui_impl_vulkan.h
         backends/imgui_impl_glfw.h
         )

         find_package(VulkanHeaders CONFIG REQUIRED)
         find_package(VulkanLoader CONFIG REQUIRED)
         find_package(glfw3 CONFIG REQUIRED)

         target_link_libraries(imgui PRIVATE Vulkan::Headers Vulkan::Loader glfw)

         set_target_properties(imgui PROPERTIES INSTALL_RPATH ${CMAKE_INSTALL_PREFIX}/lib)

install(
  TARGETS imgui
  EXPORT imgui-exports
  FILE_SET HEADERS
  DESTINATION ${CMAKE_INSTALL_PREFIX}/include/imgui)
install(
  EXPORT imgui-exports
  DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/imgui
  NAMESPACE imgui::
  FILE imgui-targets.cmake)
install(
    FILES ${CMAKE_CURRENT_SOURCE_DIR}/imguiConfig.cmake DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/imgui)
