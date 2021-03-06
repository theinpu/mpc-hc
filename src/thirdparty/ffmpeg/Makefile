# (C) 2007-2012 see Authors.txt
#
# This file is part of MPC-HC.
#
# MPC-HC is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# MPC-HC is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

CC = gcc.exe
BIN_DIR  = ../../../bin
ZLIB_DIR = ../zlib

ifeq ($(64BIT),yes)
    MY_ARCH = x64
else
    MY_ARCH = Win32
endif

ifeq ($(DEBUG),yes)
    OUT_DIR_PREFIX = Debug
else
    OUT_DIR_PREFIX = Release
endif

OBJ_DIR     = $(BIN_DIR)/obj/$(OUT_DIR_PREFIX)_$(MY_ARCH)/ffmpeg/
OUT_LIB_DIR = $(BIN_DIR)/lib/$(OUT_DIR_PREFIX)_$(MY_ARCH)
OUT_LIB     = $(OUT_LIB_DIR)/ffmpeg.lib


# Compiler and yasm flags
CFLAGS    = -I. -I.. -Ilibavcodec -Ilibavutil -I$(ZLIB_DIR) -DHAVE_AV_CONFIG_H \
            -D_ISOC99_SOURCE -D_XOPEN_SOURCE=600 -D_LARGEFILE_SOURCE \
            -D_FILE_OFFSET_BITS=64 -std=gnu99 -pipe -mthreads
OPTFLAGS  = -O2 -fno-common -fno-tree-vectorize -fomit-frame-pointer
YASMFLAGS = -I. -Pconfig.asm

ifeq ($(64BIT),yes)
    GCC_PREFIX = x86_64-w64-mingw32-
    TARGET_OS  = x86_64-w64-mingw32
    CFLAGS    += -DWIN64 -D_WIN64 -DARCH_X86_64 -DPIC
    OPTFLAGS  += -m64 -fno-leading-underscore
    YASMFLAGS += -f win64 -m amd64 -DWIN64=1 -DARCH_X86_32=0 -DARCH_X86_64=1 -DPIC
else
    TARGET_OS  = i686-w64-mingw32
    CFLAGS    += -DWIN32 -D_WIN32 -DARCH_X86_32
    OPTFLAGS  += -m32 -march=i686 -mmmx -msse -mfpmath=sse
    YASMFLAGS += -f win32 -m x86 -DWIN32=1 -DARCH_X86_32=1 -DARCH_X86_64=0 -DPREFIX
endif

ifeq ($(DEBUG),yes)
    CFLAGS    += -DDEBUG -D_DEBUG -g
else
    CFLAGS    += -DNDEBUG -UDEBUG -U_DEBUG
endif


# Object directories
OBJ_DIRS = $(OBJ_DIR) \
    $(OBJ_DIR)libavcodec \
    $(OBJ_DIR)libavcodec/x86 \
    $(OBJ_DIR)libavresample \
    $(OBJ_DIR)libavresample/x86 \
    $(OBJ_DIR)libavutil \
    $(OBJ_DIR)libavutil/x86 \
    $(OBJ_DIR)libswscale \
    $(OBJ_DIR)libswscale/x86 \
    $(OUT_LIB_DIR)


# Targets
all: make_objdirs $(OUT_LIB)

make_objdirs: $(OBJ_DIRS)
$(OBJ_DIRS):
	$(shell test -d $(@) || mkdir -p $(@))

clean:
	@echo Cleaning ffmpeg...
	rm -f $(OUT_LIB)
	rm -rf $(OBJ_DIR)


# Objects
SRCS_C = \
    libavcodec/aac_ac3_parser.c \
    libavcodec/aac_parser.c \
    libavcodec/aacadtsdec.c \
    libavcodec/aacdec.c \
    libavcodec/aacps.c \
    libavcodec/aacpsdsp.c \
    libavcodec/aacsbr.c \
    libavcodec/aactab.c \
    libavcodec/ac3.c \
    libavcodec/ac3_parser.c \
    libavcodec/ac3dec.c \
    libavcodec/ac3dec_data.c \
    libavcodec/ac3dsp.c \
    libavcodec/ac3tab.c \
    libavcodec/acelp_filters.c \
    libavcodec/acelp_pitch_delay.c \
    libavcodec/acelp_vectors.c \
    libavcodec/adpcm.c \
    libavcodec/adpcm_data.c \
    libavcodec/alac.c \
    libavcodec/allcodecs.c \
    libavcodec/alsdec.c \
    libavcodec/amrnbdec.c \
    libavcodec/amrwbdec.c \
    libavcodec/atrac.c \
    libavcodec/atrac3.c \
    libavcodec/avfft.c \
    libavcodec/avpacket.c \
    libavcodec/bgmc.c \
    libavcodec/bitstream.c \
    libavcodec/cabac.c \
    libavcodec/celp_filters.c \
    libavcodec/celp_math.c \
    libavcodec/codec_desc.c \
    libavcodec/cook.c \
    libavcodec/dca.c \
    libavcodec/dca_parser.c \
    libavcodec/dcadec.c \
    libavcodec/dcadsp.c \
    libavcodec/dct.c \
    libavcodec/dct32.c \
    libavcodec/dct32_float.c \
    libavcodec/dsputil.c \
    libavcodec/eac3_data.c \
    libavcodec/eac3dec.c \
    libavcodec/error_resilience.c \
    libavcodec/faandct.c \
    libavcodec/faanidct.c \
    libavcodec/fft.c \
    libavcodec/flac.c \
    libavcodec/flacdata.c \
    libavcodec/flacdec.c \
    libavcodec/flacdsp.c \
    libavcodec/flvdec.c \
    libavcodec/fmtconvert.c \
    libavcodec/frame_thread_encoder.c \
    libavcodec/golomb.c \
    libavcodec/h263.c \
    libavcodec/h263_parser.c \
    libavcodec/h263dec.c \
    libavcodec/h264.c \
    libavcodec/h264_cabac.c \
    libavcodec/h264_cavlc.c \
    libavcodec/h264_direct.c \
    libavcodec/h264_loopfilter.c \
    libavcodec/h264_parser.c \
    libavcodec/h264_ps.c \
    libavcodec/h264_refs.c \
    libavcodec/h264_sei.c \
    libavcodec/h264dsp.c \
    libavcodec/h264idct.c \
    libavcodec/h264pred.c \
    libavcodec/huffman.c \
    libavcodec/imgconvert.c \
    libavcodec/indeo3.c \
    libavcodec/indeo4.c \
    libavcodec/indeo5.c \
    libavcodec/intelh263dec.c \
    libavcodec/intrax8.c \
    libavcodec/intrax8dsp.c \
    libavcodec/ituh263dec.c \
    libavcodec/ivi_common.c \
    libavcodec/ivi_dsp.c \
    libavcodec/jpegls.c \
    libavcodec/jpeglsdec.c \
    libavcodec/jrevdct.c \
    libavcodec/kbdwin.c \
    libavcodec/latm_parser.c \
    libavcodec/log2_tab.c \
    libavcodec/lsp.c \
    libavcodec/mathtables.c \
    libavcodec/mdct.c \
    libavcodec/mjpeg.c \
    libavcodec/mjpegbdec.c \
    libavcodec/mjpegdec.c \
    libavcodec/mlp.c \
    libavcodec/mlp_parser.c \
    libavcodec/mlpdec.c \
    libavcodec/mlpdsp.c \
    libavcodec/mpc_helper.c \
    libavcodec/mpeg12.c \
    libavcodec/mpeg12data.c \
    libavcodec/mpeg4audio.c \
    libavcodec/mpeg4video.c \
    libavcodec/mpeg4video_parser.c \
    libavcodec/mpeg4videodec.c \
    libavcodec/mpegaudio.c \
    libavcodec/mpegaudio_parser.c \
    libavcodec/mpegaudiodata.c \
    libavcodec/mpegaudiodec_float.c \
    libavcodec/mpegaudiodecheader.c \
    libavcodec/mpegaudiodsp.c \
    libavcodec/mpegaudiodsp_data.c \
    libavcodec/mpegaudiodsp_fixed.c \
    libavcodec/mpegaudiodsp_float.c \
    libavcodec/mpegvideo.c \
    libavcodec/mpegvideo_motion.c \
    libavcodec/mpegvideo_parser.c \
    libavcodec/msmpeg4.c \
    libavcodec/msmpeg4data.c \
    libavcodec/msrle.c \
    libavcodec/msrledec.c \
    libavcodec/nellymoser.c \
    libavcodec/nellymoserdec.c \
    libavcodec/options.c \
    libavcodec/parser.c \
    libavcodec/pthread.c \
    libavcodec/ra144.c \
    libavcodec/ra144dec.c \
    libavcodec/ra288.c \
    libavcodec/rdft.c \
    libavcodec/rv10.c \
    libavcodec/rv30.c \
    libavcodec/rv30dsp.c \
    libavcodec/rv34.c \
    libavcodec/rv34dsp.c \
    libavcodec/rv40.c \
    libavcodec/rv40dsp.c \
    libavcodec/sbrdsp.c \
    libavcodec/simple_idct.c \
    libavcodec/sinewin.c \
    libavcodec/sipr.c \
    libavcodec/sipr16k.c \
    libavcodec/sp5xdec.c \
    libavcodec/svq1.c \
    libavcodec/svq1dec.c \
    libavcodec/svq3.c \
    libavcodec/svq13.c \
    libavcodec/synth_filter.c \
    libavcodec/tscc.c \
    libavcodec/utils.c \
    libavcodec/vc1.c \
    libavcodec/vc1data.c \
    libavcodec/vc1dec.c \
    libavcodec/vc1dsp.c \
    libavcodec/vorbis.c \
    libavcodec/vorbis_data.c \
    libavcodec/vorbisdec.c \
    libavcodec/vp3.c \
    libavcodec/vp3dsp.c \
    libavcodec/vp5.c \
    libavcodec/vp56.c \
    libavcodec/vp56data.c \
    libavcodec/vp56dsp.c \
    libavcodec/vp56rac.c \
    libavcodec/vp6.c \
    libavcodec/vp6dsp.c \
    libavcodec/vp8.c \
    libavcodec/vp8dsp.c \
    libavcodec/wmv2.c \
    libavcodec/wmv2dec.c \
    libavcodec/xiph.c \
    libavcodec/x86/ac3dsp_init.c \
    libavcodec/x86/dsputil_mmx.c \
    libavcodec/x86/fdct.c \
    libavcodec/x86/fft_init.c \
    libavcodec/x86/fmtconvert_init.c \
    libavcodec/x86/h264_intrapred_init.c \
    libavcodec/x86/h264dsp_init.c \
    libavcodec/x86/idct_mmx.c \
    libavcodec/x86/idct_mmx_xvid.c \
    libavcodec/x86/idct_sse2_xvid.c \
    libavcodec/x86/mlpdsp.c \
    libavcodec/x86/motion_est.c \
    libavcodec/x86/mpegaudiodec.c \
    libavcodec/x86/mpegvideo.c \
    libavcodec/x86/rv34dsp_init.c \
    libavcodec/x86/rv40dsp_init.c \
    libavcodec/x86/sbrdsp_init.c \
    libavcodec/x86/simple_idct.c \
    libavcodec/x86/vc1dsp_init.c \
    libavcodec/x86/vc1dsp_mmx.c \
    libavcodec/x86/vp3dsp_init.c \
    libavcodec/x86/vp56dsp_init.c \
    libavcodec/x86/vp8dsp_init.c \
\
    libavresample/audio_convert.c \
    libavresample/audio_data.c \
    libavresample/audio_mix.c \
    libavresample/audio_mix_matrix.c \
    libavresample/options.c \
    libavresample/resample.c \
    libavresample/utils.c \
    libavresample/x86/audio_convert_init.c \
    libavresample/x86/audio_mix_init.c \
\
    libavutil/audio_fifo.c \
    libavutil/audioconvert.c \
    libavutil/avstring.c \
    libavutil/bprint.c \
    libavutil/cpu.c \
    libavutil/crc.c \
    libavutil/dict.c \
    libavutil/error.c \
    libavutil/eval.c \
    libavutil/fifo.c \
    libavutil/float_dsp.c \
    libavutil/imgutils.c \
    libavutil/intfloat_readwrite.c \
    libavutil/lfg.c \
    libavutil/log.c \
    libavutil/mathematics.c \
    libavutil/md5.c \
    libavutil/mem.c \
    libavutil/opt.c \
    libavutil/parseutils.c \
    libavutil/pixdesc.c \
    libavutil/random_seed.c \
    libavutil/rational.c \
    libavutil/samplefmt.c \
    libavutil/sha.c \
    libavutil/timecode.c \
    libavutil/utils.c \
    libavutil/x86/cpu.c \
    libavutil/x86/float_dsp_init.c \
\
    libswscale/input.c \
    libswscale/options.c \
    libswscale/output.c \
    libswscale/rgb2rgb.c \
    libswscale/swscale.c \
    libswscale/swscale_unscaled.c \
    libswscale/utils.c \
    libswscale/yuv2rgb.c \
    libswscale/x86/rgb2rgb.c \
    libswscale/x86/swscale.c \
    libswscale/x86/yuv2rgb.c

# Yasm objects
SRCS_YASM = \
    libavcodec/x86/ac3dsp.asm \
    libavcodec/x86/dct32.asm \
    libavcodec/x86/deinterlace.asm \
    libavcodec/x86/dsputil.asm \
    libavcodec/x86/fft.asm \
    libavcodec/x86/fmtconvert.asm \
    libavcodec/x86/h264_chromamc.asm \
    libavcodec/x86/h264_chromamc_10bit.asm \
    libavcodec/x86/h264_deblock.asm \
    libavcodec/x86/h264_deblock_10bit.asm \
    libavcodec/x86/h264_idct.asm \
    libavcodec/x86/h264_idct_10bit.asm \
    libavcodec/x86/h264_intrapred.asm \
    libavcodec/x86/h264_intrapred_10bit.asm \
    libavcodec/x86/h264_qpel_10bit.asm \
    libavcodec/x86/h264_weight.asm \
    libavcodec/x86/h264_weight_10bit.asm \
    libavcodec/x86/imdct36.asm \
    libavcodec/x86/rv34dsp.asm \
    libavcodec/x86/rv40dsp.asm \
    libavcodec/x86/sbrdsp.asm \
    libavcodec/x86/vc1dsp.asm \
    libavcodec/x86/vp3dsp.asm \
    libavcodec/x86/vp56dsp.asm \
    libavcodec/x86/vp8dsp.asm \
    libavresample/x86/audio_convert.asm \
    libavresample/x86/audio_mix.asm \
    libavresample/x86/util.asm \
    libavutil/x86/cpuid.asm \
    libavutil/x86/float_dsp.asm \
    libswscale/x86/input.asm \
    libswscale/x86/output.asm \
    libswscale/x86/scale.asm

OBJS = \
    $(SRCS_C:%.c=$(OBJ_DIR)%.o) \
    $(SRCS_YASM:%.asm=$(OBJ_DIR)%.o)


# Commands
$(OBJ_DIR)%.o: %.c
	@echo CC '  ' $<
	@$(GCC_PREFIX)$(CC) -c $(CFLAGS) $(OPTFLAGS) -MMD -o $@ $<

$(OBJ_DIR)%.o: %.asm
	@echo YASM: '  ' Assembling $<
	@yasm $(YASMFLAGS) -I$(<D)/ -o $@ $<

$(OUT_LIB): $(OBJS)
	@echo AR '  ' $@
	@$(GCC_PREFIX)ar rc $@ $(OBJS)

-include $(SRCS_C:%.c=$(OBJ_DIR)%.d)

.PHONY: clean make_objdirs $(OBJ_DIRS)
