
PROJECT ( YUV C CXX )	# "C" is required even for C++ projects
CMAKE_MINIMUM_REQUIRED( VERSION 2.8.12 )


SET ( ly_base_dir	${PROJECT_SOURCE_DIR} )
SET ( ly_src_dir	${ly_base_dir}/source )
SET ( ly_inc_dir	${ly_base_dir}/include )

SET ( ly_lib_name	yuv )
SET ( ly_lib_static	${ly_lib_name} )
SET ( ly_lib_shared	${ly_lib_name}_shared )

FILE ( GLOB_RECURSE	ly_source_files ${ly_src_dir}/*.cc )
LIST ( SORT			ly_source_files )


INCLUDE_DIRECTORIES( BEFORE ${ly_inc_dir} )


# this creates the static library (.a)
ADD_LIBRARY				( ${ly_lib_static} STATIC ${ly_source_files} )

# this creates the shared library (.so)
ADD_LIBRARY				( ${ly_lib_shared} SHARED ${ly_source_files} )
SET_TARGET_PROPERTIES	( ${ly_lib_shared} PROPERTIES OUTPUT_NAME "${ly_lib_name}" )
SET_TARGET_PROPERTIES	( ${ly_lib_shared} PROPERTIES PREFIX "lib" )

find_package ( JPEG )
if (JPEG_FOUND)
  include_directories( ${JPEG_INCLUDE_DIR} )
  target_link_libraries( ${ly_lib_shared} ${JPEG_LIBRARY} )
  add_definitions( -DHAVE_JPEG )
endif()

