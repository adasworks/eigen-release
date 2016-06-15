include(RegexUtils)
test_escape_string_as_regex()

file(GLOB Eigen_directory_files "*")

escape_string_as_regex(ESCAPED_CMAKE_CURRENT_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}")

foreach(f ${Eigen_directory_files})
  if(NOT f MATCHES "\\.txt" AND NOT f MATCHES "${ESCAPED_CMAKE_CURRENT_SOURCE_DIR}/[.].+" AND NOT f MATCHES "${ESCAPED_CMAKE_CURRENT_SOURCE_DIR}/src")
    list(APPEND Eigen_directory_files_to_install ${f})
  endif()
endforeach(f ${Eigen_directory_files})

install(FILES
  ${Eigen_directory_files_to_install}
  DESTINATION ${INCLUDE_INSTALL_DIR}/Eigen COMPONENT Devel
  )

add_library(Eigen3 INTERFACE)
target_include_directories(Eigen3 INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
    $<INSTALL_INTERFACE:${INCLUDE_INSTALL_DIR}>)
install(TARGETS Eigen3 EXPORT eigen3-targets DESTINATION lib)
install(EXPORT eigen3-targets FILE eigen3-config.cmake
    NAMESPACE :: DESTINATION lib/cmake/Eigen3)

add_subdirectory(src)
