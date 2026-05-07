

# if using valgrind, set BUILD_WITH_ASAN to OFF
option(BUILD_WITH_ASAN "Build with ASAN" ON)


find_package(ZLIB REQUIRED)
# target_link_libraries(${target} PRIVATE ZLIB::ZLIB)


find_package(PNG REQUIRED)
# target_link_libraries(${target} PRIVATE PNG::PNG)


find_package(Freetype REQUIRED)
# target_link_libraries(${target} PRIVATE Freetype::Freetype)

###################################################################
#
# The list of demonstration programs to build.
#
# Note that ttdebug only works if the FreeType's `truetype' driver has
# been compiled with TT_CONFIG_OPTION_BYTECODE_INTERPRETER defined.
#
# EXES := ftbench \
#           ftdump  \
#           ftlint  \
#           ttdebug

# Comment out the next line if you don't have a graphics subsystem.
#   EXES += ftdiff   \
#           ftgamma  \
#           ftgrid   \
#           ftmulti  \
#           ftsdf    \
#           ftstring \
#           ftview

# ftvalid requires ftgxval.c and ftotval.c
#
#   ifneq ($(findstring ftgxval.c,$(BASE_EXTENSIONS)),)
#     ifneq ($(findstring ftotval.c,$(BASE_EXTENSIONS)),)
#       EXES += ftvalid
#     endif
#   endif

# The following programs are not compiled automatically; either comment
# out the affected line or use the program name as a Makefile target.
#
# EXES += ftchkwd
# EXES += ftmemchk
# EXES += ftpatchk
# EXES += fttimer
# EXES += testname


####################################################################
#
# Rules for compiling object files for text-only demos.

set(target common)
add_library(${target} STATIC)
target_sources(${target} PRIVATE
    common.c
    strbuf.c
    output.c
    md5.c
    mlgetopt.c
)
target_compile_features(${target} PRIVATE c_std_23)
target_link_libraries(${target} PRIVATE Freetype::Freetype)
set_target_properties(${target} PROPERTIES
    FOLDER "libs"
)

set(target ftview)
add_executable(${target})
target_compile_definitions(${target} PRIVATE DEVICE_X11)
target_sources(${target} PRIVATE ${target}.c ftcommon.c rsvg-port.c ftpngout.c)
target_compile_features(${target} PRIVATE c_std_23)
target_link_libraries(${target} PRIVATE Freetype::Freetype)
target_link_libraries(${target} PRIVATE common graph gralleg)
if(BUILD_WITH_ASAN)
    target_compile_options(${target} PRIVATE -fsanitize=address)
    target_link_options(${target} PRIVATE -fsanitize=address)
endif()
set_target_properties(${target} PROPERTIES
    FOLDER "graphics"
)

add_custom_target(ftview-demo
    COMMAND ftview $ENV{DEV_ROOT}/fonts/google/Roboto/OpenSans-Regular.ttf 
    DEPENDS ftview
    WORKING_DIRECTORY $<TARGET_FILE_DIR:ftview>
    COMMENT "Running ftview demo"
)

# set(target ftdump)
# add_executable(${target})
# target_sources(${target} PRIVATE ftdump.c)
# target_compile_features(${target} PRIVATE c_std_23)
# target_link_libraries(${target} PRIVATE Freetype::Freetype)
# target_link_libraries(${target} PRIVATE common)
# if(BUILD_WITH_ASAN)
#     target_compile_options(${target} PRIVATE -fsanitize=address)
#     target_link_options(${target} PRIVATE -fsanitize=address)
# endif()
# set_target_properties(${target} PROPERTIES
#     FOLDER "cli"
# )

set(targets_cli
    ftdump
    ftlint
    ftbench
    ftpatchk
    ftchkwd
    # compos
    ftmemchk
    fttimer
    fttry
    testname
    # ftsbit
)

foreach(target IN LISTS targets_cli)
    add_executable(${target})
    target_sources(${target} PRIVATE ${target}.c)
    target_compile_features(${target} PRIVATE c_std_23)
    target_link_libraries(${target} PRIVATE Freetype::Freetype)
    target_link_libraries(${target} PRIVATE common)
    if(BUILD_WITH_ASAN)
        target_compile_options(${target} PRIVATE -fsanitize=address)
        target_link_options(${target} PRIVATE -fsanitize=address)
    endif()
    set_target_properties(${target} PROPERTIES
        FOLDER "cli"
    )
endforeach()


#   $(OBJ_DIR_2)/ftbench.$(SO): $(SRC_DIR)/ftbench.c
# 	  $(COMPILE) $T$(subst /,$(COMPILER_SEP),$@ $<) $(EXTRAFLAGS)

#   $(OBJ_DIR_2)/ftpatchk.$(SO): $(SRC_DIR)/ftpatchk.c
# 	  $(COMPILE) $T$(subst /,$(COMPILER_SEP),$@ $<) $(EXTRAFLAGS)

#   $(OBJ_DIR_2)/ftchkwd.$(SO): $(SRC_DIR)/ftchkwd.c
# 	  $(COMPILE) $T$(subst /,$(COMPILER_SEP),$@ $<) $(EXTRAFLAGS)

#   $(OBJ_DIR_2)/compos.$(SO): $(SRC_DIR)/compos.c
# 	  $(COMPILE) $T$(subst /,$(COMPILER_SEP),$@ $<)


#  $(OBJ_DIR_2)/ftsbit.$(SO): $(SRC_DIR)/ftsbit.c
#	  $(COMPILE) $T$(subst /,$(COMPILER_SEP),$@ $<)


# We simplify the dependencies on the graphics library by using
# $(GRAPH_LIB) directly.

#   $(OBJ_DIR_2)/ftgamma.$(SO): $(SRC_DIR)/ftgamma.c \
#                               $(SRC_DIR)/ftcommon.h \
#                               $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftstring.$(SO): $(SRC_DIR)/ftstring.c \
#                                $(SRC_DIR)/ftcommon.h \
#                                $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftview.$(SO): $(SRC_DIR)/ftview.c \
#                              $(SRC_DIR)/ftcommon.h \
#                              $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftsdf.$(SO): $(SRC_DIR)/ftsdf.c \
#                             $(SRC_DIR)/ftcommon.h \
#                             $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftdiff.$(SO): $(SRC_DIR)/ftdiff.c \
#                              $(SRC_DIR)/ftcommon.h \
#                              $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftgrid.$(SO): $(SRC_DIR)/ftgrid.c \
#                              $(SRC_DIR)/ftcommon.h \
#                              $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftmulti.$(SO): $(SRC_DIR)/ftmulti.c \
#                               $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftcommon.$(SO): $(SRC_DIR)/ftcommon.c \
#                                $(SRC_DIR)/ftcommon.h \
#                                $(GRAPH_LIB)
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/ftpngout.$(SO): $(SRC_DIR)/ftpngout.c $(SRC_DIR)/ftcommon.h
# 	  $(COMPILE) $(GRAPH_INCLUDES:%=$I%) \
#                      $T$(subst /,$(COMPILER_SEP),$@ $<)

#   $(OBJ_DIR_2)/rsvg-port.$(SO): $(SRC_DIR)/rsvg-port.c $(SRC_DIR)/rsvg-port.h
# 	  $(COMPILE) $T$(subst /,$(COMPILER_SEP),$@ $<)

#   FTCOMMON_OBJ := $(OBJ_DIR_2)/ftcommon.$(SO) \
#                   $(OBJ_DIR_2)/ftpngout.$(SO) \
#                   $(OBJ_DIR_2)/rsvg-port.$(SO)

# valgrind target

# set(target xxx)
# add_custom_target(valgrind
#     COMMAND valgrind
#         --leak-check=full
#         --show-leak-kinds=all
#         --track-origins=yes
#         -s --suppressions=${CMAKE_SOURCE_DIR}/xxxx.valgrind.supp
#         --gen-suppressions=all
#         --log-file=${CMAKE_SOURCE_DIR}/dev/valgrind/logs/memcheck.log
#         $<TARGET_FILE:${target}>
#     DEPENDS ${target}
#     WORKING_DIRECTORY $<TARGET_FILE_DIR:${target}>
#     COMMENT "Running xxx with valgrind"
# )
