
## 安装环境：
* mac os 10.12
* python 2.7.9
* virtualenv
* 已安装tesseract

## 主要参考文章：
* http://www.pyimagesearch.com/2016/11/28/macos-install-opencv-3-and-python-2-7/

## 主要问题：
### 1. cmake设置中无法找到PYTHON2_LIBRARY、PYTHON2_INCLUDE_DIR
* PYTHON2_LIBRARY:
`echo $(python-config --prefix)/lib/libpython2.7.dylib`
* DPYTHON_INCLUDE_DIR:
`echo $(python -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())")`

### 2. cmake报路径错误
注意cmake最后的`..`不可省略

### 3. `make -j4` 报*** [all] Error 2 错误
如显示waiting jobs...类似，不断重新make即可
 
### 4. `make -j4` 报'QTKit/QTKit.h' file not found 错误
需要使用opencv及contrib的master版本，不能使用3.0

### 5. `make -j4` 报'tesseract/baseapi.h' file not found 错误
参考如下链接：https://github.com/opencv/opencv_contrib/issues/920

在cmake中添加` -D ENABLE_PRECOMPILED_HEADERS=OFF`，删除build文件夹，重新cmake

### 6. `make -j4` 报'Python.h' file not found 错误
是cmake中`PYTHON2_INCLUDE_DIR`设置错误，参考问题1即可

## cmake模板
安装的主要问题在于cmake的配置，以下是我的模板：
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=...opencv_contrib/modules \
    -D PYTHON2_LIBRARY=/System/Library/Frameworks/Python.framework/Versions/2.7/lib/libpython2.7.dylib \
    -D PYTHON2_INCLUDE_DIR=/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7 \
    -D PYTHON2_EXECUTABLE=$VIRTUAL_ENV/bin/python \
    -D BUILD_opencv_python2=ON \
    -D BUILD_opencv_python3=OFF \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D INSTALL_C_EXAMPLES=OFF \
    -D ENABLE_PRECOMPILED_HEADERS=OFF \
    -D BUILD_EXAMPLES=ON ..
```
OPENCV_EXTRA_MODULES_PATH 需要指定为你clone下来的opencv_contrib的路径

