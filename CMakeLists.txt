cmake_minimum_required(VERSION 3.28.3)
project(bno08x_sh2_libraries)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)


# == sh2 library ===============================================
# Note sh2_library had it's own CmakeLists.txt (in include/sh2)
#      renamed it so it isn't used anymore
set(SH2_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src/sh2)

add_library(sh2
  ${SH2_DIR}/sh2.c
  ${SH2_DIR}/sh2_SensorValue.c
  ${SH2_DIR}/sh2_util.c
  ${SH2_DIR}/shtp.c
)

target_compile_features(sh2 PUBLIC c_std_99)

set_target_properties(sh2 PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_compile_options(sh2
  PUBLIC -Wno-sign-compare -Wno-unused-parameter -Wno-unused-variable
)

# Make include directories available to downstream packages
target_include_directories(sh2 PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

# == bno08xx library ==================================================
set(BNO08X_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src/bno08x)

add_library(bno08x
  ${BNO08X_DIR}/bno08x.cpp
)

set_target_properties(bno08x PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_compile_options(bno08x
  PUBLIC -Wno-sign-compare -Wno-unused-parameter -Wno-unused-variable
)

target_link_libraries(bno08x
  sh2
)

# Make include directories available to downstream packages
target_include_directories(bno08x PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)


# ================================================================

# Export targets so other packages can use find_package() to find them
ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)

# --- Installation ---
install(
  TARGETS sh2 bno08x
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

install(
  DIRECTORY include/
  DESTINATION include
)


if(BUILD_TESTING)

  # = = = Linting = = =
  find_package(ament_lint_auto REQUIRED)
  # Linters to be used in the 'auto-linting' are mentioned in package.xml


  # TODO check result of linters
  # Linting disabled for 'external' files
  list(APPEND AMENT_LINT_AUTO_FILE_EXCLUDE
    "include/sh2/*"
    "src/sh2/*"
    "include/bno08x/*"
    "src/bno08x/*"
  )

  ament_lint_auto_find_test_dependencies()

  # = = = Tests = = =
  find_package(ament_cmake_gtest REQUIRED)
  find_package(ament_cmake_gmock REQUIRED)
  find_package(launch_testing_ament_cmake REQUIRED)

  # - - - bno08x tests - - -
  ament_add_gmock(test_i2c_mock
    test/i2c_interface_mock_test.cpp
  )
  if(TARGET test_i2c_mock)
    target_include_directories(test_i2c_mock PUBLIC
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    )
  endif()

  ament_add_gmock(test_watchdog
    test/watchdog_test.cpp
  )
  if(TARGET test_watchdog)
    target_include_directories(test_watchdog PUBLIC
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    )
  endif()


endif()

ament_package()
