# Install Caffe
安装Caffe 之前请先按照好Opencv，具体请参照 **InstallOpencv3.2.0**
## Download Caffe
`git clone https://github.com/BVLC/caffe.git`
## Compiling Caffe
### 1.
`cd ~/caffe` </br>
`cp Makefile.config.example Makefile.config` </br>

修改 `Makefile.config` 配置：
`USE_CUDNN := 1` </br>
`OPENCV_VERSION := 3` </br>
根据自己的需要对 Python 解释器进行选择


### 2.
`mkdir build` </br>
`cd build` </br>
`make clean #第一次编译，不需要执行` </br>
`cmake` </br>

cmake 报告当中会出现提示错误，缺少库等，需要慢慢进行解决

`make all -j4` </br>
`make test` </br>
`make runtest #可能需要指定CUDA_VISIBLE_DEVICES=*` </br>

编译pycaffe
### 3.
`make pycaffe`

### 4.
将 caffe_root/python 添加到 ~/.bashrc 中:

`export PYTHONPATH=caffe_root/python:$PYTHONPATH` </br>
`source ~/.bashrc`

或者是 github 上 clone 下来的项目
编写文件 bashcaffe:
` export PYTHONPATH=`pwd`/caffe_root/python:$PYTHONPATH `
` export PYTHONPATH=`pwd`/lib:$PYTHONPATH` `

执行:
source bashcaffe

## 重新编译项目
### (1).
从 github 上 clone 下来的 Caffe 项目，因为可能添加了一些新层，所以需要对项目进行重新编译，CUDA8.0 和cudnn5.1 在 python2.7 环境下支持 Caffe 这种操作
因为服务器的 cudnn 版本为其他(cudnn6.0) 不支持这种操作，需要进行切换，但是又不能影响其他人，在查看 cmake 生成的报告文件中，发现

`-- Found cuDNN: ver. 5.1.10 found (include: /home/bc/anaconda3/envs/python2.7/include, library: /home/bc/anaconda3/envs/python2.7/lib/libcudnn.so) ` 

尝试利用 conda 来配置 cudnn:

` conda install cudnn=5.1 `

安装成功后，重新编译项目 Caffe， 编译通过。 

### (2).(没有采用这种方法，未经过测试)

当电脑中存在多个版本的 CuDNN 的时候 caffe 该如何“抉择”？
#### 1. 从/usr/local/cuda目录下移除cudnn相关的文件(这一步其实不是必须的，因为后面的两个步骤3-4的操作会使新版本的cudnn的优先级高于原有的版本)，操作如下：

` sudo rm /usr/local/cuda/lib64/libcudnn* ` </br>
` sudo rm /usr/local/cuda/include/cudnn.h ` </br>
#### 2. 
新建一个目录集中管理各个版本的cudnn库,比如 ~/cudnn/cudnn_v5 存放第五版，~/cudnn/cudnn_v6存放第六版；
#### 3.
修改 caffe 根目录下的 Makefile.config 的 INCLUDE_DIRS, 添加对应版本的 cudnn 的include 目录，添加lib64目录到LIBRARY_DIRS(通过这一步修改之后，在编译caffe的过程中已经能成功找到对应的头文件和静态库)；
#### 4.
修改.bashrc文件或者.profile文件末尾,添加对应版本cudnn目录下的lib64到LD_LIBRARY_PATH(这一步是为了让caffe的程序在运行的过程中，注意是运行而非编译，找到对应的cudnn动态库)：

` export LD_LIBRARY_PATH=/home/XXX/cudnn/cudnn_v6/lib64:$LD_LIBRARY_PATH `
` source ~/.bashrc` 

## Error
### 使用Anaconda 环境
ubuntu16.04安装caffe时，使用Anaconda带的库进行make.将 Makefile.config 里面的设置修改为：（注意anaconda后面的数字与本机的相符）
* CUDA=8.0
* CUDNN=5.1

`sh
ANACONDA_HOME := $(HOME)/anaconda3/envs/python2.7 
PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
         $(ANACONDA_HOME)/include/python2.7 \
         $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include \
PYTHON_LIB := $(ANACONDA_HOME)/lib
`

然后make过程中遇到以下错误：

`sh
CXX/LD -o .build_release/tools/upgrade_net_proto_text.bin
/usr/bin/ld: warning: libjpeg.so.9, needed by /home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so, not found (try using -rpath or -rpath-link)
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_finish_decompress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_set_quality@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_quality_scaling@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_destroy_decompress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_resync_to_restart@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_CreateDecompress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_alloc_huff_table@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_set_defaults@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_CreateCompress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_stdio_src@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_finish_compress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_calc_output_dimensions@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_std_error@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_default_qtables@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_start_decompress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_write_scanlines@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_simple_progression@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_read_header@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_read_scanlines@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_start_compress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_destroy_compress@LIBJPEG_9.0'
/home/bc/anaconda3/envs/python2.7/lib/libopencv_imgcodecs.so: undefined reference to `jpeg_stdio_dest@LIBJPEG_9.0'
collect2: error: ld returned 1 exit status
Makefile:619: recipe for target '.build_release/tools/caffe.bin' failed
make: *** [.build_release/tools/caffe.bin] Error 1
make: *** Waiting for unfinished jobs....
`

发现是找不到libpng16.so.16与libjpeg.so.9文件．打开anaconda中的…anaconda2/lib路径，发现anaconda下有libpng16.so.16与libjpeg.so.9，于是进行如下操作：
1. warning: libjpeg.so.9, needed by /home/.../anaconda2/lib/libopencv_imgcodecs.so, not found

`sh
cd /usr/lib/x86_64-linux-gnu
sudo ln -s ~/anaconda2/lib/libpng16.so.16 /usr/lib/
sudo ln -s ~/anaconda2/lib/libjpeg.so.9 /usr/lib/
sudo ldconfig
`
为这两个文件在/usr/lib/下建立一个同步的链接． 
再次make，问题消失．
