# Generated by Vitis, modify at only sections marked as USER SETTINGS


# This will be auto-updated based on kernels added/removed from this component.


set(_KERNEL_NAMES
vector_add
)
####    USER SETTINGS  START    ####
# Add any compiler definitions to be passed to v++ --compile. This will be
# considered for all the kernels in this component. Example adding VERBOSE=1
# will pass -DVERBOSE=1 to v++
set(USER_KERNEL_COMPILE_DEFINITIONS
""
)
# Add any user include directories to be used while compiling the kernel.This
# will be considered for all the kernels in this component.
# Example 1: Adding /proj/data/include will pass -I/proj/data/include
# Example 2: Adding ../../common/include will consider the path as relative to this directory.
# Example 3: Adding ${CMAKE_SOURCE_DIR}/data/include to add data/include from this project.
set(USER_KERNEL_INCLUDE_DIRECTORIES
""
)
# Adding any other command line options to be passed to v++ --compile.This will
# be considered for all the kernels in this component.
set(USER_KERNEL_COMPILE_OPTIONS
""
)
# Per Kernel Compile Options
# Add any kernel specific compile options.
set(USER_vector_add_OPTIONS
""
)
####    End of Per Kernel Compile Options    ####



####    CAUTION in updating below section    ####
# These setting are required to build the application as per the project
# template used.Update below only to modify the default settings.

# Below compile definitions are derived from the application template used to
# create this project.Update them to suite your requirement. They will not be
# modified further by the tool.
# If new compile definitions needed, they can be
# added to USER_KERNEL_COMPILE_DEFINITIONS
set(_KERNEL_COMPILE_DEFINITIONS
${USER_KERNEL_COMPILE_DEFINITIONS}
)

# Below compile options are derived from the application template used to create
# this project.Update them to suite your requirement. They will not be modified
# further by the tool.
# If new compile definitions needed, they can be added to
# USER_KERNEL_COMPILE_OPTIONS
set(_KERNEL_COMPILE_OPTIONS
${USER_KERNEL_COMPILE_OPTIONS}
)


# Below build data files list is derived from the application template used to
# create this project. This will not be modified further by the tool.
set(_KERNEL_BUILD_DATAFILES
""
)


# Below Include directories are derived from the application template used to
# create this project. Update them to suite your requirement. They will not be
# modified further by the tool. If new sources are added, the new INCLUDE
# Directories can be added to USER_KERNEL_INCLUDE_DIRECTORIES. For kernel
# specific includes, add to the per kernel settings above.
set(_KERNEL_INCLUDE_DIRECTORIES
""
)


set(_KERNEL_PROJ
pl_kernel
)
####    Below variables will be updated by the Vitis. Do not modify.    ####
# vector_add
set(_vector_add_SRC
    ${CMAKE_SOURCE_DIR}/pl_kernel/src/vector_addition.cpp
)
set(_vector_add_CFG
    ${CMAKE_SOURCE_DIR}/pl_kernel/vector_add-compile.cfg
)
set(_vector_add_XO
vector_add.xo
)
# Below kernel specific compile options are derived from the application
# template used to create this project. Update them to suite your requirement.
# They will not be modified further by the tool. New  kernel specific compile
# options can be added with USER_<kernel_name>_OPTIONS settings listed above
set(_vector_add_OPTIONS
""
)


####    End of per kernel compile options derived from project template used to create this project   ####

foreach( datafile ${_KERNEL_BUILD_DATAFILES})
    file(COPY ${datafile} DESTINATION .)
endforeach()

set(COMPILE_OPTIONS "")
foreach( def ${_KERNEL_COMPILE_DEFINITIONS})
    list(APPEND COMPILE_DEFINITIONS  -D${def})
endforeach()

foreach( idir ${_KERNEL_INCLUDE_DIRECTORIES})
    list(APPEND COMPILE_DEFINITIONS  -I${idir})
endforeach()

foreach( idir ${USER_KERNEL_INCLUDE_DIRECTORIES})
    list(APPEND COMPILE_DEFINITIONS  -I${idir})
endforeach()

foreach(KERNEL ${_KERNEL_NAMES})

    set(CONFIG_OPTIONS "")
    foreach( k ${_${KERNEL}_CFG})
        set(CONFIG_OPTIONS "--config")
        list(APPEND CONFIG_OPTIONS ${k})
    endforeach()
    add_custom_target(mkBuildDirForKernel_${_KERNEL_PROJ}_${KERNEL}
	    COMMAND ${CMAKE_COMMAND} -E make_directory ${KERNEL}.build)

    ADD_CUSTOM_COMMAND(OUTPUT ${CMAKE_BINARY_DIR}/${_KERNEL_PROJ}/${_${KERNEL}_XO}
	COMMAND ${Vitis_COMPILER}
		    --platform ${VITIS_PLATFORM_PATH}
		    --target ${VITIS_TARGET}
		    ${COMPILE_DEFINITIONS}
		    ${_KERNEL_COMPILE_OPTIONS}
		    ${_${KERNEL}_OPTIONS}
		    ${USER_${KERNEL}_OPTIONS}
		    --compile  ${_${KERNEL}_SRC}
		    --kernel ${KERNEL} ${CONFIG_OPTIONS}
		    -o ${_${KERNEL}_XO}
	DEPENDS ${_${KERNEL}_SRC} ${_${KERNEL}_CFG} ${_KERNEL_BUILD_DATAFILES}
    )

    ADD_CUSTOM_TARGET(${_KERNEL_PROJ}_${KERNEL}_CompileXO ALL
	 DEPENDS  ${CMAKE_BINARY_DIR}/${_KERNEL_PROJ}/${_${KERNEL}_XO}
    )

endforeach()
