cmake_minimum_required(VERSION 3.0)
project(qhexedit)

set(target qhexedit2)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
find_package(Qt5Widgets)

file(GLOB Header_Files "*.h")
file(GLOB Source_Files "*.cpp")
add_library(${target} STATIC ${Header_Files} ${Source_Files})

target_link_libraries(qhexedit2 PUBLIC Qt5::Widgets)

target_include_directories(${target} 
	INTERFACE
	$<INSTALL_INTERFACE:include>)

set(cmake_dir lib/cmake/${target})
set(project_config ${PROJECT_BINARY_DIR}/${target}-config.cmake)
set(targets_export_name ${target}-targets)

install(TARGETS ${target}
	EXPORT  ${targets_export_name}
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
)

install(DIRECTORY ${CMAKE_SOURCE_DIR}/ DESTINATION include/qhexedit2 FILES_MATCHING PATTERN "*.h")

include(CMakePackageConfigHelpers)
configure_package_config_file(
	${PROJECT_SOURCE_DIR}/${target}-config.cmake.in
	${project_config}
	INSTALL_DESTINATION ${cmake_dir})

export(TARGETS ${target} FILE ${PROJECT_BINARY_DIR}/${targets_export_name}.cmake)

# Install version, config and target files.
install(FILES ${project_config} DESTINATION ${cmake_dir})
install(EXPORT ${targets_export_name} DESTINATION ${cmake_dir})
