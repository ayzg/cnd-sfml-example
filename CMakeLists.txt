cmake_minimum_required(VERSION 3.31)
project(cnd_sfml_example)

# Add the CandLanguage module to the project, which is required for using C& with CMake.
find_package(CAndLanguage REQUIRED)
cand_configure();

set(SFML_STATIC_LIBRARIES TRUE)
find_package(SFML
  COMPONENTS main system window graphics network audio 
  CONFIG 
  REQUIRED
)

# The cmake project must define targets. Then, inside your C& code you must create an external 
# process/library declaration using the expected target name. 
# The issue is that CMake targets may be named with incompatible characters to C& identifiers. 
# Such as the '-' symbol. To allow use of these targets you must alias them manually before
# linking them to your C& solution.
#
# By default these linked libraries will be available to all C& targets in the solution.
add_library(SFML::System ALIAS sfml-system)
add_library(SFML::Window ALIAS sfml-window)
add_library(SFML::Graphics ALIAS sfml-graphics)
add_library(SFML::Network ALIAS sfml-network)
add_library(SFML::Audio ALIAS sfml-audio)
add_library(SFML::Main ALIAS sfml-main)

cand_add_solution(MyCAndProject 
  SOURCES root.cnd
  TARGETS  SFML::System
           SFML::Window
           SFML::Graphics
           SFML::Network
           SFML::Audio
           SFML::Main
)