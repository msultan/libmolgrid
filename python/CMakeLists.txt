

find_package(PythonLibs)
if (NOT PYTHONLIBS_FOUND)
  message(STATUS "Python libraries NOT found")
endif()
    
find_package( Boost COMPONENTS system python REQUIRED )


include_directories(${PROJECT_SOURCE_DIR}/src)
include_directories(${PYTHON_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIRS})


set( PYMOLGRID_SOURCES
 bindings.cpp
)

# create the lib
add_library(molgrid SHARED ${PYMOLGRID_SOURCES})

# Suppress prefix "lib" because Python does not allow this prefix
set_target_properties(molgrid PROPERTIES 
									PREFIX ""
									LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/molgrid/")
									
# link
target_link_libraries(molgrid ${Boost_LIBRARIES} ${CUDA_LIBRARIES} libmolgrid_shared)

# Copy the __init__.py file
configure_file(__init__.py ${CMAKE_CURRENT_BINARY_DIR}/molgrid/__init__.py)
configure_file(setup.py ${CMAKE_CURRENT_BINARY_DIR}/molgrid/setup.py)

#TODO: figure out reasonable install, also how to build both python2 and python3
#install(TARGETS molgrid __init__.py DESTINATION "${PYTHON_INSTALL_PATH}")