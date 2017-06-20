Build Tensorflow 1.2 with GPU support on Mac: possible errors and solutions
=====

Since v1.2, tensortflow dropped GPU support on Mac machines.
Following the instructions in the documentation https://www.tensorflow.org/install/install_sources to build from source may not work smoothly, some possible errors not mentioned in the official instructions are list here.

### AssertionError: Could not find python binary: python3
Solution: compile a development version of Bazel

    git clone Bazel
    bazel build //src:bazel
    sudo cp <Bazel directory of the master branch>/Bazel-bin/src/bazel /usr/local/bin/bazel
    
optional: comment out bazel version check in tensorflow configration file

    # MIN_BAZEL_VERSION=0.4.5

### dyld: Library not loaded: @rpath/libcudart.8.0.dylib
Solution: follow instrunction provided by @Vijay https://stackoverflow.com/questions/39865212/dyld-library-not-loaded-rpath-libcudart-8-0-dylib-while-building-tensorflow

### Segmentation fault: 11 error
Solution: create a symbolic link
    
    ln -sf /usr/local/cuda/lib/libcuda.dylib /usr/local/cuda/lib/libcuda.1.dylib

### clang: warning: argument unused during compilation: '-pthread' <br/> ld: library not found for -lgomp
Solution: use brew LLVM for linking

    brew install llvm
    brew link --force llvm
    #later: brew unlink llvm
