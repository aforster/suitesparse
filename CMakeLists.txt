cmake_minimum_required(VERSION 2.8.3)
project(suitesparse)

set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED)

include(ExternalProject)

set(SUITESPARSE_INCLUDE_DEST ${CATKIN_DEVEL_PREFIX}/include/suitesparse)

file(MAKE_DIRECTORY ${SUITESPARSE_INCLUDE_DEST})

set(VERSION 4.4.1)

catkin_package(
    DEPENDS
    CATKIN_DEPENDS
    INCLUDE_DIRS include ${SUITESPARSE_INCLUDE_DEST}
    LIBRARIES ${PROJECT_NAME}
    CFG_EXTRAS suitesparse-extras.cmake
)

ExternalProject_Add(suitesparse_src
  DOWNLOAD_COMMAND rm -f SuiteSparse-${VERSION}.tar.gz && wget --retry-connrefused --waitretry=1 --timeout=40 --tries 3 http://faculty.cse.tamu.edu/davis/SuiteSparse/SuiteSparse-${VERSION}.tar.gz
  PATCH_COMMAND tar -xzf ../SuiteSparse-${VERSION}.tar.gz && rm -rf ../suitesparse_src-build/SuiteSparse && sed -i.bu "s/\\/usr\\/local\\/lib/..\\/lib/g" SuiteSparse/SuiteSparse_config/SuiteSparse_config.mk && sed -i.bu "s/\\/usr\\/local\\/include/..\\/include/g" SuiteSparse/SuiteSparse_config/SuiteSparse_config.mk && mv SuiteSparse ../suitesparse_src-build/
  CONFIGURE_COMMAND ""
  BUILD_COMMAND export CFLAGS=-DNTIMER && cd SuiteSparse && make library -j8 -l8
  INSTALL_COMMAND cd SuiteSparse && mkdir -p lib && mkdir -p include && make install && cd lib && cp libamd.2.4.1.a libcamd.2.4.1.a libcholmod.3.0.3.a libcxsparse.3.1.4.a libldl.2.2.1.a libspqr.2.0.0.a libumfpack.5.7.1.a libamd.a	libcamd.a libcholmod.a	libcxsparse.a libldl.a libspqr.a libumfpack.a libbtf.1.2.1.a	libccolamd.2.9.1.a libcolamd.2.9.1.a libklu.1.3.2.a librbio.2.2.1.a libsuitesparseconfig.4.4.0.a libbtf.a	libccolamd.a libcolamd.a		libklu.a librbio.a libsuitesparseconfig.a  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/ && cd .. && cd include && cp amd.h cholmod_matrixops.h SuiteSparseQR_definitions.h umfpack_load_symbolic.h umfpack_save_symbolic.h btf.h cholmod_modify.h SuiteSparseQR.hpp umfpack_numeric.h umfpack_scale.h camd.h cholmod_partition.h umfpack_col_to_triplet.h umfpack_qsymbolic.h umfpack_solve.h ccolamd.h cholmod_supernodal.h umfpack_defaults.h umfpack_report_control.h umfpack_symbolic.h cholmod_blas.h cholmod_template.h umfpack_free_numeric.h umfpack_report_info.h umfpack_tictoc.h cholmod_camd.h colamd.h umfpack_free_symbolic.h umfpack_report_matrix.h umfpack_timer.h cholmod_check.h cs.h umfpack_get_determinant.h umfpack_report_numeric.h umfpack_transpose.h cholmod_cholesky.h klu.h umfpack_get_lunz.h umfpack_report_perm.h umfpack_triplet_to_col.h cholmod_complexity.h ldl.h umfpack_get_numeric.h umfpack_report_status.h umfpack_wsolve.h cholmod_config.h RBio.h umfpack_get_symbolic.h umfpack_report_symbolic.h cholmod_core.h spqr.hpp umfpack_global.h umfpack_report_triplet.h cholmod.h SuiteSparse_config.h umfpack.h umfpack_report_vector.h cholmod_io64.h SuiteSparseQR_C.h umfpack_load_numeric.h umfpack_save_numeric.h ${SUITESPARSE_INCLUDE_DEST} 
)

add_library(${PROJECT_NAME} src/export_lib_hack.cc)
target_link_libraries(${PROJECT_NAME} 
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libamd.2.4.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcamd.2.4.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcholmod.3.0.3.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcxsparse.3.1.4.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libldl.2.2.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libspqr.2.0.0.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libumfpack.5.7.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libamd.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcamd.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcholmod.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcxsparse.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libldl.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libspqr.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libumfpack.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libbtf.1.2.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libccolamd.2.9.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcolamd.2.9.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libklu.1.3.2.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/librbio.2.2.1.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libsuitesparseconfig.4.4.0.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libbtf.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libccolamd.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libcolamd.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libklu.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/librbio.a
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libsuitesparseconfig.a)
add_dependencies(${PROJECT_NAME} suitesparse_src)

install(DIRECTORY include
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)
install(DIRECTORY lib
  DESTINATION ${CATKIN_GLOBAL_LIB_DESTINATION}
)
install(DIRECTORY share
  DESTINATION ${CATKIN_GLOBAL_SHARE_DESTINATION}
)

