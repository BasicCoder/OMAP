# Ubuntu install opencv-3.2.0 and opencv-contrib
## Pip install 
Do not even give a try to install the wrapper package for OpenCV python bindings:  
`pip install opencv-python`  
The reason is that this easy way of doing things will lead you to install a package which is not enough mature as you can not even deal with videos and can not even display an image using `cv2.imshow()` function. So do not be lazy and follow the below steps.
## Step 1: Install dependencies
`$ sudo apt-get install build-essential ` </br>
`$ sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev ` </br>
`$ sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev ` </br>
`$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev liblapacke-dev ` </br>
`$ sudo apt-get install libxvidcore-dev libx264-dev ` </br>
`$ sudo apt-get install libatlas-base-dev gfortran ` </br>
`$ sudo apt-get install ffmpeg ` </br>
## Step 2: Download opencv-3.2.0 and opencv-contrib
`wget -O 3.2.0.zip https://github.com/Itseez/opencv/archive/3.2.0.zip ` </br>
`wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.2.0.zip ` </br>

`unzip 3.2.0.zip ` </br>
`unzip opencv_contrib.zip` </br>

## Step 3: Intall opencv-3.2.0 and opencv-contrib
`cd ~/3.2.0/ ` </br>
`mkdir build ` </br>
`cd build ` </br>
`cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D INSTALL_C_EXAMPLES=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=~/Work/opencv_contrib-3.2.0/opencv_contrib-3.2.0/modules/ \
    -D PYTHON_EXCUTABLE=/usr/bin/python \
    -D WITH_TBB=ON \
    -D WITH_V4L=ON \
    -D WITH_QT=OFF \
    -D WITH_GTK=ON \
    -D WITH_OPENGL=ON \
    -D BUILD_EXAMPLES=ON .. ` </br>

`make -j4
sudo make install
sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
sudo ldconfig
`
## Error:
`-- Checking for module 'gtk+-3.0'
--   No package 'gtk+-3.0' found
-- Checking for module 'gtk+-2.0'
--   Found gtk+-2.0, version 2.24.30
-- Checking for module 'gthread-2.0'
--   Found gthread-2.0, version 2.48.2
-- Checking for module 'gtkglext-1.0'
--   No package 'gtkglext-1.0' found
-- Checking for module 'gstreamer-base-1.0'
--   No package 'gstreamer-base-1.0' found
-- Checking for module 'gstreamer-video-1.0'
--   No package 'gstreamer-video-1.0' found
-- Checking for module 'gstreamer-app-1.0'
--   No package 'gstreamer-app-1.0' found
-- Checking for module 'gstreamer-riff-1.0'
--   No package 'gstreamer-riff-1.0' found
-- Checking for module 'gstreamer-pbutils-1.0'
--   No package 'gstreamer-pbutils-1.0' found
-- Checking for module 'gstreamer-base-0.10'
--   No package 'gstreamer-base-0.10' found
-- Checking for module 'gstreamer-video-0.10'
--   No package 'gstreamer-video-0.10' found
-- Checking for module 'gstreamer-app-0.10'
--   No package 'gstreamer-app-0.10' found
-- Checking for module 'gstreamer-riff-0.10'
--   No package 'gstreamer-riff-0.10' found
-- Checking for module 'gstreamer-pbutils-0.10'
--   No package 'gstreamer-pbutils-0.10' found`

### Solution:
`sudo apt-get install apt-file` </br>
`apt-file update ` </br>
`apt-file find gstreamer-base-1.0`</br>
`sudo apt-get install 'find-result'`</br>