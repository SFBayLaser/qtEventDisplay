# ======================================================================
#  qtEventDsiplay main build file
#
#  cd .../path/to/build/directory
#  source .../path/to/qtEventDisplay/ups/setup_for_development <-d|-p>
#  cmake [-DCMAKE_INSTALL_PREFIX=/install/path]
#        -DCMAKE_BUILD_TYPE=$CETPKG_TYPE
#        $CETPKG_SOURCE
#  make
#  make test
#  make install
#  make package (builds distribution tarfile)
# ======================================================================


# use cmake 2.8 or later
cmake_minimum_required (VERSION 2.8)

project(uboonedev)

# cetbuildtools contains our cmake modules
SET ( CETBUILDTOOLS_VERSION $ENV{CETBUILDTOOLS_VERSION} )
IF (NOT CETBUILDTOOLS_VERSION)
    MESSAGE (FATAL_ERROR "ERROR: setup cetbuildtools to get the cmake modules")
ENDIF()

set(CMAKE_MODULE_PATH $ENV{ART_DIR}/Modules
		      $ENV{CETBUILDTOOLS_DIR}/Modules
		      ${CMAKE_MODULE_PATH})

include(CetCMakeEnv)
cet_cmake_env()

set_install_root()

cet_set_compiler_flags(DIAGS CAUTIOUS
  WERROR
  NO_UNDEFINED
  ALLOW_DEPRECATIONS
  EXTRA_FLAGS -pedantic -Wno-unused-local-typedefs
  )

cet_report_compiler_flags()

# these are minimum required versions, not the actual product versions
find_ups_product( larsoft         v1_00_00 )
find_ups_product( larana          v1_00_00 )
find_ups_product( larcore         v1_00_00 )
find_ups_product( larcoreobj      v1_00_00 )
find_ups_product( lardata         v1_00_00 )
find_ups_product( lardataobj      v1_00_00 )
find_ups_product( larevt          v1_00_00 )
find_ups_product( larsim          v1_00_00 )
find_ups_product( larreco         v1_00_00 )
find_ups_product( art             v1_08_00 )
find_ups_product( cetbuildtools   v3_07_00 )
find_ups_product( postgresql      v9_1_5 )

cet_check_gcc()

# macros for dictionary and simple_plugin
include(ArtDictionary)
include(ArtMake)
include(BuildPlugins)

# ADD SOURCE CODE SUBDIRECTORIES HERE
add_subdirectory(EventDisplay)

# add the subdirectory for the fcl files
add_subdirectory(fcl)

# tests
add_subdirectory(test)

# ups - table and config files
add_subdirectory(ups)

# packaging utility
include(UseCPack)
