
# This must be before the target_sources_local(), so that the target is added as CUDA
if(USE_CUDA)
  set_source_files_properties(Kron.cpp DIRECTORY ${CMAKE_SOURCE_DIR} PROPERTIES LANGUAGE CUDA)
endif()

# Property propagation test modes (no CUDA toolchain needed)
if(NOT KRON_PROP_MODE STREQUAL "NONE")
  message(STATUS "[KRON_PROP_TEST] mode=${KRON_PROP_MODE} (defined in src/linalg/CMakeLists.txt)")
  target_compile_definitions(cytnx PRIVATE KRON_PROPERTY_TESTING)
  if(KRON_PROP_MODE STREQUAL "TARGET")
    message(STATUS "[KRON_PROP_TEST] setting COMPILE_DEFINITIONS on Kron.cpp with TARGET_DIRECTORY cytnx")
    set_source_files_properties(
      Kron.cpp
      TARGET_DIRECTORY cytnx
      PROPERTIES COMPILE_DEFINITIONS KRON_PROPERTY_MARKER
    )
  elseif(KRON_PROP_MODE STREQUAL "ROOT_DIRECTORY")
    message(STATUS "[KRON_PROP_TEST] setting COMPILE_DEFINITIONS on Kron.cpp with DIRECTORY ${CMAKE_SOURCE_DIR}")
    # Mimic the old pattern: write properties into the root scope from a subdir
    # using a subdir-relative path.
    set_source_files_properties(
      Kron.cpp
      DIRECTORY ${CMAKE_SOURCE_DIR}
      PROPERTIES COMPILE_DEFINITIONS KRON_PROPERTY_MARKER
    )
  else()
    message(FATAL_ERROR "Unknown KRON_PROP_MODE: ${KRON_PROP_MODE}")
  endif()
endif()

target_sources_local(cytnx
    PRIVATE
    #linalg_internal_interface.hpp
    Add.cpp
    Axpy.cpp
    Ger.cpp
    Arnoldi.cpp
    Axpy.cpp
    Ger.cpp
    iAdd.cpp
    Min.cpp
    Max.cpp
    Sum.cpp
    Conj.cpp
    Conj_.cpp
    Cpr.cpp
    Diag.cpp
    Div.cpp
    iDiv.cpp
    Det.cpp
    Eig.cpp
    Eigh.cpp
    Qr.cpp
    Qdr.cpp
    Exp.cpp
    ExpM.cpp
    Exp_.cpp
    Expf.cpp
    Expf_.cpp
    ExpH.cpp
    ExpM.cpp
    InvM.cpp
    InvM_.cpp
    Inv_.cpp
    Inv.cpp
    Matmul.cpp
    Matmul_dg.cpp
    Mul.cpp
    iMul.cpp
    Mod.cpp
    Outer.cpp
    Kron.cpp
    Trace.cpp
    Sub.cpp
    iSub.cpp
    Rand_isometry.cpp
    Svd.cpp
    Gesvd.cpp
    Rsvd.cpp
    Svd_truncate.cpp
    Gesvd_truncate.cpp
    Rsvd_truncate.cpp
    Gemm.cpp
    Gemm_Batch.cpp
    Tensordot.cpp
    Directsum.cpp
    Tensordot_dg.cpp
    Vectordot.cpp
    Pow.cpp
    Pow_.cpp
    Abs.cpp
    Hosvd.cpp
    Abs_.cpp
    Dot.cpp
    Norm.cpp
    Tridiag.cpp
    Lanczos.cpp
    Lanczos_ER.cpp
    Lanczos_Exp.cpp
    Lanczos_Gnd.cpp
    Lanczos_Gnd_Ut.cpp
    Lstsq.cpp
    #linalg_internal_interface.cpp
)

#add_subdirectory(linalg_internal_cpu)
#if(USE_CUDA)
#  add_subdirectory(linalg_internal_gpu)
#endif()
