cmake_minimum_required(VERSION 3.16.0 FATAL_ERROR)

add_subdirectory(SimpleXML Main.c)

# Add in MesoXML
add_subdirectory(../../ MesoXML)
target_link_libraries(SimpleXML PRIVATE MesoXML)
target_include_directories(SimpleXML PRIVATE ${MESOXML_include_dirs})