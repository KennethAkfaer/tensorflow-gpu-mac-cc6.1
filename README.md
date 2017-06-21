Build Tensorflow 1.2 with GPU support on Mac: possible errors and solutions
=====

* As of version 1.2, TensorFlow no longer provides GPU support on Mac OS X.
-----
Following the instructions in the [offcial documentation](https://www.tensorflow.org/install/install_sources) to build from source may not always work smoothly. Some possible errors not mentioned in the official instructions are listed here.

### AssertionError: Could not find python binary: python3
Solution: compile a development version of Bazel
```Bash
git clone Bazel
bazel build //src:bazel
sudo cp /Bazel-bin/src/bazel /usr/local/bin/bazel
```

optional: comment out bazel version check in tensorflow configration file
```Bash
# MIN_BAZEL_VERSION=0.4.5
```

### dyld: Library not loaded: @rpath/libcudart.8.0.dylib
Solution: follow instrunction provided by @Vijay https://stackoverflow.com/questions/39865212/dyld-library-not-loaded-rpath-libcudart-8-0-dylib-while-building-tensorflow

### Segmentation fault: 11 error
Solution: create a symbolic link
```Bash
ln -sf /usr/local/cuda/lib/libcuda.dylib /usr/local/cuda/lib/libcuda.1.dylib
```

### clang: warning: argument unused during compilation: '-pthread' <br/> ld: library not found for -lgomp
Solution: use brew LLVM for linking
```Bash
brew install llvm
brew link --force llvm
#later: brew unlink llvm
```

    Comment: I used Command Line Tools for Xcode 8.0 for compiling and the latest llvm in brew for linking

A compiled whl for compute capability 6.1 (build on a Mac Pro with 1080ti) can be downloaded [here](https://pan.baidu.com/s/1jHM1Eh4)<br/>
The configure script: 
       
    Do you wish to build TensorFlow with MKL support? [y/N] n
    No MKL support will be enabled for TensorFlow
    Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 
    Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] n
    Do you wish to build TensorFlow with Hadoop File System support? [y/N] n
    Do you wish to build TensorFlow with the XLA just-in-time compiler (experimental)? [y/N] n
    Do you wish to build TensorFlow with VERBS support? [y/N] n
    Do you wish to build TensorFlow with OpenCL support? [y/N] n
    Do you wish to build TensorFlow with CUDA support? [y/N] y
    Do you want to use clang as CUDA compiler? [y/N] n
    nvcc will be used as CUDA compiler
    Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 8.0]: 
    Please specify the location where CUDA  toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 
    Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 
    Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 6.0]: 5
    Please specify the location where cuDNN 5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 
    Please specify a list of comma-separated Cuda compute capabilities you want to build with.
    You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
    Please note that each additional compute capability significantly increases your build time and binary size.
    [Default is: "3.5,5.2"]: 6.1
    Do you wish to build TensorFlow with MPI support? [y/N] n
    Configuration finished
