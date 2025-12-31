## Create libraries
add_library(plink2_lib plink2_adjust.cc
                        plink2_base.cc
                        plink2_bgzf.cc
                        plink2_bitmap.cc
                        plink2_bits.cc
                        plink2_cmdline.cc
                        plink2_common.cc
                        plink2_compress_stream.cc
                        plink2_cpu.cc
                        plink2_data.cc
                        plink2_decompress.cc
                        plink2_export.cc
                        plink2_export_legacy.cc
                        plink2_family.cc
                        plink2_fasta.cc
                        plink2_filter.cc
                        plink2_fmath.cc
                        plink2_glm.cc
                        plink2_glm_linear.cc
                        plink2_glm_logistic.cc
                        plink2_glm_shared.cc
                        plink2_help.cc
                        plink2_htable.cc
                        plink2_import.cc
                        plink2_import_legacy.cc
                        plink2_ld.cc
                        plink2_matrix.cc
                        plink2_matrix_calc.cc
                        plink2_memory.cc
                        plink2_merge.cc
                        plink2_misc.cc
                        plink2_psam.cc
                        plink2_pvar.cc
                        plink2_random.cc
                        plink2_set.cc
                        plink2_stats.cc
                        plink2_string.cc
                        plink2_text.cc
                        plink2_thread.cc
                        plink2_zstfile.cc
)

add_library(pgen_lib pgen_compress.cc
                    pgenlib_ffi_support.cc
                    pgenlib_misc.cc
                    pgenlib_read.cc
                    pgenlib_write.cc
)

add_library(others pvar_ffi_support.cc SFMT.c)

# Link header
target_include_directories(plink2_lib PUBLIC ${CMAKE_SOURCE_DIR}/include)
target_include_directories(pgen_lib PUBLIC ${CMAKE_SOURCE_DIR}/include)
target_include_directories(others PUBLIC ${CMAKE_SOURCE_DIR}/include)