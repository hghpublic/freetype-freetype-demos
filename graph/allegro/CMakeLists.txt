
# pkg-config --list-all
find_package(PkgConfig)
pkg_check_modules(ALLEGRO4 allegro REQUIRED)

# find_package(Allegro CONFIG REQUIRED)

message("ALLEGRO4_LIBRARIES: ${ALLEGRO4_LIBRARIES}")
message("ALLEGRO4_INCLUDE_DIRS: ${ALLEGRO4_INCLUDE_DIRS}")

set(target gralleg)
add_library(${target} STATIC)
target_include_directories(${target} PRIVATE
    ${PROJECT_SOURCE_DIR}
    ${ALLEGRO4_INCLUDE_DIRS}
)
target_sources(${target} PRIVATE gralleg.c)
# target_link_libraries(${target} PRIVATE Allegro::allegro Allegro::allegro_ttf Allegro::allegro_font Allegro::allegro_main)
target_compile_features(${target} PRIVATE c_std_23)
set_target_properties(${target} PROPERTIES
    FOLDER "graph"
)

