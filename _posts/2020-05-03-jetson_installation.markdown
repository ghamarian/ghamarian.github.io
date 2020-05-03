
---
layout: post
title:  "Jetson Tx2, Torch and Opencv"
mathjax: true
date:   2020-05-03 10:11:31 +0100

categories: jekyll update
---
[Feature engineering] [feature-engineering]

```
for i in *; do echo $i; done
```
For installing torch in nvidia jetson Tx2, you can basically follow [Nvidia forum instructions] [Nvidia-forum-instruction]. I installed version 1.5 and the torch version was winbuild/v0.6.0.

Opencv installation was the trickiest part, things good to know:
1. You can install a virtualenv in a way that takes all the system packages with --system-site-packages option. 
2. `pyenv` by default does not create python shared library `CONFIGURE_OPTS=--enable-shared pyenv install 3.6.9` but in general it is better no to use pyenv
3. This video helped with [Opencv installation][Opencv-installation] and I used this [opencv script] [opencv-srcipt] to install.
4. The only modification I have done was to change the `CUDA_ARCH_BIN="5.3,6.2,7.2"`, to only 6.2, version 5.3 is for nano and Tx1.
5. When installing opencv from source make sure that after cmake step you have all the items including the shared library, python executable and numpy library are filled in.
6. A nice tool for checking options with cmake is cmake-curses-gui, then you can see all the options using ccmake.
7. To make opencv I used the path to my virtualenv and the system python basically.
8. In order to make opencv work in my virtualenv I needed to make a symbolink link to 
`cd /fruit/fruit/lib/python3.6/site-packages/`
`ln -s /usr/local/lib/python3.6/site-packages/cv2/python-3.6/cv2.cpython-36m-aarch64-linux-gnu.so  cv2.so`


[feature-engineering]: http://www.feat.engineering/
[Nvidia-forum-instruction]: https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-5-0-now-available/72048
[Opencv-installation]: https://www.jetsonhacks.com/2019/11/22/opencv-4-cuda-on-jetson-nano/
[Opencv-installation]: https://github.com/AastaNV/JEP/blob/master/script/install_opencv4.1.1_Jetson.sh
