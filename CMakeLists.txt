cmake_minimum_required(VERSION 3.5)
project(twitch VERSION 0.1 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 98)

add_definitions(-fno-exceptions -fno-rtti)

add_library(SYS STATIC
    libSYS/src/cmdl_parser.cpp
    libSYS/src/conv_string.cpp
    libSYS/src/genericStds.cpp
    libSYS/src/wav_file.cpp)
target_include_directories(SYS PUBLIC libSYS/include)

add_library(FDK STATIC
    libFDK/src/autocorr2nd.cpp
    libFDK/src/dct.cpp
    libFDK/src/FDK_bitbuffer.cpp
    libFDK/src/FDK_core.cpp
    libFDK/src/FDK_crc.cpp
    libFDK/src/FDK_hybrid.cpp
    libFDK/src/FDK_tools_rom.cpp
    libFDK/src/FDK_trigFcts.cpp
    libFDK/src/fft.cpp
    libFDK/src/fft_rad2.cpp
    libFDK/src/fixpoint_math.cpp
    libFDK/src/mdct.cpp
    libFDK/src/qmf.cpp
    libFDK/src/scale.cpp)
target_include_directories(FDK PUBLIC libFDK/include)
target_link_libraries(FDK PUBLIC SYS)

add_library(MpegTPEnc STATIC
    libMpegTPEnc/src/tpenc_adif.cpp
    libMpegTPEnc/src/tpenc_adts.cpp
    libMpegTPEnc/src/tpenc_asc.cpp
    libMpegTPEnc/src/tpenc_latm.cpp
    libMpegTPEnc/src/tpenc_lib.cpp)
target_include_directories(MpegTPEnc PUBLIC libMpegTPEnc/include)
target_link_libraries(MpegTPEnc PUBLIC FDK)

add_library(SBRenc STATIC
    libSBRenc/src/bit_sbr.cpp
    libSBRenc/src/env_bit.cpp
    libSBRenc/src/fram_gen.cpp
    libSBRenc/src/mh_det.cpp
    libSBRenc/src/ps_bitenc.cpp
    libSBRenc/src/ps_encode.cpp
    libSBRenc/src/resampler.cpp
    libSBRenc/src/sbr_encoder.cpp
    libSBRenc/src/sbr_ram.cpp
    libSBRenc/src/ton_corr.cpp
    libSBRenc/src/code_env.cpp
    libSBRenc/src/env_est.cpp
    libSBRenc/src/invf_est.cpp
    libSBRenc/src/nf_est.cpp
    libSBRenc/src/ps_main.cpp
    libSBRenc/src/sbrenc_freq_sca.cpp
    libSBRenc/src/sbr_misc.cpp
    libSBRenc/src/sbr_rom.cpp
    libSBRenc/src/tran_det.cpp)
target_include_directories(SBRenc PUBLIC libSBRenc/include)
target_link_libraries(SBRenc PUBLIC FDK)

add_library(AACenc STATIC
    libAACenc/src/aacenc.cpp
    libAACenc/src/aacEnc_ram.cpp
    libAACenc/src/band_nrg.cpp
    libAACenc/src/block_switch.cpp
    libAACenc/src/grp_data.cpp
    libAACenc/src/metadata_main.cpp
    libAACenc/src/pre_echo_control.cpp
    libAACenc/src/quantize.cpp
    libAACenc/src/tonality.cpp
    libAACenc/src/aacEnc_rom.cpp
    libAACenc/src/bandwidth.cpp
    libAACenc/src/channel_map.cpp
    libAACenc/src/intensity.cpp
    libAACenc/src/ms_stereo.cpp
    libAACenc/src/psy_configuration.cpp
    libAACenc/src/sf_estim.cpp
    libAACenc/src/transform.cpp
    libAACenc/src/aacenc_lib.cpp
    libAACenc/src/aacenc_tns.cpp
    libAACenc/src/bit_cnt.cpp
    libAACenc/src/chaosmeasure.cpp
    libAACenc/src/line_pe.cpp
    libAACenc/src/noisedet.cpp
    libAACenc/src/psy_main.cpp
    libAACenc/src/spreading.cpp
    libAACenc/src/aacenc_pns.cpp
    libAACenc/src/adj_thr.cpp
    libAACenc/src/bitenc.cpp
    libAACenc/src/dyn_bits.cpp
    libAACenc/src/metadata_compressor.cpp
    libAACenc/src/pnsparam.cpp
    libAACenc/src/qc_main.cpp)
target_include_directories(AACenc PUBLIC libAACenc/include)
target_link_libraries(AACenc PUBLIC FDK MpegTPEnc SBRenc)

add_executable(aac-enc aac-enc.c wavreader.c)
target_link_libraries(aac-enc PUBLIC AACenc)

