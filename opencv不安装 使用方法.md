# opencv 不 make install的使用方法

写这个教程的目的是自己之前电脑上装了opencv3.2,然后由于项目需要,需要装2版本的opencv,而自己又不想装2,怕之后会出各种问题,所以就没有make install.红色部分是需要注意的地方.

方法:

1. 和ubuntu 安装opencv 方法基本一致:

    cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/home/zhang/software_install/opencv-2.4.13/build ..

    需要注意的是 cmake 的选项CMAKE_INSTALL_PREFIX <font color=red>自己不确定要不要</font> ,自己在第一次安装不成功提示:

    undefined reference to `png_write_end@PNG16_0'


   等一大堆错误的时候,自己是直接cmake . .  的,没加后面的路径,但是个人觉得不是路径的原因,只能说有可能.

2. 然后是:

   sudo make -j16

3. <font color=red>make完成后在build文件夹的终端下:</font>

    sudo ldconfig

    个人觉得是这句话起了作用,解决了上面 1 里面的一大堆未定义的问题.

在 cmkelist 中调用没有 install 的opencv2 的命令:

    set(OpenCV_DIR /home/zhang/software_install/opencv-2.4.13/build/)

    find_package(OpenCV 2.4.13 REQUIRED)

    include_directories(${OpenCV_INCLUDE_DIRS})

另外,自己安装了cuda,一开始也报了一个错,具体什么错误自己没截图保存,只记下一个解决方法吧,在cmakelist中还需要加入:

    set(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
这么一句话.