### OpenCV: Open Source Computer Vision Library

[![Gittip](http://img.shields.io/gittip/OpenCV.png)](https://www.gittip.com/OpenCV/)

#### Resources

* Homepage: <http://opencv.org>
* Docs: <http://docs.opencv.org/master/>
* Q&A forum: <http://answers.opencv.org>
* Issue tracking: <https://github.com/Itseez/opencv/issues>

#### Contributing

Please read before starting work on a pull request: <https://github.com/Itseez/opencv/wiki/How_to_contribute>

Summary of guidelines:

* One pull request per issue;
* Choose the right base branch;
* Include tests and documentation;
* Clean up "oops" commits before submitting;
* Follow the coding style guide.

### Notes from chudur-budur:

#### Some extra notes for installation on Ubuntu 14.04:

I wanted to make this installation separate from the system (i.e. `/usr/local/..`), because I might need this to be installed on a remote cluster where I do have write access to `/usr/`. Therefore, I wanted to keep everything inside the opencv folder itself, all the stuffs will be kept like this --

```shell
	/opnecv/extracted/path/build	(for the opencv build directory)
	/opt/opencv/			(everything will be installed here)
```

The main problems are the dependencies, here I have found a list of possible way to solve them:

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

#### extra repairs:

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
	sudo ln -sf ../libavcodec/*.h .
	sudo ln -sf ../libavformat/*.h .
	sudo ln -sf ../libswscale/*.h .
```

#### cmake (with python bindings):
```shell
cd /opt/
sudo mkdir opencv
cd /opencv/extracted/path
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/opt/opencv/ \
	-DBUILD_DOCS=ON -DBUILD_EXAMPLES=ON -DWITH_QT=ON -DWITH_OPENGL=ON -DWITH_TBB=ON -DWITH_XINE=ON \
	-DPYTHON2_INCLUDE_DIR=$(python -c "from distutils.sysconfig import get_python_inc; \
				print(get_python_inc())") \
	-DPYTHON2_INCLUDE_DIRS=$(python -c "from distutils.sysconfig import get_python_inc; \
				print(get_python_inc())") \
	-DPYTHON2_EXECUTABLE=$(which python) \
	-DPYTHON2_PACKAGES_PATH=$(python -c "from distutils.sysconfig import get_python_lib; \
				print(get_python_lib())") \
	-DPYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; \
				print(get_python_inc())") \
	-DPYTHON3_INCLUDE_DIRS=$(python3 -c "from distutils.sysconfig import get_python_inc; \
				print(get_python_inc())") \
	-DPYTHON3_EXECUTABLE=$(which python3) \
	-DPYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; \
				print(get_python_lib())") \
	-DPYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3/dist-packages/numpy/core/include/ .. >& log.out
```
If you want to cmake multiple times, you need to clear the `CMakeCache.txt` everytime.

#### make:
Make is very lengthy, so to do it in parallel --
```shell
	make -j7
```
this will run 7 parallel jobs. To install --
```shell
	sudo make install
```
now opencv will be installed in `/opt/opencv`

#### g++/gcc flags:
* now you need to tell the system where to find the opencv libraries
```shell
	sudo bash -c 'echo "/opt/opencv/lib" > /etc/ld.so.conf.d/opencv.conf'
	sudo ldconfig
```
* set the `PKG_CONFIG_PATH` env. variable to `/opt/opencv/lib/pkgconfig`
* also, you need to add `/opt/opencv/lib` to `LD_LIBRARY_PATH`
* now you can get the list of compile flags to use them in your Makefile --
```shell
	user@pc:~$ pkg-config --cflags --libs opencv
	-I/opt/opencv/include/opencv -I/opt/opencv/include  -L/opt/opencv/lib -lopencv_shape -lopencv_stitching -lopencv_objdetect -lopencv_superres -lopencv_videostab -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_imgcodecs -lopencv_video -lopencv_photo -lopencv_ml -lopencv_imgproc -lopencv_flann -lopencv_core -lopencv_hal 	
```

#### python (2.7.6 and 3.4.3) support:
The python supports are already enabled in the above `cmake` command, after `make` see the output if you can find this comment at the end:
```shell
	Linking CXX shared library ../../lib/cv2.so
	[100%] Built target opencv_python2
	Linking CXX shared library ../../lib/python3/cv2.cpython-34m.so
	[100%] Built target opencv_python3
```
and after `make install` you supposed to have these two libraries in your system:
```shell
	user@pc:~$ locate cv2.so
	/usr/lib/python2.7/dist-packages/cv2.so
	user@pc:~$ locate cv2.cpython-34m.so
	/usr/lib/python3/dist-packages/cv2.cpython-34m.so
```
If everything looks fine, then check if you can use `cv2` module from python:
```shell
	user@pc:~$ python
	Python 2.7.6 (default, Jun 22 2015, 17:58:13) 
	[GCC 4.8.2] on linux2
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import cv2
	>>> print (cv2.__version__)
	3.0.0-dev
	>>> 
```
and for python3:
```shell
	Python 3.4.3 (default, Jul 28 2015, 18:20:59) 
	[GCC 4.8.4] on linux
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import cv2
	>>> print (cv2.__version__)
	3.0.0-dev
>>> 
```
