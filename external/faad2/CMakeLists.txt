cmake_minimum_required(VERSION 3.6)

set(lib_name libfaad)
set(target_name ext_${lib_name})

project(${lib_name})

set(${target_name}_src
	config.h
	libfaad/analysis.h
	libfaad/bits.c
	libfaad/bits.h
	libfaad/cfft.c
	libfaad/cfft.h
	libfaad/cfft_tab.h
	libfaad/common.c
	libfaad/common.h
	libfaad/decoder.c
	libfaad/drc.c
	libfaad/drc.h
	libfaad/drm_dec.c
	libfaad/drm_dec.h
	libfaad/error.c
	libfaad/error.h
	libfaad/faad2.pc.in
	libfaad/filtbank.c
	libfaad/filtbank.h
	libfaad/fixed.h
	libfaad/hcr.c
	libfaad/huffman.c
	libfaad/huffman.h
	libfaad/ic_predict.c
	libfaad/ic_predict.h
	libfaad/iq_table.h
	libfaad/is.c
	libfaad/is.h
	libfaad/kbd_win.h
	libfaad/lt_predict.c
	libfaad/lt_predict.h
	libfaad/mdct.c
	libfaad/mdct.h
	libfaad/mdct_tab.h
	libfaad/mp4.c
	libfaad/mp4.h
	libfaad/ms.c
	libfaad/ms.h
	libfaad/output.c
	libfaad/output.h
	libfaad/pns.c
	libfaad/pns.h
	libfaad/ps_dec.c
	libfaad/ps_dec.h
	libfaad/ps_syntax.c
	libfaad/ps_tables.h
	libfaad/pulse.c
	libfaad/pulse.h
	libfaad/rvlc.c
	libfaad/rvlc.h
	libfaad/sbr_dct.c
	libfaad/sbr_dct.h
	libfaad/sbr_dec.c
	libfaad/sbr_dec.h
	libfaad/sbr_e_nf.c
	libfaad/sbr_e_nf.h
	libfaad/sbr_fbt.c
	libfaad/sbr_fbt.h
	libfaad/sbr_hfadj.c
	libfaad/sbr_hfadj.h
	libfaad/sbr_hfgen.c
	libfaad/sbr_hfgen.h
	libfaad/sbr_huff.c
	libfaad/sbr_huff.h
	libfaad/sbr_noise.h
	libfaad/sbr_qmf.c
	libfaad/sbr_qmf.h
	libfaad/sbr_qmf_c.h
	libfaad/sbr_syntax.c
	libfaad/sbr_syntax.h
	libfaad/sbr_tf_grid.c
	libfaad/sbr_tf_grid.h
	libfaad/sine_win.h
	libfaad/specrec.c
	libfaad/specrec.h
	libfaad/ssr.c
	libfaad/ssr.h
	libfaad/ssr_fb.c
	libfaad/ssr_fb.h
	libfaad/ssr_ipqf.c
	libfaad/ssr_ipqf.h
	libfaad/ssr_win.h
	libfaad/structs.h
	libfaad/syntax.c
	libfaad/syntax.h
	libfaad/tns.c
	libfaad/tns.h
	libfaad/codebook/hcb.h
	libfaad/codebook/hcb_1.h
	libfaad/codebook/hcb_10.h
	libfaad/codebook/hcb_11.h
	libfaad/codebook/hcb_2.h
	libfaad/codebook/hcb_3.h
	libfaad/codebook/hcb_4.h
	libfaad/codebook/hcb_5.h
	libfaad/codebook/hcb_6.h
	libfaad/codebook/hcb_7.h
	libfaad/codebook/hcb_8.h
	libfaad/codebook/hcb_9.h
	libfaad/codebook/hcb_sf.h
)

add_library(${target_name} STATIC
	${${target_name}_src}
)

target_include_directories(${target_name}
	PUBLIC .
	PUBLIC ./include
	PUBLIC ./libfaad
)

SET(CMAKE_C_FLAGS "-DHAVE_CONFIG_H") # common.h

if(ANDROID)
	if(${ANDROID_ABI} STREQUAL "armeabi-v7a")
		set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=armv7-a -mfloat-abi=softfp")
	endif()
endif()

set_target_properties(${target_name}
	PROPERTIES
	ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
	LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
	FOLDER "External"
)

