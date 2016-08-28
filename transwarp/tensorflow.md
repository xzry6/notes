# Note: TensorFlow
This is a learning note of tensorflow.
### Agenda
* [Installation(MacOSX)](#installation)
* [Usage](#usage)
* [Implementation](#implementation)

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

### Usage
#### Terms
* Graphs: Computations are represented as graphs in tensorflow;
  - Normal edges: Tensors;
  - Special edges: Control Dependencies;
    - Add dependencies to independent states;
    - Control the peak memory usage;
* Ops: Nodes in graphs are called ops;
  - Attributes: An op can have attributes;
    - Must be provided at construction time;
    - Can be used to make ops polymorphic;
  - Kernels: Particular implementations of ops than can be run on a particular type of device;
  - Variables: A special type of op that survives throughout the execution, typically a parameter is held as a variable;
* Tensors: a typed multi-dimensional array, could be used as inputs or produced as outputs in ops;
* Sessions: Sessions place graph ops on devices, such as CPUs and GPUs, and provide methods to excute them;
  - Use *Extend* method to add nodes and edges;
  - Use *Run* method to compute the output which were requested;

### Implementation
- Single-Device: Having dependencies' counter, a node would be executed once the dependency counter drops to 0;
- Multi-Device: 
  - Distribute the workload;
    - Use a greedy heuristic;
  - Communicate with the devices;
    - Add *Send* and *Receive* nodes;
    - Canonicalize all interfaces receiving the same tensor to a single interface;
- Fault Tolerance: 
  - An error in a communication between a *Send* and *Receive* pair;
  - Periodic health-checks from the master process to every worker process;
