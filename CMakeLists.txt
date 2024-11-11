cmake_minimum_required(VERSION 3.10)
project(Detours-project)


macro( filter_out FILTERS INPUTS OUTPUTS )
    set( FOUT "" )
    foreach( INP ${INPUTS} )
        set( FILTERED 0 )
        foreach( FILT ${FILTERS} )
            if( ${FILTERED} EQUAL 0 )
                if( "${FILT}" STREQUAL "${INP}" )
                    set( FILTERED 1 )
                endif( "${FILT}" STREQUAL "${INP}" )
            endif( ${FILTERED} EQUAL 0 )
        endforeach( FILT ${FILTERS} )
        if( ${FILTERED} EQUAL 0 )
            set( FOUT ${FOUT} ${INP} )
        endif( ${FILTERED} EQUAL 0 )
    endforeach( INP ${INPUTS} )
    set( ${OUTPUTS} ${FOUT} )
endmacro( filter_out FILTERS INPUTS OUTPUTS )


aux_source_directory(src SRCS_ALL)
set( FILTERS src/uimports.cpp )
filter_out("${FILTERS}" "${SRCS_ALL}" SRCS )

message( STATUS "outputs are: ${SRCS}")

add_library(Detours STATIC ${SRCS})
set_target_properties(Detours PROPERTIES  
    PUBLIC_HEADER "src/detours.h"  
    )  

# install
install(TARGETS Detours
        EXPORT DetoursTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        PUBLIC_HEADER DESTINATION include
        )
