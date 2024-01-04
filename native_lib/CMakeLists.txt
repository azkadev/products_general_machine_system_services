cmake_minimum_required(VERSION 3.16)

project(products_general_machine_system_services_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE PACKAGE_TEMPLATE")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(products_general_machine_system_services_shared_dir "lib")
set(products_general_machine_system_services_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "products_general_machine_system_services_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "products_general_machine_system_services")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "products_general_machine_system_services")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "products_general_machine_system_services")
endif()


add_library(products_general_machine_system_services_shared SHARED products_general_machine_system_services.cpp)
add_executable(products_general_machine_system_services_exe products_general_machine_system_services.cpp)

set_target_properties(products_general_machine_system_services_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${products_general_machine_system_services_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(products_general_machine_system_services_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${products_general_machine_system_services_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(products_general_machine_system_services_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET products_general_machine_system_services_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${products_general_machine_system_services_shared_dir}/${prefix_shared_library}products_general_machine_system_services${package_shared_extension}"
)

add_custom_command(
  TARGET products_general_machine_system_services_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${products_general_machine_system_services_exe_dir}/products_general_machine_system_services${package_exe_extension}" 
)

 