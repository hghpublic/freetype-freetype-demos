
project(graph C)

add_subdirectory(allegro)
add_subdirectory(x11)

set(target graph)
add_library(${target} STATIC)
target_sources(${target} PRIVATE
    gblblit.c
    gblender.c
    grdevice.c
    grfill.c
    grfont.c
    grinit.c
    grobjs.c
    grswizzle.c
    # xtest.c
)
target_include_directories(${target} INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}
)
target_compile_features(${target} PRIVATE c_std_23)
set_target_properties(${target} PROPERTIES
    FOLDER "graph"
)


# set(target xtest)
# add_executable(${target})
# target_sources(${target} PRIVATE
#     xtest.c
# )
# target_compile_features(${target} PRIVATE c_std_23)
# target_link_libraries(${target} PRIVATE graph)
# set_target_properties(${target} PROPERTIES
#     FOLDER "graph"
# )


# a special rule is used for 'grinit.o' as it needs the definition
# of some macros like "-DDEVICE_X11" or "-DDEVICE_OS2_PM"
#
# $(OBJ_DIR_2)/grinit.$(O): $(GRAPH)/grinit.c $(GRAPH_H)
# ifneq ($(LIBTOOL),)
# 	$(LIBTOOL) --mode=compile $(CC) -static \
#                    $(CFLAGS) $(GRAPH_INCLUDES:%=$I%) \
#                    $(DEVICES:%=$DDEVICE_%) $T$(subst /,$(COMPILER_SEP),$@ $<)
# else
# 	$(CC) $(CFLAGS) $(GRAPH_INCLUDES:%=$I%) \
#               $(DEVICES:%=$DDEVICE_%) $T$(subst /,$(COMPILER_SEP),$@ $<)
# endif
