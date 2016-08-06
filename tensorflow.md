# Note: TensorFlow
This is a learning note of tensorflow.
### Agenda
* [Installation(MacOSX)](#installation)

### Installation
Reference: 
- https://www.tensorflow.org/versions/r0.10/get_started/os_setup.html#test-the-tensorflow-installation
- https://github.com/tensorflow/tensorflow/issues/1402
```sh
# TensorFlow Version: 0.10.0;
# Installed System: Mac OS X El Capitan 10.11.5, CPU only;
# Input the following commands in order;

# Install pip
$ sudo easy_install pip

# Install tensorflow
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-0.10.0rc0-py2-none-any.whl
$ sudo pip install --upgrade $TF_BINARY_URL

# Install bazel and swig
$ brew install bazel swig

# Configuration
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] N
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with GPU support? [y/N] N

# Build
$ bazel build -c opt //tensorflow/tools/pip_package:build_pip_package
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
$ sudo pip install /tmp/tensorflow_pkg/tensorflow-0.10.0rc0-py2-none-any.whl --ignore-installed six

# Upgrade six
$ sudo easy_install --upgrade six

# Link(under tensorflow root directory)
$ mkdir _python_build
$ cd _python_build
$ ln -s ../bazel-bin/tensorflow/tools/pip_package/build_pip_package.runfiles/org_tensorflow/* .
$ ln -s ../tensorflow/tools/pip_package/* .
$ python setup.py develop

# Test
$ cd tensorflow/models/image/mnist
$ python convolutional.py
```
