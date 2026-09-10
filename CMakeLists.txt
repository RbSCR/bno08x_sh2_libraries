cmake_minimum_required(VERSION 3.28.3)
project(bno08x_sh2_driver)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)


# == SH2 LIBRARY ===============================================
# Note sh2_library had it's own CmakeLists.txt (in include/sh2)
#      renamed it so it isn't used anymore
set(SH2_LIBRARY_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src/sh2)

add_library(sh2_library
  ${SH2_LIBRARY_DIR}/sh2.c
  ${SH2_LIBRARY_DIR}/sh2_SensorValue.c
  ${SH2_LIBRARY_DIR}/sh2_util.c
  ${SH2_LIBRARY_DIR}/shtp.c
)

target_compile_features(sh2_library PUBLIC c_std_99)

set_target_properties(sh2_library PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_compile_options(sh2_library
  PUBLIC -Wno-sign-compare -Wno-unused-parameter -Wno-unused-variable
)

# Make include directories available to downstream packages
target_include_directories(sh2_library PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

# == BNO08x driver ==================================================
set(BNO08X_LIBRARY_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src/bno08x)

add_library(bno08x_library
  ${BNO08X_LIBRARY_DIR}/bno08x.cpp
)

set_target_properties(bno08x_library PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_compile_options(bno08x_library
  PUBLIC -Wno-sign-compare -Wno-unused-parameter -Wno-unused-variable
)

target_link_libraries(bno08x_library
  sh2_library
)

# Make include directories available to downstream packages
target_include_directories(bno08x_library PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)


# ================================================================

# Export targets so other packages can use find_package() to find them
ament_export_targets(bno08x_sh2_driver_targets HAS_LIBRARY_TARGET)

# --- Installation ---
install(
  TARGETS sh2_library bno08x_library
  EXPORT bno08x_sh2_driver_targets
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

  # - - - bno08x _driver tests - - -
  ament_add_gmock(test_driver_i2c_mock
    test/i2c_interface_mock_test.cpp
  )
  if(TARGET test_driver_i2c_mock)
    target_include_directories(test_driver_i2c_mock PUBLIC
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    )
  endif()

  ament_add_gmock(test_driver_watchdog
    test/watchdog_test.cpp
  )
  if(TARGET test_driver_watchdog)
    target_include_directories(test_driver_watchdog PUBLIC
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    )
  endif()


endif()

ament_package()
