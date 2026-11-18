add_library(RobotDriverRTDE SHARED RobotDriverRTDE.cpp)

target_include_directories(
  RobotDriverRTDE
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
         $<INSTALL_INTERFACE:include>)

target_link_libraries(RobotDriverRTDE
                      PUBLIC unified_robot_interface::robot_driver_api)
target_link_libraries(RobotDriverRTDE PRIVATE ur_client_library::urcl fmt::fmt)

# external_control.urscript is installed by ur_client_library under
# <prefix>/share/ur_client_library/resources; derive <prefix> from its config.
cmake_path(SET URCL_PREFIX NORMALIZE "${ur_client_library_DIR}/../../..")
target_compile_definitions(
  RobotDriverRTDE
  PRIVATE
    URCL_SCRIPT_FILE="${URCL_PREFIX}/share/ur_client_library/resources/external_control.urscript"
)

add_robot_driver(RobotDriverRTDE)

install(
  TARGETS RobotDriverRTDE
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/robot_interface
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/robot_interface)
