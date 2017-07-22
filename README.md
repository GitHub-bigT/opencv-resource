opencv 3.2.0

下载资源地址：https://github.com/opencv/opencv_3rdparty/branches/all

ffmpeg问题：

CMake Error at cmake/OpenCVUtils.cmake:1043 (file):
  file DOWNLOAD HASH mismatch
    for file: [D:/openCV/opencv-3.2.0/3rdparty/ffmpeg/downloads/f081abd9d6ca7e425d340ce586f9c090/opencv_ffmpeg.dll]
      expected hash: [f081abd9d6ca7e425d340ce586f9c090]
        actual hash: [d41d8cd98f00b204e9800998ecf8427e]
             status: [7;"Couldn't connect to server"]

Call Stack (most recent call first):
  3rdparty/ffmpeg/ffmpeg.cmake:10 (ocv_download)
  cmake/OpenCVFindLibsVideo.cmake:219 (include)
  CMakeLists.txt:557 (include)


CMake Error at cmake/OpenCVUtils.cmake:1047 (message):
  Failed to download opencv_ffmpeg.dll.  Status=7;"Couldn't connect to
  server"
  
Call Stack (most recent call first):
  3rdparty/ffmpeg/ffmpeg.cmake:10 (ocv_download)
  cmake/OpenCVFindLibsVideo.cmake:219 (include)
  CMakeLists.txt:557 (include)

解决方法：

1.在最上面链接中手动下载ffmpeg目录中的内容，解压。（仓库中存了一份，也可以直接在这里下载）

2.复制opencv_ffmpeg.dll、opencv_ffmpeg_64.dll、ffmpeg_version.cmake到xxx\opencv-3.2.0\3rdparty\ffmpeg目录下。

3.更改ffmpeg.cmake文件为

message(STATUS "FFMPEG: Package successfully downloaded")
include(${CMAKE_CURRENT_LIST_DIR}/ffmpeg_version.cmake)

参考链接：http://blog.csdn.net/yiyuehuan/article/details/52951574

ippicv问题：

CMake Error at 3rdparty/ippicv/downloader.cmake:73 (file):
  file DOWNLOAD HASH mismatch
    for file: [D:/openCV/opencv-3.2.0/3rdparty/ippicv/downloads/windows-04e81ce5d0e329c3fbc606ae32cad44d/ippicv_windows_20151201.zip]
      expected hash: [04e81ce5d0e329c3fbc606ae32cad44d]
        actual hash: [d41d8cd98f00b204e9800998ecf8427e]
             status: [7;"Couldn't connect to server"]

Call Stack (most recent call first):
  3rdparty/ippicv/downloader.cmake:110 (_icv_downloader)
  cmake/OpenCVFindIPP.cmake:243 (include)
  cmake/OpenCVFindLibsPerf.cmake:37 (include)
  CMakeLists.txt:558 (include)


CMake Error at 3rdparty/ippicv/downloader.cmake:77 (message):
  ICV: Failed to download ICV package: ippicv_windows_20151201.zip.
  Status=7;"Couldn't connect to server"
  
Call Stack (most recent call first):
  3rdparty/ippicv/downloader.cmake:110 (_icv_downloader)
  cmake/OpenCVFindIPP.cmake:243 (include)
  cmake/OpenCVFindLibsPerf.cmake:37 (include)
  CMakeLists.txt:558 (include)
  
解决方案：
1.在最上面链接中手动下载ippicv目录中的内容，解压。（仓库中存了一份，也可以直接在这里下载）

2.打开xxx\opencv_3rdparty-ippicv-master_20170418\ippicv\ippicv_2017u2_win_ia32_20170418.zip解压ippicv_wind

3.复制ippicv_wind到目录D:\openCV\opencv-3.2.0\3rdparty\ippicv\unpack（unpack是新建的文件夹）

4.修改downloader.cmake内容为

#
# The script downloads ICV package
#
# On return this will define:
# OPENCV_ICV_PATH - path to unpacked downloaded package
#

function(_icv_downloader)
  # Commit SHA in the opencv_3rdparty repo
  set(IPPICV_BINARIES_COMMIT "81a676001ca8075ada498583e4166079e5744668")
  # Define actual ICV versions
  if(APPLE)
    set(OPENCV_ICV_PACKAGE_NAME "ippicv_macosx_20151201.tgz")
    set(OPENCV_ICV_PACKAGE_HASH "4ff1fde9a7cfdfe7250bfcd8334e0f2f")
    set(OPENCV_ICV_PLATFORM "macosx")
    set(OPENCV_ICV_PACKAGE_SUBDIR "/ippicv_osx")
  elseif(UNIX)
    if(ANDROID AND NOT (ANDROID_ABI STREQUAL x86 OR ANDROID_ABI STREQUAL x86_64))
      return()
    endif()
    set(OPENCV_ICV_PACKAGE_NAME "ippicv_linux_20151201.tgz")
    set(OPENCV_ICV_PACKAGE_HASH "808b791a6eac9ed78d32a7666804320e")
    set(OPENCV_ICV_PLATFORM "linux")
    set(OPENCV_ICV_PACKAGE_SUBDIR "/ippicv_lnx")
  elseif(WIN32 AND NOT ARM)
    set(OPENCV_ICV_PACKAGE_NAME "ippicv_windows_20151201.zip")
    set(OPENCV_ICV_PACKAGE_HASH "04e81ce5d0e329c3fbc606ae32cad44d")
    set(OPENCV_ICV_PLATFORM "windows")
    set(OPENCV_ICV_PACKAGE_SUBDIR "/ippicv_win")
  else()
    return() # Not supported
  endif()

  set(OPENCV_ICV_UNPACK_PATH "${CMAKE_CURRENT_LIST_DIR}/unpack")
  set(OPENCV_ICV_PATH "${OPENCV_ICV_UNPACK_PATH}${OPENCV_ICV_PACKAGE_SUBDIR}")

  message(STATUS "ICV: Package successfully downloaded")
  set(OPENCV_ICV_PATH "${OPENCV_ICV_PATH}" PARENT_SCOPE)
endfunction()

_icv_downloader()

配置完之后cmake提示Configuring done则为成功

打开OpenCV.slu Debug 32位

rebuild

Error 4 error C1083: Cannot open include file: 'ipp.h': No such file or directory D:\openCV\opencv-3.2.0\modules\core\include\opencv2\core\private.hpp 200

解决方案：打开cmake  取消勾中with_ipp
