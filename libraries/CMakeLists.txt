# Macro utility to clone a library and source-related submodules.
macro( clone_path path )
    find_package( Git REQUIRED )
    # Attempt to clone submodules.
    if( ${BUILD_CLONE_SUBMODULES} )
        execute_process( COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive -- ${path}
                         WORKING_DIRECTORY ${ROOT_DIR}
                         RESULT_VARIABLE LIB_CLONE_RESULT )
        if( NOT ${LIB_CLONE_RESULT} STREQUAL "0" )
            message( FATAL_ERROR "Failed to clone submodules." )
        endif()
    endif()
endmacro()

# Clone the submodules only if this is a Git repo.
if( EXISTS ${ROOT_DIR}/.git )
    clone_path( ${LIBRARIES_DIR} )
endif()
