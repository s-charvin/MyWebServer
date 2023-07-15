# ###############################################################################
# CMake 基本设置
# ###############################################################################

# ###############################################################################
# 导入要使用的模块配置
# ###############################################################################

# ###############################################################################
# 变量初始化
# ###############################################################################

# ###############################################################################
# 全局函数配置
# ###############################################################################

# ###############################################################################
# 项目设置
# ###############################################################################

# 添加可执行文件
add_executable(MyWebServerExecutable
  "src/log.cpp"
  "src/sqlconnpool.cpp"
  "src/epoller.cpp"
  "src/webserver.cpp"
  "src/heaptimer.cpp"
  "src/httpconn.cpp"
  "src/httprequest.cpp"
  "src/httpresponse.cpp"
  "src/buffer.cpp"
  "main.cpp"
)

# ###############################################################################
# 注入宏定义
# ###############################################################################

# ###############################################################################
# 子项目设置
# ###############################################################################

# ###############################################################################
# 头文件库设置
# ###############################################################################
target_include_directories(MyWebServerExecutable
  INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:include>) # 构建时使用使用当前源码路径作为头文件搜索路径, 安装时使用 include 作为头文件搜索路径

# 设置头文件搜索路径

set(PUBLIC_INCLUDE_DIRS
  "${PROJECT_BINARY_DIR}"
  "${CMAKE_CURRENT_SOURCE_DIR}/include"
  "D:/vcpkg/installed/x64-windows/include"
)

set(PRIVATE_INCLUDE_DIRS
  ""
)

target_include_directories(MyWebServerExecutable
  PUBLIC ${PUBLIC_INCLUDE_DIRS}
  PRIVATE ${PRIVATE_INCLUDE_DIRS}
)

# ###############################################################################
# 依赖库设置
# ###############################################################################

# 链接子项目的依赖库
# target_link_libraries(Analyzer PRIVATE AVSAlgorithm)
# target_link_libraries(Analyzer PRIVATE AVSAlarmManage)
set(PUBLIC_LINK_LIBRARIES

  "$<$<CONFIG:Debug>:"
  ""
  ">"
  "$<$<CONFIG:Release>:"
  ""
  ">"
)

set(PRIVATE_LINK_LIBRARIES "")

target_link_libraries(MyWebServerExecutable
  PUBLIC ${PUBLIC_LINK_LIBRARIES}
  PRIVATE ${PRIVATE_LINK_LIBRARIES})

# ###############################################################################
# 文件预处理
# ###############################################################################
set(DLL_FILES
  "${CMAKE_CURRENT_SOURCE_DIR}/resources/*"
)

message(STATUS "DLL_FILES: ${DLL_FILES}")

# 拷贝文件
add_custom_command(TARGET MyWebServerExecutable POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_if_different
  ${DLL_FILES}
  $<TARGET_FILE_DIR:MyWebServerExecutable>
)
add_custom_target(copy_dlls ALL DEPENDS MyWebServerExecutable)

# ###############################################################################
# 打包规则
# ###############################################################################

# ################################################################################
# # 安装规则
# ################################################################################
