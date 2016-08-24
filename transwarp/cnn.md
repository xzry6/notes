# Note: Convolutional Neural Network(CNN)
This is a learning note of CNN. Part of this note is with reference to [Standford Deep Learning Tutorial](http://ufldl.stanford.edu/tutorial/supervised/ConvolutionalNeuralNetwork/).

### Agenda
* [Structure](#structure)
* [Convolution](#convolution)
* [Pooling](#pooling)
* [Others](#others)
* [TensorFlow Sample](https://www.tensorflow.org/versions/r0.10/tutorials/mnist/pros/index.html)

### Structure
![cnnStructure](http://parse.ele.tue.nl/cluster/2/CNNArchitecture.jpg)

### Convolution
Since images have the 'stationary' property, which implies that features that are useful in one region are also likely to be useful for other regions. Thus, we use a rather small patch than the image size to convolve this image. For example, when having an image with m * m * r, we could use a n * n * q patch(where n < m && q <= r). The output will be produced in size m - n + 1.

![convolutionGif](http://ufldl.stanford.edu/tutorial/images/Convolution_schematic.gif)

### Pooling
To further reduce computation, pooling is introduced in CNN. As mentioned previously, a mean pooling is applied into each region of the image because of the 'stationary' property. Pooling usually ranges from 2 to 5.

![poolingGif](http://ufldl.stanford.edu/tutorial/images/Pooling_schematic.gif)

### Others
* Densely Connected Layers: after several convolutional layers, a few densely conncetedly layers are usually constructed before the output layer;
* Top Layer Classifier: a top classifier is used to do supervised learning on CNN;
* Back Propogation: unsample on pooling layer and use the flipped filter on convolution layer;
