cmake_minimum_required(VERSION 3.2)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{VITASDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

project(tamatool-vita)
include("${VITASDK}/share/vita.cmake" REQUIRED)

find_package(SDL2 REQUIRED)

set(VITA_APP_NAME "Tamatool")
set(VITA_TITLEID  "TAMA99999")
set(VITA_VERSION  "00.10")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu17")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")

# add_definitions( -DIS_VITA ) #vitasdk uses __vita__

set(SRC_DIR ../src)

file(GLOB_RECURSE SOURCES "${SRC_DIR}/*.c")

include_directories(
  ${SRC_DIR}
  ${SRC_DIR}/lib
  common/
  ../res/
)

add_executable(${PROJECT_NAME}
  ${SOURCES}
  ${SRC_DIR}/lib
  common/debugScreen.c
)

target_link_libraries(${PROJECT_NAME}
  SceDisplay_stub
  SceIofilemgrForDriver_stub
  SceLibKernel_stub #needed?
  pthread           #needed?
  SDL2_image
  SDL2::SDL2
  png
  z
)

#add_subdirectory(${SRC_DIR}/lib)

vita_create_self(${PROJECT_NAME}.self ${PROJECT_NAME})
vita_create_vpk(${PROJECT_NAME}.vpk ${VITA_TITLEID} ${PROJECT_NAME}.self
  VERSION ${VITA_VERSION}
  NAME ${VITA_APP_NAME}
  #FILE tama/rom.bin rom.bin
        # Application assets
  FILE ../res/background.png res/background.png
  FILE ../res/icons.png res/icons.png
  FILE ../res/shell.png res/shell.png
        # LiveArea assets
  FILE sce_sys/icon0.png sce_sys/icon0.png
  FILE sce_sys/livearea/contents/bg.png sce_sys/livearea/contents/bg.png
  FILE sce_sys/livearea/contents/startup.png sce_sys/livearea/contents/startup.png
  FILE sce_sys/livearea/contents/template.xml sce_sys/livearea/contents/template.xml
        # LiveArea manual
  FILE sce_sys/tamamanual/fullman-001.png sce_sys/manual/001.png
  FILE sce_sys/tamamanual/fullman-002.png sce_sys/manual/002.png
  FILE sce_sys/tamamanual/fullman-003.png sce_sys/manual/003.png

)
