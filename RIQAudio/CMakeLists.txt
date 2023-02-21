cmake_minimum_required(VERSION 3.25.2) # does the cmake minium version really need to be this high?
project("RIQAudio" C)
set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 20)

file(GLOB SOURCES src/*.c src/*.cpp src/*.h src/*.hpp)

# ripping miniaudio(and some others) files straight off the repo
# git submodules are for losers
# source: https://github.com/mackron/miniaudio
set(MINIAUDIO_REPO "https://raw.githubusercontent.com/mackron/miniaudio/master")
set(MINIAUDIO_LOCAL "${PROJECT_SOURCE_DIR}/vendor/miniaudio")

set(MINIAUDIO_FILES
    miniaudio.h
    extras/miniaudio_libopus.h
    extras/miniaudio_libvorbis.h
)

foreach(i IN LISTS MINIAUDIO_FILES)
    message("dowloading: ${i}")
    file(DOWNLOAD "${MINIAUDIO_REPO}/${i}" ${MINIAUDIO_LOCAL}/${i})
endforeach(i in)


# ripping stb_vorbis from stb
# NOTE: due to turning this being a source file, including the header more than twice could be problematic
# source: https://github.com/nothings/stb
message("dowloading: extras/stb_vorbis.c")
file(DOWNLOAD "https://raw.githubusercontent.com/nothings/stb/master/stb_vorbis.c" ${MINIAUDIO_LOCAL}/extras/stb_vorbis.c)

# and lastly stealing from dr_libs
# source: https://github.com/mackron/dr_libs
message("dowloading: extras/dr_wav.h")
file(DOWNLOAD "https://raw.githubusercontent.com/mackron/dr_libs/master/dr_wav.h" ${MINIAUDIO_LOCAL}/extras/dr_wav.h)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/vendor)

add_library(${PROJECT_NAME} SHARED ${SOURCES})