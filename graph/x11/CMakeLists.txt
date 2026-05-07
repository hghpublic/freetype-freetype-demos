


# pkg-config --list-all
# find_package(PkgConfig)
# pkg_check_modules(ALLEGRO4 allegro REQUIRED)

# find_package(Allegro CONFIG REQUIRED)

# message("ALLEGRO4_LIBRARIES: ${ALLEGRO4_LIBRARIES}")
# message("ALLEGRO4_INCLUDE_DIRS: ${ALLEGRO4_INCLUDE_DIRS}")

set(target grx11)
add_library(${target} STATIC)
target_include_directories(${target} PRIVATE
    ${PROJECT_SOURCE_DIR}
)
target_sources(${target} PRIVATE grx11.c)
target_compile_features(${target} PRIVATE c_std_23)
set_target_properties(${target} PROPERTIES
    FOLDER "graph"
)

