### Notes from chudur-budur:

#### Some extra notes for installation on Ubuntu 14.04

The main problems are the dependencies, here I have found a list of possible dependencies:

* first get rid of the craps: `sudo apt-get autoremove libopencv-dev python-opncv`
* build tools: `sudo apt-get install cmake build-essential` 
* gtk: `sudo apt-get install libgtk2.0-dev libgtkglext1-dev`
* gui: `sudo apt-get install libqt4-dev libpangox-1.0-dev libxmu-dev libxmu-headers`
* media I/O: `sudo apt-get install zlib1g-dev libjpeg-dev libpng12-dev libtiff4-dev libjasper-dev libgphoto2-2-dev`
* video I/O: `sudo apt-get install libdc1394-22 libdc1394-22-dev libavcodec-dev libavformat-dev libavresample-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev v4l-utils libxine2-dev libmp3lame-dev libfaac-dev yasm`
* linear algebra: `sudo apt-get install libeigen3-dev`
* parallel computation: `sudo apt-get install libtbb-dev ocl-icd-opencl-dev opencl-headers`

#### python wrappers:
* Install these --
```shell
	sudo apt-get install python-dev python-numpy python3-dev python3-numpy
```

#### Extra repairs:

* `/usr/include/linux/videodev.h` is not found in Ubuntu 14.04, so you need to do 
```shell
	cd /usr/include/linux
	sudo ln -s ../libv4l1-videodev.h videodev.h
```

* /include/ffmpeg headers are also missing (this came from NetBSD stuffs), a possible hack might be:
```shell
	cd /usr/include
	mkdir ffmpeg
	cd ffmpeg
	sudo ln -sf ../libavcodec/* .
	sudo ln -sf ../libavformat/* .
	sudo ln -sf ../libswscale/* .
```

#### cmake:
```shell
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/media/khaled/data/research/opencv/bin -DBUILD_DOCS=ON -DBUILD_EXAMPLES=ON -DWITH_QT=ON -DWITH_OPENGL=ON -DWITH_TBB=ON -DWITH_XINE=ON -DPYTHON3_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.4 -DPYTHON_INCLUDE_DIR2=/usr/include/x86_64-linux-gnu/python2.7 -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython2.7.so -DPYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3/dist-packages/numpy/core/include/ ..
```
