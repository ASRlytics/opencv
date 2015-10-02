### OpenCV: Open Source Computer Vision Library

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

#### Some extra notes for installing on Ubuntu 14.04

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
* `sudo apt-get install python-dev python-numpy python3-dev python3-numpy`

#### Extra repairs:

* `/usr/include/linux/videodev.h` is not found in Ubuntu 14.04, so you need to do `sudo ln -s ../libv4l1-videodev.h videodev.h`

* /include/ffmpeg headers are also missing (this came from NetBSD stuffs), a possible hack might be:
    `cp /usr/include/libavcodec/*  /usr/include/ffmpeg/`
    `cp /usr/include/libavformat/*  /usr/include/ffmpeg/`
    `cp /usr/include/libswscale/*  /usr/include/ffmpeg/`


