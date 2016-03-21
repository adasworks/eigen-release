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

add_library(Eigen INTERFACE)
target_include_directories(Eigen INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
    $<INSTALL_INTERFACE:${INCLUDE_INSTALL_DIR}>)
install(TARGETS Eigen EXPORT eigen-targets DESTINATION lib)
install(EXPORT eigen-targets FILE eigen-config.cmake
    NAMESPACE :: DESTINATION lib/cmake/Eigen)

add_subdirectory(src)
