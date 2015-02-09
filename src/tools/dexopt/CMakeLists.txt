project(dexopt)

add_definitions(-DHAVE_SYS_UIO_H -DANDROID_SMP -DDVM_SHOW_EXCEPTION=1 -DWITH_JIT -DWITH_JIT_TUNING -DARCH_IA32 -DDVM_JMP_TABLE_MTERP=1 -DMTERP_STUB -fpermissive -w -DARCH_VARIANT="x86")
if(SystemIsBigEndian)
    add_definitions(-DHAVE_BIG_ENDIAN)
else()
    add_definitions(-DHAVE_LITTLE_ENDIAN)
endif()
set(REPO_PATH ${DALVIK_PATH})
set(MOD_PATH ${REPO_PATH}/dexopt)

include_directories(${REPO_PATH}
                    ${MODULE_PATH}
                    ${LIBENC_PATH}
                    ${LIBCUTILS_INCLUDES}
                    ${CMAKE_INSTALL_PREFIX}/include/android-libcore #FIXME
                    ${LIBNATIVEHELPER_INCLUDEDIRS}
                    ${LIBANDROIDCORE_INCLUDEDIRS}
                    ${LIBANDROIDLIBCORE_INCLUDEDIRS}
                    ${DALVIK_PATH}/vm
)

set(dexopt_SRCS
   ${MOD_PATH}/OptMain.cpp
)

add_executable(dexopt ${dexopt_SRCS})
target_link_libraries(dexopt ${STANDARD_SYS_LIBS} corkscrew dex cutils log nativehelper safe-iop utils dvm jni binder shashlik_launcher_helper icuuc icui18n)
install(TARGETS dexopt DESTINATION ${ANDROID_BIN_DIR})