cmake_minimum_required(VERSION 3.6.0)
project(rubberband-android)


#  "src/audiocurves/*.*"
#  "src/base/*.*"
#  "src/dsp/*.*"
#  "src/float_cast/*.*"
#  "src/kissfft/*.*"
#  "src/pommier/*.*"
#  "src/speex/*.*"
#  "src/system/*.*"
#  "src/*.*"

file(GLOB rubberband_src
    "src/base/Profiler.cpp"
    "src/system/Thread.cpp"
    "src/system/Allocators.cpp"
    "src/system/sysutils.cpp"
    "src/system/VectorOpsComplex.cpp"
    "src/StretcherChannelData.cpp"
    "src/dsp/AudioCurveCalculator.cpp"
    "src/dsp/FFT.cpp"
    "src/dsp/Resampler.cpp"
    "src/audiocurves/SilentAudioCurve.cpp"
    "src/audiocurves/CompoundAudioCurve.cpp"
    "src/audiocurves/HighFrequencyAudioCurve.cpp"
    "src/audiocurves/SpectralDifferenceAudioCurve.cpp"
    "src/audiocurves/ConstantAudioCurve.cpp"
    "src/audiocurves/PercussiveAudioCurve.cpp"
    "src/StretcherImpl.cpp"
    "src/StretcherProcess.cpp"
    "src/StretchCalculator.cpp"
    "src/RubberBandStretcher.cpp"
    "src/rubberband-c.cpp"
    "src/speex/resample.c"
    "src/kissfft/kiss_fft.c"
    "src/kissfft/kiss_fftr.c"
)

add_library(rubberband-android STATIC ${rubberband_src})

include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/src ${CMAKE_CURRENT_SOURCE_DIR}/src/kissfft ${CMAKE_CURRENT_SOURCE_DIR}/src/system)

# Macro definitions
add_definitions(-DUSE_PTHREADS)
add_definitions(-D_LIB -D_USE_MATH_DEFINES -DHAVE_KISSFFT -DUSE_SPEEX)

# CXX_FLAGS
# set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -DNDEBUG -DNO_THREAD_CHECKS -DNO_TIMING")
# set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -ffast-math -mfpmath=sse -msse -msse2 -O3 -ftree-vectorize")

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -DDEBUG -DFFT_MEASUREMENT")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3 -DNDEBUG -DNO_THREAD_CHECKS -DNO_TIMING -DNO_TIMIN_COMPLETE_NOOP -DPOMMIER_MATHFUN -D__ARM_ARCH_/__")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -mfpu=neon -mfloat-abi=softfp -ffast-math -freciprocal-math -mfpmath=sse -msse -msse2 -O3 -ftree-vectorize -fsingle-precision-constant")
