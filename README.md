1) build-shared/ 的作用
是 out-of-source 构建目录（构建产物目录），用于存放 CMake 生成的中间文件、Makefile、依赖关系和最终二进制，避免污染源码目录。
删掉后可重新 cmake -S . -B build-shared 再生成。

2) 目录/文件作用
顶层关键文件：

CMakeCache.txt：本次配置缓存（编译器、选项、路径等）。
Makefile：顶层构建入口（cmake --build 实际调用它）。
cmake_install.cmake：安装脚本入口，会 include 各子目录安装脚本。
cmake_install.cmakeL40-L46
if(NOT CMAKE_INSTALL_LOCAL_ONLY)
  include(".../platform/cmake_install.cmake")
  include(".../middleware/cmake_install.cmake")
  include(".../utils/cmake_install.cmake")
  include(".../src/cmake_install.cmake")
endif()
CMakeFiles/：

CMake 内部生成文件（依赖、编译器探测、二级 Makefile、进度标记等）。
3.16.3/CompilerIdC/*：编译器识别测试产物。
模块目录：utils/、platform/、middleware/、src/

各自的 Makefile：该模块的构建规则。
各自的 cmake_install.cmake：该模块安装规则脚本。
各自 CMakeFiles/<target>.dir/ 下常见文件：
build.make：具体编译/链接命令规则
flags.make：编译参数
DependInfo.cmake / depend.make：头文件依赖
link.txt：最终链接命令
cmake_clean.cmake：清理规则
lib/

目标库输出目录（.so/.a 预期在这里）。
由顶层 CMake 指定：
CMakeLists.txtL61-L64
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
3) 能否把 .so 编译到虚拟机 /opt？
可以。

当前工程没有 install(TARGETS ...)，所以 cmake --install 基本不会真正安装库文件（各子目录 cmake_install.cmake 也基本是空模板）。

因此推荐直接把输出目录改到 /opt：

bash
cmake -S /home/vboxuser/MQTT_SEVR/tuya-iot-core-sdk \
  -B /home/vboxuser/MQTT_SEVR/tuya-iot-core-sdk/build-shared \
  -DBUILD_SHARED_LIBS=ON \
  -DBUILD_EXAMPLES=OFF \
  -DCMAKE_LIBRARY_OUTPUT_DIRECTORY=/opt/tuya-iot-core-sdk/lib \
  -DCMAKE_ARCHIVE_OUTPUT_DIRECTORY=/opt/tuya-iot-core-sdk/lib \
  -DCMAKE_RUNTIME_OUTPUT_DIRECTORY=/opt/tuya-iot-core-sdk/bin
cmake --build /home/vboxuser/MQTT_SEVR/tuya-iot-core-sdk/build-shared -j
如果 /opt 无写权限，前面先执行：

bash
sudo mkdir -p /opt/tuya-iot-core-sdk/{lib,bin}
sudo chown -R $USER:$USER /opt/tuya-iot-core-sdk
